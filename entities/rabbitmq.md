# RabbitMQ

> 消息队列/流式broker产品，官网 [rabbitmq.com](https://rabbitmq.com)，GitHub [rabbitmq/rabbitmq-server](https://github.com/rabbitmq/rabbitmq-server)。

## 基本信息

| 项目 | 内容 |
|------|------|
| **类型** | 消息队列 / 流式消息代理（Message Broker） |
| **开源协议** | MPL 2.0 |
| **当前版本** | v4.2.5（2026-04-21） |
| **版权所有** | (c) 2007-2026 Broadcom |
| **官网** | https://rabbitmq.com |
| **GitHub** | https://github.com/rabbitmq/rabbitmq-server |
| **官方文档** | https://www.rabbitmq.com/docs |
| **支持协议** | AMQP 1.0 / AMQP 0-9-1 / MQTT 3.1~5.0 / STOMP 1.0~1.2 / RabbitMQ Stream / WebSocket |

## 核心协议支持

| 协议 | 版本 | 说明 |
|------|------|------|
| AMQP | 1.0 / 0-9-1 | 核心协议，最常用 |
| MQTT | 3.1 / 3.1.1 / 5.0 | IoT场景 |
| STOMP | 1.0 / 1.1 / 1.2 | WebSocket支持 |
| RabbitMQ Stream | — | 追加日志式持久化队列 |
| WebSocket | — | MQTT over WS / STOMP over WS |

## 关键特性

### 队列类型

- **Classic Queue**：传统队列，简单场景
- **Quorum Queue**：多数派队列，复制/数据安全/一致性导向（生产推荐）
- **Stream**：只追加日志，持久化，非破坏性消费

### 集群与高可用

- [Clustering](https://www.rabbitmq.com/docs/clustering)：普通集群，队列内容分布在多个节点
- [Cluster Formation](https://www.rabbitmq.com/docs/cluster-formation)：自动集群 formation（Kubernetes / AWS / DNS）
- 跨数据中心复制

### 监控与可观测性

- 内置管理界面（Web UI）
- [Prometheus + Grafana](https://www.rabbitmq.com/docs/prometheus) 集成
- [CLI 工具](https://www.rabbitmq.com/docs/cli) 完整
- [生产检查清单](https://www.rabbitmq.com/docs/production-checklist)

## 版本演进

| 系列 | 状态 | 说明 |
|------|------|------|
| **v4.x** | ⚡ 当前最新 | 2026年主流版本 |
| v3.13.x | 🔧 维护中 | 3.13为3系列最后一个次版本 |
| v3.12.x | 🔒 安全维护 | LTS过渡期 |

> ⚠️ MaxMES 系统目前使用 RabbitMQ 3.12.x（参考 [[mes/database]]），建议后续评估升级到 v4.x。

## 安装方式

### 推荐：Generic Unix 包（离线友好，内嵌 Erlang）

> 适合 SUSE 15 SP6 离线场景，无需预装 Erlang。

```bash
# 下载（带内嵌 Erlang，~150MB）
wget https://github.com/rabbitmq/rabbitmq-server/releases/download/v3.13.2/rabbitmq-server-generic-unix-3.13.2.tar.xz

# 解压
tar -xJvf rabbitmq-server-generic-unix-3.13.2.tar.xz -C /opt/
ln -s /opt/rabbitmq_server-3.13.2 /opt/rabbitmq

# 配置 PATH（erl 在 erts-*/bin/ 下）
echo 'export PATH=$PATH:/opt/rabbitmq/erts-*/bin:/opt/rabbitmq/sbin' >> /etc/profile.d/rabbitmq.sh
source /etc/profile.d/rabbitmq.sh

# 验证
rabbitmq-server --version
```

### 源码编译

见 [Building RabbitMQ from Source](https://www.rabbitmq.com/docs/build-server)。

### Kubernetes

使用 [Cluster Operator](https://www.rabbitmq.com/kubernetes/operator/operator-overview)。

## 常用端口

| 端口 | 用途 |
|------|------|
| 5672 | AMQP 客户端连接（默认） |
| 15672 | Web 管理界面（需启用 `rabbitmq_management` 插件） |
| 25672 | 集群内部通信 |
| 1883 | MQTT（需启用插件） |
| 61613 | STOMP over TCP |

## 重要文档索引

- ⭐ [生产检查清单](https://www.rabbitmq.com/docs/production-checklist)
- ⭐ [Quorum Queue](https://www.rabbitmq.com/docs/quorum-queues)
- ⭐ [集群配置](https://www.rabbitmq.com/docs/clustering)
- [升级指南](https://www.rabbitmq.com/docs/upgrade)
- [配置参考](https://www.rabbitmq.com/docs/configure)
- [监控指南](https://www.rabbitmq.com/docs/monitoring)
- [Stream 队列](https://www.rabbitmq.com/docs/streams)
- [Erlang 版本要求](https://www.rabbitmq.com/docs/which-erlang)

## 相关页面

- [[mes/database]] — MaxMES 数据库设计（含 RabbitMQ 3.12.x 配置）
- [[mes/architecture]] — MaxMES 系统架构（含消息队列设计）

## 更新记录

- 2026-04-24：初始化 RabbitMQ 实体页（来源：GitHub README + Releases）
