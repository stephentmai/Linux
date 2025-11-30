# 常见Linux目录名称
|     目录      |               用途               |
|:-----------:|:------------------------------:|
|      /      |      虚拟目录的根目录。通常不会在这里存储文件      |
|    /bin     |      二进制目录，存放许多用户级的GNU工具       |
|    /boot    |          启动目录，存放启动文件           |
|    /dev     |      设备目录，Linux在这里创建设备节点       |
|    /etc     |            系统配置文件目录            |
|    /home    |       主目录，Linux在这里创建用户目录       |
|    /lib     |       库目录，存放系统和应用程序的库文件        |
|   /media    |       媒体目录，可移动媒体设备的常用挂载点       |
|    /mnt     |     挂载目录，另一个可移动媒体设备的常用挂载点      |
|    /opt     |     可选目录，常用于存放第三方软件包和数据文件      |
|    /proc    |     进程目录，存放现有硬件及当前进程的相关信息      |
|    /root    |           root用户的主目录           |
|    /sbin    |     系统二进制目录，存放许多GNU管理员级工具      |
|    /run     |       运行目录，存放系统运行时的运行时数据       |
|    /srv     |        服务目录，存放本地服务的相关文件        |
|    /sys     |       系统目录，存放系统硬件信息的相关文件       |
|    /tmp     |    临时目录，可以在该目录中创建和删除临时工作文件     |
|    /usr     | 用户二进制目录，大量用户级的GNU工具和数据文件都存储在这里 |
|    /var     |    可变目录，用以存放经常变化的文件，比如日志文件     |
| /usr/local  |          本地安装的软件和应用程序          |
|  /usr/sbin  |        系统管理员使用的非基本管理命令         |
| /usr/share  |         共享数据，如文档和帮助文件          |
|    /var     |        动态数据，如日志文件和临时文件         |
| /lost+found |       文件系统恢复区，存放丢失的文件碎片        |

# 安装软件的方法
<div style="text-align: left;">常见的Yum命令</div>

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
<div style="text-align: left;">服务等常用命令</div>

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

<div style="text-align: left;">man命令中帮助信息的结构以及意义</div>

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

<div style="text-align: left;">Linux中快捷键</div>

<kbd>**Tab**</kbd>自动补全命令

<kbd>**Ctrl+A**</kbd>移动光标到开头

<kbd>**Ctrl+E**</kbd>移动光标到结尾

<kbd>**Ctrl+F**</kbd>往光标后面移动一个字符

<kbd>**Ctrl+B**</kbd>往光标前面移动一个字符

<kbd>**Ctrl+K**</kbd>剪切光标处到行尾的字符

<kbd>**Ctrl+U**</kbd>剪切光标处到行首的字符

<kbd>**Ctrl+Y**</kbd>将剪切的字符进行粘贴

<kbd>**Ctrl+Ins**</kbd>复制

<kbd>**Shift+Ins**</kbd>粘贴

<kbd>**Ctrl+C**</kbd>终止当前进程的运行

<kbd>**Ctrl+D**</kbd>退出当前Xshell

<kbd>**Ctrl+R**</kbd>搜索命令行使用过的历史命令记录

<kbd>**ESC+.**</kbd>获取上一条命令的最后部分

<kbd>**Ctrl+L**</kbd>清屏

<kbd>**Ctrl+Z**</kbd>暂停

<kbd>**Ctrl+Q**</kbd>解除锁屏

<kbd>**!+命令**</kbd>执行上一条命令，<kbd>**!!**</kbd>执行上两条命令

# 常用系统工作命令
**echo命令**<br>
执行“echo 字符串”或“echo $变量”

**date命令**<br>
date命令用于显示或设置系统的时间与日期，语法格式为date"[+指定的格式]"。

<div style="text-align: left;">date命令中的参数及其作用</div>

| 参数 |          作用          |
|:--:|:--------------------:|
| %S |       秒（00~59）       |
| %M |      分钟（00~59）       |
| %H |      小时（00~23）       |
| %I |      小时（00~12）       |
| %P |       显示出AM或PM       |
| %a |   缩写的工作日名称（例如：Sun）   |
| %A | 完整的工作日名称（例如：Sunday）  |
| %b |   缩写的月份名称（例如：Jan）    |
| %B | 完整的月份呢名称（例如：January） |
| %q |       季度（1~4）        |
| %y |     简写年份（例如：25）      |
| %Y |    完整年份（例如：2025）     |
| %d |       本月中的第几天        |
| %j |       今年中的第几天        |
| %n |    换行符（相当于按下回车键）     |
| %t |    跳格（相当于按下Tab键）     |

按照“年-月-日 小时:分钟:秒”的格式查看当前系统时间的date命令如下所示：
```shell
[root@linuxprobe ~]# date "+%Y-%m-%d %H:%M:%S"
2020-09-05 09:14:35
```

<div style="text-align: left;">Linux系统中的通配符及含义</div>

|     通配符     |   含义    |
|:-----------:|:-------:|
|      *      |  任意字符   |
|      ？      | 单个任意字符  |
|    [a-z]    | 单个小写字符  |
|    [A-Z]    | 单个大写字符  |
|    [0-9]    |  单个数字   |
| [[:alpha:]] |  任意字母   |
| [[:upper:]] | 任意大写字母  |
| [[:lower:]] | 任意小写字母  |
| [[:digit:]] |  所有数字   |
| [[:alnum:]] | 任意字母加数字 |
| [[:punct:]] |  标点符号   |

反斜杠（\)：使反斜杠后面的一个变量变为单纯的字符。
单引号（''）：转移其中所有的变量为单层的字符层。
双引号（""）：保留其中的变量属性，不进行转义处理。
反引号（``）：把其中的命令执行后返回结果。

<div style="text-align: left;">Linux系统中最重要的10个环境变量</div>

|     变量名称     |        作用        |
|:------------:|:----------------:|
|     HOME     |   用户的主目录（即家目录）   |
|    SHELL     | 用户在使用的Shell解释器名称 |
|   HISTSIZE   |   输出的历史命令记录条数    |
| HISTFILESIZE |   保存的历史命令记录条数    |
|     MAIL     |      邮件保存路径      |
|     LANG     |    系统语言、语系名称     |
|    RANDOM    |     生成一个随机数字     |
|     PS1      |   Bash解释器的提示符    |
|     PATH     | 定义解释器搜索用户执行命令的路径 |
|    EDITOR    |    用户默认的文本编译器    |

