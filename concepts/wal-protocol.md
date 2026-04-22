---
title: WAL 协议
created: 2026-04-22
updated: 2026-04-22
type: concept
tags: [workflow, ai-ml, agent]
sources: [notion/max-notion-workflow]
---

# WAL 协议

## 概述
WAL（Write-Ahead Logging）是一种主动式信息捕获协议，源自 Proactive Agent 架构。核心思想：**在回复用户之前，先把关键信息写进去**。

## 触发条件
检测到以下任一内容时立即触发：
- ✏️ **纠正** — "是 X 不是 Y"、"其实..."
- 📍 **专有名词** — 名字、地点、公司、产品
- 🎨 **偏好** — "我喜欢/不喜欢"
- 📋 **决策** — "用 X"、"选 Y"
- 📝 **草稿修改** — 对内容的编辑
- 🔢 **具体值** — 数字、日期、ID、URL

## 执行流程

```
1. STOP  — 不要开始写回复
2. WRITE — 先更新 SESSION-STATE.md
3. THEN  — 再回复用户
```

## 示例

> 用户说："用蓝色主题，不是红色"

| 做法 | 结果 |
|------|------|
| ❌ 错误 | "好的，蓝色！"（看似明显，不记录）|
| ✅ 正确 | 先写 SESSION-STATE.md："主题：蓝色（非红色）" → 再回复 |

> 用户说："帮我查一下 192.168.1.100 这个 IP"

| 做法 | 结果 |
|------|------|
| ❌ 错误 | 直接查询，不记录 |
| ✅ 正确 | 先写 SESSION-STATE.md："当前查询IP：192.168.1.100" → 查询 → 回复 |

## 为什么重要

### 上下文压缩危险区
会话进行到中后期，上下文窗口接近满时：
- 旧对话被压缩成摘要
- 细节丢失（专有名词、具体数值）
- 用户的纠正和偏好可能被忽视

WAL 协议通过**提前外化**，确保关键信息不依赖上下文保留。

### 与传统记忆的区别
| 传统方式 | WAL 协议 |
|----------|----------|
| 回复后偶尔补记 | **回复前必须写** |
| 依赖 Agent 自觉 | 协议强制执行 |
| 写在对话里 | 写在独立文件里 |
| 易被上下文压缩 | 持久化存储 |

## Working Buffer（危险区日志）
当上下文已严重压缩时使用的兜底机制：

```
# Working Buffer (Danger Zone Log)
**Status:** ACTIVE
**Started:** [timestamp]

## [timestamp] Human
[用户消息]

## [timestamp] Agent (summary)
[1-2句回复摘要 + 关键信息]
```

## 相关页面
- [[proactive-agent]] — Proactive Agent 实体页
- [[session-state-management]] — 会话状态管理
- [[agent-memory-patterns]] — Agent 记忆模式

## 来源
Notion: Proactive Agent 技能详解
