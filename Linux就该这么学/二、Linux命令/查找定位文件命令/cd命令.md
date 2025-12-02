# cd命令
cd 命令用于切换当前的工作路径，英文全称为 change directory，语法格式为“cd [参数] [目录]”。

这个命令应该是最常用的一个 Linux 命令了。可以通过 cd 命令迅速、灵活地切换到不同的工作目录。除了常见的切换目录方式，还可以使用 cd -命令返回到上一次所处的目录，使用 cd ..命令进入上级目录，以及使用 cd ~命令切换到当前用户的家目录，抑或使用cd ~username 命令切换到其他用户的家目录（就像在游戏中使用了“回城”技能一样）。例如，使用下述的 cd 命令切换到/etc 目录中：
```shell
root@linuxprobe:~# cd /etc
```
同样的道理，可使用下述命令切换到/bin目录中：
```shell
root@linuxprobe:/etc# cd /bin
```
此时，要返回上一次的目录（即/etc 目录），可执行如下命令：
```shell
root@linuxprobe:/bin# cd -
/etc
root@linuxprobe:/etc#
```
还可以通过下面的命令快速切换到用户的家目录：
```shell
root@linuxprobe:/etc# cd ~
root@linuxprobe:~#
```
Tips ：
随着切换目录的操作，命令提示符也在发生变化，例如 root@linuxprobe:/etc#就是在告诉我们当前处于/etc 中。