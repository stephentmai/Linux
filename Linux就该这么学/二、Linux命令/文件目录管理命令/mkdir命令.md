# mkdir命令

mkdir 命令用于创建空白的目录，英文全称为 make directory ，语法格式为“mkdir [参数] 目录名称”。

除了能创建单个空白目录外，mkdir 命令还可以结合-p 参数递归创建出具有嵌套层叠关系的文件目录：
```shell
root@linuxprobe:~# mkdir linuxprobe
root@linuxprobe:~# cd linuxprobe
root@linuxprobe:~/linuxprobe# mkdir -p a/b/c/d/e
root@linuxprobe:~/linuxprobe# cd a
root@linuxprobe:~/linuxprobe/a# cd b
root@linuxprobe:~/linuxprobe/a/b#
```