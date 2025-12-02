# kill命令
kill 命令用于终止某个指定 PID 的服务进程，语法格式为“kill [参数] 进程的 PID”。

接下来，使用 kill 命令把上面用 pidof 命令查询到的 PID 所代表的进程终止掉，其命令如下所示。这种操作的效果等同于强制停止sshd 服务。
```shell
root@linuxprobe:~# kill 1340
```
但有时系统会提示进程无法被终止，此时可以添加参数-9（SIGKILL 信号），此信号为系统中强制终止进程的最高级别命令，进程无法抗拒：
```shell
root@linuxprobe:~# kill -9 1340
```