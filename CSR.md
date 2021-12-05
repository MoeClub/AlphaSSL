# Get CSR With OpenSSL
### ECC (`prime256v1`, `secp384r1`)
```
openssl ecparam -name prime256v1 -genkey -out server.key.pem
openssl req -new -key server.key.pem -out server.csr

```

### RSA
```
openssl req -new -newkey rsa:2048 -nodes -keyout server.key.pem -out server.csr

```
