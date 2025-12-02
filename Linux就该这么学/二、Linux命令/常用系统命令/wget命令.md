# wget命令
wget 命令用于在终端命令行中下载网络文件，英文全称为 World Wide Web get，语法格式为“wget [参数] 网址”。

借助于 wget 命令，可以无须打开浏览器，直接在命令行界面中就能下载文件。如果你没有 Linux 系统的管理经验，当前只需了解一下 wget 命令的参数以及作用，然后看一眼下面的演示实验就够了，切记不要急于求成。后面章节将逐步讲解Linux 系统的配置管理方法， 可以在掌握了网卡的配置方法后再来进行这个实验。表 2-6 所示为wget 命令中的参数以及参数的作用。

# wget 命令中的参数以及作用
| 参数  |                   作用                   |
| :---: | :--------------------------------------: |
|  -b   |               后台下载模式               |
|  -P   |              下载到指定目录              |
| 参数  |                   作用                   |
|  -t   |               最大尝试次数               |
|  -c   |                 断点续传                 |
|  -p   | 下载页面所需的资源（如 CSS、JS、图片等） |

Tips：
由于本实验需要从外部网络下载文件，但虚拟机默认是无法连接外网的，在操作后会提示响应超时的报错，因此可先不进行实操。

尝试使用 wget 命令从本书的配套站点中下载本书最新的PDF 格式的电子文档。执行该命令后的下载效果如下：

```shell
root@linuxprobe:~# wget https://www.linuxprobe.com/docs/LinuxProbe.pdf
--2025-05-18 09:36:47--  https://www.linuxprobe.com/docs/LinuxProbe.pdf

Resolving www.linuxprobe.com (www.linuxprobe.com)... 117.72.45.242
Connecting to www.linuxprobe.com (www.linuxprobe.com)|117.72.45.242|:443... connected.
HTTP request sent, awaiting response... 200 OK
Length: 23335661 (22M) [application/pdf]
Saving to: ‘LinuxProbe.pdf’

LinuxProbe.pdf      100%[===================>]  22.25M  1.54MB/s    in 14s     

2025-05-18 09:37:02 (1.54 MB/s) - ‘LinuxProbe.pdf’ saved [23335661/23335661]
```
接下来，使用 wget 命令递归下载 www.linuxprobe.com 网站内的所有页面数据以及文件， 下载完后会自动保存到当前路径下一个名为 www.linuxprobe.com 的目录中。该命令的执行结果如下：
