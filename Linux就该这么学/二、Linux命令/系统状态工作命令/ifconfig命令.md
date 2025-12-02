 # ifconfig命令
 ifconfig 命令用于获取网卡配置与网络状态等信息，英文全称为 interface configurator， 语法格式为“ifconfig [参数] [网络设备]”。

使用ifconfig 命令查看本机当前的网卡配置与网络状态等信息时，其实主要查看的就是网卡名称、inet 参数后面的 IP 地址、ether 参数后面的网卡物理地址（又称为 MAC 地址），以及 RX 的接收数据包的个数、TX 的发送数据包的个数、累计流量。
```shell
root@linuxprobe:~# ifconfig
ens160: flags=4163<UP,BROADCAST,RUNNING,MULTICAST>  mtu 1500
        inet 192.168.10.10  netmask 255.255.255.0  broadcast 192.168.31.255
        inet6 fe80::20c:29ff:fee5:e733  prefixlen 64  scopeid 0x20<link>
        ether 00:0c:29:e5:e7:33  txqueuelen 1000  (Ethernet)
        RX packets 27660  bytes 27151953 (25.8 MiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 9449  bytes 652999 (637.6 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0

lo: flags=73<UP,LOOPBACK,RUNNING>  mtu 65536
        inet 127.0.0.1  netmask 255.0.0.0
        inet6 ::1  prefixlen 128  scopeid 0x10<host>
        loop  txqueuelen 1000  (Local Loopback)
        RX packets 120  bytes 10704 (10.4 KiB)
        RX errors 0  dropped 0  overruns 0  frame 0
        TX packets 120  bytes 10704 (10.4 KiB)
        TX errors 0  dropped 0 overruns 0  carrier 0  collisions 0
```
在 RHEL 5、RHEL 6 系统及其他大多数早期的Linux 系统中，网卡的名称一直都是 eth0、eth1、eth2、……；在 RHEL 7 中则变成了类似于 eno16777736 这样的名字；而在 RHEL 8、9、10 系统中网卡的最新名称类似于 ens160、ens192 这样，所以经验丰富的运维老手光看网卡名称大致就能猜出系统的版本了