# Agent 核心公式

```
Agent = LLM (大脑) + Planning (规划) + Tool use (执行) + Memory (记忆)
```

## 四要素

| 要素 | 作用 |
|------|------|
| **LLM** | 理解意图、推理、决策——大脑 |
| **Planning** | 将复杂目标拆解为可执行步骤 |
| **Tool use** | 调用 API、代码、文件等外部资源执行 |
| **Memory** | 存储上下文、历史、多轮对话记忆 |

## 菜鸟教程示例中的体现

天气穿衣助手 Agent：
- LLM（伪代码中隐式）：理解"北京天气、穿衣"
- Planning：解析城市 → 判断温度区间
- Tool use：`get_weather_api` 调用天气接口
- Memory：`self.memory` 记录执行步骤

## 相关实体

- [[ai-agent]]

## 标签
#ai-agent #核心概念
