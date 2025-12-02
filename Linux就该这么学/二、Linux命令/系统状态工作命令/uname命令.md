# uname命令
uname 命令用于查看系统内核版本与系统架构等信息，英文全称为 UNIX Name，语法格式为“uname [参数]”。

在使用 uname 命令时，一般要固定搭配-a 参数来完整地查看当前系统的内核名称、主机名、内核发行版本、节点名、编译时间、硬件名称、硬件平台、处理器类型以及操作系统名称等信息：
```shell
root@linuxprobe:~# uname -a
Linux linuxprobe.com 6.12.0-55.9.1.el10….x86_64 #1 SMP PREEMPT_DYNAMIC Mon Sep 23 04:19:12 EDT 2025 x86_64 GNU/Linux
```

顺带一提，如果要查看当前系统版本的详细信息，则需要查看 redhat-release 文件， 其命令以及相应的结果如下：
```shell
root@linuxprobe:~# cat /etc/redhat-release 
Red Hat Enterprise Linux release 10.0
```