---
title: MaxMES 巡检报告 + 问题修复汇总
created: 2026-04-23
updated: 2026-04-23
type: concept
tags: [mes, troubleshooting, inspection, security]
sources: [raw/maxmes-knowledge-202604.pdf]
---

# MaxMES 巡检报告 + 问题修复汇总

> 来源：MaxMES 系统全面巡检报告 - 2026-04-18（52项问题）+ 2026-04-21 巡检

## 巡检概览

| 项目 | 值 |
|------|-----|
| 巡检时间 | 2026-04-18 19:30 / 2026-04-21 14:44 |
| 执行者 | 小马（AI助手） |
| 系统版本 | v1.3.0-p3 |
| 技术栈 | React + Ant Design + Node.js + MySQL + Redis |
| 问题总数 | 52 个（FE:20 / BE:20 / OPS:12） |
| P1 | 8 个 |
| P2 | 20 个 |
| P3 | 8 个 |

## 问题汇总

| 分类 | P1 | P2 | P3 | 状态 |
|------|----|----|----|------|
| 前端 (FE) | 2 | 7 | 11 | 大部分已修复 |
| 后端 (BE) | 5 | 10 | 5 | 大部分已修复 |
| 运维 (OPS) | 1 | 3 | 8 | 部分待处理 |
| **总计** | **8** | **20** | **8** | |

---

## P0 严重问题（GitHub 网络阻塞）

- **OPS-GH-1**: GitHub push 挂起 — HTTPS 443 端口正常，git ls-remote 正常，但 git push 超时。公司防火墙对 git 大流量传输限制。**2个 commit 待推送**（dfb4f65, 9ef2380）

---

## P1 紧急问题

### 前端 (FE)

| ID | 问题 | 影响文件 | 状态 |
|----|------|----------|------|
| FE-1 | XSS 漏洞 — dangerouslySetInnerHTML 未做过滤 | DocViewer.js | ✅ 已修复 |
| FE-3 | 依赖安全漏洞 — axios/react-router/lodash 版本过旧 | package.json | ✅ 已修复 |

### 后端 (BE)

| ID | 问题 | 影响文件 | 状态 |
|----|------|----------|------|
| BE-1 | 库存 API 返回硬编码模拟数据 | inventory.js | ✅ 已修复 |
| BE-2 | WebSocket 嵌套回调语法错误 | app.js 第144-152行 | ✅ 已修复 |
| BE-3 | CORS 配置允许所有来源 | app.js | ✅ 已修复 |
| BE-17 | 库存出库操作未实际更新数据库 | inventory.js 第78-81行 | ✅ 已修复 |

### 运维 (OPS)

| ID | 问题 | 状态 |
|----|------|------|
| OPS-1 | 数据库备份失败 — mysqldump PROCESS 权限不足 | ✅ 已修复 |
| OPS-5 | API 服务在 19:15 后无响应（PM2 重启94次） | 待调查 |
| OPS-6 | 备份脚本命令行明文密码泄露风险 | ✅ 已修复 |

---

## P2 重要问题

### 前端 (FE) — 7个

| ID | 问题 | 状态 |
|----|------|------|
| FE-2 | 登录页硬编码测试账号密码（quickLoginUsers） | ✅ 已修复 |
| FE-4 | localStorage 明文存储 token，无加密保护 | 待处理 |
| FE-5 | Dashboard useEffect dateRange 依赖导致无限循环风险 | 待处理 |
| FE-6 | safeMessage 递归调用可能导致堆栈溢出 | 待处理 |
| FE-11 | EmployeePersistence 定时器未清理，内存泄漏风险 | 待处理 |
| FE-15 | DataService 大部分方法返回 Mock 数据，未集成真实 API | 待处理 |
| FE-16/17 | axios 和 react-router-dom 版本过旧 | ✅ 已修复 |

### 后端 (BE) — 10个

| ID | 问题 | 影响文件 | 状态 |
|----|------|----------|------|
| BE-4 | 限流器使用内存存储，集群部署不共享 | rateLimiter.js | 待处理 |
| BE-5 | 输入清理过度移除合法字符（< >） | validation.js | ✅ 已修复 |
| BE-6 | masterData.js N+1 查询问题 | masterData.js | 待处理 |
| BE-7 | 三种缓存机制混用，职责不清 | responseCache/cacheService/middleware | 待处理 |
| BE-9 | 大量使用 console.error 而非结构化 logger | masterData.js 等多处 | 待处理 |
| BE-10 | enhancedCrud.js 动态表名拼接存在风险 | enhancedCrud.js | 待处理 |
| BE-11 | 缺少 helmet.js 安全响应头 | app.js | ✅ 已修复 |
| BE-13 | auth.js token 无效应返回 401 而非 403 | auth.js | ✅ 已修复 |
| BE-19 | 响应格式不一致 | 多个路由文件 | 待处理 |
| BE-20 | 密码强度验证不足（仅6位） | validation.js | ✅ 已修复 |

### 运维 (OPS) — 3个

| ID | 问题 | 状态 |
|----|------|------|
| OPS-2 | Redis 缓存命中率 0%，缓存未实际使用 | 待处理 |
| OPS-3 | PM2 进程名不一致导致监控告警失效 | 待处理 |
| OPS-4 | 监控检查 5002 端口但实际服务在 5000 | 待处理 |

---

## P3 已解决（上次巡检）

- FE-1: XSS 漏洞 ✅
- FE-3: 依赖安全漏洞 ✅
- BE-17: 库存出库不写DB ✅
- OPS-1: 数据库备份权限 ✅

---

## 本次修复汇总（FE-3 + BE-8）

**前端（FE）**：3 项
- FE-1 | DocViewer.js | XSS 漏洞修复 — 安装 dompurify@3.2.4，使用 DOMPurify.sanitize() 过滤 HTML
- FE-2 | LoginPage.js | 删除硬编码测试账号 quickLoginUsers
- FE-3 | package.json | 依赖安全更新 — axios 1.7.4→1.15.0，react-router-dom 6.22.0→6.30.0，lodash 4.17.15→4.17.25，新增 dompurify

**后端（BE）**：6 项
- BE-1 | inventory.js | 库存API硬编码→真实MySQL查询
- BE-2 | app.js | WebSocket嵌套回调→独立函数 handleJoinAndon()
- BE-3 | app.js | CORS全开→环境变量 CORS_ORIGIN 控制
- BE-5 | validation.js | 输入清理过度→移除XSS向量模式，保留合法业务字符
- BE-8 | mysql.js | 连接池大小不一致→统一为 connectionLimit: 20
- BE-11 | app.js | 缺少安全响应头→引入 express-helmet
- BE-13 | auth.js | 401误用403→jwt.verify失败返回401
- BE-17 | inventory.js | 出库不写DB→inventory.save() 持久化
- BE-20 | validation.js | 密码强度不足→最小8位，大小写字母+数字+特殊字符

---

## 2026-04-21 巡检状态

| 项目 | 状态 |
|------|------|
| Nginx 运行 | ✅ 正常（1.24.0），端口80/443监听 |
| PM2 服务 | ✅ mes-backend 和 mes-client-dev 均在线 |
| Redis | ✅ PONG响应，正常 |
| 磁盘空间 | ✅ 42GB可用（55%使用率） |
| Uptime-Kuma | ✅ 生产环境200 OK |
| API 认证 | ✅ 受保护接口正确返回401 |
| 生产构建 | ✅ build 14:21生成（main.b3d9fddd.js） |

**P0**: GitHub push 阻塞
**P1**: BE-1 错误率9.73% 需调查；BE-2 库存API硬编码待修复；Swap占用58% 观察

---

## 处理优先级建议

1. **立即处理**: FE-1(XSS) → BE-2(WebSocket语法) → BE-3(CORS) → OPS-5(API挂掉) → OPS-6(密码泄露)
2. **今日处理**: BE-1/BE-17(库存数据) → FE-3(依赖漏洞) → OPS-1(备份)
3. **本周处理**: 其余 P2 问题（安全+性能优化）
4. **计划处理**: P3 优化问题可纳入迭代计划

## 相关页面
- [[mes/index]] — MaxMES 知识库总览
- [[mes/operations]] — 运维手册
- [[mes/deployment]] — 部署手册
