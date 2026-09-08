> 🌐 本文档由 [yt-dlp/yt-dlp](https://github.com/yt-dlp/yt-dlp) 翻译,英文原版见原项目。

# 生成客户端证书测试所需证书

## CA(证书颁发机构)
```sh
openssl ecparam -name prime256v1 -genkey -noout -out ca.key
openssl req -new -x509 -sha256 -days 6027 -key ca.key -out ca.crt -subj "/CN=ytdlptest"
```

## 客户端
```sh
openssl ecparam -name prime256v1 -genkey -noout -out client.key
openssl ec -in client.key -out clientencrypted.key -passout pass:foobar -aes256
openssl req -new -sha256 -key client.key -out client.csr -subj "/CN=ytdlptest2"
openssl x509 -req -in client.csr -CA ca.crt -CAkey ca.key -CAcreateserial -out client.crt -days 6027 -sha256
cp client.crt clientwithkey.crt
cp client.crt clientwithencryptedkey.crt
cat client.key >> clientwithkey.crt
cat clientencrypted.key >> clientwithencryptedkey.crt
```
