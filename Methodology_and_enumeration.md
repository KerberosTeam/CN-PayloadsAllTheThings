# 寻找漏洞的方法和列表

## 枚举所有子域名 (只针对*.domain.ext有效)

* 使用子域名爆破
```bash
git clone https://github.com/TheRook/subbrute
python subbrute.py domain.example.com
```

* 使用带有Daniel Miessler的SecLists（"/Discover/DNS"子域名）的KnockPy 
```bash
git clone https://github.com/guelfoweb/knock
git clone https://github.com/danielmiessler/SecLists.git
knockpy domain.com -w subdomains-top1mil-110000.txt
```

* 使用Google高级搜索功能
```bash
site:*.domain.com -www
site:http://domain.com filetype:pdf
site:http://domain.com inurl:&
site:http://domain.com inurl:login,register,upload,logout,redirect,redir,goto,admin
site:http://domain.com ext:php,asp,aspx,jsp,jspa,txt,swf
```

* 使用HostileSubBruteForcer
```bash
git clone https://github.com/nahamsec/HostileSubBruteforcer
chmox +x sub_brute.rb
./sub_brute.rb
```

* 源自KnockPy和enumall scans的EyeWitness and Nmap扫描
```bash
git clone https://github.com/ChrisTruncer/EyeWitness.git
./setup/setup.sh
./EyeWitness.py -f filename -t optionaltimeout --open (Optional)
./EyeWitness -f urls.txt --web
./EyeWitness -x urls.xml -t 8 --headless
./EyeWitness -f rdp.txt --rdp
```

## 被动侦查
* 使用Shodan (https://www.shodan.io/) 来识别类似的app

* 使用Wayback Machine (https://archive.org/web/) 来检测被遗忘的节点,
  ```
  look for JS files, old links
  ```

* 使用Harvester (https://github.com/laramies/theHarvester)
  ```
  python theHarvester.py -b all -d domain.com
  ```


## 主动侦查
* NMAP基础用法
  ```bash
  sudo nmap -sSV -p- 192.168.0.1 -oA OUTPUTFILE -T4
  sudo nmap -sSV -oA OUTPUTFILE -T4 -iL INPUTFILE.csv

  • the flag -sSV defines the type of packet to send to the server and tells Nmap to try and determine any service on open ports
  • the -p- tells Nmap to check all 65,535 ports (by default it will only check the most popular 1,000)
  • 192.168.0.1 is the IP address to scan
  • -oA OUTPUTFILE tells Nmap to output the findings in its three major formats at once using the filename "OUTPUTFILE"
  • -iL INPUTFILE tells Nmap to use the provided file as inputs
  ```

* 入侵式NMAP
  ```bash
  nmap -A -T4 scanme.nmap.org
  • -A: Enable OS detection, version detection, script scanning, and traceroute
  • -T4: Defines the timing for the task (options are 0-5 and higher is faster)
  ```

* NMAP和扩展
  1. 使用searchsploit 来检测漏洞服务
  ```bash
  nmap -p- -sV -oX a.xml IP_ADDRESS; searchsploit --nmap a.xml
  ```
  2. 生成有用的扫描报告
  ```bash
  nmap -sV IP_ADDRESS -oX scan.xml && xsltproc scan.xml -o "`date +%m%d%y`_report.html"
  ```


* NMAP脚本
  ```bash
  nmap -sC : equivalent to --script=default

  nmap --script 'http-enum' -v web.xxxx.com -p80 -oN http-enum.nmap
  PORT   STATE SERVICE
  80/tcp open  http
  | http-enum:
  |   /phpmyadmin/: phpMyAdmin
  |   /.git/HEAD: Git folder
  |   /css/: Potentially interesting directory w/ listing on 'apache/2.4.10 (debian)'
  |_  /image/: Potentially interesting directory w/ listing on 'apache/2.4.10 (debian)'

  nmap --script smb-enum-users.nse -p 445 [target host]
  Host script results:
  | smb-enum-users:
  |   METASPLOITABLE\backup (RID: 1068)
  |     Full name:   backup
  |     Flags:       Account disabled, Normal user account
  |   METASPLOITABLE\bin (RID: 1004)
  |     Full name:   bin
  |     Flags:       Account disabled, Normal user account
  |   METASPLOITABLE\msfadmin (RID: 3000)
  |     Full name:   msfadmin,,,
  |     Flags:       Normal user account

  List Nmap scripts : ls /usr/share/nmap/scripts/
  ```

* RPCClient
  ```bash
  ╰─$ rpcclient -U "" [target host]
  rpcclient $> querydominfo
  Domain:		WORKGROUP
  Server:		METASPLOITABLE
  Comment:	metasploitable server (Samba 3.0.20-Debian)
  Total Users:	35

  rpcclient $> enumdomusers
  user:[games] rid:[0x3f2]
  user:[nobody] rid:[0x1f5]
  user:[bind] rid:[0x4ba]
  ```
* Enum4all
  ```
  Usage: ./enum4linux.pl [options]ip
  -U        get userlist
  -M        get machine list*
  -S        get sharelist
  -P        get password policy information
  -G        get group and member list
  -d        be detailed, applies to -U and -S
  -u user   specify username to use (default “”)
  -p pass   specify password to use (default “”
  -a        Do all simple enumeration (-U -S -G -P -r -o -n -i).
  -o        Get OS information
  -i        Get printer information
  ==============================
  |   Users on XXX.XXX.XXX.XXX |
  ==============================
  index: 0x1 Account: games	Name: games	Desc: (null)
  index: 0x2 Account: nobody	Name: nobody	Desc: (null)
  index: 0x3 Account: bind	Name: (null)	Desc: (null)
  index: 0x4 Account: proxy	Name: proxy	Desc: (null)
  index: 0x5 Account: syslog	Name: (null)	Desc: (null)
  index: 0x6 Account: user	Name: just a user,111,,	Desc: (null)
  index: 0x7 Account: www-data	Name: www-data	Desc: (null)
  index: 0x8 Account: root	Name: root	Desc: (null)

  ```  

## 列出所有的子目录和文件

* 使用备份历史文件检测器(Backup File Artifacts Checker): 一个检查（可能暴露站点源码）备份历史文件的自动化扫描器 
```bash
git clone https://github.com/mazen160/bfac

Check a single URL
bfac --url http://example.com/test.php --level 4

Check a list of URLs
bfac --list testing_list.txt
```

* 使用 DirBuster 或者 GoBuster
```bash
./gobuster -u http://buffered.io/ -w words.txt -t 10
-u url
-w wordlist
-t threads

More subdomain :
./gobuster -m dns -w subdomains.txt -u google.com -i

gobuster -w wordlist -u URL -r -e
```

*使用 Sublist3r
```bash
To enumerate subdomains of specific domain and show the results in realtime:
python sublist3r.py -v -d example.com

To enumerate subdomains and enable the bruteforce module:
python sublist3r.py -b -d example.com

To enumerate subdomains and use specific engines such Google, Yahoo and Virustotal engines
python sublist3r.py -e google,yahoo,virustotal -d example.com

python sublist3r.py -b -d example.com
```

* 使用脚本来检测所有在一个ip段的phpinfo.php文件
```bash
#!/bin/bash
for ipa in 98.13{6..9}.{0..255}.{0..255}; do
wget -t 1 -T 3 http://${ipa}/phpinfo.php; done &
```

* 使用脚本来检测所有在一个ip段的.htpasswd文件
```bash
#!/bin/bash
for ipa in 98.13{6..9}.{0..255}.{0..255}; do
wget -t 1 -T 3 http://${ipa}/.htpasswd; done &
```

## 寻找Web漏洞

* 使用GitRob寻找在GitHub repos中的私人信息
```
git clone https://github.com/michenriksen/gitrob.git
gitrob analyze johndoe --site=https://github.acme.com --endpoint=https://github.acme.com/api/v3 --access-tokens=token1,token2
```

* 使用代理 (ZAP/Burp Suite)探索站点
 1. 开启代理, 访问目标站点并进行强制浏览来发现文件和目录
 2. Wappalyzer and Burp Suite (或者 ZAP)代理采用的Map技术
 3. 浏览并理解可获取的功能, 着重注意与漏洞类型相关的区域。
```bash
Burp Proxy configuration on port 8080 (in .bashrc):
alias set_proxy_burp='gsettings set org.gnome.system.proxy.http host "http://localhost";gsettings set org.gnome.system.proxy.http port 8080;gsettings set org.gnome.system.proxy mode "manual"'
alias set_proxy_normal='gsettings set org.gnome.system.proxy mode "none"'

then launch Burp with : java -jar burpsuite_free_v*.jar &
```

* Web漏洞的检查列表
```
[] AWS Amazon Bucket S3  
[] Git Svn insecure files   
[] CVE Shellshock Heartbleed  
[] Open redirect            
[] Traversal directory    
[] XSS injection
[] CRLF injection  
[] CSRF injection          
[] SQL injection            
[] NoSQL injection                 
[] PHP include      
[] Upload insecure files     
[] SSRF injection         
[] XXE injections
[] CSV injection
[] PHP serialization
...   
```

* 订阅这个站点并支付额外测试功能

* 启动Nikto扫描防止你错过了一些东西
```
nikto -h http://domain.example.com
```

## 感谢
* http://blog.it-securityguard.com/bugbounty-yahoo-phpinfo-php-disclosure-2/
