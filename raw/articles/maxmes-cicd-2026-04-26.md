---
title: MaxMES CI/CD 自动化部署系统
created: 2026-04-26
updated: 2026-04-26
type: concept
tags: [infrastructure, automation, workflow]
sources: []
---

# MaxMES CI/CD 自动化部署系统

> 🔧 更新时间：2026-04-26 | 适用版本：MaxMES v1.3.0+

## 一、系统架构
MaxMES CI/CD 采用 GitHub Actions + 服务器轮询混合模式，突破内网服务器无法被 GitHub 主动触发的限制，实现代码 push 到 GitHub 后自动完成构建、测试并部署上线。

### 1.1 整体架构图

```bash
开发者 push -> GitHub Actions CI
                    |
            Lint (ESLint)
                    |
            Test (Jest - 314 cases)
                    |
            Build (React + Node.js)
                    |
            所有 check 通过 -> 可 merge PR
                    |
        (每分钟轮询机制)
                    |
    服务器 poller 检测到新 SHA
                    |
    deploy.sh 执行完整部署
                    |
            健康检查通过
                    |
        pm2 显示新版本 online
```


### 1.2 核心技术方案
- 轮询替代 Webhook：服务器主动每分钟查询 GitHub API，不依赖 GitHub 主动推送
- systemd timer 替代 cron：更可靠的定时触发，支持 oneshot 服务模型
- gh api 优先：经过 API 网关，比 git protocol 更稳定，失败时自动降级
- deploy.sh 专用脚本：不依赖 sudo、不依赖 npm ci（用 npm install offline），端口正确（5002）
- 自动回滚：健康检查失败自动回滚到上一稳定版本

---

## 二、CI/CD 完整流程

### Step 1 - 开发者推送代码
开发者将代码 push 到 GitHub 仓库的 main 或 develop 分支。
- 直接 push 到 main：需通过 PR + review（已配置 branch protection）
- develop 分支：可自由 push，用于预发验证

### Step 2 - GitHub Actions 自动构建

```bash
Push 到 main/develop 分支 -> 触发 CI Pipeline:

  [Lint]     ESLint 代码风格检查          (~30s)
     |
  [Test]     Jest 单元测试 (314 cases)    (~45s)
     |
  [Build]    前端 React Build             (~2-3min)
     |
  [Deploy]   生成部署触发文件（不实际部署到服务器）
```


### Step 3 - PR + Branch Protection
main 分支已配置强制保护规则：

```bash
Branch Protection 规则（main 分支）:
  require status checks    -> ci/test 必须通过（strict 模式）
  require 1 approving review -> PR 必须有至少 1 人批准
  dismiss stale reviews     -> 新 commit 自动清除旧审批
  require last push approval -> push 后需重新批准
  block force pushes       -> 禁止强制推送
  block branch deletion    -> 禁止删除 main 分支
```


### Step 4 - 服务器轮询检测新 Commit
systemd timer 每 60 秒触发一次 poller 服务，检测 GitHub 最新 commit SHA：

```bash
# 查看 poller 状态
systemctl status mes-cicd-poller.timer
systemctl list-timers | grep mes-cicd

# 查看 poller 日志
tail -f /opt/mes-system/scripts/cicd/poller.log
```


### Step 5 - 自动部署
检测到 SHA 差异后，deploy.sh 执行完整部署流程：

```bash
deploy.sh 执行步骤:
  1. 备份当前版本 SHA -> .rollback_sha
  2. git fetch --depth=1 origin main
  3. git reset --hard origin/main
  4. npm install --omit=dev --prefer-offline   (后端)
  5. cd client && npm install && npm run build
  6. pm2 restart all
  7. 健康检查 http://localhost:5002/api/health (重试 5 次)
     通过 -> 部署完成
     失败 -> 自动回滚到 .rollback_sha
```


### Step 6 - 健康检查验证

```bash
# 直接验证后端健康
curl http://localhost:5002/api/health

# 预期返回
{
  "status": "ok",
  "uptime": 5.28,
  "environment": "production",
  "version": "1.3.0-p3"
}

# pm2 服务状态
pm2 list
```


---

## 三、关键组件说明

### 3.1 GitHub Actions Workflow

```bash
.github/workflows/ci.yml

触发条件:
  push -> main / develop 分支
  pull_request -> main 分支

Jobs 顺序:
  lint (ESLint)
    |
  test (Jest 314 cases)
    |
  build (前端构建, needs: [lint, test])
    |
  trigger-deploy (生成触发文件, needs: [build])
```


### 3.2 服务器轮询脚本

```bash
# /opt/mes-system/scripts/cicd/github-poller.sh
# systemd oneshot 服务，每分钟触发一次
#
# 核心逻辑:
#   1. get_remote_sha() -> 用 gh api 查询 GitHub SHA（优先）
#                          降级用 git ls-remote（最多等 8s）
#   2. get_local_sha()  -> git rev-parse HEAD
#   3. SHA 不同 -> 调用 deploy.sh 执行部署
#   4. SHA 相同 -> 静默退出（无输出）

# 状态文件
/opt/mes-system/scripts/cicd/.last_sha
/opt/mes-system/scripts/cicd/.rollback_sha
/opt/mes-system/scripts/cicd/.lock
```


### 3.3 部署脚本

```bash
# /opt/mes-system/scripts/cicd/deploy.sh
# 特点:
#   端口正确: 5002（不是文档里的 5001）
#   不依赖 sudo: 直接用 pm2（systemd 以 root 运行）
#   npm install --prefer-offline: 优先用本地缓存，网络抖动可重试
#   自动回滚: 健康检查失败自动回退到上一版本
#   拉链式回滚: 支持跨轮次连续回滚（.rollback_sha 文件）

# 关键路径
后端端口:    http://localhost:5002/api/health
前端构建物: /opt/mes-system/client/build/
备份目录:   /opt/mes-system/backups/deploy/
```


### 3.4 systemd 服务配置

```bash
# /etc/systemd/system/mes-cicd-poller.service
[Unit]
Description=MaxMES CI/CD Poller - GitHub Commit Watcher

[Service]
Type=oneshot
User=root
WorkingDirectory=/opt/mes-system/scripts/cicd
ExecStart=/bin/bash /opt/mes-system/scripts/cicd/github-poller.sh
TimeoutStartSec=600
StandardOutput=append:/opt/mes-system/scripts/cicd/poller.log
StandardError=append:/opt/mes-system/scripts/cicd/poller.log

# /etc/systemd/system/mes-cicd-poller.timer
[Timer]
OnBootSec=30
OnUnitActiveSec=60
Unit=mes-cicd-poller.service

# 启用命令
systemctl enable --now mes-cicd-poller.timer
```


### 3.5 cron Fallback
> 🔧 重要性：中等 | cron 作为 timer 的备份，确保即使 timer 有故障也能维持 git fetch

```bash
# /etc/cron.d/mes-cicd-sync
# 每分钟 git fetch origin main，保持本地 origin/main tracking 最新
* * * * * root cd /opt/mes-system && git fetch origin main >> /opt/mes-system/scripts/cicd/fetch.log 2>&1
```


---

## 四、使用规范

### 4.1 标准开发流程
1. 从 main 或 develop 创建功能分支
1. 在分支上开发、commit、push
1. 在 GitHub 创建 Pull Request -> main
1. 等待 CI 全部通过（Lint + Test + Build）
1. 至少 1 人 review approve
1. 合并 PR -> 自动触发服务器部署

### 4.2 紧急 hotfix 流程
如需跳过 PR 直接部署（比如凌晨故障恢复）：

```bash
# 方式 1: 手动触发 poller（会检测最新 SHA 并部署）
bash /opt/mes-system/scripts/cicd/github-poller.sh

# 方式 2: 强制部署指定 SHA
bash /opt/mes-system/scripts/cicd/deploy.sh <commit-sha> <rollback-sha>

# 方式 3: 手动回滚到上一稳定版本
cd /opt/mes-system
ROLLBACK=$(cat scripts/cicd/.rollback_sha)
git reset --hard $ROLLBACK
pm2 restart all
curl http://localhost:5002/api/health
```


### 4.3 查看部署历史

```bash
# poller 日志
tail -50 /opt/mes-system/scripts/cicd/poller.log

# pm2 日志
pm2 logs mes-backend --lines 30

# systemd journal
journalctl -u mes-cicd-poller.service -u mes-cicd-poller.timer --no-pager -n 50
```


---

## 五、故障排查

### 5.1 Poller 没有自动触发部署

```bash
# 检查 timer 是否 active
systemctl list-timers | grep mes-cicd

# 检查服务是否 enabled
systemctl is-enabled mes-cicd-poller.timer mes-cicd-poller.service

# 手动触发一次 poller 看报错
bash -x /opt/mes-system/scripts/cicd/github-poller.sh 2>&1 | tail -30

# 检查 gh 是否登录
gh auth status
```


### 5.2 部署后健康检查失败

```bash
# 查看最新 pm2 错误日志
pm2 logs mes-backend --err --lines 50

# 手动重启 pm2
pm2 restart all
sleep 5
curl http://localhost:5002/api/health

# 回滚到上一版本
cd /opt/mes-system
ROLLBACK=$(cat scripts/cicd/.rollback_sha)
git reset --hard $ROLLBACK
pm2 restart all
```


### 5.3 GitHub 网络不通（git fetch 超时）
> 🔧 已知问题：服务器到 github.com:443 偶发超时（15-20s），但 api.github.com 通常正常。poller 会自动降级：gh api -> git ls-remote -> git rev-parse

```bash
# 测试 GitHub API 是否可达
gh api repos/xiaomaimax/mes-system --jq .full_name

# 测试 git fetch
cd /opt/mes-system
timeout 15 git fetch origin main
```


### 5.4 PM2 服务异常

```bash
# 查看 pm2 进程状态
pm2 list

# 重启服务
pm2 restart mes-backend
pm2 restart mes-client-dev
pm2 restart all
```


---

## 六、相关文件路径速查

```bash
# 服务器文件
/opt/mes-system/                              # 项目根目录
/opt/mes-system/scripts/cicd/
    github-poller.sh                           # 轮询主脚本
    deploy.sh                                  # 部署脚本（含回滚）
    poller.log                                 # poller 运行日志
    .last_sha                                  # 上次已处理的 commit SHA
    .rollback_sha                              # 可回滚的上一版本 SHA
    .lock                                      # 单实例锁

# systemd 服务
/etc/systemd/system/mes-cicd-poller.service
/etc/systemd/system/mes-cicd-poller.timer

# cron
/etc/cron.d/mes-cicd-sync

# pm2
mes-backend    (id:1)  port:5002
mes-client-dev (id:2)

# GitHub
仓库: https://github.com/xiaomaimax/mes-system
CI:   https://github.com/xiaomaimax/mes-system/actions
```


---

## 七、当前系统状态
> 🔧 以下信息为文档生成时的实时状态，最新状态请用上述命令自行查询

```bash
# === GitHub Branch Protection (main) ===
require status checks: ci/test (strict)
required approving review count: 1
dismiss stale reviews: true
require last push approval: true
allow force pushes: false
allow deletions: false

# === systemd Timer ===
ActiveState: active
触发频率: 每 60 秒

# === pm2 ===
mes-backend    online  port:5002
mes-client-dev online

# === 健康检查 ===
curl http://localhost:5002/api/health
-> {"status":"ok","uptime":5.28,"environment":"production","version":"1.3.0-p3"}

# === 最新 CI Run ===
CI  Workflow: success
E2E Workflow: failure (Playwright E2E，非阻塞)

# === 端到端验证记录 (2026-04-26) ===
commit: f03ddd7ecc9311d38259e373d18c443e0a322271
触发方式: GitHub push -> CI -> poller 自动检测 -> 部署
pm2 restart | 健康检查通过 | 版本 1.3.0-p3
```
