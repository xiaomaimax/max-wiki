# AI Agent（智能体）

> 来源：[菜鸟教程 AI Agent](https://www.runoob.com/ai-agent/ai-agent-tutorial.html)

## 定义

**AI Agent（人工智能代理）** 是能够感知环境、进行决策并执行行动，以达成特定目标的智能软件实体。

### 核心公式

```
Agent = LLM (大脑) + Planning (规划) + Tool use (执行) + Memory (记忆)
```

## Agent vs 传统程序

| 传统程序 | AI Agent |
|---------|----------|
| 自动售货机：投币 → 按按钮 → 出商品 | 私人助理：告诉需求 → 助理规划 → 完成任务并汇报 |
| 生成文本 | **生成行动并执行行动** |
| 固定指令流程 | 自主性员工 |

## 能力

- **理解任务目标**：明白你想要什么结果
- **制定计划**：思考如何达成目标
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

## 相关概念

- [[agent-vs-traditional-program]]
- [[agent-core-formula]]
