# cut命令

cut 命令用于按“列”提取文本内容，语法格式为“cut [参数] 文件名”。

系统文件在保存用户数据信息时，每一项值之间是采用冒号来间隔的，先查看一下：
```shell
root@linuxprobe:~# head -n 2 /etc/passwd 
root:x:0:0:Super User:/root:/bin/bash
bin:x:1:1:bin:/bin:/usr/sbin/nologin
```
一般而言，按基于“行”的方式提取数据是比较简单的，只需要设置好要搜索的关键词即可。但是如果按“列”搜索，不仅要使用-f 参数设置需要查看的列数，还需要使用-d 参数设置间隔符号。

接下来使用下述命令尝试提取 passwd 文件中的用户名信息，即提取以冒号（：）为间隔符号的第一列内容：
```shell
root@linuxprobe:~# cut -d : -f 1 /etc/passwd
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
operator
games
ftp
………………省略部分输出信息………………
```