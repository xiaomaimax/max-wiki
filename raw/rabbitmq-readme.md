# RabbitMQ README (来源: https://github.com/rabbitmq/rabbitmq-server)

> 原文存档时间: 2026-04-24 | 版本: v4.2.5 (2026-04-21)

# RabbitMQ Server

[![CI](https://github.com/rabbitmq/rabbitmq-server/actions/workflows/test-make.yaml/badge.svg)](https://github.com/rabbitmq/rabbitmq-server/actions/workflows/test-make.yaml)

[RabbitMQ](https://rabbitmq.com) is a [feature rich](https://www.rabbitmq.com/docs), multi-protocol messaging and streaming broker. It supports:

- AMQP 1.0
- AMQP 0-9-1
- RabbitMQ Stream Protocol
- MQTT 3.1, 3.1.1, and 5.0
- STOMP 1.0 through 1.2
- MQTT over WebSocket
- STOMP over WebSocket
- AMQP 1.0 over WebSocket (supported in VMware Tanzu RabbitMQ)

## Installation

- [Currently supported](https://www.rabbitmq.com/release-information) released series
- [Installation guides](https://www.rabbitmq.com/docs/download) for various platforms
- [Kubernetes Cluster Operator](https://www.rabbitmq.com/kubernetes/operator/operator-overview)
- [Changelog](https://www.rabbitmq.com/release-information)
- [Releases](https://github.com/rabbitmq/rabbitmq-server/releases) on GitHub
- [Supported Erlang versions](https://www.rabbitmq.com/docs/which-erlang)

## Tutorials and Documentation

- [RabbitMQ tutorials](https://www.rabbitmq.com/tutorials) and their [executable versions on GitHub](https://github.com/rabbitmq/rabbitmq-tutorials)
- [Documentation guides](https://rabbitmq.com/docs/)
- [RabbitMQ blog](https://blog.rabbitmq.com/)

Some key doc guides include:

- [CLI tools guide](https://www.rabbitmq.com/docs/cli)
- [Clustering](https://www.rabbitmq.com/docs/clustering) and [Cluster Formation](https://www.rabbitmq.com/docs/cluster-formation)
- [Configuration guide](https://www.rabbitmq.com/docs/configure)
- [Client libraries and tools](https://www.rabbitmq.com/client-libraries/devtools)
- [Upgrading](https://www.rabbitmq.com/docs/upgrade)
- [Quorum queues](https://www.rabbitmq.com/docs/quorum-queues): a replicated, data safety- and consistency-oriented queue type
- [Streams](https://www.rabbitmq.com/docs/streams): a persistent and replicated append-only log with non-destructive consumer semantics
- [Monitoring](https://www.rabbitmq.com/docs/monitoring) and [Prometheus/Grafana](https://www.rabbitmq.com/docs/prometheus)
- [Production checklist](https://www.rabbitmq.com/docs/production-checklist)

## Commercial Features and Support

- [Commercial editions of RabbitMQ](https://tanzu.vmware.com/rabbitmq)
- [Commercial support](https://tanzu.vmware.com/rabbitmq/oss) from Broadcom for open source RabbitMQ

## Getting Help from the Community

- GitHub Discussions: https://github.com/rabbitmq/rabbitmq-server/discussions/
- Community Discord server: https://rabbitmq.com/discord/
- `#rabbitmq` on Libera Chat

## Contributing

See [CONTRIBUTING.md](./CONTRIBUTING.md) and our [development process overview](https://www.rabbitmq.com/github).

## Licensing

RabbitMQ server is licensed under the MPL 2.0 (see LICENSE-MPL-RabbitMQ).

## Building From Source and Packaging

- [Contributor resources](https://github.com/rabbitmq/contribute)
- [Building RabbitMQ from Source](https://www.rabbitmq.com/docs/build-server)
- [Building RabbitMQ Distribution Packages](https://www.rabbitmq.com/docs/build-server)

## Copyright

(c) 2007-2026 Broadcom. All Rights Reserved.
