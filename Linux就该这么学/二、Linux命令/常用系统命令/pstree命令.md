# pstree命令
pstree 命令用于以树状图的形式展示进程之间的关系，英文全称为 process tree，输入该命令后按回车键执行即可。

前文提到，在执行 ps 命令后，产生的信息量太大又没有规律，很难让人再想看第二眼。如果想让进程以树状图的形式有层次地展示出进程之间的关系，则可以使用pstree 命令：
```shell
root@linuxprobe:~# pstree
systemd─┬─ModemManager───3*[{ModemManager}]
        ├─NetworkManager───3*[{NetworkManager}]
        ├─VGAuthService
        ├─accounts-daemon───3*[{accounts-daemon}]
        ├─atd
        ├─auditd─┬─sedispatch
        │        └─2*[{auditd}]
        ├─avahi-daemon───avahi-daemon
        ├─colord───3*[{colord}]
………………省略部分输出信息………………
```