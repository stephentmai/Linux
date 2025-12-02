# locate命令
locate 命令用于按照名称快速搜索文件所对应的位置，语法格式为“locate [参数] 对象”。

使用 find 命令进行全盘搜索虽然更准确，但是效率有点低。如果仅仅是想找一些常见的且又知道大概名称的文件，不如试试 locate 命令。在使用 locate 命令时，先使用 updatedb 命令生成一个索引库文件，这个库文件的名字是/var/lib/plocate/plocate.db，后续在使用locate 命令搜索文件时就是在该库中进行查找操作，速度会快很多。

第一次使用 locate 命令之前，记得先执行 updatedb 命令生成索引数据库，然后再进行查找：
```shell
root@linuxprobe:~# updatedb 
root@linuxprobe:~# ls -l /var/lib/plocate/plocate.db
-rw-r-----. 1 root plocate 2527288 May 18 10:25 /var/lib/plocate/plocate.db
```
使用locate 命令搜索出所有包含 whereis 名称的文件所在的位置：
```shell
root@linuxprobe:~# locate whereis
/usr/bin/whereis
/usr/share/bash-completion/completions/whereis
/usr/share/man/man1/whereis.1.gz
```