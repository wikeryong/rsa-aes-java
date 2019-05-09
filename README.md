# Usage
### Client
```java
//客户端代码
String text = "hello server!";

//随机生成16位aes密钥
byte[] aesKey = SecureRandomUtil.getRandom(16).getBytes();

//使用aes密钥对数据进行加密
String encryptText = AESCipher.encrypt(aesKey, text);

//使用客户端私钥对aes密钥签名
String signature = RSACipher.sign(Config.CLIENT_PRIVATE_KEY, aesKey);

//使用服务端公钥加密aes密钥
byte[] encryptKey = RSACipher.encrypt(Config.SERVER_PUBLIC_KEY, aesKey);
```

### Server
```java
//服务端代码
//模拟接收到客户端的加密的aes密钥和签名和加密的密文
byte[] encryptKey = "bJT8chyoHaFNG+Cvrc11sPSYzO0WeiiO/UJTHp44On7sfTOxmfard7z0GHPqp3HDT1qFkILUUPQqF7NZSys2VwgYHFxWgisif/CPHWAJ2t7zWSTunOChd6OGqH+n4pbzIsJjYJLFgNYPCNA6IB3Qlcm7DYDjDCVbvttVjHxSyRU=".getBytes();
String signature = "Cbcrf/WrmXMovWyueuDwfQtCRIY9PUDBZOf9npnuCj3tBlzZe2OJdZzlvXC2b5C4EC90eRR/NUTGFu5/HZ0n3+p/PtQBWMjEuuIDCC7+xraCG7MlcQPgpHOxyuZJRlRz8nNND8j4C6t/GJ6p5U80nTz3QD4QXHh2m3CKeNmt/B4=";
String encryptText = "9VpY0ZHvCSbfapOn/obRvg==";

//使用服务端私钥对加密后的aes密钥解密
byte[] aesKey = RSACipher.decrypt(Config.SERVER_PRIVATE_KEY, encryptKey);

//使用客户端公钥验签
Boolean result = RSACipher.checkSign(Config.CLIENT_PUBLIC_KEY, aesKey1, signature);

//使用aes私钥解密密文
String decryptText = AESCipher.decrypt(aesKey1, encryptText);
```

# Test secret key
### Client public key(PKCS#8)
```text
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDv/LnAoDkyewwjwqwgi9VSg51F
+tUJ8cGwL6Rqdf5ZXrRCHI1KLjOxdFbzB81YjS76cOzezQRz2vuYDo7OvLfYSjFI
fmukUxN+EliKkg0TwswylVroLBW9OKN70Zd62dc+gfkA3Vu8cDoRKzz6BKpo4yDo
0D3FOsbNEj80opjmtQIDAQAB
```
### Client private key(PKCS#1)
```text
MIICXAIBAAKBgQDv/LnAoDkyewwjwqwgi9VSg51F+tUJ8cGwL6Rqdf5ZXrRCHI1K
LjOxdFbzB81YjS76cOzezQRz2vuYDo7OvLfYSjFIfmukUxN+EliKkg0TwswylVro
LBW9OKN70Zd62dc+gfkA3Vu8cDoRKzz6BKpo4yDo0D3FOsbNEj80opjmtQIDAQAB
AoGADuZtDgWkp3q2TT4X+8lSzFW5nQ+uzHhDI1JB7g43ZYsYvAYTy6hEs17ayyoP
2NCjOw9p1Yd7IEpXVqCIw1M6QsfGdshy1NStsGpDHQYBBd8XiT8cWUaT/nmq5dEs
i0wOITMZePLgI5/5pD4M6DIEJKskM+Rzlo47AiyRchL6pqECQQD+XAZNCl6R5wjI
DrqW4v6Vw8mhdaPnQhPexmhHa1f9D7sA32A2H2N8M3dUDOwuG+DJhPkjVaQtFvT8
mjDjSZTdAkEA8Yj4hncF/WnLTDSXmiWfpNwYwjfpjOj8e4/5rWHF1jWZMgl0l1AS
Otna2dIbXp64dqsInITJTIDSQpbxuhrvuQJBAN9Ee6toLLa5KzYf55zGR13Ca9wz
3NkDYVmsop/+E0/oXOdZK6SWTMcajeXTKgUXJ2r8M4vWgrOpcQXBeqQnVGkCQDYX
e7j5bOD80Wemm5EM/fy4wd61ENvazbiKXNske17msAFRtsewSfTeFzIS6Mg++Yax
9QLAhihY7T22ejo4kBkCQBdg2yKHQrmG+njGfLsdQG9MARFlnOfohoBFQTYdtrmf
5JRNfwtPiis2YaoM2gP7z2qaunYbibDV5SYmtdD8GK0=
```
### Client private key(PKCS#8)
```text
MIICdgIBADANBgkqhkiG9w0BAQEFAASCAmAwggJcAgEAAoGBAO/8ucCgOTJ7DCPC
rCCL1VKDnUX61QnxwbAvpGp1/lletEIcjUouM7F0VvMHzViNLvpw7N7NBHPa+5gO
js68t9hKMUh+a6RTE34SWIqSDRPCzDKVWugsFb04o3vRl3rZ1z6B+QDdW7xwOhEr
PPoEqmjjIOjQPcU6xs0SPzSimOa1AgMBAAECgYAO5m0OBaSnerZNPhf7yVLMVbmd
D67MeEMjUkHuDjdlixi8BhPLqESzXtrLKg/Y0KM7D2nVh3sgSldWoIjDUzpCx8Z2
yHLU1K2wakMdBgEF3xeJPxxZRpP+earl0SyLTA4hMxl48uAjn/mkPgzoMgQkqyQz
5HOWjjsCLJFyEvqmoQJBAP5cBk0KXpHnCMgOupbi/pXDyaF1o+dCE97GaEdrV/0P
uwDfYDYfY3wzd1QM7C4b4MmE+SNVpC0W9PyaMONJlN0CQQDxiPiGdwX9actMNJea
JZ+k3BjCN+mM6Px7j/mtYcXWNZkyCXSXUBI62drZ0htenrh2qwichMlMgNJClvG6
Gu+5AkEA30R7q2gstrkrNh/nnMZHXcJr3DPc2QNhWayin/4TT+hc51krpJZMxxqN
5dMqBRcnavwzi9aCs6lxBcF6pCdUaQJANhd7uPls4PzRZ6abkQz9/LjB3rUQ29rN
uIpc2yR7XuawAVG2x7BJ9N4XMhLoyD75hrH1AsCGKFjtPbZ6OjiQGQJAF2DbIodC
uYb6eMZ8ux1Ab0wBEWWc5+iGgEVBNh22uZ/klE1/C0+KKzZhqgzaA/vPapq6dhuJ
sNXlJia10PwYrQ==
```
### Server public key(PKCS#8)
```text
MIGfMA0GCSqGSIb3DQEBAQUAA4GNADCBiQKBgQDxpMZQCTykdwUcUzyHgd6Q79de
4/F26bhIpOVCDpWNxlLQFdbGneTTQ1AJz/wwfNMgEPMnJvV3ZLrbqH9uV5W+8NG0
UDaXyZYo8fXhfD7Aeret6/CgH1iZamzR4DfADCvT+V81cjeGIhJ1JSYfxGIsC4mM
35TZ5p530ayOJ1KPHwIDAQAB
```
### Server private key(PKCS#1)
```text
MIICXQIBAAKBgQDxpMZQCTykdwUcUzyHgd6Q79de4/F26bhIpOVCDpWNxlLQFdbG
neTTQ1AJz/wwfNMgEPMnJvV3ZLrbqH9uV5W+8NG0UDaXyZYo8fXhfD7Aeret6/Cg
H1iZamzR4DfADCvT+V81cjeGIhJ1JSYfxGIsC4mM35TZ5p530ayOJ1KPHwIDAQAB
AoGBAL8E8ZvNYXnleE3G4t9/41ARuOATMws8gOg0KeMJImI7t7U0vl6t7HixCnFn
T8WIt2Du5Tg7DOo/35LK5Ul1xTHtYmQBdxTbg1WT89s3RWEvL4epHZQxzCQFJ1Pz
zjDFifPNDEA7ZME7sx/E2qPDinAD+JHNELhtNMDq5rhPuYGRAkEA/W6mZCbDfrY9
/qcbOe2s8ugeoDqoJueP7owfrut11KzvUn+e2rIBeDn1BZbWlYY5VDEq1nIKen+B
0su4QUiiaQJBAPQXjC+YgyBzoEyMLPiA+eRC28zuwP2LXGJ1FNPZpCgnaJzF215H
vhISwKX+b1/WZq3qBHyQnbE5zBHEIn5I5EcCQQC6X9k15dv3H4bP84xuOX/q0xFS
vFBU7A5JW/sg5EAvO0502S21nxq9k8HBboA4ThFy/QWH1y4lkAelQfQq7oOhAkBh
Jk4hU249KEgQr2nmrk7HTuT0t8IQJ7tpZHgZqXHwmV7FpuocqCk6QER0zMO/PTI4
3f9TJKvescZK++lOoexZAkB0XNnpYA4fOMKhyKbcGvpqKLFb3e2ks5LbjgeAkETt
dKipEh1RbiPrJeOwOChsx/51/cnVJrabE50AJV8AXM3e
```
### Server private key(PKCS#8)
```text
MIICdwIBADANBgkqhkiG9w0BAQEFAASCAmEwggJdAgEAAoGBAPGkxlAJPKR3BRxT
PIeB3pDv117j8XbpuEik5UIOlY3GUtAV1sad5NNDUAnP/DB80yAQ8ycm9Xdkutuo
f25Xlb7w0bRQNpfJlijx9eF8PsB6t63r8KAfWJlqbNHgN8AMK9P5XzVyN4YiEnUl
Jh/EYiwLiYzflNnmnnfRrI4nUo8fAgMBAAECgYEAvwTxm81heeV4Tcbi33/jUBG4
4BMzCzyA6DQp4wkiYju3tTS+Xq3seLEKcWdPxYi3YO7lODsM6j/fksrlSXXFMe1i
ZAF3FNuDVZPz2zdFYS8vh6kdlDHMJAUnU/POMMWJ880MQDtkwTuzH8Tao8OKcAP4
kc0QuG00wOrmuE+5gZECQQD9bqZkJsN+tj3+pxs57azy6B6gOqgm54/ujB+u63XU
rO9Sf57asgF4OfUFltaVhjlUMSrWcgp6f4HSy7hBSKJpAkEA9BeML5iDIHOgTIws
+ID55ELbzO7A/YtcYnUU09mkKCdonMXbXke+EhLApf5vX9ZmreoEfJCdsTnMEcQi
fkjkRwJBALpf2TXl2/cfhs/zjG45f+rTEVK8UFTsDklb+yDkQC87TnTZLbWfGr2T
wcFugDhOEXL9BYfXLiWQB6VB9Crug6ECQGEmTiFTbj0oSBCvaeauTsdO5PS3whAn
u2lkeBmpcfCZXsWm6hyoKTpARHTMw789Mjjd/1Mkq96xxkr76U6h7FkCQHRc2elg
Dh84wqHIptwa+moosVvd7aSzktuOB4CQRO10qKkSHVFuI+sl47A4KGzH/nX9ydUm
tpsTnQAlXwBczd4=
```