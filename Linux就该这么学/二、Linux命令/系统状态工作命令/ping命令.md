# ping命令
ping 命令用于测试主机之间的网络连通性，语法格式为“ping [参数] 主机地址”。

即便大家没有学习过 Linux 系统，相信也肯定见过别人使用 ping 命令。执行 ping 命令时，系统会使用 ICMP 向远端主机发出要求回应的信息，若连接远端主机的网络没有问题， 远端主机会回应该信息。由此可见，ping 命令可用于判断远端主机是否在线并且网络是否正常。ping 命令的常见参数以及作用如表 2-11 所示。
**ping 命令的常见参数以及作用**
| 参数  |        作用        |
| :---: | :----------------: |
|  -c   |    总共发送次数    |
|  -i   | 每次间隔时间（秒） |
|  -W   | 最长等待时间（秒） |
使用 ping 命令测试一台在线的主机（其 IP 地址为 192.168.10.10），得到的回应是这样的：
```shell
root@linuxprobe:~# ping -c 4 192.168.10.10
PING 192.168.10.10 (192.168.10.10) 56(84) bytes of data.
64 bytes from 192.168.10.10: icmp_seq=1 ttl=64 time=0.054 ms
64 bytes from 192.168.10.10: icmp_seq=2 ttl=64 time=0.053 ms
64 bytes from 192.168.10.10: icmp_seq=3 ttl=64 time=0.052 ms
64 bytes from 192.168.10.10: icmp_seq=4 ttl=64 time=0.042 ms

--- 192.168.10.10 ping statistics ---
4 packets transmitted, 4 received, 0% packet loss, time 3090ms
rtt min/avg/max/mdev = 0.042/0.050/0.054/0.004 ms
```
测试一台不在线的主机（其IP 地址为 192.168.10.20），得到的回应是这样的：
```shell
root@linuxprobe:~# ping -c 4 192.168.10.20
PING 192.168.10.20 (192.168.10.20) 56(84) bytes of data.
From 192.168.10.10 icmp_seq=1 Destination Host Unreachable
From 192.168.10.10 icmp_seq=2 Destination Host Unreachable
From 192.168.10.10 icmp_seq=3 Destination Host Unreachable
From 192.168.10.10 icmp_seq=4 Destination Host Unreachable

--- 192.168.10.20 ping statistics ---
4 packets transmitted, 0 received, +4 errors, 100% packet loss, time 3051ms
pipe 3
```
细心的同学一定注意到了-c 参数，这是因为在 Windows 系统下执行 ping 命令时，默认只会发送 4 次数据包，而 Linux 系统下的 ping 命令会一直执行下去，需要运维人员手动限制次数。