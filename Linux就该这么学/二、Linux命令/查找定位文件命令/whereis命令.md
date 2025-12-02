# whereis命令
whereis 命令用于按照名称快速搜索二进制程序（命令）、源代码以及帮助文件所对应的位置，语法格式为“whereis 命令名称”。

简单来说，whereis 命令也是基于 updatedb 命令生成的索引库文件进行搜索，它与locate 命令的区别是不关心那些相同名称的文件，仅仅是快速找到对应的命令文件及其帮助文件所在的位置。

下面使用 whereis 命令分别查找出 ls 和pwd 命令所在的位置：
```shell
root@linuxprobe:~# whereis ls
ls: /usr/bin/ls /usr/share/man/man1/ls.1.gz
root@linuxprobe:~# whereis pwd
pwd: /usr/bin/pwd /usr/share/man/man1/pwd.1.gz
```