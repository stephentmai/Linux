# 常见Linux目录名称
|   目录   |               用途               |
|:------:|:------------------------------:|
|   /    |      虚拟目录的根目录。通常不会在这里存储文件      |
|  /bin  |      二进制目录，存放许多用户级的GNU工具       |
| /boot  |          启动目录，存放启动文件           |
|  /dev  |      设备目录，Linux在这里创建设备节点       |
|  /etc  |            系统配置文件目录            |
| /home  |       主目录，Linux在这里创建用户目录       |
|  /lib  |       库目录，存放系统和应用程序的库文件        |
| /media |       媒体目录，可移动媒体设备的常用挂载点       |
|  /mnt  |     挂载目录，另一个可移动媒体设备的常用挂载点      |
|  /opt  |     可选目录，常用于存放第三方软件包和数据文件      |
| /proc  |     进程目录，存放现有硬件及当前进程的相关信息      |
| /root  |           root用户的主目录           |
| /sbin  |     系统二进制目录，存放许多GNU管理员级工具      |
|  /run  |       运行目录，存放系统运行时的运行时数据       |
|  /srv  |        服务目录，存放本地服务的相关文件        |
|  /sys  |       系统目录，存放系统硬件信息的相关文件       |
|  /tmp  |    临时目录，可以在该目录中创建和删除临时工作文件     |
|  /usr  | 用户二进制目录，大量用户级的GNU工具和数据文件都存储在这里 |
|  /var  |    可变目录，用以存放经常变化的文件，比如日志文件     |

# 安装软件的方法
<div style="text-align: center;">常见的Yum命令</div>

|             命令              |       作用       |
|:---------------------------:|:--------------:|
|      yum repolist all       |     列出所有仓库     |
|        yum list all         |   列出仓库中所有软件包   |
|       yum info 软件包名称        |    查看软件包信息     |
|      yum install 软件包名称      |     安装软件包      |
| yum reinstall 软件包名称}重新安装软件包 |
|      yum update 软件包名称       |     升级软件包      |
|      yum remove 软件包名称       |     移除软件包      |
|        yum clean all        |    清除所有仓库缓存    |
|      yum check-update       |   检查可更新的软件包    |
|        yum grouplist        | 查看系统中已经安装的软件包组 |
|    yum groupinstall 软件包组    |   安装指定的软件包组    |
|     yum goupremove 软件包组     |   移除指定的软件包组    |
|     yum groupinfo 软件包组      |  查询指定的软件包组信息   |

# 配置服务的方法
<div style="text-align: center;">服务等常用命令</div>

|                  命令                   |        作用         |
|:-------------------------------------:|:-----------------:|
|         systemctl enable sshd         |      开机自动启动       |
|        systemctl disable sshd         |      开机不自动启       |
|       systemctl is-enabled sshd       |  查看特定服务是否为开机自启动   |
| systemctl list-unit-files --type=sshd | 查看各个级别下服务的启动与禁用情况 |
|         systemctl start sshd          |       启动服务        |
|        systemctl restart sshd         |       重启服务        |
|          systemctl stop sshd          |       停止服务        |
|         systemctl reload sshd         |  重新加载配置文件（不终止服务）  |
|         systemctl status sshd         |      查看服务状态       |

# man命令中常用按键以及作用

|    按键     |          作用           |
|:---------:|:---------------------:|
|    空格键    |         向下翻一页         |
| Page down |         向下翻一页         |
|  Page up  |         向上翻一页         |
|   home    |        直接前往首页         |
|    end    |        直接前往尾页         |
|     /     | 从上至下搜索某个关键词，如“linux”  |
|     ？     | 从上至下搜索牧某个关键词，如“linux” |
|     n     |     定位到下一个搜索到的关键词     |
|     N     |     定位到上一个搜索到的关键词     | 
|     b     |         向上翻一页         |
|     g     |        跳转到第一页         |
|     h     |        显示帮助界面         |
|     u     |        向上滚动半页         |
|     d     |        向下滚动半页         |
|     q     |        退出帮助文档         |

<div style="text-align: center;">man命令中帮助信息的结构以及意义</div>

|     结构      |     代表意义     |
|:-----------:|:------------:|
|    NAME     |    命令的名称     |
|  SYNOPSIS   |  参数的大致使用方法   |
| DESCRIPTION |     介绍说明     |
|  EXAMPLES   |  演示（附带简单说明）  |
|  OVERVIEW   |      概述      |
|  DEFAULTS   |    默认的功能     |
|   OPTIONS   | 具体的可用选项（带介绍） |
| ENVIRONMENT |     环境变量     |
|    FILES    |    用到的文件     |
|  SEE ALSO   |    相关的资料     |
|   HISTORY   |  维护历史与联系方式   |
