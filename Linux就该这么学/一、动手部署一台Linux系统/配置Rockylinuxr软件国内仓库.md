# 1、Rocky Linux 10软件仓库基础
## 1·1、软件仓库概述
软件仓库（Repository）是存储软件包及其元数据的服务器。Rocky Linux通过这些仓库获取系统更新和安装新软件。每个仓库都包含大量软件包，以及描述这些软件包之间依赖关系的元数据。
## 1·2、Rocky Linux的主要软件仓库
Rocky Linux包含几个核心软件仓库：
* BaseOS：提供核心软件包，构成Rocky Linux的基础功能
* AppStream：提供额外的应用程序和运行时环境
* Extras：提供额外的软件包，不包括在BaseOS或AppStream中
* EPEL (Extra Packages for Enterprise Linux)：虽非官方仓库，但提供大量额外软件包
# 2、选择合适的镜像源
## 2·1 国内常用镜像源
对于中国大陆用户，以下镜像源通常提供较好的访问速度：
* 阿里云镜像：https://mirrors.aliyun.com/rockylinux/
* 清华大学镜像：https://mirrors.tuna.tsinghua.edu.cn/rocky/
* 华为镜像：https://mirrors.huaweicloud.com/rockylinux/
* 网易镜像：http://mirrors.163.com/rocky/
# 3、配置镜像源的详细步骤
## 3·1备份原有配置
在修改任何配置文件之前，建议先备份原有的配置文件：
```shell
sudo cp /etc/yum.repos.d/Rocky-BaseOS.repo /etc/yum.repos.d/Rocky-BaseOS.repo.bak
sudo cp /etc/yum.repos.d/Rocky-AppStream.repo /etc/yum.repos.d/Rocky-AppStream.repo.bak
sudo cp /etc/yum.repos.d/Rocky-Extras.repo /etc/yum.repos.d/Rocky-Extras.repo.bak
```

## 3·5手动配置阿里云镜像源
执行以下命令替换默认源
```shell 
sudo sed -e 's|^mirrorlist=|#mirrorlist=|g' \
    -e 's|^#baseurl=http://dl.rockylinux.org/$contentdir|baseurl=https://mirrors.aliyun.com/rockylinux|g' \
    -i.bak \
    /etc/yum.repos.d/rocky*.repo

dnf makecache
```

## 3·6 配置EPEL仓库
如果需要使用EPEL仓库，可以按照以下步骤配置：
安装EPEL仓库：
```shell
sudo yum install -y https://mirrors.aliyun.com/epel/epel-release-latest-10.noarch.rpm
```
备份EPEL仓库配置文件：
```shell
mv /etc/yum.repos.d/epel.repo /etc/yum.repos.d/epel.repo.backup
mv /etc/yum.repos.d/epel-testing.repo /etc/yum.repos.d/epel-testing.repo.backup
```
将 repo 配置中的地址替换为阿里云镜像站地址：
```shell
sudo sed -i 's|^#baseurl=https://download.example/pub|baseurl=https://mirrors.aliyun.com|' /etc/yum.repos.d/epel*
sudo sed -i 's|^metalink|#metalink|' /etc/yum.repos.d/epel*
```

保存并退出编辑器。
## 3.7配置 RPM Fusion仓库
免费仓库:
```shell
sudo dnf install --nogpgcheck https://mirrors.rpmfusion.org/free/el/rpmfusion-free-release-10.noarch.rpm -y
```
非免费仓库:
```shell
sudodnf install --nogpgcheck https://mirrors.rpmfusion.org/nonfree/el/rpmfusion-nonfree-release-10.noarch.rpm -y
```
**验证是否安装成功**
```shell
dnf repolist | grep rpmfusion
```
rpmfusion-free-updates    RPM Fusion for EL 10 - Free - Updates
rpmfusion-nonfree-updates RPM Fusion for EL 9 - Nonfree - Updates
**列出 RPM Fusion 中可用的包**
免费包
```shell
dnf repository-packages rpmfusion-free-updates list
```
非免费包
```shell
dnf repository-packages rpmfusion-nonfree-updates list
```
**在 RPM Fusion 中搜索包**
搜索 VirtualBox 相关软件包
```shell
dnf repository-packages rpmfusion-free-updates list | grep -i virtualbox
```
VirtualBox.x86_64                                     6.1.40-1.el9                        rpmfusion-free-updates
VirtualBox-devel.x86_64                               6.1.40-1.el9                        rpmfusion-free-updates
VirtualBox-kmodsrc.noarch                             6.1.40-1.el9                        rpmfusion-free-updates
VirtualBox-server.x86_64                              6.1.40-1.el9                        rpmfusion-free-updates
VirtualBox-webservice.x86_64                          6.1.40-1.el9                        rpmfusion-free-updates
akmod-VirtualBox.x86_64                               6.1.40-1.el9                        rpmfusion-free-updates
kmod-VirtualBox.x86_64                                6.1.40-1.el9                        rpmfusion-free-updates
kmod-VirtualBox-5.14.0-70.el9_0.x86_64                6.1.40-1.el9                        rpmfusion-free-updates
python3-VirtualBox.x86_64                             6.1.40-1.el9                        rpmfusion-free-updates

## 3·8 清理并更新缓存
完成所有配置后，清理旧的缓存并生成新的缓存：
```shell
sudo dnf clean all
sudo dnf makecache
```
## 3·9 测试下载速度
测试下载速度，验证镜像源配置是否生效：
```shell
sudo dnf update --downloadonly
```
这个命令只会下载更新包，不会实际安装，可以用来测试下载速度。

# 4. 常见问题和解决方案
## 4.1 无法连接到镜像源
如果无法连接到镜像源，可能是网络问题或镜像源暂时不可用。可以尝试以下解决方案：
* 检查网络连接是否正常
* 尝试使用其他镜像源
* 检查防火墙和代理设置
## 4.2 GPG密钥验证失败
如果遇到GPG密钥验证失败的问题，可以尝试导入正确的GPG密钥：
```shell
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-rockyofficial
```
对于EPEL仓库：
```shell
sudo rpm --import /etc/pki/rpm-gpg/RPM-GPG-KEY-EPEL-10
```
## 4.3监控镜像源状态
定期检查所使用的镜像源状态，确保它们正常运行。可以使用以下命令检查仓库状态：
```shell
sudo dnf repolist -v
```