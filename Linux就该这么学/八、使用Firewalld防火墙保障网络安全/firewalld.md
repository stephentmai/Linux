# firewalld
RHEL 10 系统中默认的防火墙管理工具是 firewalld，它拥有基于 CLI（命令行界面） 和基于 GUI（图形用户界面）的两种管理方式。

相较于传统的防火墙管理配置工具，firewalld 支持动态更新技术并加入了区域（zone）的概念。简单来说，区域即 firewalld 预先准备的几套防火墙策略集合（策略模板），用户能够根据生产场景的不同而选择合适的策略集合，从而实现防火墙策略之间的快速切换。例如， 我们有一台笔记本电脑，每天都要在办公室、咖啡厅和家里使用。按常理来讲，这三者的安全性按照由高到低的顺序来排列，应该是家庭、公司办公室、咖啡厅。当前，我们希望为这台笔记本电脑制定如下防火墙策略规则：在家中允许访问所有服务；在办公室内仅允许访问文件共享服务； 在咖啡厅仅允许上网浏览。以往，我们需要频繁地手动设置防火墙策略规则，而现在只需要预设好区域集合，然后轻点鼠标就可以自动切换了，从而极大地提升了防火墙策略的应用效率。firewalld 中常见的区域名称（默认为public）以及相应的策略规则如表 8-2 所示。

**firewalld 中常用的区域名称及策略规则**

|   区域   |                                                       默认规则策略                                                       |
| :------: | :----------------------------------------------------------------------------------------------------------------------: |
| trusted  |                                                     允许所有的数据包                                                     |
|   home   | 拒绝流入的流量，除非与流出的流量相关；如果流量与 ssh、mdns、ipp-client、 amba-client、dhcpv6-client 服务相关，则允许流量 |
| internal |                                                     等同于 home 区域                                                     |
|   work   |          拒绝流入的流量，除非与流出的流量相关；如果流量与 ssh 、ipp-client、 dhcpv6-client 服务相关，则允许流量          |
|  public  |                拒绝流入的流量，除非与流出的流量相关；如果流量与 ssh、dhcpv6-client 服务 相关，则允许流量                 |
| external |                        拒绝流入的流量，除非与流出的流量相关；如果流量与 ssh 服务相关，则允许流量                         |
|   dmz    |                        拒绝流入的流量，除非与流出的流量相关；如果流量与 ssh 服务相关，则允许流量                         |
|  block   |                                           拒绝流入的流量，除非与流出的流量相关                                           |
|   drop   |                                           拒绝流入的流量，除非与流出的流量相关                                           |

## 终端管理工具
第 2 章在讲解 Linux 命令时曾提到，命令行终端是一种极富效率的工作方式， firewall-cmd 是 firewalld 防火墙配置管理工具的 CLI（命令行界面）版本。它的参数一般都是以“长格式”来提供的。大家不要一听到长格式就头大，因为 RHEL 10 系统支持部分命令的参数补齐，其中就包含这条命令（很酷吧）。也就是说，现在除了能用 Tab 键自动补齐命令或文件名等内容之外，还可以用 Tab 键来补齐表 8-3 中所示的长格式参数。这太棒了！

**firewall-cmd 命令中使用的参数以及作用**

|             参数              | 作用                                                 |
| :---------------------------: | :--------------------------------------------------- |
|          --get-zones          | 显示所有可用的区域                                   |
|        --get-services         | 显示所有预先定义的服务                               |
|      --get-active-zones       | 显示当前正在使用的区域及其对应的网卡名称             |
|      --get-default-zone       | 查询默认的区域名称                                   |
| --set-default-zone=<区域名称> | 设置默认的区域，并使其永久生效                       |
|         --add-source=         | 将源自指定 IP 或子网的流量导向指定的区域             |
|       --remove-source=        | 取消将源自指定 IP 或子网的流量导向某个区域           |
|  --add-interface=<网卡名称>   | 将源自指定网卡的所有流量导向指定的区域               |
| --change-interface=<网卡名称> | 将指定网卡与指定区域进行关联                         |
|        --list-services        | 列出当前区域允许的服务                               |
|         --list-ports          | 列出当前区域允许的端口                               |
|          --list-all           | 显示当前区域的所有配置参数、资源、端口及服务信息     |
|       --list-all-zones        | 显示所有区域的配置参数、资源、端口及服务信息         |
|    --add-service=<服务名>     | 允许默认区域的指定服务流量                           |
|   --remove-service=<服务名>   | 禁止默认区域的指定服务流量                           |
|   --add-port=<端口号或协议>   | 允许默认区域的指定端口流量                           |
| --remove-port=<端口号或协议>  | 禁止默认区域的指定端口流量                           |
|   --query-service=<服务名>    | 查询指定服务是否被允许                               |
|  --query-port=<端口号或协议>  | 查询指定端口是否被允许                               |
|   --add-rich-rule='<规则>'    | 添加一条复杂的规则                                   |
|  --remove-rich-rule='<规则>'  | 删除一条复杂的规则                                   |
|           --reload            | 重新加载配置，让永久生效的规则立即生效，覆盖当前配置 |
|          --panic-on           | 开启应急模式，拒绝所有流量                           |
|          --panic-off          | 关闭应急模式，恢复正常流量处理                       |
|          --permanent          | 将规则设置为永久生效（重启后依然有效）               |

与 Linux 系统中其他的防火墙策略配置工具一样，使用 firewalld 配置的防火墙策略默认为运行时（Runtime）模式，又称为当前生效模式，而且会随着系统的重启而失效。如果想让配置策略一直存在，就需要使用永久（Permanent）模式了，方法就是在用 firewall-cmd 命令正常设置防火墙策略时添加--permanent 参数，这样配置的防火墙策略就永久生效了。

但是，永久生效模式有一个“不近人情”的特点，就是使用它设置的策略只有在系统重启之后才能自动生效。如果想让配置的策略立即生效，需要手动执行 firewall-cmd --reload 命令。

Tips ：
Runtime：当前立即生效，重启后失效。
Permanent：当前不生效，重启后生效。
接下来的实验都很简单，但是提醒大家一定要仔细查看使用的是 Runtime 模式还是Permanent 模式。如果不关注这个细节，就算正确配置了防火墙策略，也可能无法达到预期的效果。
实验 1：查看firewalld 服务当前所使用的区域。
这是一步非常重要的操作。在配置防火墙策略前，必须查看当前生效的是哪个区域，否则配置的防火墙策略将不会立即生效。
```shell
root@linuxprobe:~# firewall-cmd --get-default-zone
public
```
实验 2：查询指定网卡在firewalld 服务中绑定的区域。
在生产环境中，服务器大多不止有一块网卡。一般来说，充当网关的服务器有两块网卡， 一块对公网，另外一块对内网，这两块网卡在审查流量时所用的策略肯定也是不一致的。因此，可以根据网卡针对的流量来源，为网卡绑定不同的区域，实现对防火墙策略的灵活管控。
```shell
root@linuxprobe:~# firewall-cmd --get-zone-of-interface=ens160
public
```
实验 3：把网卡默认区域修改为external，并在系统重启后生效。
使用 change-interface 参数指定的网卡名称（切记要以实际为准，不确定的话用ifconfig 命令进行查询）。设置成功后再用 get-zone-of-interface 参数查询网卡更改后的区域。
```shell
root@linuxprobe:~# firewall-cmd --permanent --zone=external --change-interface=ens160
The interface is under control of NetworkManager, setting zone to 'external'.
success
root@linuxprobe:~# firewall-cmd --permanent --get-zone-of-interface=ens160
external
```
实验 4：把firewalld 服务的默认区域设置为public。
默认区域也叫全局配置，指的是对所有网卡都生效的配置，优先级较低。在下面的代码中可以看到，当前默认区域为 public，而 ens160 网卡的区域为 external，此时便是以网卡的区域名称为准。
通俗来说，默认区域就是一种通用的策略。例如，食堂为所有人准备了一次性餐具，而环保主义者则会自己携带碗筷。如果你自带了碗筷，就可以用自己的；反之就用食堂统一提供的。
```shell
root@linuxprobe:~# firewall-cmd --set-default-zone=public
Warning: ZONE_ALREADY_SET: public
success
root@linuxprobe:~# firewall-cmd --get-default-zone 
public
root@linuxprobe:~# firewall-cmd --get-zone-of-interface=ens160
external
```
实验 5：启动和关闭firewalld 防火墙服务的应急模式。
如果想在 1s 的时间内阻断一切网络连接，有什么好办法呢？大家下意识地会说：“拔掉网线！”这是一个物理级别的高招。但是，如果人在北京，服务器在异地呢？应急模式在这个时候就派上用场了。使用--panic-on 参数会立即切断一切网络连接，而使用--panic-off 则会恢复网络连接。切记，应急模式会切断一切网络连接，因此在远程管理服务器时，在按下回车键前一定要三思。
```shell
root@linuxprobe:~# firewall-cmd --panic-on
success
root@linuxprobe:~# firewall-cmd --panic-off
success
```
实验 6：查询是否放行SSH 和HTTPS 的流量。
在工作中可以不使用--zone 参数指定区域名称，firewall-cmd 命令会自动依据默认区域进行查询，从而减少用户的输入量。但是，如果默认区域与网卡所绑定的不一致时，就会发生冲突，因此规范写法的 zone 参数是一定要加的。
```shell
root@linuxprobe:~# firewall-cmd --query-service=ssh
yes
root@linuxprobe:~# firewall-cmd --query-service=https
no
```
实验 7：把HTTPS 的流量设置为永久放行，并立即生效。
默认情况下对防火墙策略进行的修改都属于 Runtime 模式，即当前生效而重启后失效， 因此在工作和考试中尽量避免使用。而在使用--permanent 参数时，则是当前不会立即看到效果，而在重启或重新加载后方可生效。于是，在添加了放行HTTPS 流量的策略后，查询当前模式策略，发现依然是不放行HTTPS 的流量：
```shell
root@linuxprobe:~# firewall-cmd --permanent --add-service=https
success
root@linuxprobe:~# firewall-cmd --query-service=https
no
```
不想重启服务器的话，就用--reload 参数吧：
```shell
root@linuxprobe:~# firewall-cmd --reload
success
root@linuxprobe:~# firewall-cmd --query-service=https
yes
```
实验 8：把HTTP 的流量设置为永久拒绝，并立即生效。
由于在默认情况下HTTP 的流量没有被放行，所以会有Warning:NOT_ENABLED: http 这样的提示信息，因此对实际操作没有影响。
```shell
root@linuxprobe:~# firewall-cmd --permanent --remove-service=http
Warning: NOT_ENABLED: http
success
root@linuxprobe:~# firewall-cmd --reload 
success
```
实验 9：把访问 8080 和 8081 端口的流量策略设置为允许，但仅限当前生效。
由于是仅限当前生效，因此不需要使用 permanent 参数，设置成功后添加 list-ports
参数可查询到系统中所有针对端口号的防火墙策略。
```shell
root@linuxprobe:~# firewall-cmd --add-port=8080-8081/tcp
success
root@linuxprobe:~# firewall-cmd --list-ports
8080-8081/tcp
```
实验 10：把原本访问本机 888 端口的流量转发到 22 端口，且要求当前和长期均有效。
第 9 章介绍的 SSH 远程控制协议是基于 TCP/22 端口传输控制命令的，如果想让用户通过其他端口号也能访问 ssh 服务，就可以试试端口转发技术了。通过这项技术，新的端口在收到用户请求后会自动将其转发到原本服务的端口上，使得用户能够通过新的端口访问到原本的服务。
来举个例子帮助大家理解。假设小强是电子厂的工人，他喜欢上了三号流水线上的工人小花，但不好意思表白，于是写了一封情书并交给门卫张大爷，希望由张大爷转交给小花。这样一来，情书（信息）的传输由从小强到小花，变成了从小强到张大爷再到小花，情书（信息）依然能顺利送达。
使用firewall-cmd 命令实现端口转发的格式有点长，这里为大家总结好了：
firewall-cmd --permanent --zone=<区域> --add-forward-port=port=<源端口号>:proto=<协议>:toport=<目标端口号>:toaddr=<目标IP地址>
上述命令中的目的 IP 地址一般是服务器本地的 IP 地址：
```shell
root@linuxprobe:~# firewall-cmd --permanent --add-forward-port=port=888:proto=tcp:toport=22:toaddr=192.168.10.10
success
root@linuxprobe:~# firewall-cmd --reload
success
```
在客户端使用 ssh 命令尝试访问 192.168.10.10 主机的 888 端口，访问成功：
```shell
[root@client A ~]# ssh -p 888 192.168.10.10
The authenticity of host '[192.168.10.10]:888 ([192.168.10.10]:888)' can't be established.
ED25519 key fingerprint is SHA256:0R7Kuk/yCTlJ+E4G9y9iX/A/hAklHkALm5ZUgnJ01cc.
This key is not known by any other names.
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added '[192.168.10.10]:888' (ED25519) to the list of known hosts.
root@192.168.10.10's password: 此处输入远程root管理员的密码
Last login: Thu Mar 20 11:36:54 2025 from 192.168.10.20
```
实验 11：设置富规则。
富规则也叫复杂规则，表示更细致、更详细的防火墙策略配置，它可以针对系统服务、端口号、源地址和目的地址等诸多信息进行更有针对性的策略配置。它的优先级在所有的防火墙策略中也是最高的。比如，我们在 firewalld 服务中配置一条富规则，使其拒绝192.168.10.0/24 网段的所有用户访问本地的ssh 服务（22 端口）：
```shell
root@linuxprobe:~# firewall-cmd --permanent --add-rich-rule="rule family="ipv4" source address="192.168.10.0/24" service name="ssh" reject"
success
root@linuxprobe:~# firewall-cmd --reload
success
```
在客户端使用 ssh 命令尝试访问 192.168.10.10 主机的 ssh 服务（22 端口）：
```shell
[root@client A ~]# ssh 192.168.10.10
ssh: connect to host 192.168.10.10 port 22: Connection refused
```
Tips ：
“一个男人”和“一个高个子、大眼睛、脸右侧有个酒窝的男人”相比，很明显后者的描述更加精准，减少了错误匹配的概率，应该优先匹配。

## 图形管理工具
在各种版本的 Linux 系统中，几乎没有能让刘遄老师欣慰并推荐的图形化工具，但是firewall-config 做到了。它是 firewalld 防火墙配置管理工具的 GUI（图形用户界面） 版本，几乎能够实现所有以命令行执行的操作。毫不夸张地说，即使读者没有扎实的 Linux 命令基础，也完全可以通过它来妥善配置 RHEL 10 中的防火墙策略。但在默认情况下系统并没有提供 firewall-config 命令，我们需要自行用 dnf 命令进行安装，所以需要先配置软件仓库。

首先将虚拟机的“CD/DVD（SATA）”光盘选项设置为“使用 ISO 映像文件”，然后选择已经下载好的系统镜像，如图 8-2 所示。
随书配套的软件资源请在这里下载：https://www.linuxprobe.com/tools
Red Hat Enterprise Linux (RHEL) 10 —— 红帽操作系统（必需）：
由全球领先的开源软件及企业级服务提供商红帽公司出品，是一款相当稳定、出色的 Linux 操作系统。

![](img/图2025-12-09-22-30-24.png)
将虚拟机的光盘设备指向ISO镜像


Tips ：
下载后的系统镜像是以.iso 结尾的文件，选中即可，无须解压。

然后，把光盘设备中的系统镜像挂载到/media/cdrom 目录：
```shell
root@linuxprobe:~# mkdir -p /media/cdrom
root@linuxprobe:~# mount /dev/cdrom /media/cdrom
mount: /media/cdrom: WARNING: source write-protected, mounted read-only.
```
为了能够让软件仓库一直为用户提供服务，更加严谨的做法是将系统镜像文件的挂载信息写入/etc/fstab 文件中，以保证万无一失：
```shell
root@linuxprobe:~# vim /etc/fstab
#
# /etc/fstab
# Created by anaconda on Wed Mar 12 19:35:26 2025
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
UUID=408f4a3d-a4d3-4a44-bb23-6988cdbd10bf /                  xfs     defaults        0 0
UUID=4cf8ecae-bcb6-4b1e-8001-968b33643a8a /boot              xfs     defaults        0 0
UUID=1FB8-9199          /boot/efi               vfat    umask=0077,shortname=winnt	 0 2
UUID=d936c726-45a7-4ca2-8932-c54f84a3d787 none               swap    defaults        0 0
/dev/cdrom 								  /media/cdrom		iso9660	 defaults 		 0 0
```
最后，使用 Vim 文本编辑器创建软件仓库的配置文件。与之前版本的系统不同，RHEL 10 需要配置两个软件仓库（即[BaseOS]与[AppStream]），且缺一不可。下述命令中用到的具体参数的含义可参见 4.1.4 节。
```shell
root@linuxprobe:~# vim /etc/yum.repos.d/rhel10.repo
[BaseOS]
name=BaseOS
baseurl=file:///media/cdrom/BaseOS
enabled=1
gpgcheck=0
[AppStream]
name=AppStream
baseurl=file:///media/cdrom/AppStream
enabled=1
gpgcheck=0
```
在正确配置软件仓库文件后，就可以开始用 yum 或dnf 命令安装软件了。这两个命令在实际操作中除了名字不同外，执行方法完全一致，大家可随时用 yum 来替代 dnf 命令。下面安装firewalld 图形用户界面工具：
```
root@linuxprobe:~# dnf install firewall-config
Updating Subscription Management repositories.
BaseOS                                          2.7 MB/s | 2.7 kB     00:00    
AppStream                                       2.7 MB/s | 2.8 kB     00:00    
Dependencies resolved.
================================================================================
 Package              Arch        Version                  Repository      Size
================================================================================
Installing:
 firewall-config      noarch      2.2.1-1.el10             AppStream       90 k
Installing dependencies:
 dbus-x11             x86_64      1:1.14.10-4.el10         AppStream       26 k

Transaction Summary
================================================================================
Install  2 Packages

Total size: 116 k
Installed size: 1.0 M
Is this ok [y/N]: y
Downloading Packages:
Running transaction check
Transaction check succeeded.
Running transaction test
Transaction test succeeded.
Running transaction
  Preparing        :                                                        1/1 
  Installing       : dbus-x11-1:1.14.10-4.el10.x86_64                       1/2 
  Installing       : firewall-config-2.2.1-1.el10.noarch                    2/2 
  Running scriptlet: firewall-config-2.2.1-1.el10.noarch                    2/2 
Installed products updated.

Installed:
  dbus-x11-1:1.14.10-4.el10.x86_64      firewall-config-2.2.1-1.el10.noarch     

Complete!
```
安装成功后，firewall-config 工具的图形界面如图 8-3 所示，其功能具体如下。
1：选择运行时（Runtime）或永久（Permanent）模式的配置。
2：可选的策略集合区域列表。
3：常用的系统服务列表。
4：主机地址的黑白名单。
5：当前正在使用的区域。
6：管理当前被选中区域中的服务。
7：管理当前被选中区域中的端口。
8：设置允许被访问的协议。
9：设置允许被访问的端口。
10：开启或关闭 SNAT（源网络地址转换）技术。
11：设置端口转发策略。
12：控制请求icmp 服务的流量。
13：管理防火墙的富规则。
14：被选中区域的服务，若勾选了相应服务前面的复选框，则表示放行与之相关的流量。
15：firewall-config 工具的运行状态。

![](img/图2025-12-09-22-59-58.png)
firewall-config的图形界面

除了图 8-3 中列出的功能，还有用于将网卡与区域绑定的 Interfaces 选项，以及用于将 IP 地址与区域绑定的 Sources 选项。另外再啰嗦一句，在使用 firewall-config 工具配置完防火墙策略之后，无须进行二次确认，因为只要有修改内容，它就自动进行保存。

下面进行动手实践环节。

先将当前区域内请求http 服务的流量设置为放行，但仅限当前生效，具体配置如图 8-4 所示。

![](img/图2025-12-09-23-00-16.png)
允许放行请求http服务的流量

尝试添加一条防火墙策略规则，使其放行访问 8080～8088 端口（TCP）的流量，并将其设置为永久生效，以达到系统重启后防火墙策略依然生效的目的。在按照图 8-5 所示的界面配置完毕之后，还需要在 Options 菜单中单击 Reload Firewalld 命令，让配置的防火墙策略立即生效（见图 8-6）。这与在命令行中使用--reload 参数的效果一样。

![](img/图2025-12-09-23-00-41.png)
放行访问8080～8088端口的流量

![](img/图2025-12-09-23-01-05.png)
让配置的防火墙策略规则立即生效

前面在讲解 firewall-config 工具的功能时，曾经提到了 SNAT（Source Network Address Translation，源网络地址转换）技术。SNAT 是一种为了解决 IP 地址匮乏而设计的技术，它能够使得多个内网中的用户通过同一个外网 IP 接入互联网。该技术的应用非常广泛， 我们每天都在使用，只不过没有察觉到罢了。比如，当通过家中的网关设备（无线路由器） 访问本书配套站点 www.linuxprobe.com 时，就用到了 SNAT 技术。

大家可以看一下在网络中不使用 SNAT 技术（见图 8-7）和使用 SNAT 技术（见图 8-8） 时的情况。在图 8-7 所示的局域网中有多台 PC，如果网关服务器没有应用 SNAT 技术，则互联网中的网站服务器在收到PC 的请求数据包，并回送响应数据包时，将无法在网络中找到这个私有网络的 IP 地址，所以 PC 也就收不到响应数据包了。在图 8-8 所示的局域网中，由于网关服务器应用了 SNAT 技术，所以互联网中的网站服务器会将响应数据包发给网关服务器， 再由后者转发给局域网中的 PC。

![](img/图2025-12-09-23-06-55.png)
没有使用SNAT技术的网络

![](img/图2025-12-09-23-07-29.png)
使用SNAT技术处理过的网络

使用iptables 命令实现SNAT 技术是一件很麻烦的事情，但是在 firewall-config 中却是小菜一碟。用户只需按照图 8-9 进行配置，并选中 Masquerade zone 复选框，就自动开启了SNAT 技术。

![](img/图2025-12-09-23-15-38.png)
开启防火墙的SNAT技术

为了让大家直观查看不同工具在实现相同功能时的区别，针对前面使用 firewall-cmd 配置的防火墙策略规则，这里使用 firewall-config 工具进行了重新演示：将本机 888 端口的流量转发到 22 端口，且要求当前和长期均有效，具体如图 8-10 和图 8-11 所示。

![](img/图2025-12-09-23-16-32.png)
 配置本地的端口转发

![](img/图2025-12-09-23-16-49.png)
让防火墙策略规则立即生效

用命令配置富规则可真辛苦，幸好我们现在有了图形用户界面工具。设置当192.168.10.20 主机访问本机的 1234 端口号时，防火墙会自动执行的动作，如图 8-12 所示。其中 Element 选项能够根据服务名称、端口号、协议等信息进行匹配；Source 与 Destination 选项后的 inverted 复选框代表反选功能，将其选中则代表对已填写信息进行反选，即选中填写信息以外的主机地址；Log 复选框在选中后，日志不仅会被记录到日志文件中，而且还可以在设置日志的等级（Level）后，再将日志记录到日志文件中，以方便后续的筛查。

![](img/图2025-12-09-23-17-11.png)
配置防火墙富规则策略

如果生产环境中的服务器有多块网卡在同时提供服务（这种情况很常见），则对内网和对外网提供服务的网卡要选择的防火墙策略区域是不一样的。也就是说，可以把网卡与防火墙策略区域进行绑定（见图 8-13 和图 8-14），这样就能够使用不同的防火墙区域策略，对源自不同网卡的流量进行有针对性的监控，效果会更好。

![](img/图2025-12-09-23-17-29.png)
把网卡与防火墙策略区域进行绑定

![](img/图2025-12-09-23-17-41.png)
网卡与策略区域绑定完成

最后再提一句，firewall-config 工具真的非常实用，很多原本复杂的长命令被图形化按钮替代，设置规则也简单明了，足以应对日常工作。所以再次向大家强调配置防火墙策略的原则—只要能实现所需的功能，用什么工具请随君便。