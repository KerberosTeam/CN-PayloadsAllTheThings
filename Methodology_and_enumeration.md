# 漏洞搜寻方式和枚举

## 列举所有的子域 (只有当范围是 *.domain.ext)

* 使用子域名爆破工具
```bash
git clone https://github.com/TheRook/subbrute
python subbrute.py domain.example.com
```

* Using KnockPy with Daniel Miessler’s SecLists for subdomain "/Discover/DNS"
```bash
git clone https://github.com/guelfoweb/knock
git clone https://github.com/danielmiessler/SecLists.git
knockpy domain.com -w subdomains-top1mil-110000.txt
```

* Using Google Dorks
```bash
site:*.domain.com -www
site:http://domain.com filetype:pdf
site:http://domain.com inurl:&
site:http://domain.com inurl:login,register,upload,logout,redirect,redir,goto,admin
site:http://domain.com ext:php,asp,aspx,jsp,jspa,txt,swf
```

* Subdomain take over using HostileSubBruteForcer
```bash
git clone https://github.com/nahamsec/HostileSubBruteforcer
chmox +x sub_brute.rb
./sub_brute.rb
```

* EyeWitness and Nmap scans from the KnockPy and enumall scans
```bash
git clone https://github.com/ChrisTruncer/EyeWitness.git
./setup/setup.sh
./EyeWitness.py -f filename -t optionaltimeout --open (Optional)
./EyeWitness -f urls.txt --web
./EyeWitness -x urls.xml -t 8 --headless
./EyeWitness -f rdp.txt --rdp
```

## 被动侦察
* 使用 Shodan (https://www.shodan.io/) 来探测相似的应用程序

* 使用 The Wayback Machine (https://archive.org/web/) 来探测被遗忘的端点,
  ```
  look for JS files, old links
  ```

* 使用 The Harvester (https://github.com/laramies/theHarvester)
  ```
  python theHarvester.py -b all -d domain.com
  ```


## 主动侦察
* Basic NMAP
  ```bash
  sudo nmap -sSV -p- 192.168.0.1 -oA OUTPUTFILE -T4
  sudo nmap -sSV -oA OUTPUTFILE -T4 -iL INPUTFILE.csv

  • the flag -sSV defines the type of packet to send to the server and tells Nmap to try and determine any service on open ports
  • the -p- tells Nmap to check all 65,535 ports (by default it will only check the most popular 1,000)
  • 192.168.0.1 is the IP address to scan
  • -oA OUTPUTFILE tells Nmap to output the findings in its three major formats at once using the filename "OUTPUTFILE"
  • -iL INPUTFILE tells Nmap to use the provided file as inputs
  ```

* Aggressive NMAP
  ```bash
  nmap -A -T4 scanme.nmap.org
  • -A: Enable OS detection, version detection, script scanning, and traceroute
  • -T4: Defines the timing for the task (options are 0-5 and higher is faster)
  ```

* NMAP and add-ons
  1. 使用 searchsploit 来探测易受攻击的服务器
  ```bash
  nmap -p- -sV -oX a.xml IP_ADDRESS; searchsploit --nmap a.xml
  ```
  2. 合成精密的扫描报告
  ```bash
  nmap -sV IP_ADDRESS -oX scan.xml && xsltproc scan.xml -o "`date +%m%d%y`_report.html"
  ```


* NMAP Scripts
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

## 列举出所有的子目录和文档

* 使用 BFAC (Backup File Artifacts Checker): 一个自动化的可以检测是否有能够揭露网页应用程序源代码备份工件的工具 .
```bash
git clone https://github.com/mazen160/bfac

Check a single URL
bfac --url http://example.com/test.php --level 4

Check a list of URLs
bfac --list testing_list.txt
```

* Using DirBuster or GoBuster
```bash
./gobuster -u http://buffered.io/ -w words.txt -t 10
-u url
-w wordlist
-t threads

More subdomain :
./gobuster -m dns -w subdomains.txt -u google.com -i

gobuster -w wordlist -u URL -r -e
```

* Using Sublist3r
```bash
To enumerate subdomains of specific domain and show the results in realtime:
python sublist3r.py -v -d example.com

To enumerate subdomains and enable the bruteforce module:
python sublist3r.py -b -d example.com

To enumerate subdomains and use specific engines such Google, Yahoo and Virustotal engines
python sublist3r.py -e google,yahoo,virustotal -d example.com

python sublist3r.py -b -d example.com
```

* 使用一个脚本在IPs (CIDR can be found with a whois)系列里探测 phpinfo.php 文档
```bash
#!/bin/bash
for ipa in 98.13{6..9}.{0..255}.{0..255}; do
wget -t 1 -T 3 http://${ipa}/phpinfo.php; done &
```

* 使用一个脚本在一系列IPs里探测所有的 .htpasswd 文档
```bash
#!/bin/bash
for ipa in 98.13{6..9}.{0..255}.{0..255}; do
wget -t 1 -T 3 http://${ipa}/.htpasswd; done &
```

## 寻找网页的弱点

* 用GitRob在GitHub repos寻找私人信息
```
git clone https://github.com/michenriksen/gitrob.git
gitrob analyze johndoe --site=https://github.acme.com --endpoint=https://github.acme.com/api/v3 --access-tokens=token1,token2
```

* 用一个代理 (ZAP/Burp Suite)来浏览网站
 1. 启动代理，访问目标站点并且强制打开浏览器来发现文档和目录
 2. 映射出使用 Wappalyzer and Burp Suite (or ZAP) 代理的技术
 3.探索和理解可用的功能，注解出符合漏洞类型的范围
```bash
Burp Proxy configuration on port 8080 (in .bashrc):
alias set_proxy_burp='gsettings set org.gnome.system.proxy.http host "http://localhost";gsettings set org.gnome.system.proxy.http port 8080;gsettings set org.gnome.system.proxy mode "manual"'
alias set_proxy_normal='gsettings set org.gnome.system.proxy mode "none"'

then launch Burp with : java -jar burpsuite_free_v*.jar &
```

* Checklist for Web vulns
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

* 订阅该网站并且支付额外的功能来测试 

* 发起 Nikto 扫描以防你遗漏了一些东西
```
nikto -h http://domain.example.com
```

## 致谢
* http://blog.it-securityguard.com/bugbounty-yahoo-phpinfo-php-disclosure-2/


