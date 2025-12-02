# timedatectl命令
timedatectl 命令用于设置系统的时间，英文全称为 time date control，语法格式为“timedatectl [参数]”。

发现电脑时间跟实际时间不符？如果只差几分钟的话，我们可以直接调整。但是，如果差几个小时，那么除了调整当前的时间，还有必要检查一下时区了。timedatectl 命令中常见的参数及作用如表 2-5 所示。

# timedatectl 命令中常见的参数以及作用
|      参数      |     作用      |  |
| :------------: | :-----------: |
|     status     | 显示状态信息  |
| list-timezones | 列出已知时区  |
|    set-time    | 设置系统时间  |
|  set-timezone  | 设置生效时区  |
|    set-ntp     | 设置 NTP 服务 |

查看系统时间与时区的方法如下：
```shell
root@ste:~# timedatectl status
               Local time: Sun 2025-05-18 08:32:23 CST
           Universal time: Sun 2025-05-18 00:32:23 UTC
                 RTC time: Mon 2025-03-10 12:39:43
                Time zone: Asia/Shanghai (CST, +0800)
System clock synchronized: no
              NTP service: active
          RTC in local TZ: no
```

如果你查到的时区不是上海（Asia/Shanghai），可以手动进行设置：
```shell
root@ste:~# timedatectl set-timezone Asia/Shanghai
```

如果时间还是不正确，可再手动修改系统日期，不过要记得先关闭 NTP 同步功能，否则手动修改后的时间可能会被自动同步回原来的时间：
```shell
root@linuxprobe:~# timedatectl set-ntp  false
root@linuxprobe:~# timedatectl set-time 2025-05-18
```

而如果想修改时间的话，也很简单：
```shell
root@linuxprobe:~# timedatectl set-time 9:30
root@linuxprobe:~# date 
Sun May 18 09:30:02 AM CST 2025
```
