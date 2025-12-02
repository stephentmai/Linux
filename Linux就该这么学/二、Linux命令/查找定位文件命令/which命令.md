# which命令
which 命令用于按照指定名称快速搜索二进制程序（命令）对应的位置，语法格式为“which 命令名称”。

which 命令是在 PATH 变量指定的路径中，按照指定条件搜索命令所在的路径。也就是说，如果我们既不关心同名文件（find 与 locate），也不关心命令所对应的源代码和帮助文件（whereis），仅仅是想找到命令本身所在的路径，那么这个 which 命令就太合适了。下面查找一下 locate 和whereis 命令所对应的路径：
```shell
root@linuxprobe:~# which locate
/usr/bin/locate
root@linuxprobe:~# which whereis
/usr/bin/whereis
```