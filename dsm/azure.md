# Azure云架构

## 门户

- 链接：https://portal.azure.cn
- 账号：lmin@supor1.partner.onmschina.cn

## 订阅

| 订阅名 | 用途 |
|--------|------|
| Digital Shopfloor Management - PPD | 测试环境 |
| Digital Shopfloor Management - PRD | 生产环境 |
| Digital Shopfloor Management - SHA | 共享资源 |

> 注：将DSM-PPD、DSM-PRD共享资源放到DSM-SHA订阅中，实现共享资源的独立分账

## Thingworx

**PPD测试环境：**
- 地址：https://ppd-cn-dsm.supor.com/Thingworx
- Key：2a8c8941-dbc5-4a6b-a758-fa6967e20d1a

**生产环境：**
- 地址：https://cn-dsm.supor.com/Thingworx
- Key：7757cf1a-175b-4a75-bae8-b4d0dab70361

### Thingworx访问权限申请
1. 账号申请网址：https://cn-dsm.supor.com/Thingworx
2. 从AccessRequest组查找申请用户，编辑该用户
3. 指定该用户登录页，复制home mashup：SEB.ENER.CONFIG.Mashup.EnergyMetersSetting
4. 添加该用户到DSMAdmin管理员组，从access group删除

### 各基地Thing Name
| 基地 | Site Code | Thing Name |
|------|-----------|------------|
| WUH 武汉 | 1210 | SEB.172.OPC.DSM_IC |
| YUH 玉环 | 1110 | SEB.174.OPC.DSM.SUPOR_IC |
| SHX 绍兴 | 2210 | SEB.178.OPC.DSM.SUPOR_IC |
| HAN 杭州 | 2110 | SEB.177.OPC.DSM_IC |

## WAF防火墙

### Azure WAF
- 定价：约$10/月（每小时$0.0144 × 730小时）
- 功能：检测SQL注入、XSS等攻击
- 日志与Azure Monitor集成

### AWS WAF
- 按Web ACL和规则数收费
- Bot Control：每月前10M请求免费

> 注意：AWS WAF只能集成REST API，无法保护HTTP API。如有HTTP API需改为REST。

## Azure用户管理

### 新建SEB用户（2025.2.5）
用户：Alexis MARTINIER
邮箱：alexis.martinier@viseo.com
Azure登入：alexis.martinier@supor1.partner.onmschina.cn
登入密码：Nuna628774
订阅：PPD / PRD / SHA
角色：所有者

## HTTPS证书

- Azure Key Vault：https://portal.azure.cn/.../Microsoft.KeyVault/vaults/gsebdsprdshae2chkvt01/certificates
- dsm.supor.com证书由SEB提供

## PPD自动关机

> Azure中国区不提供auto-shutdown功能，需寻找替代方案
