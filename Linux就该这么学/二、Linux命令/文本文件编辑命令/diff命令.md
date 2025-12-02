# diff命令

diff 命令用于比较多个文件之间内容的差异，英文全称为 difference，语法格式为“diff [参数] 文件名 A 文件名 B”。

在使用 diff 命令时，不仅可以使用--brief 参数确认两个文件是否相同，还可以使用-c 参数详细比较多个文件的差异之处。这绝对是判断文件是否被篡改的有力神器。例如，先使用cat 命令分别查看 diff_A.txt 和diff_B.txt 文件的内容，然后进行比较：
```shell
root@linuxprobe:~# cat diff_A.txt
Welcome to linuxprobe.com
Red Hat certified
Free Linux Lessons
Professional guidance
Linux Course
root@linuxprobe:~# cat diff_B.txt
Welcome tooo linuxprobe.com

Red Hat certified
Free Linux LeSSonS
////////.....////////
Professional guidance
Linux Course
```
接下来使用 diff --brief 命令显示比较后的结果，判断文件是否相同：
```shell
root@linuxprobe:~# diff --brief diff_A.txt diff_B.txt
Files diff_A.txt and diff_B.txt differ
```
最后使用带有-c 参数的 diff 命令来描述文件内容具体的不同：
```shell
root@linuxprobe:~# diff -c diff_A.txt diff_B.txt
*** diff_A.txt	2025-05-18 10:34:30.829180338 +0800
--- diff_B.txt	2025-05-18 10:34:40.334180802 +0800
***************
*** 1,5 ****
! Welcome to linuxprobe.com
  Red Hat certified
! Free Linux Lessons
  Professional guidance
  Linux Course
--- 1,7 ----
! Welcome tooo linuxprobe.com
! 
  Red Hat certified
! Free Linux LeSSonS
! ////////.....////////
  Professional guidance
  Linux Course
```