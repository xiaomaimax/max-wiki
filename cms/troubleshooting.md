# CMS故障处理时间线

## 2024年8月 绍兴CMS严重故障

### 背景
8月5日用户反馈系统卡顿，随后一周经历多次故障和优化。

### 处理时间线

#### 8月5日 周一
1. 接到绍兴用户反馈系统卡顿
2. 连接服务器，发现应用服务器输入指令也卡顿
3. 反馈给供应商排查
4. 确认苏泊尔和供应商最近都未做变更
5. 下班后安排重启应用服务器

#### 8月6日 周二
6. 发现卡顿时间点和网络延迟高的时间一致，监控应用服务器网络延迟
7. 联系Infra查看服务器状态、日志，发现`/var/log/message`报错：**CPU stuck**
8. 供应商升级Linux核心并重启服务器

#### 8月7日 周三
9. 联系李金泉协助排查，发现**IO很高**，将应用服务器磁盘类别从精简置备升级为厚置备，增加8G内存
10. 建议供应商后续考虑将Tomcat和Kafka分开部署
11. 用户反馈系统卡顿频率有所降低，但还是会出现

#### 8月8日 周四
12. 怀疑杀毒软件导致，**11:24关闭两台服务器的falcon进程**
13. 卡顿频率继续降低

#### 8月9日 周五
14. 15:55-16:10 数据库服务器突然连接不上，通过jms也无法连接，后自动恢复
15. 汪经理协助排查

#### 8月10日-11日 系统优化
16. 优化PostgreSQL数据库配置参数
17. Linux系统升级：CentOS Linux release 7.9.2009 (Core)
18. 解决CPU stuck问题：修改`/proc/sys/kernel/watchdog_thresh`从10改为20
19. 关闭selinux和firewalld
20. KVM JVM内存从1G改为16G
21. 配置swap文件系统
22. 内核参数优化
23. 两台服务器CPU扩容到16核、内存调整为64G

#### 8月12日 周一
**仍存在问题：** 每天00:00 xxl job海关手册同步作业，运行后CPU、内存均拉满，kafka占用大量内核动态内存不释放。

**处理：**
- 所有java程序日志向kafka再写一次日志，全部注释掉
- tom-xxl的JVM参数16G→8G（后证明为负优化，改回16G）
- 苏泊尔新增一台服务器（32核、32G内存、Redhat8.7）

**JVM参数问题：**
- 错误配置：`-Xms8192M -Xmx8192M -Xmn8192M` → 新生代占满整个堆，老年代无空间，频繁Full GC
- 正确配置：删除`-Xmn8192M`参数

#### 8月13日 周二
24. 8:55左右 CPU 95%
25. war包接口程序导致GC回收重启，CPU冲高
26. 14:05 HAHPD01CMS无响应
27. 14:35-14:36 虚拟机重启，业务恢复
28. 22:00 服务器重新开关机
29. 22:00 **卸载Crowd Strike软件**

#### 8月22日 用户反馈历史文件丢失
- 【出口发货排程】模块全部文档打不开（21-24年，7000-8000集装箱/年）
- **根本原因分析：**
  - `/home/DATA/data/eternal` 目录28G（正常应有85G），文件丢失28万个
  - 最晚丢文件日期：8月11日周日
  - fsck前：159G → fsck后：88G
  - Photorec恢复10w+文件到`/recovery2/20240827`

---

## 故障根因总结

| 原因 | 解决措施 |
|------|----------|
| CPU stuck | watchdog_thresh 10→20，CentOS升级 |
| JVM参数错误 | 删除`-Xmn8192M`，保持`-Xms/-Xmx`一致 |
| Crowd Strike占用内核内存 | 卸载，启用后有条件观察 |
| 文件系统异常 | fsck修复，Photorec恢复文件 |
| xxl job定时任务 | 分离到独立服务器 |

---

## 运维命令

```bash
# 查看内核内存
smem --system -k

# 查看应用重启时间
cat /proc/[pid]

# 查看Falcon进程
ps -ef|grep falcon

# 文件数量
ls /data/eternal | wc -l

# 审计日志
ausearch -k CMS

# 文件系统检查
fsck

# 配置auditd监控
vi /etc/audit/rules.d/audit.rules
-w /home/DATA/data/eternal/ -p wa -k CMS
augenrules --load
ausearch -k CMS
```
