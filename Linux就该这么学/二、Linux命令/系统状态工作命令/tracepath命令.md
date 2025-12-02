# tracepath命令
tracepath 命令用于显示数据包到达目的主机时途中经过的所有路由信息，语法格式为“tracepath [参数] IP 地址或域名”。

当两台主机之间无法正常 ping 通时，要考虑两台主机之间是否有错误的路由信息，导致数据被某一台设备错误地丢弃。这时便可以使用 tracepath 命令追踪数据包到达目的主机时途中的所有路由信息，以分析是哪台设备出了问题。下面的情况就很清晰了：
```shell
root@linuxprobe:~# tracepath www.linuxprobe.com
 1?: [LOCALHOST]                                          pmtu 1500
 1:  no reply
 2:  11.223.0.189                                          5.954ms asymm  1 
 3:  11.223.0.14                                           6.256ms asymm  2 
 4:  11.220.159.62                                         3.313ms asymm  3 
 5:  116.251.107.13                                        1.841ms 
 6:  140.205.50.237                                        2.416ms asymm  5 
 7:  101.95.211.117                                        2.772ms 
 8:  101.95.208.45                                        40.839ms 
 9:  101.95.218.217                                       13.898ms asymm  8 
10:  202.97.81.162                                         8.113ms asymm  9 
11:  221.229.193.238                                      15.693ms asymm 10 
12:  no reply
13:  no reply
14:  no reply
15:  no reply
16:  no reply
………………省略部分输出信息………………
```