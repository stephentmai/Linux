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

