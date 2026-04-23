---
title: MaxMES 系统知识库
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [mes, manufacturing, maxmes]
sources: [raw/maxmes-knowledge-202604.pdf]
---

# MaxMES 系统知识库

> 公司MES制造执行系统官方知识库，来源：MaxMES知识库202604.pdf

## 系统概述

MaxMES 是公司正在实施/使用的 **MES（制造执行系统）**，实现生产全流程数字化管理，连接 ERP 与底层生产设备。IT 部门负责运维和二次开发。

**生产环境**: `http://192.168.100.6:80`
**开发环境**: `http://192.168.100.6:3000`
**安装目录**: `/opt/mes-system`
**GitHub**: `https://github.com/xiaomaimax/mes-system`
**技术栈**: React + Ant Design + TypeScript + Node.js (Express) + MySQL 8.0 + Redis 7.x

---

## 知识库结构

### 项目文档
- [[mes/project-overview]] — 项目概述（背景/目标/范围/团队/风险/预算/验收标准）
- [[mes/requirements]] — 需求规格说明书（功能需求+非功能需求）
- [[mes/architecture]] — 系统架构设计（技术栈/部署架构/数据架构/性能架构）
- [[mes/database]] — 数据库设计（MySQL/Redis/RabbitMQ 配置/索引/备份/优化）
- [[mes/blueprint]] — 蓝图设计报告（设计目标/系统集成/实施计划）
- [[mes/project-acceptance]] — 项目验收报告

### 运维文档
- [[mes/training]] — 培训计划
- [[mes/testing]] — UAT测试文档
- [[mes/deployment]] — 部署手册 + 上线计划
- [[mes/operations]] — 运维手册（职责/监控/故障分级/备份恢复）
- [[mes/troubleshooting]] — 巡检报告 + 问题修复汇总（共52项问题）

---

## 核心模块

| 模块 | 说明 |
|------|------|
| 仪表盘管理 | 生产/质量/设备/库存/人员概览仪表盘，自定义仪表盘 |
| 工艺管理 | 工艺路线/参数/变更/版本/执行监控 |
| 辅助排程 | ERP计划导入/自动排程/甘特图可视化 |
| 生产管理 | 工单管理/生产执行/数据采集/进度跟踪/异常管理 |
| 设备管理 | 台账/维护计划/状态监控/OEE统计 |
| 质量管理 | IQC/PQC/FQC/合格率统计/TOP不良/批次追溯 |
| 库存管理 | 入库/出库/盘点/安全库存 |
| 人员管理 | 考勤/绩效/培训 |
| 报表管理 | 生产/质量/设备/库存/人员/自定义报表 |
| 系统设置 | 用户/角色/权限/日志/备份恢复 |

---

## 相关页面
- [[maxmes]] — MaxMES 系统实体（系统信息/技术架构）
- [[maxmes-production]] — 生产管理模块
- [[maxmes-equipment]] — 设备管理模块
- [[maxmes-quality]] — 质量管理模块
- [[maxmes-inventory]] — 库存管理模块
- [[maxmes-operations]] — 日常操作指南
- [[maxmes-admin]] — 系统管理员指南
