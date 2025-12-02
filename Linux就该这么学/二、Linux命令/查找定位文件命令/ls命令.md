# ls命令
ls 命令用于显示目录中的文件信息，英文全称为 list，语法格式为“ls [参数] [文件名]”。当所处的工作目录不同时，看到的文件肯定也不同。使用ls 命令的-a 参数可以看到全

部文件（包括隐藏文件），使用-l 参数能够查看文件的属性、大小等详细信息。将这两个参数整合之后再执行 ls 命令，即可查看当前目录中的所有文件并输出这些文件的属性信息：
```shell
root@linuxprobe:~# ls -al
total 40
dr-xr-x---. 15 root root     4096 May 18 09:37 .
dr-xr-xr-x. 18 root root      235 Mar  8 20:02 ..
-rw-r--r--.  1 root root      769 Mar 10 20:04 1.txt
-rw-------.  1 root root     1019 Mar  8 20:06 anaconda-ks.cfg
-rw-------.  1 root root      652 May 18 09:32 .bash_history
-rw-r--r--.  1 root root       18 Jun 24  2024 .bash_logout
-rw-r--r--.  1 root root      141 Jun 24  2024 .bash_profile
-rw-r--r--.  1 root root      429 Jun 24  2024 .bashrc
drwx------.  8 root root      117 Mar 10 20:01 .cache
drwx------. 11 root root     4096 May 18 09:47 .config
………………省略部分输出信息………………
```
如果想要查看目录属性信息，则需要额外添加一个-d 参数。例如，可使用如下命令查看/etc 目录的权限与属性信息：
```shell
root@linuxprobe:~# ls -ld /etc
drwxr-xr-x. 134 root root 8192 May 18 09:33 /etc
```