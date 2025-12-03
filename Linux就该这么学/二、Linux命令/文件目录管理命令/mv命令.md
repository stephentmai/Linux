# mv命令

mv 命令用于剪切或重命名文件，英文全称为 move，语法格式为“mv [参数] 源文件名目标文件名”。

剪切操作不同于复制操作，因为它默认会把源文件删除，只保留剪切后的文件。如果在同一个目录中将某个文件剪切后还粘贴到当前目录下，其实也就是对该文件进行了重命名操作：
```shell
root@linuxprobe:~# mv x.log linux.log
root@linuxprobe:~# ls
install.log linux.log
```