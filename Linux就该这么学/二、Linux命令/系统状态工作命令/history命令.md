# history命令
history 命令用于显示执行过的命令历史，语法格式为“history [参数]”。

history 命令应该是运维人员最喜欢的命令之一。执行 history 命令能显示出当前用户在本机上执行过的最近 1000 条命令记录。如果觉得 1000 条不够用，可以自定义/etc/profile 文件中的 HISTSIZE 变量值。在使用 history 命令时，可以使用-c 参数清空所有的命令历史记录。还可以使用“!编码数字”的方式来重复执行某一次的命令。总之，history 命令有很多有趣的玩法等待大家去开发。
```shell
root@linuxprobe:~# history
1 ifconfig
2 uname -a
3 cat /etc/redhat-release
4 uptime
5 free -h
6 who
7 last
8 ping -c 192.168.10.10
9 ping -c 192.168.10.20
10 tracepath www.linuxprobe.com
11 netstat -a
12 netstat -i
13 history
root@linuxprobe:~# !3
cat /etc/redhat-release
Red Hat Enterprise Linux release 10
```
历史命令会被保存到用户家目录中的.bash_history 文件中。Linux 系统中以点（.） 开头的文件均代表隐藏文件，这些文件大多数为系统服务文件，可以用 cat 命令查看其文件内容：
```shell
root@linuxprobe:~# cat ~/.bash_history
```
要清空当前用户在本机上执行的 Linux 命令历史记录信息，可执行如下命令：
```shell
root@linuxprobe:~# history -c
```
