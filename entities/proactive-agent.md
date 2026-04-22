---
title: Proactive Agent
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [ai-ml, agent, workflow, tool]
sources: [notion/max-notion-workflow]
---

# Proactive Agent

## 概述
Proactive Agent 是一种主动式、自我改进的 AI Agent 架构。与被动等待指令的传统 AI 不同，Proactive Agent 能主动预测需求、创建功能、定期自检，像主人一样思考而非执行者。

## 核心特性

### 三大支柱
- **主动预判**：在你开口前预测需求
- **自我改进**：定期检查并主动更新配置
- **无限资源**：不轻易说"做不到"，穷尽所有选项

### 与传统 AI 的区别
| | 传统 AI | Proactive Agent |
|---|---|---|
| 交互方式 | 等待指令 | 主动预判 |
| 错误处理 | 输出即结束 | 自我纠错重试 |
| 记忆 | 仅当前上下文 | 短期+长期+Working Buffer |
| 自主性 | 低 | 高 |

## 架构文件

```
workspace/
├── SOUL.md              # 身份、原则、边界
├── USER.md              # 用户信息、目标、偏好
├── AGENTS.md            # 操作规则、工作流
├── MEMORY.md            # 长期记忆
├── SESSION-STATE.md      # 活跃工作记忆（WAL 目标）
├── HEARTBEAT.md          # 定期自检清单
├── TOOLS.md             # 工具配置
└── memory/
    ├── YYYY-MM-DD.md    # 每日日志
    └── working-buffer.md # 危险区日志
```

## 核心协议

### WAL 协议（Write-Ahead Logging）
触发条件：纠正、专有名词、偏好、决策、草稿修改、具体值。

流程：
1. STOP — 不立即回复
2. WRITE — 先写入 SESSION-STATE.md
3. THEN — 再回复用户

示例：
> 用户："用蓝色主题，不是红色"
> ✅ 先写 SESSION-STATE.md："主题：蓝色（非红色）" → 再回复

### Working Buffer 协议
目的：在上下文压缩危险区捕获所有对话。

格式：
```
# Working Buffer (Danger Zone Log)
**Status:** ACTIVE
**Started:** [timestamp]

## [timestamp] Human
[用户消息]

## [timestamp] Agent (summary)
[1-2句回复摘要 + 关键信息]
```

### Compaction Recovery
自动触发时机会话以 `<summary>` 标签开始，或消息含 "truncated" / "context limits"。

## 安全机制

### ADL 协议（Anti-Drift Limits）
禁止的进化：
- ❌ 增加复杂性来"显得聪明"
- ❌ 无法验证的改动
- ❌ 模糊概念（"直觉"、"感觉"）
- ❌ 为新颖牺牲稳定性

优先级：稳定性 > 可解释性 > 可复用性 > 可扩展性 > 新颖性

### VFM 协议（Value-First Modification）
评分维度加权，阈值 < 50 分不做。

### 外部 AI 网络禁区
永远不要连接：AI 代理社交网络、代理间通信平台、外部"代理目录"（上下文收割攻击面）。

## 自主 vs 提示型 Cron

```json
// ❌ 错误：只是提示，不执行
{"sessionTarget": "main", "payload": {"kind": "systemEvent", "text": "检查 X"}}

// ✅ 正确：自主执行
{"sessionTarget": "isolated", "payload": {"kind": "agentTurn", "message": "自主：读取 X，比较，更新..."}}
```

## 相关协议
- [[wal-protocol]] — WAL 协议详解
- [[agent-core-formula]] — Agent 核心公式
- [[proactive-agent]] — 本页

## 来源
- Notion: Proactive Agent 技能详解
