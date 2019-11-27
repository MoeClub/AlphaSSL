# AlphaSSL 申请教程
# [前往申请](https://api.moeclub.org/SSL)

## 申请前的准备
- 友好的域名邮箱(admin@domain.tld)
- 不友好的邮件服务商将会提示 "MX Record Not Allow."

### 如果没有域名邮箱,可用临时邮箱
- 公开的临时邮箱平台
- 内置API邮箱

#### 内置API邮箱使用方法
- 把 MX 记录解析至 api.moeclub.org 权重 10
- 只保留这一条 MX 记录
- 等待 MX 记录生效

## 准备CSR,并保存匹配的私钥
- *.domian.tld

## 申请证书
- 填入准备的CSR和Apply Token信息
- 点击 GetSSL

## 确认邮件
- 通过邮箱获得确认邮件
- 内置API邮箱获得确认邮件
  - 在填 CSR 的框框内填上 "MAIL" (不包括引号)
  - Apply Token 框内填申请时的 Apply Token
  - 点击 GetSSL, 即可看到提示.
  
## 获得证书
- 通过邮箱获得证书
- 内置API邮箱获得证书
  - 在填 CSR 的框框内填上 "VIEW" (不包括引号)
  - Apply Token 框内填申请时的 Apply Token
  - 点击 GetSSL, 即可看到申请的证书.

## 注意事项
- 确认链接有效期 30 天
- 由于与邮箱链接的不确定性, 邮箱可能在1秒或1个星期内才会收到确认邮件.请耐心等待

