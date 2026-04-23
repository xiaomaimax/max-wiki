# AWS云架构

## AWS账户

**DSM PPD测试：**
- 控制台：https://260547014985.signin.amazonaws.cn/console
- Account ID：260547014985

**DSM PRD生产：**
- 控制台：https://823841347192.signin.amazonaws.cn/console
- Account ID：823841347192

## IAM Role

Role Name：`deployrole`
Role ARN：`arn:aws-cn:iam::260547014985:role/deployrole`
权限：除CreateUser、CreateRole、CreateAccessKey外的所有管理员权限

> IAM Role类似临时门卡，比IAM User（员工卡）更安全

## S3存储桶

| 环境 | Bucket | Path |
|------|--------|------|
| QA | d4i-s3-energy-raw-devcn | /data/in/sap |
| Production | d4i-s3-energy-raw-prodcn | /data/in/sap |

### QA账号
- Username：qa-bpm-dsm-rw-s3
- Group：AccessS3Bucket
- Access Key：AKIATZ...RC6A
- Secret：44Nu46jUASbymT8Mdf1m7shZLgqSdctemNT7Yfl1

### Production账号
- Username：pro-dsm-rw-s3
- Group：AccessS3Bucket
- Access Key：AKIA37...LLPX
- Secret：o4XdIf6WPptJaKwiQ5plAYn786fPdgIaXd5Q1CCw

## API Gateway

> 中国区需单独申请开通80与443端口

测试命令：
```bash
curl POST https://qm81smsjfk.execute-api.cn-northwest-1.amazonaws.com.cn/energy/calendar \
  -H "Content-Type: application/json" \
  -d 'testData' -k
```

测试地址：https://zo8t0njxr6.execute-api.cn-northwest-1.amazonaws.com.cn/my-function

账号权限：
- DSM QA Account：260547014985
- DSM PROD Account：823841347192

## AWS成本问题

**问题：** test环境（PPD）费用高于生产环境
**原因：** 供应商月账单将所有虚拟机包年费用归属到PPD订阅
**现状：** 待解决
