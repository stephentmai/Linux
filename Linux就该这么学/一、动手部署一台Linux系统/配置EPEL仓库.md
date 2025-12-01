# 在Rocky Linux上启用EPEL仓库
1.先确认系统版本
cat /etc/rocky-release
2.备份原有配置（必要动作）
mkdir -p /etc/yum.repos.d/backup
mv /etc/yum.repos.d/*.repo /etc/yum.repos.d/backup/
** 设置对应源为清华大学源 **
```
sed -e 's|^metalink=|#metalink=|g' \
    -e 's|^#baseurl=|baseurl=|g' \
    -e 's|https\?://download\.fedoraproject\.org/pub/epel|https://mirrors.tuna.tsinghua.edu.cn/epel|g' \
    -e 's|https\?://download\.example/pub/epel|https://mirrors.tuna.tsinghua.edu.cn/epel|g' \
    -i.bak /etc/yum.repos.d/epel{,-testing}.repo
```
3.建立配置文件
vim /etc/yum.repos.d/Rocky-aliyun.repo
写入内容
```
[BaseOS]name=Rocky Linux $releasever - BaseOS - aliyunbaseurl=https://mirrors.aliyun.com/rocky/$releasever/BaseOS/$basearch/os/gpgcheck=1enabled=1gpgkey=https://download.rockylinux.org/pub/rocky/RPM-GPG-KEY-Rocky-10
[AppStream]name=Rocky Linux $releasever - AppStream aliyunbaseurl=https://mirrors.aliyun.com/rocky/$releasever/AppStream/$basearch/os/gpgcheck=1enabled=1gpgkey=https://download.rockylinux.org/pub/rocky/RPM-GPG-KEY-Rocky-10
[extras]name=Rocky Linux $releasever - Extras - aliyunbaseurl=https://mirrors.aliyun.com/rocky/$releasever/extras/$basearch/os/gpgcheck=1enabled=1gpgkey=https://download.rockylinux.org/pub/rocky/RPM-GPG-KEY-Rocky-10
```
**其中内容网址需自行去官网修改正确**
5.清除旧缓存
dnf clean all
6.生成新缓存
dnf makeeache
7.验证仓库是否生效
dnf repolist enabled
8.配置EPEL源
(1).安装EPEL源配置包
dnf install epel-release
(2).手动创建EPEL配置文件
vim /etc/yum.repos.d/epel-aliyum.repo
```
name=Extra Packages for Enterprise Linux $releasever - $basearch -aliyunbaseurl=https://mirrors.aliyun.com/epel/$releasever/Everything/$basearch/
gpgcheck=1
enabled=1
gpgkey=https://mirrors.aliyun.com/epel/RPM-GPG-KEY-EPEL-$releasever
```
9.重新生成缓存
dnf makecache