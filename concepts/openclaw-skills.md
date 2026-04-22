---
title: OpenClaw Skill 能力
created: 2026-04-22
updated: 2026-04-22
type: concept
tags: [tool, workflow, ai-assistant]
sources: [raw/articles/学习笔记-2026.txt]
---

# OpenClaw Skill 能力

## 概述
OpenClaw 通过 skill 扩展助手能力，支持 Notion、Tavily 搜索、飞书等多种工具集成。

## 已配置 Skill
| Skill | 用途 |
|-------|------|
| Notion | 读写 Notion 数据库和页面 |
| Qqmail | QQ 邮箱管理 |
| Tavily | 网络搜索 → 筛选汇总 → 飞书 → Notion |

## 工作流示例
1. **Tavily 搜索 → 飞书 → Notion 知识库**：Tavily 搜索筛选汇总内容，推送飞书后存入 Notion
2. **飞书 ↔ Notion 任务跟踪**：设备间共享任务状态
3. **每日 22:00 小虾日报**：自动生成并存入 Notion 知识库

## ClawHub
从 ClawHub 安装/发布技能：
```bash
clh_gh80K_jc2XX1NY2aSYDazZiG_Fb7IdPZtUPWFUYzoT4
```

## 相关
- [[openclaw]]
- [[openclaw-soul-triplet]]
