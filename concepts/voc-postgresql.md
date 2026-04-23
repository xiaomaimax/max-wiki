---
title: PostgreSQL 安装配置（VOC服务器）
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [infrastructure, industry]
sources: [raw/voc-knowledge-202604.pdf]
---

# PostgreSQL 安装配置（VOC服务器）

## 背景
达观数据 VOC-AI 分析系统需要本地数据库存储：
- 原始评论数据
- 处理后数据
- 标签数据

## 系统架构中的数据库角色
```
VOC电商平台 → 本地数据库(PostgreSQL) → AI分析引擎 → PowerBI → 业务报告
```

## 相关概念
- [[voc-ai-analysis]] — VOC-AI 分析系统
- [[voc-system]] — VOC 系统概览
