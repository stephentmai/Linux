# file命令
file 命令用于查看文件的类型，语法格式为“file 文件名”。

在 Linux 系统中，由于文本、目录、设备等所有这些一切都统称为文件，但是它们又不像 Windows 系统那样都有后缀，因此很难通过文件名一眼判断出具体的文件类型，这时就需要使用 file 命令来查看文件类型了。
```shell
root@linuxprobe:~# file anaconda-ks.cfg 
anaconda-ks.cfg: ASCII text
root@linuxprobe:~# file /dev/sda
/dev/sda: block special (8/0)
```
Tips ：
在 Windows 系统中打开文件时，一般是通过用户双击鼠标完成的，系统会自行判断用户双击的文件是什么类型，因此需要有后缀进行区别。而 Linux 系统则是根据用户执行的命令来调用文件，例如执行cat 命令查看文本，执行bash 命令执行脚本等，所以也就不需要强制让用户给文件设置后缀了。