# 编写 Shell 脚本
可以将 Shell 终端解释器当作人与计算机硬件之间的“翻译官”，它作为用户与 Linux 系统内部的通信媒介，除了能够支持各种变量与参数外，还提供了诸如循环、分支等高级编程语言才有的控制结构。要想正确使用 Shell 中的这些功能特性，准确下达命令尤为重要。Shell 脚本命令的工作方式有以下两种。

交互式（Interactive）：用户每输入一条命令，系统就立即执行。

批处理（Batch）：用户事先编写一个完整的 Shell 脚本，Shell 会一次性执行脚本中的所有命令。

在 Shell 脚本中，不仅会用到前面学习过的许多 Linux 命令，还会涉及正则表达式、管道符、数据流重定向等语法规则。我们还需要将内部功能模块化，通过逻辑语句进行处理， 最终形成日常所见的 Shell 脚本。

通过查看 SHELL 变量，发现当前系统已经默认使用 Bash 作为命令行终端解释器：
```shell
root@linuxprobe:~# echo $SHELL
/bin/bash
```
## 编写简单的脚本
估计读者在看完上文中有关Shell 脚本的复杂描述后，会“累觉不爱”吧。但是，上文指的是一个高级 Shell 脚本的编写原则。其实，使用 Vim 编辑器把Linux 命令按照顺序依次写入一个文件，就是一个简单的脚本了。

例如，如果想查看当前所在工作路径并列出当前目录下所有的文件及属性信息，实现这个功能的脚本应该类似于下面这样：
```shell
root@linuxprobe:~# vim example.sh
#!/bin/bash 
#For Example by linuxprobe.com
pwd 
ls -al
```
Shell 脚本文件的名称可以任意，但为了避免被误以为是普通文件，建议将.sh 后缀加上， 以表示是一个脚本文件。

在上面的这个 example.sh 脚本中实际上出现了 3 种不同的元素：

第一行的脚本声明（#!）用来告诉系统使用哪种 Shell 解释器执行该脚本；

第二行的注释信息（#）是对脚本功能和某些命令的介绍信息，使得自己或他人在日后看到这个脚本内容时，可以快速知道该脚本的作用或一些警告信息；

第三、四行的可执行语句是我们平时执行的 Linux 命令。

你们不相信这么简单就编写出了一个脚本程序？！执行一下看看结果：
```shell
root@linuxprobe:~# bash example.sh
/root
total 44
dr-xr-x---. 14 root root 4096 Mar 12 23:53 .
dr-xr-xr-x. 18 root root  235 Mar 10 02:32 ..
-rw-------.  1 root root 1019 Mar 10 02:37 anaconda-ks.cfg
-rw-------.  1 root root   52 Mar 10 10:51 .bash_history
-rw-r--r--.  1 root root   18 Jun 24  2024 .bash_logout
-rw-r--r--.  1 root root  141 Jun 24  2024 .bash_profile
-rw-r--r--.  1 root root  429 Jun 24  2024 .bashrc
………………省略部分输出信息………………
```
除了上面的使用 Bash 解释器命令直接运行 Shell 脚本文件外，第二种运行脚本程序的方法是通过输入完整路径的方式来执行。但默认情况下会因为权限不足而提示报错信息，此时只需要为脚本文件增加执行权限即可（详见第 5 章）。初次学习 Linux 系统的读者不用心急， 等在下一章学完用户身份和权限后再来做这个实验也不迟。
```
root@linuxprobe:~# ./example.sh
bash: ./example.sh: Permission denied
root@linuxprobe:~# chmod u+x example.sh
root@linuxprobe:~# ./example.sh
/root
total 44
dr-xr-x---. 14 root root 4096 Mar 12 23:53 .
dr-xr-xr-x. 18 root root  235 Mar 10 02:32 ..
-rw-------.  1 root root 1019 Mar 10 02:37 anaconda-ks.cfg
-rw-------.  1 root root   52 Mar 10 10:51 .bash_history
-rw-r--r--.  1 root root   18 Jun 24  2024 .bash_logout
-rw-r--r--.  1 root root  141 Jun 24  2024 .bash_profile
-rw-r--r--.  1 root root  429 Jun 24  2024 .bashrc
………………省略部分输出信息………………
```
## 接收用户的参数
但是，像上面这样的脚本程序只能执行一些预先定义好的功能，未免太过死板。为了让Shell 脚本程序更好地满足用户的一些实时需求，以便灵活完成工作，必须要让脚本程序能够像之前执行命令时那样接收用户输入的参数。

例如，当用户执行某一个命令时，加或不加参数的输出结果是不同的：
```shell
root@linuxprobe:~# wc -l anaconda-ks.cfg 
40 anaconda-ks.cfg
root@linuxprobe:~# wc -c anaconda-ks.cfg 
1019 anaconda-ks.cfg
root@linuxprobe:~# wc -w anaconda-ks.cfg 
92 anaconda-ks.cfg
```
这意味着命令不仅要能接收用户输入的内容，还要有能力进行判断区别，根据不同的输入调用不同的功能。

其实，Linux 系统中的 Shell 脚本语言早就考虑到了这些，已经内设了用于接收参数的变量，变量之间使用空格间隔。例如，$0 对应的是当前 Shell 脚本程序的名称，$#对应的是总共有几个参数，$*对应的是所有位置的参数值，$?对应的是显示上一次命令的执行返回值，而$1、$2、$3……则分别对应着第 N 个位置的参数值，如图 4-17 所示。

![](img/图2025-12-04-00-14-52.png)
Shell 脚本程序中的参数位置变量

理论过后再来练习一下。尝试编写一个脚本程序示例，通过引用上面的变量参数来看一下真实效果：
```shell
root@linuxprobe:~# vim Example.sh
#!/bin/bash
echo "当前脚本名称为$0"
echo "总共有$#个参数，分别是$*。"
echo "第1个参数为$1，第5个为$5。"
root@linuxprobe:~# bash Example.sh one two three four five six
当前脚本名称为Example.sh
总共有6个参数，分别是one two three four five six。
第1个参数为one，第5个为five。
```
## 判断用户的参数
学习是一个登堂入室、由浅入深的过程。在学习完 Linux 命令，掌握了 Shell 脚本语法变量，以及了解了接收用户的参数之后，就要迈向新的高度—进一步处理接收到的用户参数。

本书在前面章节中讲到，系统在执行 mkdir 命令时会判断用户输入的信息，即判断用户指定的文件夹名称是否已经存在，如果存在则提示报错；反之则自动创建。Shell 脚本中的条件测试语法可以判断表达式是否成立，若条件成立则返回数字 0，否则便返回非零值。条件测试语法的执行格式如图 4-18 所示。切记，条件表达式两边均应有一个空格。

![](img/图2025-12-04-00-16-53.png)
条件测试语句的执行格式

按照测试对象来划分，条件测试语句常被分为 4 种类型：文件测试语句、逻辑测试语句、整数值比较语句、字符串比较语句。
### 文件测试语句
文件测试语句是使用指定条件来判断文件是否存在或权限是否满足等情况的语句结构， 它使用的操作符以及作用如表 4-3 所示。
**文件测试语句使用的操作符以及作用**

| 操作符 |             作用             |
| :----: | :--------------------------: |
|   -d   |    测试文件是否为目录类型    |
|   -e   |       测试文件是否存在       |
|   -f   |      判断是否为一般文件      |
|   -r   |  测试当前用户是否有可读权限  |
|   -w   |  测试当前用户是否有可写权限  |
|   -x   | 测试当前用户是否有可执行权限 |

下面使用文件测试语句来判断/etc/fstab 是否为一个目录类型的文件，然后通过 Shell 解释器内设的$?变量显示上一条命令执行后的返回值。如果返回值为 0，则目录存在；如果返回值为非零值，则意味着它不是目录，或这个目录不存在：
```shell
root@linuxprobe:~# [ -d /etc/fstab ]
root@linuxprobe:~# echo $?
1
```
再使用文件测试语句来判断/etc/fstab 是否为一般文件，如果返回值为 0，则代表文件存在，且为一般文件：
```shell
root@linuxprobe:~# [ -f /etc/fstab ]
root@linuxprobe:~# echo $?
0
```
### 逻辑测试语句
判断与查询一定要敲两次命令吗？其实也能一次性搞定。

逻辑测试语句用于对测试结果进行逻辑分析，根据测试结果可实现不同的效果。例如，在Shell 终端中逻辑“与”的运算符是&&，表示当该运算符前面的命令执行成功后才会执行运算符后面的命令，因此可以用来判断/dev/cdrom 文件是否存在，若存在则输出 Exist 字样。
```shell
root@linuxprobe:~# [ -e /dev/cdrom ] && echo "Exist"
Exist
```
除了逻辑“与”外，还有逻辑“或”，它在 Linux 系统中的运算符为||，表示当该运算符前面的命令执行失败后才会执行该运算符后面的命令，因此可以用系统环境变量 USER 来判断当前登录的用户是否为非管理员身份：
```shell
root@linuxprobe:~# echo $USER
root
root@linuxprobe:~# [ $USER = root ] || echo "user"
root@linuxprobe:~# su - linuxprobe 
linuxprobe@linuxprobe:~$ [ $USER = root ] || echo "user"
user
```
第三种逻辑运算符是“非”，在 Linux 系统中用一个叹号（!）来表示，它表示将条件测试中的判断结果取相反值。也就是说，如果原本测试的结果是正确的，则将其变成错误的； 原本测试错误的结果，则将其变成正确的。

我们现在切换回到 root 管理员身份，再判断当前用户是否为一个非管理员的用户。由于判断结果因为两次否定而变成正确，因此会正常地输出预设信息：
```shell
linuxprobe@linuxprobe:~$ exit
root@linuxprobe:~# [ ! $USER = root ] || echo "administrator"
administrator
```
叹号应该放到判断语句的前面，代表对整个逻辑测试语句进行取相反值操作，而不应该写成“$USER != root”，因为“!=”代表的是不等于符号（≠），尽管执行效果一样，但缺少了逻辑关系，这一点还请多加注意。

Tips ：

&&是逻辑“与”，只有当前面的语句执行成功时才会执行后面的语句。

||是逻辑“或”，只有当前面的语句执行失败时才会执行后面的语句。

!是逻辑“非”，代表对逻辑测试结果取反值；之前若为正确则变成错误，若为错误则变成正确。

就技术图书的写作来讲，一般有两种套路：让读者真正搞懂技术了；让读者觉得自己搞懂技术了。因此市面上很多浅显的图书会让读者在学完之后感觉进步很快，这基本上是作者有意为之，目的就是让你觉得“图书很有料，自己收获很大”，但是在步入工作岗位后就露出短板吃大亏。所以刘遄老师决定继续提高难度，为读者增加一个综合的示例，一方面作为前述知识的总结，另一方面帮助读者夯实基础，以便在今后的工作中更灵活地使用逻辑运算符。

当前我们是使用 root 管理员身份登录的。下面这个示例的执行顺序是，先判断当前登录用户的 USER 变量名称是否等于root，然后用逻辑“非”运算符进行取反操作，效果就变成判断当前登录的用户是否为非管理员用户。最后若条件成立，则会根据逻辑“与”运算符输出 user 字样；若条件不满足，则会通过逻辑“或”运算符输出 root 字样。
```shell
root@linuxprobe:~# [ ! $USER = root ] && echo "user" || echo "root"
root
```
在这个 Shell 脚本的逻辑运算中，&&和||遵循短路求值规则，仅当前面&&左侧条件为假时，才会执行后面的||运算符。

### 整数值比较语句
整数值比较语句是 Shell 脚本中用于判断两个整数值之间关系的语句，通过特定的比较运算符来实现数值大小、相等与否的判断，从而根据判断结果执行不同的操作逻辑。在此基础上，需要明确：整数比较运算符仅适用于对数字的操作，不能将数字与字符串、文件等内容一起操作， 而且不能想当然地使用日常生活中的等号、大于号、小于号等来判断。等号与赋值命令符冲突， 大于号和小于号分别与输出重定向命令符和输入重定向命令符冲突，因此一定要使用规范的整数比较运算符来进行操作。在整数值比较语句中，可用的整数比较运算符如表 4-4 所示。

**可用的整数比较运算符**
| 运算符 |      作用      |
| :----: | :------------: |
|  -eq   |    是否等于    |
|  -ne   |   是否不等于   |
|  -gt   |    是否大于    |
|  -lt   |    是否小于    |
|  -le   | 是否等于或小于 |
|  -ge   | 是否大于或等于 |

接下来小试牛刀。先测试一下 10 是否大于 10 以及 10 是否等于 10（通过输出的返回值内容来判断）：
```shell
root@linuxprobe:~# [ 10 -gt 10 ]
root@linuxprobe:~# echo $?
1
root@linuxprobe:~# [ 10 -eq 10 ]
root@linuxprobe:~# echo $?
0
```
在 2.4 节曾经讲过 free 命令，它能够用来获取当前系统正在使用及可用的内存量信息。接下来先使用 free -m 命令查看内存使用量情况（单位为 MB），然后通过 grep Mem:命令过滤出与剩余内存量相关的行，再用 awk '{print $4}'命令只保留第 4 列。这个演示确实有些难度，但看懂后会觉得很有意思，没准在运维工作中也会用得上。
```shell
root@linuxprobe:~# free -m
               total        used        free      shared  buff/cache   available
Mem:            3879        1370        2008          14         746        2509
Swap:           2047           0        2047
root@linuxprobe:~# free -m | grep Mem:
Mem:            3879        1371        2006          14         746        2508
root@linuxprobe:~# free -m | grep Mem: | awk '{print $4}'
2006
```
如果想把这个命令写入 Shell 脚本，那么建议把输出结果赋值给一个变量，以方便其他命令进行调用：
```shell
root@linuxprobe:~# FreeMem=`free -m | grep Mem: | awk '{print $4}'`
root@linuxprobe:~# echo $FreeMem 
2006
```

上面用于获取内存可用量的命令以及步骤可能有些超纲了，如果不能理解领会也不用担心，接下来才是重点。使用整数运算符来判断内存可用量的值是否小于 2024，若小于则会提示“Insufficient Memory”（内存不足）的字样：
```shell
root@linuxprobe:~# [ $FreeMem -lt 2024 ] && echo "Insufficient Memory"
Insufficient Memory
```
### 字符串比较语句
字符串比较语句用于判断测试字符串是否为空值，或两个字符串是否相同。它经常用来判断某个变量是否未被定义（即内容为空值），理解起来也比较简单。字符串比较语句中常见的运算符如表 4-5 所示。

**常见的字符串比较运算符**
| 运算符 |          作用          |
| :----: | :--------------------: |
|   =    | 比较字符串内容是否相同 |
|   !=   | 比较字符串内容是否不同 |
|   -z   | 判断字符串内容是否为空 |

接下来通过判断 String 变量是否为空值，进而判断是否定义了这个变量：
```shell
root@linuxprobe:~# [ -z $String ]
root@linuxprobe:~# echo $?
0
```
再次尝试引入逻辑运算符来试一下。当用于保存当前语系的环境变量值 LANG 不是英语（en.US）时，则会满足逻辑测试条件并输出“Not en.US”（非英语）的字样：
```shell
root@linuxprobe:~# echo $LANG
en_US.UTF-8
root@linuxprobe:~# [ ! $LANG = "en.US" ] && echo "Not en.US"
Not en.US
```
