# pidof命令
pidof 命令用于查询某个指定服务进程的 PID，语法格式为“pidof [参数] 服务名称”。

每个进程在其生命周期内的进程号（PID）是唯一的，可以用于区分正在运行的进程。例如，执行如下命令查询本机上 sshd 服务程序的 PID：
```shell
root@linuxprobe:~# pidof sshd
1340
```