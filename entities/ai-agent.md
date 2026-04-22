---
title: AI Agent（智能体）
created: 2026-04-10
updated: 2026-04-22
type: entity
tags: [ai-ml, agent]
sources: [notion/max-notion-workflow, raw/runoob-ai-agent.md]
---

# AI Agent（智能体）

## 定义

**AI Agent（人工智能代理）** 是能够感知环境、进行决策并执行行动，以达成特定目标的智能软件实体。它不仅仅是回答问题的聊天机器人，更是能够动手做事的智能执行者。

> 核心公式：`Agent = LLM (大脑) + Planning (规划) + Tool use (执行) + Memory (记忆)`

## 发展历程

| 时期 | 阶段 | 代表 |
|------|------|------|
| 1950s-2010s | 概念萌芽期 | 图灵测试、多智能体系统、规则驱动聊天机器人 |
| 2010s-2020 | 深度学习赋能期 | Transformer、BERT、GPT 系列 |
| 2021-至今 | 大模型 Agent 爆发期 | ChatGPT、GPT-4+Plugins、AutoGPT、LangChain |

## 核心特性

- **自主性（Autonomy）**：无需人类逐步指导，独立运作
- **反应性（Reactivity）**：感知环境变化并及时响应
- **主动性（Proactivity）**：不仅被动响应，还能主动采取行动
- **社交能力（Social Ability）**：能与人类或其他 Agent 交互
- **学习能力（Learning）**：从历史交互中学习，记住偏好和上下文

## 能力

- **理解任务目标**：明白你想要什么结果
- **制定计划**：思考如何达成目标（CoT/ReAct/ToT）
- **使用工具**：调用各种资源和 API
- **自我调整**：根据反馈优化策略
- **持续执行**：直到完成任务或遇阻时调整

## 结构（三块）

1. **目标**：明确任务意图
2. **逻辑**：按规则拆成可执行步骤
3. **工具**：通过代码或 API 让步骤落地

## 运行方式

```
接收输入 → 判断当前任务 → 调用对应工具执行 → 返回结果 → 保留上下文 → 支持多轮连续操作 → 遇阻时调整
```

## Agent Loop（运行循环）

```
感知输入 → 大脑思考 → 工具行动 → 观察结果 → 调整策略（失败时自我纠错）
```

## 伪代码示例：天气穿衣助手

```python
class WeatherAgent:
    def __init__(self):
        self.memory = []
        self.tools = {
            'get_weather': self.get_weather_api,
            'give_advice': self.generate_advice
        }

    def run(self, user_input):
        if "天气" in user_input and "北京" in user_input:
            city = "北京"
        else:
            return "请告诉我您想查询哪个城市？"
        weather_info = self.tools['get_weather'](city)
        self.memory.append({'step': 'fetched_weather', 'data': weather_info})
        final_advice = self.tools['give_advice'](weather_info)
        return final_advice
```

## 学习资源

- [微软 AI Agents 初学者课程](https://github.com/microsoft/ai-agents-for-beginners)
- [Hello-Agents](https://github.com/datawhalechina/hello-agents)
- [500 个智能体案例](https://github.com/ashishpatel26/500-AI-Agents-Projects)
- [HuggingFace 智能体课程](https://github.com/huggingface/agents-course)
- [菜鸟教程 AI Agent](https://www.runoob.com/ai-agent/)

## 相关概念

- [[agent-core-formula]] — Agent 核心公式（五大架构层次）
- [[agent-vs-traditional-program]] — Agent vs 传统程序对比
- [[proactive-agent]] — Proactive Agent（主动式 Agent）
- [[wal-protocol]] — WAL 协议（记忆捕获协议）
- [[openclaw]] — OpenClaw 框架（Max 使用的 Agent 平台）
