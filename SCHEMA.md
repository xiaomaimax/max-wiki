# Wiki Schema

## Domain
通用知识库——工作笔记、技术文档、学习心得、行业研究。

## Conventions
- 文件名：小写、中划线、无空格（如 `transformer-architecture.md`）
- 每个 wiki 页面以 YAML frontmatter 开头（见下方）
- 使用 `[[wikilinks]]` 进行页面间链接（每页至少 2 个出站链接）
- 更新页面时，必须更新 `updated` 日期
- 新页面必须加入 `index.md` 对应板块
- 每次操作必须追加到 `log.md`

## Frontmatter
```yaml
---
title: Page Title
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [from taxonomy below]
sources: [raw/articles/source-name.md]
---
```

## Tag Taxonomy
[定义 10-20 个顶层标签，先在此添加再使用]

- 人物：person, company, lab, open-source
- 技术：programming, infrastructure, security, ai-ml
- 工作流：workflow, automation, tool
- 领域：industry, finance, manufacturing
- 元：comparison, timeline, opinion, meta

## Page Thresholds
- **创建页面**：实体/概念在 2+ 来源中出现，或在一个来源中处于核心地位
- **追加到现有页面**：来源提到已覆盖的内容
- **不创建页面**：仅在注释中提及、次要细节或超出领域范围的内容
- **拆分页面**：超过 ~200 行时，拆分为子主题并交叉链接
- **归档页面**：内容完全被取代时，移至 `_archive/`，从 index.md 中移除

## Update Policy
新信息与现有内容冲突时：
1. 查看日期——较新的来源通常覆盖较旧的
2. 确实矛盾时，两种观点都注明日期和来源
3. 在 frontmatter 中标记：`contradictions: [page-name]`
4. 在 lint 报告中标记，待用户审核

## Entity Pages
每个 notable 实体一个页面，包含：
- 概述 / 是什么
- 关键事实和日期
- 与其他实体的关系（[[wikilinks]]）
- 来源引用

## Concept Pages
每个概念/主题一个页面，包含：
- 定义 / 解释
- 当前知识状态
- 开放问题或争议
- 相关概念（[[wikilinks]]）

## Comparison Pages
对比分析，包含：
- 比较对象和原因
- 对比维度（建议表格格式）
- 结论或综合判断
- 来源
