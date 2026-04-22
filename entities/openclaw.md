---
title: OpenClaw
created: 2026-04-22
updated: 2026-04-22
type: entity
tags: [tool, ai-assistant, workflow]
sources: [raw/articles/学习笔记-2026.txt]
---

# OpenClaw

## 概述
OpenClaw 是一个 AI 助手框架，支持多平台（Ubuntu/Windows/Mac）、多渠道（飞书/Telegram/WhatsApp/Discord/Slack），通过 skill 扩展能力。

## 关键路径
| 路径 | 说明 |
|------|------|
| `~/.openclaw/openclaw.json` | 主配置 |
| `~/.openclaw/workspace/` | 默认工作区 |
| `~/.openclaw/agents/<cid>/` | 代理状态目录 |
| `~/.openclaw/credentials/` | OAuth & API 密钥 |
| `~/.openclaw/memory/<cid>.sqlite` | 向量索引存储 |
| `~/.openclaw/skills/` | 全局共享技能 |
| `/tmp/openclaw/*.log` | 网关日志 |

## 核心 CLI 命令
```bash
openclaw onboard          # 重新运行向导
openclaw status           # 查看整体状态
openclaw gateway status   # Gateway 运行状态
openclaw health           # 健康检查
openclaw configure        # 重新配置
openclaw daemon install   # 创建系统服务（自动启动）
openclaw daemon restart   # 重启后台服务
openclaw daemon logs      # 查看运行日志
openclaw gateway start/stop/restart  # WebSocket 网关
openclaw channels login   # WhatsApp QR 配对
openclaw channels add     # 添加 Telegram/Discord/Slack 机器人
openclaw channels status --probe  # 检查通道健康
openclaw doctor --deep    # 健康检查与快速修复
openclaw config get|set|unset  # 读写配置
openclaw models list|set|status  # 模型管理
openclaw devices list|approve <id>  # 设备审批
```

## 聊天内斜杠命令
| 命令 | 作用 |
|------|------|
| `/status` | 健康 + 上下文 |
| `/model <m>` | 切换模型 |
| `/compact` | 释放窗口空间 |
| `/new` | 全新会话 |
| `/stop` | 中止当前运行 |
| `/tts on\|off` | 切换语音 |
| `/think` | 切换推理模式 |

## 灵魂三件套
[[openclaw-soul-triplet]]

## Skill 能力
[[openclaw-skills]]

## 安装方式
- **Mac/Linux/WSL2**: `curl -fsSL https://openclaw.ai/install.sh | bash`
- **Windows**: 需可访问 GitHub 的网络
  ```powershell
  $env:OPENCLAW_INSTALL_DIR = "D:\OpenClaw"
  iwr -useb https://openclaw.ai/install.ps1 | iex
  ```

## 相关
- [[openclaw-soul-triplet]]
- [[openclaw-skills]]
- [[沟通技巧]]
