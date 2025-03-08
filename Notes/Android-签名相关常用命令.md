# Android-签名相关常用命令

- 将jks转换成pck12类型的签名文件

```bash
keytool -importkeystore -srckeystore /xxx/xxx.jks -destkeystore /xxx/xxx.jks -deststoretype pkcs12
```