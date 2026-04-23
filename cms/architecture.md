# CMS系统架构

## 服务器

| 服务器 | IP | 职责 |
|--------|-----|------|
| HAHPA01CMS | - | 应用服务器（绍兴） |
| HAHPA02CMS | - | 应用服务器（绍兴） |
| PostgreSQL | - | 数据库服务器 |

## 文件存储

| 目录 | 说明 |
|------|------|
| /home/DATA/data/eternal | 上传文件存储（28G/85G，丢过文件） |
| /home/DATA/ics/gw-client | 关务客户端 |
| /recovery2/20240827 | Photorec恢复文件（10w+ jpg） |

## 系统要求

- **OS：** RedHat 8.7以上（支持Linux）
- **JDK：** Java 17（苏泊尔要求）
- **中间件：** Tomcat + Kafka

## 软件问题

**Crowd Strike杀毒软件：**
- 8月8日 11:24 关闭两台服务器falcon进程
- 8月9日 23:00 卸载
- 8月12日 11:00 重新安装
- 8月13日 22:00 再次卸载
- **核心内存被占用与Crowd Strike有关，卸载后可正常释放**

## 文件备份方案

| 方案 | 说明 |
|------|------|
| NBU备份 | 备份目标服务器/home/DATA/data，支持增量+全量 |
| LVM快照 | 创建时间点快照，接近无中断备份 |
| 裸设备备份 | 直接备份/dev/sdb1 |
| rsync同步 | 分布式存储iSCSI Block Device |

**恢复目标路径：** /backup/DATA/data
