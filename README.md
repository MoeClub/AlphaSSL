# AlphaSSL 申请教程 [[`前往申请`](https://api.moeclub.org/SSL)]

## AlpahSSL 证书申请流程概要
```

          确认拥有该域名的解析权或者 "admin@域名" 能够接收邮件
                           |
                           V
                 生成该域名的 CSR, 并保存好私钥
                           |
                           V
          检查域名的 MX 解析, 并确认使用 8.8.8.8 能过获得正确解析记录
          检查域名的 CAA 解析, 添加 0 issuewild "globalsign.com" 记录或暂停所有CAA解析
          检查域名的 CNAME 解析, 如果有则暂停解析
                     /               \
    内置API模式      /                 \
 (无需邮箱,强烈推荐) /                   \   自己使用 "admin@域名" 邮箱接收确认邮件
                 /                      \
                /                        \
   1. 将域名的 MX 记录解析到内置API           \
      "api.moeclub.org" 权重 10          1. 提交 CSR 并等待邮件到达
      等待解析生效并且稳定                  2. 按邮件提示确认申请
   2. 提交 CSR 并 等待5分钟                3. 等待证书内容邮件到达
   3. 【可选】使用 "MAIL" 控制命令             或者使用 "VIEW" 获取已补全证书链的证书内容
      确认邮件并返回成功提示
   4. 【可选】操作成功完成后即可改回解析
   5. 使用 "VIEW" 获取已补全证书链的证书内容

```

## 申请前须知
- 将会使用 `admin@domain.tld` 接收确认邮件及证书.
- 如遇到域名记录解析相关问题, 请检查域名注册商的`DNSSEC配置`和域名解析提供商的`DNSSEC配置`是否同时开启或者同时关闭.
- **删除所有 CAA 解析记录**或**添加值为 `0 issuewild "globalsign.com"`的 CAA 解析记录**       
  否则可能会**无法成功签发证书**. (***必须***)
- 暂停解析 CNAME 记录, 否则可能会**无法收到证书确认邮件**. (***必须***)      
  (如果为主域申请证书,只需要暂停主域的 CNAME 解析. 不需要暂停子域名的 CNAME 解析.)
- **如果没有域名邮箱, 可用`内置API邮箱`完成.**    
  使用的是`腾讯云(DNSPOD)`,`阿里云`等服务商提供的解析服务,请暂停该级域名一切解析并使用内置API邮箱完成签发.
- **AlphaSSL** 支持 **`RSA`** 和 **`ECC`** (`prime256v1`, `secp384r1`)

## 准备CSR并保存匹配的私钥
- 域名: `*.domian.tld`
- `自行准备CSR文件`或使用[`在线工具生成`](https://api.moeclub.org/SSL/CSR)
- 创建私钥文件 `server.key.pem`
   - 新建空白文本文件(txt文档)
   - 粘贴私钥内容到空白文本内并保存
   - 将文件重命名为 `server.key.pem` 得到私钥文件
- 创建证书文件 `server.cert.pem`
   - 新建空白文本文件(txt文档)
   - 等待签发下来的证书并粘贴的证书内容
   - 将文件重命名为 `server.cert.pem` 得到证书文件

## 申请证书的步骤
- **注意**: 如果使用`内置API邮箱`,请先查看并完成[`内置API邮箱使用方法`](https://github.com/MoeClub/AlphaSSL/blob/master/README.md#%E5%86%85%E7%BD%AEapi%E9%82%AE%E7%AE%B1%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90%E6%9C%80%E5%BF%AB%E5%8F%AA%E9%9C%80%E4%B8%A4%E5%88%86%E9%92%9F%E5%B0%B1%E5%8F%AF%E8%8E%B7%E5%BE%97%E8%AF%81%E4%B9%A6)章节中的步骤.
- 填入准备的CSR和Apply Token信息, 点击 "Get AlphaSSL!"
- 确认邮件 (手动点击收件箱中的邮件或者由内置API自动确认).
- 获得证书文件内容并填充证书文件.
- 将证书文件`(server.cert.pem)`与私钥文件`(server.key.pem)`打包成一组.

## 内置API邮箱使用方法(强烈推荐,最快只需两分钟就可获得证书)
- 修改待申请证书的域名的 MX 记录(主域名一般为`@`)
- **将 MX 记录解析至** `api.moeclub.org` **权重** `10`
- 如果有其他 MX 记录将其暂停, 只保留这一条 MX 记录
- 等待 MX 记录生效 (更改之前 TTL 设置的多少就等多少秒)
  ```
  设置解析示例: MX(解析类型)  10(权重)  @(主机名)  api.moeclub.org(目标值)
  *.abc.com --> MX 10 @ api.moeclub.org     
  *.sub.abc.com --> MX 10 sub api.moeclub.org
  ```
- **填入准备的CSR和Apply Token信息, 点击 "Get AlphaSSL!"**
- 等待 `5` 分钟
- **通过内置API获得证书**
  - **注意**:此项步骤前需要**确认邮件**, 内置API模式会自动确认邮件.
  - 在填 CSR 的框框内填上 "VIEW" (不包括引号,仅4个字母)
  - Apply Token 框内填申请时的 Apply Token
  - 点击 "Get AlphaSSL!", 即可看到申请的证书(已补全证书链).
  - **注意**: 此项操作如果未查询到证书,将会自动重发确认邮件.

## 通过内置API邮箱手动确认邮件
  - 在填 CSR 的框框内填上 "MAIL" (不包括引号,仅4个字母)
  - Apply Token 框内填申请时的 Apply Token
  - 点击 "Get AlphaSSL!", 即可看到提示.
  - **注意**: 内置API模式非特殊情况一般不需要进行此操作.

## 补全证书链(可选) [2027/10/12]
- 将下面字段粘贴至证书文件`(server.cert.pem)`末尾即可
```
-----BEGIN CERTIFICATE-----
MIIEijCCA3KgAwIBAgIQfU1CqStDHX5kU+fBmo1YdzANBgkqhkiG9w0BAQsFADBX
MQswCQYDVQQGEwJCRTEZMBcGA1UEChMQR2xvYmFsU2lnbiBudi1zYTEQMA4GA1UE
CxMHUm9vdCBDQTEbMBkGA1UEAxMSR2xvYmFsU2lnbiBSb290IENBMB4XDTIyMTAx
MjAzNDk0M1oXDTI3MTAxMjAwMDAwMFowTDELMAkGA1UEBhMCQkUxGTAXBgNVBAoT
EEdsb2JhbFNpZ24gbnYtc2ExIjAgBgNVBAMTGUFscGhhU1NMIENBIC0gU0hBMjU2
IC0gRzQwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQCtJCmVZhWIPzOH
A3jP1QwkuDFT8/+DImyZlSt85UpZwq7G0Sqd+n8gLlHIZypQkad5VkT7OLU+MI78
lC7LVwxpU19ExlaWL67ANyWG8XHx3AJFQoZhuDbvUeNzRQyQs6XS5wN6uDlF0Bf1
AtCUQWrGGLGYwyC1xTrzgrFKpESsIXMqklUGTsh8i7DKZhRUVfgrPLJUkbbLUrLY
42+KRCiwfSvBloC5PgDYnj3oMZ1aTe3Wfk3l1I4D3RKaJ4PU1qHXhHJOge2bjGIG
l6MsaBN+BB2sr6EnxX0xnMIbew2oIfOFoLqs47vh/GH4JN0qql2WBHfDPVDm3b+G
QxY6N/LXAgMBAAGjggFbMIIBVzAOBgNVHQ8BAf8EBAMCAYYwHQYDVR0lBBYwFAYI
KwYBBQUHAwEGCCsGAQUFBwMCMBIGA1UdEwEB/wQIMAYBAf8CAQAwHQYDVR0OBBYE
FE/LrKjC76vdg29rv86YPVxYJXYVMB8GA1UdIwQYMBaAFGB7ZhpFDZfKiVAvfQTN
NKj//P1LMHoGCCsGAQUFBwEBBG4wbDAtBggrBgEFBQcwAYYhaHR0cDovL29jc3Au
Z2xvYmFsc2lnbi5jb20vcm9vdHIxMDsGCCsGAQUFBzAChi9odHRwOi8vc2VjdXJl
Lmdsb2JhbHNpZ24uY29tL2NhY2VydC9yb290LXIxLmNydDAzBgNVHR8ELDAqMCig
JqAkhiJodHRwOi8vY3JsLmdsb2JhbHNpZ24uY29tL3Jvb3QuY3JsMCEGA1UdIAQa
MBgwCAYGZ4EMAQIBMAwGCisGAQQBoDIKAQMwDQYJKoZIhvcNAQELBQADggEBABol
9nNkiECpWQenQ7oVP1FhvRX/LWTdzXpdMmp/SELnEJhoOe+366E0dt8tWGg+ezAc
DPeGYPmp83nAVLeDpji7Nqu8ldB8+G/B6U9GB8i2DDIAqSsFEvcMbWb5gZ2/DmRN
cifGi9FKAuFu2wyft4s4DHwzL2CJ2zjMlUOM3RaE1cxuOs+Om6MCD9G7vnkAtSiC
/OOfHO902f4yI2a48K+gKaAf3lISFXjd32pwQ21LpM3ueIGydaJ+1/z8nv+C7SUT
5bHoz7cYU27LUvh1n2WSNnC6/QwFSoP6gNKa4POO/oO13xjhrLRHJ/04cKMbRALt
JWQkPacJ8SJVhB2R7BI=
-----END CERTIFICATE-----
```

# 校验证书文件与密钥文件是否匹配
```
# 密钥文件 server.key.pem
# 证书文件 server.cert.pem
# 下面两行命令结果输出一致则为匹配的证书

openssl x509 -in server.cert.pem -pubkey -noout -outform pem |openssl md5
openssl pkey -in server.key.pem -pubout -outform pem |openssl md5


```
  

## 注意事项
- 确认链接有效期 30 天
- 由于与邮箱链接的不确定性, 邮箱可能在1秒或1个星期内才会收到确认邮件.请耐心等待     
- 由于 `DNSPOD` 的解析是非标准实现,会**优化**一些基本特性.     
  如果您使用的是 `DNSPOD` 托管域名, 申请前请先暂停解析该级域名下的 CNAME, A, MX 记录. 然后使用内置API辅助签发.         
  建议更换至[**华为云DNS(无需实名,无需手机号)**](https://www.huaweicloud.com/intl/zh-cn/)或其他**标准实现**的DNS解析. (`DOSPOD`收费的功能`华为云DNS`几乎白给,TTL最短可设置为1)
