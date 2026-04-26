---
title: MaxMES CI/CD 自动化部署系统
created: 2026-04-26
updated: 2026-04-26
type: concept
tags: [infrastructure, automation, workflow, maxmes]
sources: [raw/articles/maxmes-cicd-2026-04-26.md]
---

# MaxMES CI/CD 自动化部署系统

[[maxmes]] 的自动化部署系统，采用 GitHub Actions + 服务器轮询混合模式，突破内网服务器无法被 GitHub Webhook 触发的限制。

## 核心架构

```
开发者 push -> GitHub Actions CI -> 可 merge PR
        (每分钟轮询) -> 服务器 poller 检测新 SHA -> deploy.sh 部署 -> pm2 重启
```

**核心技术方案：**
- 轮询替代 Webhook：服务器每分钟查询 GitHub API，不依赖 GitHub 主动推送
- systemd timer 替代 cron：更可靠的定时触发
- gh api 优先：经过 API 网关，比 git protocol 更稳定，失败自动降级
- 自动回滚：健康检查失败自动回滚到上一稳定版本
- 端口正确：5002（不是文档里的 5001）

## 关键组件

| 组件 | 路径 |
|------|------|
| 轮询脚本 | `/opt/mes-system/scripts/cicd/github-poller.sh` |
| 部署脚本 | `/opt/mes-system/scripts/cicd/deploy.sh` |
| systemd timer | `/etc/systemd/system/mes-cicd-poller.timer` |
| systemd service | `/etc/systemd/system/mes-cicd-poller.service` |
| cron fallback | `/etc/cron.d/mes-cicd-sync` |
| pm2 服务 | `mes-backend` (port:5002) / `mes-client-dev` |

## 标准开发流程

1. 从 main/develop 创建功能分支
2. 开发 → commit → push
3. GitHub 创建 PR -> main
4. 等待 CI 通过（Lint + Test + Build）
5. 至少 1 人 review approve
6. 合并 PR → 自动触发服务器部署

## 紧急 hotfix

```bash
# 手动触发 poller
bash /opt/mes-system/scripts/cicd/github-poller.sh

# 强制部署指定 SHA
bash /opt/mes-system/scripts/cicd/deploy.sh <commit-sha> <rollback-sha>

# 手动回滚
cd /opt/mes-system
ROLLBACK=$(cat scripts/cicd/.rollback_sha)
git reset --hard $ROLLBACK
pm2 restart all
```

## 故障排查

| 问题 | 排查命令 |
|------|----------|
| Poller 不触发 | `systemctl list-timers \| grep mes-cicd` |
| 部署后健康检查失败 | `pm2 logs mes-backend --err --lines 50` |
| GitHub 网络超时 | `gh api repos/xiaomaimax/mes-system --jq .full_name` |
| PM2 异常 | `pm2 list` |

## 相关页面

- [[maxmes]] — MES 系统总览
- [[mes/architecture]] — 系统架构
- [[mes/operations]] — 运维手册
