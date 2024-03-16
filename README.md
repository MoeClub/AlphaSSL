# AlphaSSL 申请教程 [[`前往申请`](https://alphassl.libmk.com/)]

## AlpahSSL 证书申请流程概要
```

          确认拥有该域名的解析权或者 "admin@域名" 能够接收邮件
                           |
                           V
                 生成该域名的 CSR, 并保存好私钥
                           |
                           V
          检查域名的 A 解析, 确认为 69.175.0.0/17。比如：69.175.0.0。
          检查域名的 MX 解析, 并确认使用 8.8.8.8 能过获得正确解析记录
          检查域名的 CAA 解析, 添加 0 issuewild "globalsign.com" 记录或暂停所有CAA解析
          检查域名的 CNAME 解析, 如果有则暂停解析
                     /               \
    内置API模式      /                 \
 (无需邮箱,强烈推荐) /                   \   自己使用 "admin@域名" 邮箱接收确认邮件
                 /                      \
                /                        \
   1. 将域名的 MX 记录解析到内置API           \
      "api.libmk.com" 权重 10          1. 提交 CSR 并等待邮件到达
      等待解析生效并且稳定                  2. 按邮件提示确认申请
   2. 提交 CSR 并等待2分钟                 3. 等待证书内容邮件到达
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
- `自行准备CSR文件`或使用[`在线工具生成`](https://api.libmk.com/SSL/CSR)
- 创建私钥文件 `server.key.pem`
   - 新建空白文本文件(txt文档)
   - 粘贴私钥内容到空白文本内并保存
   - 将文件重命名为 `server.key.pem` 得到私钥文件
- 创建证书文件 `server.crt.pem`
   - 新建空白文本文件(txt文档)
   - 等待签发下来的证书并粘贴的证书内容
   - 将文件重命名为 `server.crt.pem` 得到证书文件

## 申请证书的步骤
- **注意**: 如果使用`内置API邮箱`,请先查看并完成[`内置API使用方法`](https://github.com/MoeClub/AlphaSSL/blob/master/README.md#%E5%86%85%E7%BD%AEapi%E4%BD%BF%E7%94%A8%E6%96%B9%E6%B3%95%E5%BC%BA%E7%83%88%E6%8E%A8%E8%8D%90%E6%9C%80%E5%BF%AB%E5%8F%AA%E9%9C%80%E4%B8%A4%E5%88%86%E9%92%9F%E5%B0%B1%E5%8F%AF%E8%8E%B7%E5%BE%97%E8%AF%81%E4%B9%A6)章节中的步骤.
- 填入准备的CSR和Apply Token信息, 点击 "Get AlphaSSL!"
- 确认邮件 (手动点击收件箱中的邮件或者由内置API自动确认).
- 获得证书文件内容并填充证书文件.
- 将证书文件`(server.crt.pem)`与私钥文件`(server.key.pem)`打包成一组.

## 内置API使用方法(强烈推荐,最快只需两分钟就可获得证书)
- 修改待申请证书的域名的 MX 记录(主域名一般为`@`)
- **将 MX 记录解析至** `api.libmk.com` **权重** `10`
- 如果有其他 MX 记录将其暂停, 只保留这一条 MX 记录
- 等待 MX 记录生效 (更改之前 TTL 设置的多少就等多少秒)
  ```
  设置解析示例: MX(解析类型)  10(权重)  @(主机名)  api.libmk.com(目标值)
  *.abc.com --> MX 10 @ api.libmk.com    
  *.sub.abc.com --> MX 10 sub api.libmk.com
  ```
- **填入准备的CSR和Apply Token信息, 点击 "Get AlphaSSL!"**
- 等待 `2` 分钟
- **通过内置API获得证书**
  - **注意**:此项步骤前需要**确认邮件**, 内置API模式会自动确认邮件.
  - 在 Apply Token 框内填申请时的 Apply Token
  - 点击 "View AlphaSSL!", 即可看到申请的证书(已补全证书链).
  - **注意**: 此项操作如果未查询到证书,将会自动重发确认邮件.

## 通过内置API邮箱手动确认邮件
  - 在填 CSR 的框框内填上 "MAIL" (不包括引号,仅4个字母)
  - Apply Token 框内填申请时的 Apply Token
  - 点击 "Get AlphaSSL!", 即可看到提示.
  - **注意**: 内置API模式非特殊情况一般不需要进行此操作.

## 补全证书链    
##### 将下面字段`(证书链)`粘贴至证书文件`(server.crt.pem)`末尾即可
- ##### GlobalSign GCC R6 AlphaSSL CA 2023 [2024/01/29 ~]
```
-----BEGIN CERTIFICATE-----
MIIFjDCCA3SgAwIBAgIQfx8skC6D0OO2+zvuR4tegDANBgkqhkiG9w0BAQsFADBM
MSAwHgYDVQQLExdHbG9iYWxTaWduIFJvb3QgQ0EgLSBSNjETMBEGA1UEChMKR2xv
YmFsU2lnbjETMBEGA1UEAxMKR2xvYmFsU2lnbjAeFw0yMzA3MTkwMzQzMjVaFw0y
NjA3MTkwMDAwMDBaMFUxCzAJBgNVBAYTAkJFMRkwFwYDVQQKExBHbG9iYWxTaWdu
IG52LXNhMSswKQYDVQQDEyJHbG9iYWxTaWduIEdDQyBSNiBBbHBoYVNTTCBDQSAy
MDIzMIIBIjANBgkqhkiG9w0BAQEFAAOCAQ8AMIIBCgKCAQEA00Jvk5ADppO0rgDn
j1M14XIb032Aas409JJFAb8cUjipFOth7ySLdaWLe3s63oSs5x3eWwzTpX4BFkzZ
bxT1eoJSHfT2M0wZ5QOPcCIjsr+YB8TAvV2yJSyq+emRrN/FtgCSTaWXSJ5jipW8
SJ/VAuXPMzuAP2yYpuPcjjQ5GyrssDXgu+FhtYxqyFP7BSvx9jQhh5QV5zhLycua
n8n+J0Uw09WRQK6JGQ5HzDZQinkNel+fZZNRG1gE9Qeh+tHBplrkalB1g85qJkPO
J7SoEvKsmDkajggk/sSq7NPyzFaa/VBGZiRRG+FkxCBniGD5618PQ4trcwHyMojS
FObOHQIDAQABo4IBXzCCAVswDgYDVR0PAQH/BAQDAgGGMB0GA1UdJQQWMBQGCCsG
AQUFBwMBBggrBgEFBQcDAjASBgNVHRMBAf8ECDAGAQH/AgEAMB0GA1UdDgQWBBS9
BbfzipM8c8t5+g+FEqF3lhiRdDAfBgNVHSMEGDAWgBSubAWjkxPioufi1xzWx/B/
yGdToDB7BggrBgEFBQcBAQRvMG0wLgYIKwYBBQUHMAGGImh0dHA6Ly9vY3NwMi5n
bG9iYWxzaWduLmNvbS9yb290cjYwOwYIKwYBBQUHMAKGL2h0dHA6Ly9zZWN1cmUu
Z2xvYmFsc2lnbi5jb20vY2FjZXJ0L3Jvb3QtcjYuY3J0MDYGA1UdHwQvMC0wK6Ap
oCeGJWh0dHA6Ly9jcmwuZ2xvYmFsc2lnbi5jb20vcm9vdC1yNi5jcmwwIQYDVR0g
BBowGDAIBgZngQwBAgEwDAYKKwYBBAGgMgoBAzANBgkqhkiG9w0BAQsFAAOCAgEA
fMkkMo5g4mn1ft4d4xR2kHzYpDukhC1XYPwfSZN3A9nEBadjdKZMH7iuS1vF8uSc
g26/30DRPen2fFRsr662ECyUCR4OfeiiGNdoQvcesM9Xpew3HLQP4qHg+s774hNL
vGRD4aKSKwFqLMrcqCw6tEAfX99tFWsD4jzbC6k8tjSLzEl0fTUlfkJaWpvLVkpg
9et8tD8d51bymCg5J6J6wcXpmsSGnksBobac1+nXmgB7jQC9edU8Z41FFo87BV3k
CtrWWsdkQavObMsXUPl/AO8y/jOuAWz0wyvPnKom+o6W4vKDY6/6XPypNdebOJ6m
jyaILp0quoQvhjx87BzENh5s57AIOyIGpS0sDEChVDPzLEfRsH2FJ8/W5woF0nvs
BTqfYSCqblQbHeDDtCj7Mlf8JfqaMuqcbE4rMSyfeHyCdZQwnc/r9ujnth691AJh
xyYeCM04metJIe7cB6d4dFm+Pd5ervY4x32r0uQ1Q0spy1VjNqUJjussYuXNyMmF
HSuLQQ6PrePmH5lcSMQpYKzPoD/RiNVD/PK0O3vuO5vh3o7oKb1FfzoanDsFFTrw
0aLOdRW/tmLPWVNVlAb8ad+B80YJsL4HXYnQG8wYAFb8LhwSDyT9v+C1C1lcIHE7
nE0AAp9JSHxDYsma9pi4g0Phg3BgOm2euTRzw7R0SzU=
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIFUTCCBDmgAwIBAgIQdR4/VknnTLv2nQAmtnyqjDANBgkqhkiG9w0BAQwFADBX
MQswCQYDVQQGEwJCRTEZMBcGA1UEChMQR2xvYmFsU2lnbiBudi1zYTEQMA4GA1UE
CxMHUm9vdCBDQTEbMBkGA1UEAxMSR2xvYmFsU2lnbiBSb290IENBMB4XDTE5MDYx
OTAwMDAwMFoXDTI4MDEyODEyMDAwMFowTDEgMB4GA1UECxMXR2xvYmFsU2lnbiBS
b290IENBIC0gUjYxEzARBgNVBAoTCkdsb2JhbFNpZ24xEzARBgNVBAMTCkdsb2Jh
bFNpZ24wggIiMA0GCSqGSIb3DQEBAQUAA4ICDwAwggIKAoICAQCVB+hzymb57BTK
ezz3DQjxtEULLIK0SMbrWzyug7hBkjMUpG9/6SrMxrCIa8W2idHGsv8UzlEUIexK
3RtaxtaH7k06FQbtZGYLkoDKRN5zlE7zp4l/T3hjCMgSUG1CZi9NuXkoTVIaihqA
txmBDn7EirxkTCEcQ2jXPTyKxbJm1ZCatzEGxb7ibTIGph75ueuqo7i/voJjUNDw
GInf5A959eqiHyrScC5757yTu21T4kh8jBAHOP9msndhfuDqjDyqtKT285VKEgdt
/Yyyic/QoGF3yFh0sNQjOvddOsqi250J3l1ELZDxgc1Xkvp+vFAEYzTfa5MYvms2
sjnkrCQ2t/DvthwTV5O23rL44oW3c6K4NapF8uCdNqFvVIrxclZuLojFUUJEFZTu
o8U4lptOTloLR/MGNkl3MLxxN+Wm7CEIdfzmYRY/d9XZkZeECmzUAk10wBTt/Tn7
g/JeFKEEsAvp/u6P4W4LsgizYWYJarEGOmWWWcDwNf3J2iiNGhGHcIEKqJp1HZ46
hgUAntuA1iX53AWeJ1lMdjlb6vmlodiDD9H/3zAR+YXPM0j1ym1kFCx6WE/TSwhJ
xZVkGmMOeT31s4zKWK2cQkV5bg6HGVxUsWW2v4yb3BPpDW+4LtxnbsmLEbWEFIoA
GXCDeZGXkdQaJ783HjIH2BRjPChMrwIDAQABo4IBIjCCAR4wDgYDVR0PAQH/BAQD
AgEGMA8GA1UdEwEB/wQFMAMBAf8wHQYDVR0OBBYEFK5sBaOTE+Ki5+LXHNbH8H/I
Z1OgMB8GA1UdIwQYMBaAFGB7ZhpFDZfKiVAvfQTNNKj//P1LMD0GCCsGAQUFBwEB
BDEwLzAtBggrBgEFBQcwAYYhaHR0cDovL29jc3AuZ2xvYmFsc2lnbi5jb20vcm9v
dHIxMDMGA1UdHwQsMCowKKAmoCSGImh0dHA6Ly9jcmwuZ2xvYmFsc2lnbi5jb20v
cm9vdC5jcmwwRwYDVR0gBEAwPjA8BgRVHSAAMDQwMgYIKwYBBQUHAgEWJmh0dHBz
Oi8vd3d3Lmdsb2JhbHNpZ24uY29tL3JlcG9zaXRvcnkvMA0GCSqGSIb3DQEBDAUA
A4IBAQDHrE3fEsZgYRw59I03e5wt03B45il4hAHmquLc33pbkGZn6r3GgoKVzvwC
aBgtl6Jp93gZD8G5UjAFLj840jWDhOP7KSX6Q7rGjOsWNFFDJJLDUKQeJpB1PTRu
HqVI15zxiCl/VCP7mbTW7X/pILaFOPO+T0Qj+TUOU37WOjk6wdeyyOFiDhKSwH2Y
VE4YlAo0R10Jo3uNnSCFBgPw7gy1xt1+ajCbnzZYpQNXFy/0Lp9h3JOClE7TGvli
FUazCjxvhHm5YWqulA51wFT2K9LRiiEWw3UJAgTTmxASitVHHLb3erkETk6SCwGv
OG1eD0qLwuSeARZmhw3xFOCvMHeQ
-----END CERTIFICATE-----
-----BEGIN CERTIFICATE-----
MIIDdTCCAl2gAwIBAgILBAAAAAABFUtaw5QwDQYJKoZIhvcNAQEFBQAwVzELMAkG
A1UEBhMCQkUxGTAXBgNVBAoTEEdsb2JhbFNpZ24gbnYtc2ExEDAOBgNVBAsTB1Jv
b3QgQ0ExGzAZBgNVBAMTEkdsb2JhbFNpZ24gUm9vdCBDQTAeFw05ODA5MDExMjAw
MDBaFw0yODAxMjgxMjAwMDBaMFcxCzAJBgNVBAYTAkJFMRkwFwYDVQQKExBHbG9i
YWxTaWduIG52LXNhMRAwDgYDVQQLEwdSb290IENBMRswGQYDVQQDExJHbG9iYWxT
aWduIFJvb3QgQ0EwggEiMA0GCSqGSIb3DQEBAQUAA4IBDwAwggEKAoIBAQDaDuaZ
jc6j40+Kfvvxi4Mla+pIH/EqsLmVEQS98GPR4mdmzxzdzxtIK+6NiY6arymAZavp
xy0Sy6scTHAHoT0KMM0VjU/43dSMUBUc71DuxC73/OlS8pF94G3VNTCOXkNz8kHp
1Wrjsok6Vjk4bwY8iGlbKk3Fp1S4bInMm/k8yuX9ifUSPJJ4ltbcdG6TRGHRjcdG
snUOhugZitVtbNV4FpWi6cgKOOvyJBNPc1STE4U6G7weNLWLBYy5d4ux2x8gkasJ
U26Qzns3dLlwR5EiUWMWea6xrkEmCMgZK9FGqkjWZCrXgzT/LCrBbBlDSgeF59N8
9iFo7+ryUp9/k5DPAgMBAAGjQjBAMA4GA1UdDwEB/wQEAwIBBjAPBgNVHRMBAf8E
BTADAQH/MB0GA1UdDgQWBBRge2YaRQ2XyolQL30EzTSo//z9SzANBgkqhkiG9w0B
AQUFAAOCAQEA1nPnfE920I2/7LqivjTFKDK1fPxsnCwrvQmeU79rXqoRSLblCKOz
yj1hTdNGCbM+w6DjY1Ub8rrvrTnhQ7k4o+YviiY776BQVvnGCv04zcQLcFGUl5gE
38NflNUVyRRBnMRddWQVDf9VMOyGj/8N7yy5Y0b2qvzfvGn9LhJIZJrglfCm7ymP
AbEVtQwdpf5pLGkkeB6zpxxxYu7KyJesF12KwvhHhm4qxFYxldBniYUr+WymXUad
DKqC5JlR3XC321Y9YeRq4VzW9v493kHMB65jUr9TU/Qr6cf9tveCX4XSQRjbgbME
HMUfpIBvFSDJ3gyICh3WZlXi/EjJKSZp4A==
-----END CERTIFICATE-----
```

# 校验证书文件与密钥文件是否匹配
```
# 密钥文件 server.key.pem
# 证书文件 server.crt.pem
# 下面两行命令结果输出一致则为匹配的证书

openssl x509 -in server.crt.pem -pubkey -noout -outform pem |openssl md5
openssl pkey -in server.key.pem -pubout -outform pem |openssl md5


# 查看证书信息

openssl x509 -noout -text -in server.crt.pem

```

# 生成PFX格式证书
```
# 密钥文件 server.key.pem
# 证书文件 server.crt.pem
# PFX证书文件 server.pfx (默认密码为空)

openssl pkcs12 -export -passout pass: -in server.crt.pem -inkey server.key.pem -out server.pfx

```

# 检查 OCSP
```
# 中间证书 intermediate.pem
# 域名证书 server.crt.pem

openssl ocsp -text -no_nonce -url 'http://ocsp.globalsign.com/alphasslcasha256g4' -header 'HOST=ocsp.globalsign.com' -issuer intermediate.pem -cert server.crt.pem

```

## 注意事项
- 确认链接有效期 30 天
- 由于与邮箱链接的不确定性, 邮箱可能在1秒或1个星期内才会收到确认邮件.请耐心等待     
- 由于 `DNSPOD` 的解析是非标准实现,会**优化**一些基本特性.     
  如果您使用的是 `DNSPOD` 托管域名, 申请前请先暂停解析该级域名下的 CNAME, A, MX 记录. 然后使用内置API辅助签发.         
  建议更换至[**华为云DNS(无需实名,无需手机号)**](https://www.huaweicloud.com/intl/zh-cn/)或其他**标准实现**的DNS解析. (`DOSPOD`收费的功能`华为云DNS`几乎白给,TTL最短可设置为1)
