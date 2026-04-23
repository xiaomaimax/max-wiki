---
title: MaxMES 数据库设计
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [mes, database, mysql, redis]
sources: [raw/maxmes-knowledge-202604.pdf]
---

# MaxMES 数据库设计

## 2. 数据库选择

| 数据库类型 | 数据库名称 | 版本 | 用途 |
|------------|------------|------|------|
| 关系型数据库 | MySQL | 8.0 | 核心业务数据存储 |
| 缓存数据库 | Redis | 7.x | 高频访问数据缓存 |
| 消息队列 | RabbitMQ | 3.12.x | 异步消息处理 |

## 3. 数据库配置

### 3.1 MySQL配置

| 配置项 | 配置值 | 说明 |
|--------|--------|------|
| 字符集 | utf8mb4 | 支持多语言和表情符号 |
| 排序规则 | utf8mb4_unicode_ci | 统一的排序规则 |
| 存储引擎 | InnoDB | 支持事务和外键约束 |
| 最大连接数 | 1000 | 支持并发访问 |
| 查询缓存 | 关闭 | 使用Redis进行缓存 |
| 日志级别 | ERROR | 只记录错误日志 |

## 5. 索引设计

| 表名 | 索引名称 | 索引字段 | 索引类型 | 说明 |
|------|----------|----------|----------|------|
| user | idx_user_username | username | UNIQUE | 加速用户名查询 |
| user | idx_user_role_id | role_id | 普通索引 | 加速角色关联查询 |
| work_order | idx_work_order_no | order_no | UNIQUE | 加速工单编号查询 |
| work_order | idx_work_order_status | status | 普通索引 | 加速工单状态查询 |
| production_execution | idx_execution_work_order | work_order_id | 普通索引 | 加速工单关联查询 |
| production_execution | idx_execution_equipment | equipment_id | 普通索引 | 加速设备关联查询 |
| data_collection | idx_data_execution | execution_id | 普通索引 | 加速生产执行关联查询 |
| data_collection | idx_data_collection_time | collection_time | 普通索引 | 加速时间范围查询 |
| quality_inspection | idx_inspection_execution | execution_id | 普通索引 | 加速生产执行关联查询 |
| equipment | idx_equipment_code | equipment_code | UNIQUE | 加速设备编码查询 |
| equipment | idx_equipment_status | status | 普通索引 | 加速设备状态查询 |
| material | idx_material_code | material_code | UNIQUE | 加速物料编码查询 |

## 9. 安全设计

### 9.2 数据加密

1. **敏感数据加密**：用户密码使用BCrypt算法加密存储
2. **传输加密**：使用SSL/TLS协议进行数据传输加密
3. **存储加密**：使用MySQL的透明数据加密(TDE)功能

### 9.3 数据备份与恢复

| 备份类型 | 备份频率 | 备份方式 | 备份保留期 |
|----------|----------|----------|------------|
| 全量备份 | 每天 | MySQL Dump | 30天 |
| 增量备份 | 每小时 | Binary Log | 7天 |
| 差异备份 | 每周 | MySQL Dump | 90天 |

## 10. 数据库性能优化

### 10.1 查询优化

1. **使用索引**：为频繁查询的字段创建索引
2. **避免全表扫描**：使用WHERE条件过滤数据
3. **优化JOIN查询**：减少JOIN的表数量和数据量
4. **使用LIMIT**：限制返回的数据行数
5. **避免SELECT \* **：只查询需要的字段

### 10.2 写入优化

1. **批量插入**：使用批量插入减少网络开销
2. **使用事务**：将多个操作合并到一个事务中
3. **关闭自动提交**：减少事务提交的次数
4. **使用延迟索引**：在数据导入后创建索引

### 10.3 存储优化

1. **分区表**：对大表进行分区，提高查询效率
2. **归档历史数据**：将历史数据归档到单独的表中
3. **使用合适的数据类型**：选择合适的数据类型，减少存储空间
4. **定期清理无用数据**：删除无用的数据，释放存储空间

## 相关页面
- [[mes/index]] — MaxMES 知识库总览
- [[mes/architecture]] — 系统架构设计
- [[mes/operations]] — 运维手册（备份恢复）
