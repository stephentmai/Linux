# su 命令与 sudo 服务
各位读者在实验环境中很少遇到安全问题，并且为了避免因权限因素导致配置服务失败， 建议使用 root 管理员的身份来学习本书。但是，在生产环境中，我们必须对安全保持敬畏之心，不要用 root 管理员身份去做所有事情。一旦执行了错误命令，可能会直接导致系统崩溃。这样一来，不但客户指责，领导批评，没准奖金也会鸡飞蛋打。但转念一想，尽管 Linux 系统为了安全性考虑，使得许多系统命令和服务只能被 root 管理员使用，但这也让普通用户受到了更多的权限束缚，导致无法顺利完成特定的工作任务。

su 命令可以解决切换用户身份的需求，使得当前用户在不退出登录的情况下，顺畅地切换到其他用户，比如从 root 管理员切换至普通用户：
```shell
root@linuxprobe:/# su - linuxprobe
linuxprobe@linuxprobe:~$ id
uid=1000(linuxprobe) gid=1000(linuxprobe) groups=1000(linuxprobe),10(wheel) context=unconfined_u:unconfined_r:unconfined_t:s0-s0:c0.c1023
```
细心的读者一定会发现，上面的 su 命令与用户名之间有一个减号（-），这意味着完全切换到新的用户，即把环境变量信息也变更为新用户的相应信息，而不是保留原始的信息。强烈建议在切换用户身份时添加这个减号（-）。

另外，当从 root 管理员切换到普通用户时是不需要密码验证的，而从普通用户切换成 root 管理员就需要进行密码验证了，这也是一个必要的安全检查：
```shell
linuxprobe@linuxprobe:~$ su - root
Password: 此处输入管理员密码
```
尽管像上面这样使用 su 命令后，普通用户可以完全切换到 root 管理员的身份来完成相应工作，但这将暴露 root 管理员的密码，从而增大了系统密码被黑客获取的概率。这并不是最安全的方案。

接下来继续学习如何使用 sudo 命令把特定命令的执行权限赋予指定用户，这样既可保证普通用户能够完成特定的工作，也能避免泄露 root 管理员密码。我们要做的就是合理配置sudo 服务，以便兼顾系统的安全性和用户的便捷性。

Tips ：
授权原则：在保证普通用户完成相应工作的前提下，尽可能少地赋予额外的权限。

sudo 命令用于给普通用户提供额外的权限来执行指定的命令，语法格式为“sudo [参数] 命令”。

使用 sudo 命令能够给普通用户提供额外的权限来完成原本只有 root 管理员才能完成的任务，还能限制用户执行指定的命令，记录用户执行过的每一条命令，集中管理用户与权限（/etc/sudoers），以及允许在验证密码后的一段时间内不再需要用户再次验证密码。sudo命令的常用参数以及作用如表 5-10 所示。

**sudo 命令的常用参数以及作用**

| 参数  |                           作用                           |
| :---: | :------------------------------------------------------: |
|  -h   |                       列出帮助信息                       |
|  -l   |                 列出当前用户可执行的命令                 |
|  -u   |         用户名或 UID 值	以指定的用户身份执行命令         |
|  -k   | 清空密码的有效时间，下次执行 sudo 时需要再次进行密码验证 |
|  -b   |                   在后台执行指定的命令                   |
|  -p   |                   更改询问密码的提示语                   |

当然，如果担心直接修改配置文件会出现问题，推荐使用与 sudo 命令配套的 visudo 命令来配置用户权限。

visudo 命令用于编辑、配置用户 sudo 的权限文件，语法格式为“visudo [参数]”。该命令会自动调用 Vi 编辑器来配置/etc/sudoers 权限文件，能够解决多个用户同时

修改权限而导致的冲突问题。不仅如此，visudo 命令还可以对配置文件内的参数进行语法检查，并在发现参数错误时进行报错提醒。这要比用户直接修改文件更友好、安全、方便。

```shell
/etc/sudoers:1:7: syntax error
aaabbbcccddd
      ^
What now? 
```
使用visudo 命令配置权限文件时，其操作方法与 Vim 编辑器中用到的方法完全一致， 因此在编写完成后记得在末行模式下保存并退出。在配置权限文件时，按照下面的格式在第101 行（大约）填写上指定的信息。

谁可以使用 允许使用的主机 = （以谁的身份） 可执行命令的列表

谁可以使用：稍后要为哪位用户进行命令授权。

允许使用的主机：填写 ALL 表示不限制来源的主机，也可填写如 192.168.10.0/24 这样的网段限制来源地址，使得只有从允许网段登录时才能使用 sudo 命令。

以谁的身份：填写 ALL 表示系统最高权限，也可以填写另外一位用户的名字。

可执行命令的列表：填写 ALL 表示不限制命令，也可填写如/usr/bin/cat 这样的文件名称来限制命令列表，多个命令文件之间用逗号（,）间隔。

在 Linux 系统中配置服务文件时，虽然没有硬性规定，但从经验来讲新增参数的位置不建议太靠上，以免我们新填写的参数在执行时失败，导致一些必要的服务功能没有成功加载。一般建议在配置文件中找一下相似的参数，然后在相邻位置进行新的修改，或者在文件的中下部位置进行添加修改。
```shell
root@linuxprobe:~# visudo
 99 ## Allow root to run any commands anywhere 
100 root            ALL=(ALL)       ALL
101 linuxprobe      ALL=(ALL)       ALL
```
在填写完毕后记得要先保存再退出，然后切换至指定的普通用户身份，此时就可以用sudo -l 命令查看所有可执行的命令了（在下面的命令中，验证的是普通用户的密码，而不是 root 管理员的密码，请读者不要搞混了）：
```shell
root@linuxprobe:~# su - linuxprobe
linuxprobe@linuxprobe:~$ sudo -l

We trust you have received the usual lecture from the local System
Administrator. It usually boils down to these three things:

    #1) Respect the privacy of others.
    #2) Think before you type.
    #3) With great power comes great responsibility.

For security reasons, the password you type will not be visible.

[sudo] password for linuxprobe: 
Matching Defaults entries for linuxprobe on linuxprobe:
    !visiblepw, always_set_home, match_group_by_gid, always_query_group_plugin,
    env_reset, env_keep="COLORS DISPLAY HOSTNAME HISTSIZE KDEDIR LS_COLORS",
    env_keep+="MAIL PS1 PS2 QTDIR USERNAME LANG LC_ADDRESS LC_CTYPE",
    env_keep+="LC_COLLATE LC_IDENTIFICATION LC_MEASUREMENT LC_MESSAGES",
    env_keep+="LC_MONETARY LC_NAME LC_NUMERIC LC_PAPER LC_TELEPHONE",
    env_keep+="LC_TIME LC_ALL LANGUAGE LINGUAS _XKB_CHARSET XAUTHORITY",
    secure_path=/sbin\:/bin\:/usr/sbin\:/usr/bin

User linuxprobe may run the following commands on linuxprobe:
    (ALL) ALL
    (ALL) ALL
```
接下来是见证奇迹的时刻！作为一名普通用户，是肯定不能看到 root 管理员的家目录 （/root）中的文件信息的，但是，只需要在想执行的命令前面加上sudo 命令就行了：
```shell
linuxprobe@linuxprobe:~$ ls /root
ls: cannot open directory '/root': Permission denied
linuxprobe@linuxprobe:~$ sudo ls /root
anaconda-ks.cfg  Documents  Music     Public	 Videos
Desktop			 Downloads  Pictures  Templates
```
效果立竿见影！但是考虑到生产环境中不允许某个普通用户拥有整个系统中所有命令 的最高执行权（这也不符合前文提到的权限赋予原则，即尽可能少地赋予权限），ALL 参数就有些不合适了。因此只能赋予普通用户具体的命令以满足工作需求，这也受到了必要的 权限约束。如果需要让某个用户只能以 root 管理员的身份执行指定的命令，切记一定要给出该命令的绝对路径，否则系统无法识别。这时，先使用 whereis 命令找出命令所对应的保存路径：
```shell
linuxprobe@linuxprobe:~$ exit
root@linuxprobe:~# whereis cat
cat: /usr/bin/cat /usr/share/man/man1/cat.1.gz
root@linuxprobe:~# whereis reboot
reboot: /usr/sbin/reboot /usr/share/man/man2/reboot.2.gz /usr/share/man/man8/reboot.8.gz
```
然后使用 visudo 命令继续编辑权限文件，将原先第 101 行新增的参数作如下修改，且多个命令之间用逗号（,）间隔。
```shell
root@linuxprobe:~# visudo
 99 ## Allow root to run any commands anywhere
100 root       ALL=(ALL)   ALL
101 linuxprobe ALL=(ALL)   /usr/bin/cat,/usr/sbin/reboot
```
在编辑好后依然是先保存再退出。再次切换到指定的普通用户，然后尝试正常查看某个系统文件的内容，此时系统提示没有权限（Permission denied）。这时再使用 sudo 命令就能顺利地查看文件内容了：
```shell
root@linuxprobe:~# su - linuxprobe
linuxprobe@linuxprobe:~$ cat /etc/shadow
cat: /etc/shadow: Permission denied
linuxprobe@linuxprobe:~$ sudo cat /etc/shadow
root:$y$j9T$pg8bgLsCm0JreeO8laDHb8qW$cNPfXxGES.qDqkCTrpJUgM3GKju1jD3T9qQ1HMcxvr/::0:99999:7:::
bin:*:19898:0:99999:7:::
daemon:*:19898:0:99999:7:::
adm:*:19898:0:99999:7:::
lp:*:19898:0:99999:7:::
sync:*:19898:0:99999:7:::
shutdown:*:19898:0:99999:7:::
………………省略部分输出信息………………
linuxprobe@linuxprobe:~$ exit
```
大家千万不要以为到这里就结束了，刘遄老师还有更压箱底的宝贝。不知大家是否发觉在每次执行 sudo 命令后都会要求验证一下密码。虽然这个密码就是当前登录用户的密码， 但是每次执行 sudo 命令时都要输入一次密码其实也挺麻烦的，这时可以添加 NOPASSWD 参数，使得用户下次再执行 sudo 命令时不用密码验证：
```shell
root@linuxprobe:~# visudo
 99 ## Allow root to run any commands anywhere
100 root       ALL=(ALL)   ALL
101 linuxprobe ALL=(ALL)   NOPASSWD:/usr/bin/cat,/usr/sbin/reboot
```
这样，当切换到普通用户后再执行命令时，就不用再频繁地验证密码了，我们在日常工作中也就痛快至极了。
```shell
root@linuxprobe:~# su - linuxprobe
linuxprobe@linuxprobe:~$ reboot
User root is logged in on tty2.
Please retry operation after closing inhibitors and logging out other users.
Alternatively, ignore inhibitors and users with 'systemctl reboot -i'.
linuxprobe@linuxprobe:~$ sudo reboot
```
请同学们仔细留意上面的用户身份变换，visudo 命令只有 root 管理员才可以执行，普通用户在使用时会提示权限不足。