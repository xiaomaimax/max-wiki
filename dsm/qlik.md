# Qlik看板

## 访问地址

- Qlik官网：https://login.qlik.com/
- Qlik测试看板：https://suporqs02.t.supor.com/sense/app/1cf49995-d194-4175-a9cb-047f70a6533f/overview
- SAAS(Edge)：https://sebdataviz.eu.qlikcloud.com/

## 账号

| 账号 | 用户名 | 密码 |
|------|--------|------|
| lmin@supor.com | — | Max@2021 |
| seb_dsmbi（admin） | — | AXDHs78Q |
| emorelle | seb_emorelle | PDYJcwQ3 |
| ccolle | seb_ccolle | 66dlBEG^ |
| aghesquieres | seb_aghesquieres | AYHB6wMs |
| abec | seb_abec | fvEC2MeR |
| csmaroni | seb_csmaroni | mf3jPgBK |
| msouchon | seb_msouchon | pJRU35Yh |
| vguers | seb_vguers | TX8NsQ3C |
| fchen | seb_fchen | 9NANqPde |
| afavetto | seb_afavetto | kyrCFZ3R |
| haliu | seb_haliu | f2EaZ0dH |

## QMC配置

### 新建用户步骤
1. 访问QMC → License usage summary
2. Analyzer/Professional管理员组
3. License usage → Analyzer access allocated，搜索用户域账号，点击Allocate
4. 选择Security rules，添加账号

> 新建域账号到QMC需要手动做同步任务

### Streaming Buffer配置
1. 停止Engine Server
2. 打开 `C:\ProgramData\Qlik\Sense\Engine`
3. 备份Settings.ini
4. 添加 `StreamingBufferSizeMB=1024`
5. 保存并重启Engine Service

## 邮件提醒配置

- 配置邮箱：seb_dsmbi@supor.com
- SMTP：mail.supor.com
- Port：25

## 刷新机制

- 每天4次：8点、12点、16点、24点
- 今天看到的是昨天的数据（产量数据是前天）
- 刷新任务：任务1触发任务2，任务2触发任务3

## 各基地Qlik账号

### 武汉
| 部门 | 账号 |
|------|------|
| 何健飞 | dsm_zjb |
| 总经办 | — |
| OPS项目部 | dsm_ops |
| 财务部 | dsm_zzyb |
| 制造一部 | dsm_zzeb |
| 设备动力部 | dsm_sb |
| 工艺部 | dsm_gy |

### 绍兴
| 部门 | 账号 |
|------|------|
| 制造部 | sx_dsm_zz |
| 注塑部 | sx_dsm_zs |
| 财务部 | sx_dsm_cw |
| 工艺部 | sx_dsm_gy |
| HPC | sx_dsm_hpc |

### 杭州
| 部门 | 账号 |
|------|------|
| 财务部 | hz_dsm_cw |
| 设备工程部 | hz_dsm_sbgc |
| 物控部 | hz_dsm_wc |
| 工艺部 | hz_dsm_gy |
| 制造一部 | hz_dsm_zzr |
| 制造二部 | hz_dsm_zzs |

### 玉环WMF
待补充

## 报警功能

- DSM费用每年7.1万欧，报警模块每年2.1万欧
- 非生产时间设定能耗上限，超出报警
- 关键设备设定单位能耗上限，超出报警
