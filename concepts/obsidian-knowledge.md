---
title: Obsidian 知识汇总
created: 2026-04-22
updated: 2026-04-22
type: concept
tags: [workflow, tool, knowledge-management]
sources: [notion/max-notion-workflow]
---

# Obsidian 知识汇总

## 概述
Obsidian 是一款本地优先的个人知识管理（PKM）软件，基于 Markdown 文件存储，以双向链接为核心特性，帮助构建"第二大脑"。Max 用它作为 max-wiki 的查看端。

## Obsidian vs Notion 对比

| | Notion | Obsidian |
|---|---|---|
| 定位 | 团队协作工具 + 数据库 | 个人知识管理系统 |
| 存储 | 云端（Notion 服务器） | 本地（电脑文件夹） |
| 格式 | 块编辑器 + 数据库 | 纯 Markdown 文件 |
| 链接 | 单向页面引用 | 双向链接（图谱） |
| 协作 | 多人实时编辑 | 纯单机 |
| 离线 | 必须联网 | 完全离线可用 |
| 迁移 | 导出有限，绑定 Notion | 完全开放，文件随便搬 |
| 价格 | 免费个人够用，团队付费 | 完全免费 |

## 什么时候用 Notion
- 团队协作（多人同时编辑）
- 项目管理（看板、表格、甘特图）
- 数据库需求（筛选、排序、关联的结构化数据）
- 快速搭系统（用现成模板）

## 什么时候用 Obsidian
- 个人知识积累（长期阅读笔记、研究资料沉淀）
- 构建知识网络（双向链接发现关联）
- 需要完全隐私（数据不放云端）
- 需要版本控制（配合 Git）
- 离线写作

## Max 的使用场景

### Notion（快速收集）
- 手机随手记
- 项目数据库（看板/表格）
- 团队共享知识库

### Obsidian（知识沉淀）
- 深度整理的笔记
- 双链知识图谱
- 长期积累的个人 Wiki（max-wiki）

## Obsidian 跨平台同步方案

| 方案 | 平台 | 特点 |
|------|------|------|
| iCloud | iPhone | 推荐苹果生态 |
| LocalSend | Android/Win | 局域网，免费 |
| Syncthing | 全平台 | P2P，免费，需在线 |
| **Git + GitHub** | **全平台** | **推荐，配合 Obsidian Git 插件** |

## 常用插件

| 插件 | 用途 |
|------|------|
| Templater / Templates | 笔记模板 |
| Dataview | 类似数据库的笔记查询 |
| Obsidian Git | 自动同步到 Git |
| Kanban | 看板视图 |
| Importer | 导入其他工具的内容 |

## Notion → Obsidian 同步

### 方案一：Importer 插件（一次性）
Obsidian 官方插件，支持从 Notion 导入页面为 Markdown。
注意：只是单向导入，Notion 后续更新不会自动同步。

### 方案二：定时脚本同步（自动化，推荐）
环境要求：Windows 电脑已安装 Node.js。

步骤：
1. 创建同步文件夹 `D:\notion-sync\`
2. 初始化项目：`npm init -y && npm install @notionhq/client`
3. 获取 Notion 集成密钥
4. 设置 Windows 定时任务

## 相关页面
- [[max-wiki]] — max-wiki 技能（当前知识库）
- [[openclaw-skills]] — OpenClaw 技能总览

## 来源
Notion: Obsidian 知识汇总
