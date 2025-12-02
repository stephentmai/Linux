# sos命令
sos 命令用于收集系统配置及架构信息并输出诊断文档，语法格式为“sos [参数]”。

当 Linux 系统出现故障，需要联系技术支持人员时，大多数时候都要先使用这个命令简单收集系统的运行状态和服务配置信息，以便让技术支持人员能够远程解决一些小问题，抑或让他们能提前了解某些复杂问题。sos 命令的常用参数以及作用如表 2-13 所示。
**sos 命令的常用参数以及作用**
|  参数   |          作用          |
| :-----: | :--------------------: |
| report  |   收集整理并输出报告   |
|  clean  |  混淆报告中的敏感信息  |
|  help   |   显示详细的帮助信息   |
| collect | 同时从多个节点收集报告 |

```shell
root@linuxprobe:~# sos report
sosreport (version 4.8.2)

This command will collect diagnostic and configuration information from
this Red Hat Enterprise Linux system and installed applications.

An archive containing the collected information will be generated in
/var/tmp/sos.4p3s8647 and may be provided to a Red Hat support
representative.

Any information provided to Red Hat will be treated in accordance with
the published support policies at:

        Distribution Website : https://www.redhat.com/
        Commercial Support   : https://access.redhat.com/

The generated archive may contain data considered sensitive and its
content should be reviewed by the originating organization before being
passed to any third party.

No changes will be made to system configuration.

Press ENTER to continue, or CTRL-C to quit.此处敲回车

Optionally, please enter the case id that you are generating this report for []:  此处再敲回车
 Setting up archive ...
 Setting up plugins ...
………………省略部分输出信息………………
Creating compressed archive...

Your sosreport has been generated and saved in:
	/var/tmp/sosreport-linuxprobe-2025-05-18-xqbxbhp.tar.xz

 Size	18.45MiB
 Owner	root
 sha256	16cfa2860460de92daa5923a84e60b4889afe15522fa5c81ee18606e5fd512b0

Please send this file to your support representative.
```
Tips ： 
sos 命令有点像是远程问诊。假如我们今天有点咳嗽发烧不舒服，可以先从网上搜索相关症状的病因，如果仅仅是感冒的话那就多喝水，这就免去了到医院挂号看病的车马劳顿之苦；如果怀疑出了大毛病，再请专业人员进行处理也不迟。