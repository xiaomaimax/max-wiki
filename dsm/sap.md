# SAP数据接口

## SAP文件服务器

- 服务器：HAHPD01FS（172.24.20.195）
- 武汉FTP目录：D:\DSM\WUH

## SAP导出数据类型

文件名格式：`DATALAKE_FILE_TYPE_YYYYMMDDHHMMSS.csv`

每天上传4个CSV文件到AWS S3：
1. `DATALAKE_BOM_date.csv` — BOM数据（18个字段）
2. `DATALAKE_HEADER_date.csv` — 工单头数据（44个字段）
3. `DATALAKE_ROUTING_date.csv` — 工艺路线数据（17个字段）
4. `DATALAKE_DECLARATION_date.csv` — 报工数据（20个字段）

## SAP程序

| 程序 | 变式 | 用途 | 运行时间 |
|------|------|------|----------|
| ZZJ2D005 | 1210等 | 工单数据 | 每天01:00 |
| ZZJ2D006 | 1210等 | 报工数据 | 每天02:00 |

用户：JOBADMIN，语言：EN，频率：每天

## 报工数据说明

**字段6类型说明：**
- `qty_log`：工单MIGO入库数量
- `qty_shit`：CO11报工数量（合格）
- `scrap_ope`：CO11报工数量（报废）

**输入参数说明：**
- Logistic quantity：取SAP入库数量（Storage quantity）
- Shift quantity：AFRU-GMNGA字段（去掉ZB01限制）
- Operation scrap：正常
- Component scrap quantity：SUPOR不存在此业务
- Rework quantity：SUPOR没有

## 各基地SAP对接

| 基地 | 工厂代码 | 开始时间 |
|------|----------|----------|
| 武汉 | 1210 | 2023.7.14 |
| 绍兴 | 2210 | 2024.3.26 |
| WMF | 1510 | 2024.3.30 |
| 玉环 | 1110 | 2024.4.18 |
| 杭州 | 2110 | 2024.12.6 |

## SAP RFC连接

| 服务器 | 主机名 | IP |
|--------|--------|-----|
| C86 | rfc-c86.supor.com | 172.24.18.13 |
| P86 | rfc-sap.supor.com | 172.24.20.29 |

## 管理结构讨论

**问题：** SAP多个工作中心对应一个成本中心对应车间线体
**影响：** 注塑机只能统计到吨位产量，无法精确到每台注塑机

**解决方案：**
- 总装线用工作中心
- 其他线用班组对应（但班组信息失真）
- 最终结论：继续使用工作中心，不做单台注塑机分析

**SAP管理结构：** 工厂 → 车间 → 产线 → 设备

## FTP同步脚本

**QA环境：**
```batch
"C:\Program Files\S3 Browser\s3browser-cli.exe" /file sync DSM-QA d:\DSM\WUH s3:d4i-s3-energy-raw-devcn/data/in/sap nchs
```

**PRO环境：**
```batch
"C:\Program Files\S3 Browser\s3browser-cli.exe" /file sync DSM-PRO d:\DSM\WUH s3:d4i-s3-energy-raw-prodcn/data/in/sap nchs
```

Access Key: AKIA37...LLPX
Secret: o4XdIf6WPptJaKwiQ5plAYn786fPdgIaXd5Q1CCw

## 备份策略

每天备份D:\DSM到D:\backup\backup-YYYYMMDD
每周一删除7天前备份
