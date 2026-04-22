---
title: Agent 核心公式
created: 2026-04-10
updated: 2026-04-22
type: concept
tags: [ai-ml, agent]
sources: [notion/max-notion-workflow, raw/runoob-ai-agent.md]
---

# Agent 核心公式

```
Agent = LLM (大脑) + Planning (规划) + Tool use (执行) + Memory (记忆)
```

## 五大架构层次

```
感知层(Perception) → 大脑(Brain) → 规划层(Planning) → 工具层(Tools) → 记忆层(Memory)
                                          ↺（贯穿始终）
```

### 0. 感知层 (Perception)
现代 Agent 具备多模态感知能力：文本输入、图像/视频、结构化数据、环境状态、工具返回结果。

### 1. 大脑 (Brain) — 大语言模型
核心组件，如 GPT-4、Claude、DeepSeek、通义千问。
- **意图理解**：解析用户输入，明确目标
- **推理决策**：综合上下文和记忆，判断下一步
- **工具调用判断**：选择工具、传入参数

### 2. 工具 (Tools)
- **信息获取**：联网搜索、网页抓取、文档读取、数据库查询
- **计算执行**：代码解释器、数学计算引擎、沙箱环境
- **内容生成**：图像生成、语音合成、文档导出
- **系统交互**：API 接口、邮件、日历、文件操作、消息发送

### 3. 记忆 (Memory)
- **短期记忆**：当前对话上下文窗口（8K-200K token）
- **长期记忆**：存储长期偏好，向量数据库（Pinecone、Milvus、Chroma）
- **情节记忆**：历史任务执行过程记录
- **语义记忆**：抽象知识和事实，预训练 + RAG

### 4. 规划 (Planning)
- **CoT** (Chain-of-Thought)：一步步写出推理过程
- **ReAct** (Reasoning + Acting)：交替推理与行动，根据结果再推理
- **ToT** (Tree-of-Thoughts)：同时探索多条推理分支，选择最优
- **Reflection**：自我反思，任务完成后批判性审查并修正

## ReAct 工作流程

```
思考 → 行动 → 观察 → 再思考 → 再行动 → ... → 最终响应
```

示例（"帮我在北京找评分高于4.5的意大利餐厅"）：
- **思考 (Think)**：分析任务 → 生成搜索指令
- **行动 (Act)**：调用搜索工具
- **观察 (Observe)**：接收搜索结果
- **再思考**：分析结果 → 生成筛选指令
- **最终响应**：整合所有结果，生成结构化答案

## 四要素（基础版）

| 要素 | 作用 |
|------|------|
| **LLM** | 理解意图、推理、决策——大脑 |
| **Planning** | 将复杂目标拆解为可执行步骤 |
| **Tool use** | 调用 API、代码、文件等外部资源执行 |
| **Memory** | 存储上下文、历史、多轮对话记忆 |

## Agent Loop（运行循环）

```
感知输入 → 大脑思考 → 工具行动 → 观察结果 → 调整策略（失败时自我纠错）
```

## 来源
- [菜鸟教程 AI Agent](https://www.runoob.com/ai-agent/)
- Notion: AI Agent 简介 / 核心组件 / 工作原理

## 相关页面
- [[ai-agent]] — AI Agent 实体页
- [[agent-vs-traditional-program]] — Agent vs 传统程序对比
- [[wal-protocol]] — WAL 协议（Proactive Agent 记忆协议）
