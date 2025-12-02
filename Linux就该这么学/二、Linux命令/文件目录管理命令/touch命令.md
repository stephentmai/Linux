# touch命令
touch 命令用于创建空白文件或设置文件的时间，语法格式为“touch [参数] 文件名”。在创建空白的文本文件方面，这个 touch 命令相当简洁，简洁到没有必要铺开来讲。

比如，touch linuxprobe 命令可以创建出一个名为 linuxprobe 的空白文本文件。对touch 命令来讲，有难度的操作主要是体现在设置文件内容的修改时间（Mtime）、文件权限或属性的更改时间（Ctime）与文件的访问时间（Atime）上面。touch 命令的参数及其作用如表 2-18 所示。
** touch 命令的参数及其作用**

| 参数  |           作用            |
| :---: | :-----------------------: |
|  -a   | 仅修改“访问时间”（Atime） |
|  -m   | 仅修改“修改时间”（Mtime） |
|  -d   |  同时修改 Atime 与 Mtime  |
接下来，先使用 ls 命令查看一个文件的修改时间，随后修改这个文件，再查看一下文件的修改时间，看是否发生了变化：
```shell
root@linuxprobe:~# ls -l anaconda-ks.cfg
-rw-------. 1 root root 1019 Mar  8 20:06 anaconda-ks.cfg
root@linuxprobe:~# echo "Visit LinuxProbe.com to learn linux" >> anaconda-ks.cfg
root@linuxprobe:~# ls -l anaconda-ks.cfg
-rw-------. 1 root root 1062 May 18 10:38 anaconda-ks.cfg
```
如果不想让别人知道我们修改了它，那么这时就可以用 touch 命令把修改后的文件时间设置成修改之前的时间（很多黑客就是这样做的呢）：
```shell
root@linuxprobe:~# touch -d "2025-05-18 15:44" anaconda-ks.cfg 
root@linuxprobe:~# ls -l anaconda-ks.cfg 
-rw-------. 1 root root 1062 May 18  2025 anaconda-ks.cfg
```