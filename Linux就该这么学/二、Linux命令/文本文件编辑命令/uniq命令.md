# uniq命令

uniq 命令用于去除文本中连续的重复行，英文全称为unique，语法格式为“uniq [参数] 文件名”。

由 uniq 命令的英文全称 unique（独特的，唯一的）可知，该命令的作用是去除文本文件中连续的重复行，中间不能夹杂其他文本行（非相邻的默认不会去重）—去除了重复的，保留的都是唯一的，自然也就是“独特的”“唯一的”了。

我们使用 uniq 命令对两个文本内容进行操作，区别一目了然：
```shell
root@linuxprobe:~# cat uniq.txt 
Welcome to linuxprobe.com
Welcome to linuxprobe.com
Welcome to linuxprobe.com
Welcome to linuxprobe.com
Red Hat certified
Free Linux Lessons
Professional guidance
Linux Course
root@linuxprobe:~# uniq uniq.txt 
Welcome to linuxprobe.com
Red Hat certified
Free Linux Lessons
Professional guidance
Linux Course
```