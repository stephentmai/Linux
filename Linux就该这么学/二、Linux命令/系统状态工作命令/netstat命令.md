# netstat命令
netstat 命令用于显示网络连接、路由表、接口状态等网络相关信息，英文全称为network status，语法格式为“netstat [参数]”。

只要 netstat 命令使用得当，便可以查看到网络状态的方方面面信息。我们找出一些常用的参数让大家感受一下，如表 2-12 所示。
**netstat 命令的常用参数以及作用**
| 参数  |            作用            |
| :---: | :------------------------: |
|  -a   |  显示所有连接中的 Socket   |
|  -p   | 显示正在使用的 Socket 信息 |
|  -t   |    显示 TCP 的连接状态     |
|  -u   |    显示 UDP 的连接状态     |
|  -n   |  使用 IP 地址，不使用域名  |
|  -l   |  仅列出正在监听的服务状态  |
|  -i   |      显示网卡列表信息      |
|  -r   |       显示路由表信息       |
使用netstat 命令显示详细的网络状况：
```shell
root@linuxprobe:~# netstat -a
Active Internet connections (servers and established)
Proto Recv-Q Send-Q Local Address           Foreign Address         State      
tcp        0      0 localhost:ipp           0.0.0.0:*               LISTEN     
tcp6       0      0 localhost:ipp           [::]:*                  LISTEN     
tcp6       0      0 [::]:websm              [::]:*                  LISTEN     
udp        0      0 linuxprobe.com:bootpc   _gateway:bootps         ESTABLISHED
udp        0      0 0.0.0.0:39400           0.0.0.0:*                          
udp        0      0 0.0.0.0:mdns            0.0.0.0:*                          
udp6       0      0 [::]:47721              [::]:*                             
udp6       0      0 [::]:mdns               [::]:*       
………………省略部分输出信息………………   
```
使用netstat 命令显示网卡列表：
```shell
root@linuxprobe:~# netstat -i 
Kernel Interface table
Iface             MTU    RX-OK RX-ERR RX-DRP RX-OVR    TX-OK TX-ERR TX-DRP TX-OVR Flg
ens160           1500    32543      0      0 0          9508      0      0      0 BMRU
lo              65536      126      0      0 0           126      0      0      0 LRU
```