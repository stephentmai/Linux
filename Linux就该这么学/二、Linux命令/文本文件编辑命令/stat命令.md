# stat命令

stat 命令用于查看文件的具体存储细节和时间等信息，英文全称为status，语法格式为“stat 文件名”。

大家都知道，文件有一个修改时间。其实，Linux 系统中的文件包含 3 种时间状态，分别是Access Time（内容最后一次被访问的时间，简称为 Atime），Modify Time（内容最后一次被修改的时间，简称为 Mtime）以及 Change Time（文件属性最后一次被修改的时间，简称为 Ctime）。

下面使用 stat 命令查看文件的这 3 种时间状态信息：
```shell
root@linuxprobe:~# stat anaconda-ks.cfg
  File: anaconda-ks.cfg
  Size: 1019      	Blocks: 8          IO Block: 4096   regular file
Device: 253,0	Inode: 33826925    Links: 1
Access: (0600/-rw-------)  Uid: (    0/    root)   Gid: (    0/    root)
Context: system_u:object_r:admin_home_t:s0
Access: 2025-05-18 10:05:30.651095368 +0800
Modify: 2025-03-08 20:06:44.282103236 +0800
Change: 2025-03-08 20:06:44.282103236 +0800
  Birth: 2025-03-08 20:06:44.235103234 +0800
```