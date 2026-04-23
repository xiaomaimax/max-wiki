# OPC Server (Kepware)

## 下载与账号

- 下载：https://my.kepware.com/s/login/SelfRegister
- 账号：lmin@supor.com
- 密码：Max@2021
- Activation ID：8b35c16a-23f5-431f-8553-65eafc1622a2

## 服务器配置

硬件要求：CPU 4核，内存8G，C盘100G，Windows 2019 Server 64位英文版

### 各基地服务器

| 基地 | 生产服务器 | 开发服务器 | IP |
|------|-----------|-----------|-----|
| 武汉WUH | WUHDSM01 | WUHDSM02 | 172.24.84.129 / 172.24.84.130 |
| 绍兴SHX | SHXDSM01 | SHXDSM02 | 172.24.96.142 / 172.24.96.148 |
| 玉环YUH | YUHDSM01 | YUHDSM02 | 172.24.55.25 / 172.24.55.26 |
| 杭州HAH | HANPA01DSM | — | 172.24.24.75 |

DSM终端：10.65.53.100-130，255.255.255.0，网关10.65.53.254

## OPC Tag

- Tag取值：`EPI[#24*#2*#1] 400043`
- 正向有功电量-吸收有功电能
- 绍兴14块主电表ItemID：`SX-GW-172-24-96-153-P.sx-meter-172-24-96-153-01.EPI`

## Kepware到Thingworx配置

### 连接信息
| 环境 | 地址 | Key |
|------|------|-----|
| PPD | ppd-cn-dsm.supor.com | 2a8c8941-dbc5-4a6b-a758-fa6967e20d1a |
| PRD | cn-dsm.supor.com | 7757cf1a-175b-4a75-bae8-b4d0dab70361 |

### Thing Name
| 基地 | Thing Name |
|------|-----------|
| WUH | SEB.172.OPC.DSM_IC |
| YUH | SEB.174.OPC.DSM.SUPOR_IC |
| SHX | SEB.178.OPC.DSM.SUPOR_IC |
| HAN | SEB.177.OPC.DSM_IC |

### 通道配置
1. OPC_Server_To_ThingWorx（Sourcer: Kepware IP，Destination: cn-dsm.supor.com）
2. OPC_Server_To_Meters（Sourcer: Kepware IP，Destination: Lora网关IP）

### 端口要求
- 到Thingworx：all ports
- 到智能表/网关：ALL_ICMP, TCP_502

## Heartbeat配置

```
RAMP(1000,0,32000,1)
```

## 批量导入电表到Thingworx

1. Notepad++打开TemplateImportEnergyMeters.txt模板，按格式填写
   - SiteCode
   - Parent
   - CounterDesignation / InternationalName
   - CounterType（AM）
   - Formula（[A1]）
   - TagName（Virtual=False）
   - ConversionType（no_conversion）
2. Notepad++修改编码为UTF-8，另存为CSV格式
3. Thingworx选择能源主目录导入仪表
4. 选择csv文件，核对后导入

> 后续新增/删除电表只需在Kepware配置，自动同步Thingworx

## 防火墙

Kepware服务器防火墙需允许502、503端口

## 常见故障

### Kepware正常但Thingworx不更新
原因：OPC标签与Thingworx里不匹配 → 修改Thingworx配置

### 同一Kepware服务多个Site区分
> YUH和WMF可用同一台Kepware，Thingworx配置相同Industrial Connection Thing
建议：Kepware端按Site使用不同Channel区分，便于Thingworx配置
