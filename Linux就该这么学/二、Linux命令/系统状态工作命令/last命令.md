# last
last 命令用于查看主机的历史登录记录，输入该命令后按回车键执行即可。

Linux 系统会将每次的登录信息都记录到日志文件中，如果哪天想翻阅了，直接执行这条命令就行：
```shell
root@linuxprobe:~# last
root     tty2         tty2             Sun May 18 09:36   still logged in
root     seat0        login screen     Sun May 18 09:36   still logged in
reboot   system boot  6.11.0-0.rc5.23. Sun May 18 09:32   still running
root     tty2         tty2             Sun May 18 09:31 - down   (00:01)
root     seat0        login screen     Sun May 18 09:31 - down   (00:01)

wtmp begins Sun Mar  9 02:33:32 2025
```