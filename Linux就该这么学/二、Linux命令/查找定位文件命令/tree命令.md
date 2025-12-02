# tree命令
tree 命令用于以树状图的形式列出目录内容及结构，输入该命令后按回车键执行即可。

虽然 ls 命令可以很便捷地查看目录内有哪些文件，但无法直观地获取目录内文件的层次结构。比如，假如目录 A 中有个 B，B 中又有个 C，那么 ls 命令就只能看到最外面的 A 目录，显然有些时候这不太够用。tree 命令则能够以树状图的形式列出目录内所有文件的结构。我们来对比一下两者的区别。

使用ls 命令查看目录内的文件：
```shell
root@linuxprobe:~# ls
A                Desktop    Downloads  Pictures  Templates
anaconda-ks.cfg  Documents  Music      Public    Videos
```
使用tree 命令查看目录内的文件以及结构：
```shell
root@linuxprobe:~# tree
.
├── A
│   └── B
│       └── C
├── anaconda-ks.cfg
├── Desktop
├── Documents
├── Downloads
├── Music
├── Pictures
├── Public
├── Templates
└── Videos

12 directories, 1 file
```