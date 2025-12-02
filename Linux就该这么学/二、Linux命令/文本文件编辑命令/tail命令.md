# tail命令

tail 命令用于查看纯文本文件的后 N 行或持续刷新文件的最新内容，语法格式为“tail [参数] 文件名”。

我们可能还会遇到另外一种情况，比如需要查看文本内容的最后 10 行，这时就需要用到 tail 命令了。tail 命令的操作方法与head 命令非常相似，只需要执行“tail -n 10 文件名”命令就可以达到这样的效果：
```shell
root@linuxprobe:~# tail -n 10 anaconda-ks.cfg 
autopart
# Partition clearing information
clearpart --none --initlabel

# System timezone
timezone Asia/Shanghai --utc

# Root password
rootpw --iscrypted --allow-ssh $y$j9T$9ksHS/agdH0v1Da0mwbUp6Kg$G.cHNWJ1EJEaBWJxwahrFRlQkqRs82A0ajf32ZTB.PB
user --groups=wheel --name=linuxprobe --password=$y$j9T$nv/rjZ.6xpHhLADJDvIa3uGH$f623oMdVJdxy/QygxwnuZc0FsAdcj8amW9
```
tail 命令最强悍的功能是能够持续刷新一个文件的内容，当想要实时查看最新的日志文件时，这特别有用，此时的命令格式为“tail -f 文件名”：
```shell
root@linuxprobe:~# tail -f /var/log/messages
May 18 10:28:07 linuxprobe systemd[1]: packagekit.service: Deactivated successfully.
May 18 10:28:43 linuxprobe systemd[1]: Starting packagekit.service - PackageKit Daemon...
May 18 10:28:43 linuxprobe packagekitd[9218]: g_path_get_basename: assertion 'file_name != NULL' failed
May 18 10:28:43 linuxprobe systemd[1]: Started packagekit.service - PackageKit Daemon.
May 18 10:28:43 linuxprobe packagekitd[9218]: Skipping refresh of media: Cannot update read-only repo
May 18 10:29:13 linuxprobe ptyxis[2945]: context mismatch in svga_surface_destroy
May 18 10:29:13 linuxprobe ptyxis[2945]: context mismatch in svga_surface_destroy
May 18 10:29:13 linuxprobe ptyxis[2945]: context mismatch in svga_surface_destroy
```