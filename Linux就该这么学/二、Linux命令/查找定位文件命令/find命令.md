# find命令
find 命令用于按照指定条件查找文件及目录，语法格式为“find [查找范围] 寻找条件”。

本书中曾多次提到“Linux 系统中的一切都是文件”，接下来就要见证这句话的分量了。在 Linux 系统中，搜索工作一般都是通过 find 命令完成的，它可以使用不同的文件特性作为寻找条件（如文件名、大小、修改时间、权限等信息），一旦匹配成功则默认将信息显示到屏幕上。find 命令的参数以及作用如表 2-14 所示。
**find 命令的参数以及作用**
|       参数        |                                        作用                                         |
| :---------------: | :---------------------------------------------------------------------------------: |
|       -name       |                                      匹配名称                                       |
|       -perm       |                   匹配权限（+mode 为完全匹配，-mode 为包含即可）                    |
|       -user       |                                     匹配所有者                                      |
|      -group       |                                     匹配所属组                                      |
|   -mtime -n +n    |                匹配修改内容的时间（-n 指 n 天以内，+n 指 n 天以前）                 |
|   -atime -n +n    |                匹配访问文件的时间（-n 指 n 天以内，+n 指 n 天以前）                 |
|   -ctime -n +n    |              匹配修改文件权限的时间（-n 指 n 天以内，+n 指 n 天以前）               |
|      -nouser      |                                 匹配无所有者的文件                                  |
|     -nogroup      |                                 匹配无所属组的文件                                  |
|   -newer f1 !f2   |                          匹配比文件 f1 新但比 f2 旧的文件                           |
| -type b/d/c/p/l/f | 匹配文件类型（后面的字母依次表示块设备、目录、字符设备、管道、链接 文件、普通文件） |
|       -size       |    匹配文件的大小（+50k 为查找超过 50KB 的文件，而-50k 为查找小于 50KB 的文件）     |
|      -prune       |                                    忽略某个目录                                     |
|    -exec …… ;     |                后面可跟用于进一步处理搜索结果的命令（下文会有演示）                 |

这里需要重点讲解-exec 参数的重要作用。这个参数用于把 find 命令搜索到的结果交由紧随其后的命令作进一步处理。它十分类似于第 3 章将要讲解的管道符技术，并且由于 find 命令对参数有特殊要求，因此虽然 exec 是长格式形式，但它的前面依然只需要一个减号（-）。
根据文件系统层次标准（Filesystem Hierarchy Standard），Linux 系统中的配置文件会保存到/etc 目录中（详见第 6 章）。如果要想获取该目录中所有以 host 开头的文件列表，可以执行如下命令：
```shell
root@linuxprobe:~# find /etc -name "host*"
/etc/nvme/hostnqn
/etc/nvme/hostid
/etc/host.conf
/etc/hosts
/etc/avahi/hosts
/etc/hostname
```
如果要在整个系统内搜索权限中包含 SUID 权限的所有文件（详见第 5 章），只需使用-4000 即可：
```shell
root@linuxprobe:~# find / -perm -4000
/usr/bin/umount
/usr/bin/chage
/usr/bin/gpasswd
/usr/bin/newgrp
/usr/bin/passwd
/usr/bin/mount
/usr/bin/fusermount3
………………省略部分输出信息………………
```
Tips ：
进阶实验

在整个文件系统中找出所有归属于 linuxprobe 用户的文件并复制到/root/findresults 目录中。

该实验的重点是“-exec ;”参数，其中的表示 find 命令搜索出的每一个文件，并且命令的结尾必须是“;”。完成该实验的具体命令如下：
```shell
root@linuxprobe:~# mkdir /root/findresults
root@linuxprobe:~# find / -user linuxprobe -exec cp -a {} /root/findresults \;
```