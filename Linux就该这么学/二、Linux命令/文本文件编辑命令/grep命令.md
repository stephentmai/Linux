# grep 
grep 命令用于根据指定的模式（如字符串、正则表达式等）来搜索和提取对应的文本内容，英文全称为 Global Regular Expression Print，语法格式为“grep [参数] 模式 文件名”。

grep 命令是用途最广泛的文本搜索匹配工具。它虽然有很多参数，但是大多数基本上都用不到。有鉴于此，经过深思熟虑的提炼，这里只讲 grep 命令两个最常用的参数。

-n 参数：用来显示搜索到的信息的行号。

-v 参数：用于反选信息（即没有包含关键词的所有信息行）。

这两个参数几乎能完成你日后 80%的工作需要，至于其他上百个参数，即使以后在工作期间遇到了，再使用 man grep 命令查询也来得及。

grep 命令中的常见参数以及作用如表 2-16 所示。

**grep 命令中的常见参数以及作用**
| 参数  |                       作用                       |
| :---: | :----------------------------------------------: |
|  -b   | 将可执行文件（binary）当作文本文件（text）来搜索 |
|  -c   |                 仅显示找到的行数                 |
|  -i   |                    忽略大小写                    |
|  -n   |                     显示行号                     |
|  -v   |         反向选择—仅列出没有“关键词”的行          |
在 Linux 系统中，/etc/passwd 文件保存着所有的用户信息，而一旦用户的登录终端被设置为/sbin/nologin，则不再允许登录系统，因此可以使用 grep 命令查找出当前系统中不允许登录系统的所有用户的信息：
```shell
root@linuxprobe:~# grep /sbin/nologin /etc/passwd
bin:x:1:1:bin:/bin:/usr/sbin/nologin
daemon:x:2:2:daemon:/sbin:/usr/sbin/nologin
adm:x:3:4:adm:/var/adm:/usr/sbin/nologin
lp:x:4:7:lp:/var/spool/lpd:/usr/sbin/nologin
mail:x:8:12:mail:/var/spool/mail:/usr/sbin/nologin
operator:x:11:0:operator:/root:/usr/sbin/nologin
games:x:12:100:games:/usr/games:/usr/sbin/nologin
ftp:x:14:50:FTP User:/var/ftp:/usr/sbin/nologin
………………省略部分输出信息………………
```