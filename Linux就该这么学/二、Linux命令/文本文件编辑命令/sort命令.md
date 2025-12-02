# sort命令

sort 命令用于对文本内容进行排序，语法格式为“sort [参数] 文件名”。

有时文本中的内容顺序不正确，一行行地手动修改实在太麻烦。此时使用 sort 命令就再合适不过了，它能够对文本内容进行排序。这个命令千万不能只讲理论，一定要借助实战让大家一看就懂。sort 命令的参数及其作用如表 2-17 所示。

**sort 命令的参数及其作用**
| 参数  |      作用      |
| :---: | :------------: |
|  -f   |   忽略大小写   |
|  -b   | 忽略缩进与空格 |
|  -n   |  以数值型排序  |
|  -r   |    反向排序    |
|  -u   |   去除重复行   |
|  -t   |   指定间隔符   |
|  -k   |  设置字段范围  |

首先，在执行 sort 命令后默认会按照字母顺序进行排序，非常方便：
```shell
root@linuxprobe:~# cat fruit.txt 
banana
pear
apple
orange
raspberry
root@linuxprobe:~# sort fruit.txt 
apple
banana
orange
pear
raspberry
```
此外，与 uniq 命令不同，sort 命令是无论内容行之间是否夹杂有其他内容，只要有两个完全一样的内容行，立马就可以使用-u 参数进行去重操作：
```shell
root@linuxprobe:~# cat sort.txt 
Welcome to linuxprobe.com
Red Hat certified
Welcome to linuxprobe.com
Free Linux Lessons
Linux Course
root@linuxprobe:~# sort -u sort.txt 
Free Linux Lessons
Linux Course
Red Hat certified
Welcome to linuxprobe.com
```
想对数字进行排序？一点问题都没有，而且完全不用担心出现“1 大于 20”这种问题（因为有些命令只比较数字的第一位，忽略了十、百、千的位）：
```shell
root@linuxprobe:~# cat number.txt 
45
12
3
98
82
67
24
56
9
root@linuxprobe:~# sort -n number.txt 
3
9
12
24
45
56
67
82
98
```
最后，我们挑战一个“高难度”的小实验。下面的内容节选自/etc/passwd 文件中的前 5 个字段，并且进行了混乱排序。
```shell
root@linuxprobe:~# cat user.txt 
tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon
polkitd:x:998:996:User for polkitd
geoclue:x:997:995:User for geoclue
rtkit:x:172:172:RealtimeKit
pulse:x:171:171:PulseAudio System Daemon
qemu:x:107:107:qemu user
usbmuxd:x:113:113:usbmuxd user
unbound:x:996:991:Unbound DNS resolver
rpc:x:32:32:Rpcbind Daemon
gluster:x:995:990:GlusterFS daemons
```
不难看出，上面其实是 5 个字段，各个字段之间使用冒号进行间隔，如果想以第 3 个字段中的数字作为排序依据，那么可以用-t 参数指定间隔符，用-k 参数指定第几列，用-n 参数进行数字排序来搞定：
```shell
root@linuxprobe:~# sort -t : -k 3 -n user.txt 
rpc:x:32:32:Rpcbind Daemon
tss:x:59:59:Account used by the trousers package to sandbox the tcsd daemon
qemu:x:107:107:qemu user
usbmuxd:x:113:113:usbmuxd user
pulse:x:171:171:PulseAudio System Daemon
rtkit:x:172:172:RealtimeKit
gluster:x:995:990:GlusterFS daemons
unbound:x:996:991:Unbound DNS resolver
geoclue:x:997:995:User for geoclue
polkitd:x:998:996:User for polkitd
```
