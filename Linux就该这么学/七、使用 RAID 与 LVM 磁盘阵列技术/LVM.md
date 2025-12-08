# LVM
前面学习的磁盘设备管理技术虽然能够有效地提高磁盘设备的 IO 速率以及数据的安全性，但是在磁盘分好区或者部署为 RAID 磁盘阵列之后，再想修改磁盘分区大小就不容易了。换句话说，当用户想要随着实际需求的变化调整磁盘分区的大小时，会受到磁盘“灵活性” 的限制。这时就需要用到另外一项非常普及的磁盘设备资源管理技术了—逻辑卷管理器（Logical Volume Manager，LVM）。LVM 允许用户对磁盘资源进行动态调整。

LVM 是Linux 系统用于对磁盘分区进行管理的一种机制，理论性较强，其创建初衷是解决磁盘设备在创建分区后不易修改分区大小的缺陷。尽管对传统的磁盘分区进行强制扩容或缩容从理论上来讲是可行的，但是却可能造成数据的丢失。而 LVM 技术是在磁盘分区和文件系统之间添加了一个逻辑层，它提供了一个抽象的卷组，可以把多块磁盘进行卷组合并。这样一来，用户不必关心物理磁盘设备的底层架构和布局，就能够实现对磁盘分区的动态调整。LVM 的技术架构如图 7-8 所示。

![](img/图2025-12-09-00-20-30.png)
逻辑卷管理器的技术结构

为了帮助大家理解，我们来看一个“吃货”的例子。比如小明家想吃馒头，但是面粉不够了，于是妈妈从隔壁老王家、老李家、老张家分别借来一些面粉，准备蒸馒头吃。首先需要把这些面粉（物理卷[Physical Volume，PV]）揉成一个大面团（卷组[Volume Group]，VG），然后再把这个大面团分割成一个个小馒头（逻辑卷[Logical Volume，LV]），而且每个小馒头的重量必须是每勺面粉（基本单元[Physical Extent，PE]）的倍数。

在日常的使用中，如果卷组（VG）的剩余容量不足，可以随时将新的物理卷（PV）加入到里面，不断地扩容。由于担心同学们还是不理解，这里准备了一张逻辑卷管理器的使用流程示意图，如图 7-9 所示。

![](img/图2025-12-09-00-20-49.png)
逻辑卷管理器使用流程图

物理卷处于 LVM 中的最底层，可以将其理解为物理磁盘、磁盘分区或者 RAID 磁盘阵列。卷组建立在物理卷之上，一个卷组能够包含多个物理卷，而且在卷组创建之后也可以继续向其中添加新的物理卷。逻辑卷是用卷组中空闲的资源建立的，并且逻辑卷在建立后能够动态地扩展或缩小空间。这就是 LVM 的核心理念。

## 部署逻辑卷
一般而言，在生产环境中，很难在初始阶段就能精确预估每个磁盘分区在后续的使用需求，因此经常会出现分区容量不够使用的情况。比如，伴随着业务量的增加，用于存放交易记录的数据库目录的体积也随之增加；因为分析并记录用户的行为从而导致日志目录的体积不断变大，这些都会导致原有的磁盘分区在使用上捉襟见肘。此外，还存在对较大的磁盘分区进行精简缩容的情况。

我们可以通过部署 LVM 来解决上述问题。在部署时，需要逐个配置物理卷、卷组和逻辑卷，常用的部署命令如表 7-3 所示。

**常用的 LVM 部署命令**
| 功能/命令 | 物理卷管理 | 卷组管理  | 逻辑卷管理 |
| :-------: | :--------: | :-------: | :--------: |
|   扫描    |   pvscan   |  vgscan   |   lvscan   |
|   建立    |  pvcreate  | vgcreate  |  lvcreate  |
|   显示    | pvdisplay  | vgdisplay | lvdisplay  |
|   删除    |  pvremove  | vgremove  |  lvremove  |
|   扩展    |            | vgextend  |  lvextend  |
|   缩小    |            | vgreduce  |  lvreduce  |

为了避免多个实验之间相互发生冲突，请大家自行将虚拟机还原到初始状态，并重新添加两块新磁盘设备，如图 7-10 所示。然后开机。

![](img/图2025-12-09-01-06-28.png)
在虚拟机中添加两块新的硬盘设备

在虚拟机中添加两块新磁盘设备，是为了更好地演示 LVM 理念中用户无须关心底层物理磁盘设备的特性。我们先对这两块新磁盘进行创建物理卷的操作，可以将该操作简单理解为让磁盘设备支持 LVM 技术，或者理解为将磁盘设备加入 LVM 能够管理和使用的硬件资源池内，然后对这两块磁盘进行卷组合并，卷组的名称允许用户自定义。接下来，根据需求把合并后的卷组切割出一个约为 150MB 的逻辑卷设备，最后把这个逻辑卷设备格式化为 Ext4 文件系统后挂载使用。下文将对每一个步骤进行简单的描述。

第 1 步：让新添加的两块磁盘设备支持 LVM 技术。
```shell
root@linuxprobe:~# pvcreate /dev/sdb /dev/sdc
  Physical volume "/dev/sdb" successfully created.
  Physical volume "/dev/sdc" successfully created.
```
第 2 步：把两块磁盘设备加入 storage 卷组，然后查看卷组的状态。
```shell
root@linuxprobe:~# vgcreate storage /dev/sdb /dev/sdc
  Volume group "storage" successfully created
root@linuxprobe:~# vgdisplay
  --- Volume group ---
  VG Name               storage
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  1
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                0
  Open LV               0
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               39.99 GiB
  PE Size               4.00 MiB
  Total PE              10238
  Alloc PE / Size       0 / 0   
  Free  PE / Size       10238 / 39.99 GiB
  VG UUID               M9xB4b-sydm-G6tZ-K00e-RO45-iqL2-4bRlw1
………………省略部分输出信息………………
```
第 3 步：切割出一个约为 150MB 的逻辑卷设备。

这里需要注意切割单位的问题。在对逻辑卷进行切割时有两种计量单位。第一种是以容量为单位，使用的参数为-L。例如，使用-L 150M 生成一个大小为 150MB 的逻辑卷。另外一种是以基本单元的个数为单位，使用的参数为-l。每个基本单元的大小默认为 4MB。例如， 使用-l 37 可以生成一个大小为 37×4MB=148MB 的逻辑卷。
```shell
root@linuxprobe:~# lvcreate -n vo -l 37 storage
  Logical volume "vo" created.
root@linuxprobe:~# lvdisplay
  --- Logical volume ---
  LV Path                /dev/storage/vo
  LV Name                vo
  VG Name                storage
  LV UUID                9Tuv9a-FjmN-Cbye-CqC6-709V-zJMh-MQ9iIr
  LV Write Access        read/write
  LV Creation host, time linuxprobe.com, 2025-03-18 01:15:48 +0800
  LV Status              available
  # open                 0
  LV Size                148.00 MiB
  Current LE             37
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     8192
  Block device           253:2
………………省略部分输出信息………………
```
第 4 步：把生成的逻辑卷进行格式化，然后挂载使用。

Linux 系统会把 LVM 中的逻辑卷设备存放在/dev 设备目录中（实际上就是个快捷方式），同时会以卷组的名称来建立一个目录，其中保存了逻辑卷的设备映射文件（即/dev/卷组名称/逻辑卷名称）。
```shell
root@linuxprobe:~# mkfs.ext4 /dev/storage/vo 
mke2fs 1.47.1 (20-May-2024)
Creating filesystem with 151552 1k blocks and 37848 inodes
Filesystem UUID: c5b6efaf-0b25-4ad2-81ac-afb8bc0c99a9
Superblock backups stored on blocks: 
	8193, 24577, 40961, 57345, 73729

Allocating group tables: done                            
Writing inode tables: done                            
Creating journal (4096 blocks): done
Writing superblocks and filesystem accounting information: done 

root@linuxprobe:~# mkdir /linuxprobe
root@linuxprobe:~# mount /dev/storage/vo /linuxprobe
```
对了，如果使用了逻辑卷管理器，则不建议使用 XFS 文件系统，因为 XFS 文件系统自身就可以使用 xfs_growfs 命令进行磁盘扩容。这虽然没有 LVM 灵活，但起码也够用。我们在实测阶段发现，在有一些服务器上，XFS 与 LVM 的兼容性并不好。

第 5 步：查看挂载状态，并写入配置文件，使其永久生效。
```shell
root@linuxprobe:~# df -h
Filesystem              Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root    17G  3.7G   13G  23% /
devtmpfs                4.0M     0  4.0M   0% /dev
tmpfs                   1.9G   84K  1.9G   1% /dev/shm
efivarfs                256K   56K  196K  23% /sys/firmware/efi/efivars
tmpfs                   776M  9.7M  767M   2% /run
tmpfs                   1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
/dev/sda2               960M  272M  689M  29% /boot
/dev/sr0                6.5G  6.5G     0 100% /media/cdrom
/dev/sda1               599M  8.3M  591M   2% /boot/efi
tmpfs                   388M  124K  388M   1% /run/user/0
/dev/mapper/storage-vo  134M   14K  123M   1% /linuxprobe
root@linuxprobe:~# echo "/dev/storage/vo /linuxprobe ext4 defaults 0 0" >> /etc/fstab
root@linuxprobe:~# cat /etc/fstab
#
# /etc/fstab
# Created by anaconda on Wed Mar 12 19:35:26 2025
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
UUID=408f4a3d-a4d3-4a44-bb23-6988cdbd10bf /                       xfs     defaults        0 0
UUID=4cf8ecae-bcb6-4b1e-8001-968b33643a8a /boot                   xfs     defaults        0 0
UUID=1FB8-9199          /boot/efi                      vfat    umask=0077,shortname=winnt 0 2
UUID=d936c726-45a7-4ca2-8932-c54f84a3d787 none                    swap    defaults        0 0
/dev/cdrom 								  /media/cdrom 			iso9660   defaults		  0 0
/dev/storage/vo 						  /linuxprobe 			  ext4    defaults 		  0 0
```
Tips ：
细心的同学应该又发现了一个小问题：刚才明明写的是148MB，怎么这里只有140MB 了呢？ 这是因为硬件厂商在标识容量时，采用的换算标准是 1GB=1000MB、1MB＝1000KB、1KB＝ 1000B；而计算机系统在识别和计算存储容量时，遵循的是二进制算法，即 1GB= 1024MB、
1MB＝1024KB、1KB＝1024B，因此有 3%左右的“缩水”是正常情况。

## 扩容逻辑卷
在前面的实验中，卷组是由两块磁盘设备共同组成的。用户在使用存储设备时感知不到设备底层的架构和布局，更不用关心底层是由多少块磁盘组成的，只要卷组中有足够的资源， 就可以一直为逻辑卷扩容。扩容前请一定要记得卸载设备和挂载点之间的关联。
```shell
root@linuxprobe:~# umount /linuxprobe
```
第 1 步：把上一个实验中的逻辑卷vo 扩展至 290MB。
```shell
root@linuxprobe:~# lvextend -L 290M /dev/storage/vo
  Rounding size to boundary between physical extents: 292.00 MiB.
  Size of logical volume storage/vo changed from 148.00 MiB (37 extents) to 292.00 MiB (73 extents).
  Logical volume storage/vo successfully resized.
```
第 2 步：检查磁盘的完整性，确认目录结构、内容和文件内容没有丢失。一般情况下没有报错，表示运行正常。
```shell
root@linuxprobe:~# e2fsck -f /dev/storage/vo
e2fsck 1.47.1 (20-May-2024)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/storage/vo: 11/37848 files (0.0% non-contiguous), 15165/151552 blocks
```
第 3 步：重置设备在系统中的容量。刚刚是对 LV（逻辑卷）设备进行了扩容操作，但系统内核还没有同步到这部分新修改的信息，需要手动进行同步。****
```shell
root@linuxprobe:~# resize2fs /dev/storage/vo
resize2fs 1.47.1 (20-May-2024)
Resizing the filesystem on /dev/storage/vo to 299008 (1k) blocks.
The filesystem on /dev/storage/vo is now 299008 (1k) blocks long.
```
第 4 步：重新挂载磁盘设备并查看挂载状态。
```shell
root@linuxprobe:~# systemctl daemon-reload
root@linuxprobe:~# mount -a
root@linuxprobe:~# df -h
Filesystem              Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root    17G  3.7G   13G  23% /
devtmpfs                4.0M     0  4.0M   0% /dev
tmpfs                   1.9G   84K  1.9G   1% /dev/shm
efivarfs                256K   56K  196K  23% /sys/firmware/efi/efivars
tmpfs                   776M  9.7M  767M   2% /run
tmpfs                   1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
/dev/sda2               960M  272M  689M  29% /boot
/dev/sr0                6.5G  6.5G     0 100% /media/cdrom
/dev/sda1               599M  8.3M  591M   2% /boot/efi
tmpfs                   388M  124K  388M   1% /run/user/0
/dev/mapper/storage-vo  268M   14K  250M   1% /linuxprobe
```
## 缩小逻辑卷
相较于扩容逻辑卷，在对逻辑卷进行缩容操作时，数据丢失的风险更大。所以在生产环境中执行相应操作时，一定要提前备份好数据。另外，Linux 系统规定，在对 LVM 逻辑卷进行缩容操作之前，要先检查文件系统的完整性（当然这也是为了保证数据的安全）。在执行缩容操作前记得先把文件系统卸载掉。
```shell
root@linuxprobe:~# umount /linuxprobe
```
第 1 步：检查文件系统的完整性。
```shell
root@linuxprobe:~# e2fsck -f /dev/storage/vo
e2fsck 1.47.1 (20-May-2024)
Pass 1: Checking inodes, blocks, and sizes
Pass 2: Checking directory structure
Pass 3: Checking directory connectivity
Pass 4: Checking reference counts
Pass 5: Checking group summary information
/dev/storage/vo: 11/73704 files (0.0% non-contiguous), 24683/299008 blocks
```
第 2 步：通知系统内核将逻辑卷 vo 的容量减小到 120MB。
```shell
root@linuxprobe:~# resize2fs /dev/storage/vo 120M
resize2fs 1.47.1 (20-May-2024)
Resizing the filesystem on /dev/storage/vo to 122880 (1k) blocks.
The filesystem on /dev/storage/vo is now 122880 (1k) blocks long.
```
咦？缩容的步骤跟扩容的步骤不一样啊。缩容操作为什么是先通知系统内核设备的容量要改变成 120MB，然后再正式进行缩容操作呢？举个例子大家就明白了。小强是一名初中生，开学后看到班里有位同学文了身，他感觉很酷，自己也想文身但又怕被家里责骂，于是他回家后就说：“妈妈，我文身了。”如果妈妈的反应很平和，那么他就可以放心大胆地去文身了。如果妈妈强烈不同意，他马上就哈哈一笑：“逗你玩呢。”这样他就不会挨打了。

缩容操作也是同样的道理，先通知系统内核自己想缩小逻辑卷，如果在执行 resize2fs

命令后系统没有报错，再正式操作。

第 3 步：将 LV 逻辑卷的容量修改为 120MB。
```shell
root@linuxprobe:~# lvreduce -L 120M /dev/storage/vo
  File system ext4 found on storage/vo.
  File system size (120.00 MiB) is equal to the requested size (120.00 MiB).
  File system reduce is not needed, skipping.
  Size of logical volume storage/vo changed from 292.00 MiB (73 extents) to 120.00 MiB (30 extents).
  Logical volume storage/vo successfully resized.
```
使用lvreduce 命令正式缩小文件系统容量时，大小一定要跟之前申报的保持一致，说好的是 120MB，差 1MB、1KB，甚至差 1B 都不行。小明好不容易说服妈妈同意纹一只小猫咪了，结果纹了只下山虎，那最后还是免不了挨一顿打（缩容失败）。

第 4 步：重新挂载文件系统并查看系统状态。
```shell
root@linuxprobe:~# mount -a
root@linuxprobe:~# df -h
Filesystem              Size  Used Avail Use% Mounted on
/dev/mapper/rhel-root    17G  3.7G   13G  23% /
devtmpfs                4.0M     0  4.0M   0% /dev
tmpfs                   1.9G   84K  1.9G   1% /dev/shm
efivarfs                256K   56K  196K  23% /sys/firmware/efi/efivars
tmpfs                   776M  9.7M  767M   2% /run
tmpfs                   1.0M     0  1.0M   0% /run/credentials/systemd-journald.service
/dev/sda2               960M  272M  689M  29% /boot
/dev/sr0                6.5G  6.5G     0 100% /media/cdrom
/dev/sda1               599M  8.3M  591M   2% /boot/efi
tmpfs                   388M  124K  388M   1% /run/user/0
/dev/mapper/storage-vo  108M   14K   99M   1% /linuxprobe
```
## 逻辑卷快照
LVM 还具备有“逻辑卷快照”功能，该功能类似于虚拟机软件的还原时间点功能。例如， 对某一个逻辑卷设备做一次快照，如果日后发现数据被改错了，就可以利用之前做好的逻辑卷快照进行覆盖还原。LVM 的逻辑卷快照功能有两个特点：

逻辑卷快照的容量必须等同于逻辑卷的容量；

逻辑卷快照仅一次有效，一旦执行还原操作后则会被立即自动删除。

在正式操作前，先看看VG（卷组）中的容量是否够用：
```shell
root@linuxprobe:~# vgdisplay
  --- Volume group ---
  VG Name               storage
  System ID             
  Format                lvm2
  Metadata Areas        2
  Metadata Sequence No  4
  VG Access             read/write
  VG Status             resizable
  MAX LV                0
  Cur LV                1
  Open LV               1
  Max PV                0
  Cur PV                2
  Act PV                2
  VG Size               39.99 GiB
  PE Size               4.00 MiB
  Total PE              10238
  Alloc PE / Size       30 / 120.00 MiB
  Free  PE / Size       10208 / <39.88 GiB
  VG UUID               M9xB4b-sydm-G6tZ-K00e-RO45-iqL2-4bRlw1
………………省略部分输出信息………………
```
通过卷组的输出信息可以清晰看到，卷组中已经使用了 120MB 的容量，空闲容量还有39.88GB。接下来用重定向往逻辑卷设备挂载的目录中写入一个文件。
```shell
root@linuxprobe:~# echo "Welcome to Linuxprobe.com" > /linuxprobe/readme.txt
root@linuxprobe:~# ls -l /linuxprobe
total 13
drwx------. 2 root root 12288 Mar 18 01:16 lost+found
-rw-r--r--. 1 root root    26 Mar 18 01:43 readme.txt
```
第 1 步：使用-s 参数生成一个逻辑卷快照，使用-L 参数指定切割的大小，需要与要做快照的设备容量保持一致。另外，还需要在命令后面写上是针对哪个逻辑卷执行的快照操作， 稍后数据也会还原到这个相应的设备上。
```shell
root@linuxprobe:~# lvcreate -L 120M -s -n SNAP /dev/storage/vo
 Logical volume "SNAP" created.
root@linuxprobe:~# lvdisplay
  --- Logical volume ---
  LV Path                /dev/storage/SNAP
  LV Name                SNAP
  VG Name                storage
  LV UUID                XsCyvN-NiXP-M6tD-CfXk-TCQU-PSbQ-eYHaV2
  LV Write Access        read/write
  LV Creation host, time linuxprobe.com, 2025-03-18 01:43:57 +0800
  LV snapshot status     active destination for vo
  LV Status              available
  # open                 0
  LV Size                120.00 MiB
  Current LE             30
  COW-table size         120.00 MiB
  COW-table LE           30
  Allocated to snapshot  0.01%
  Snapshot chunk size    4.00 KiB
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:5
………………省略部分输出信息………………
```
第 2 步：在逻辑卷挂载的目录中创建一个 100MB 的垃圾文件，然后再查看逻辑卷快照的状态。可以发现存储空间的占用量上升了。
```shell
root@linuxprobe:~# dd if=/dev/zero of=/linuxprobe/files count=1 bs=100M
1+0 records in
1+0 records out
104857600 bytes (105 MB, 100 MiB) copied, 0.461243 s, 227 MB/s
root@linuxprobe:~# lvdisplay
  --- Logical volume ---
  LV Path                /dev/storage/SNAP
  LV Name                SNAP
  VG Name                storage
  LV UUID                XsCyvN-NiXP-M6tD-CfXk-TCQU-PSbQ-eYHaV2
  LV Write Access        read/write
  LV Creation host, time linuxprobe.com, 2025-03-18 01:43:57 +0800
  LV snapshot status     active destination for vo
  LV Status              available
  # open                 0
  LV Size                120.00 MiB
  Current LE             30
  COW-table size         120.00 MiB
  COW-table LE           30
  Allocated to snapshot  67.28%
  Snapshot chunk size    4.00 KiB
  Segments               1
  Allocation             inherit
  Read ahead sectors     auto
  - currently set to     256
  Block device           253:5
………………省略部分输出信息………………
```
第 3 步：为了校验逻辑卷快照的效果，需要对逻辑卷进行快照还原操作。在此之前记得先卸载掉逻辑卷设备与目录的挂载。

lvconvert 命令用于管理逻辑卷快照，语法格式为“lvconvert [参数] 逻辑卷快照名称”。使用lvconvert 命令能自动恢复逻辑卷快照，在早期的 RHEL/CentOS 5 版本中要写全

格式：--mergesnapshot，而从 RHEL 6 一直到 RHEL 10，已经允许用户只输入--merge

参数进行操作了，系统会自动分辨设备的类型。
```shell
root@linuxprobe:~# umount /linuxprobe
root@linuxprobe:~# lvconvert --merge /dev/storage/SNAP
  Merging of volume storage/SNAP started.
  storage/vo: Merged: 100.00%
```
第 4 步：逻辑卷快照会被自动删除掉，并且刚才在对逻辑卷设备执行快照操作后创建出来的 100MB 的垃圾文件也被清除了。
```shell
root@linuxprobe:~# mount -a
root@linuxprobe:~# cd /linuxprobe
root@linuxprobe:/linuxprobe# ls
lost+found  readme.txt
root@linuxprobe:/linuxprobe# cat readme.txt 
Welcome to Linuxprobe.com
```
## 删除逻辑卷
当生产环境中想要重新部署 LVM 或者不再需要使用 LVM 时，则需要执行 LVM 的删除操作。为此，需要提前备份好重要的数据信息，然后依次删除逻辑卷、卷组、物理卷设备， 这个顺序不可颠倒。

第 1 步：取消逻辑卷与目录的挂载关联，删除配置文件中永久生效的设备参数。
```shell
root@linuxprobe:/linuxprobe# cd ~
root@linuxprobe:~# umount /linuxprobe
root@linuxprobe:~# vim /etc/fstab
#
# /etc/fstab
# Created by anaconda on Tue Jul 21 05:03:40 2020
#
# Accessible filesystems, by reference, are maintained under '/dev/disk/'.
# See man pages fstab(5), findfs(8), mount(8) and/or blkid(8) for more info.
#
# After editing this file, run 'systemctl daemon-reload' to update systemd
# units generated from this file.
#
/dev/mapper/rhel-root                       /                       xfs      defaults        0 0
UUID=2db66eb4-d9c1-4522-8fab-ac074cd3ea0b   /boot                   xfs      defaults        0 0
/dev/mapper/rhel-swap                       swap                    swap     defaults        0 0
/dev/cdrom                                  /media/cdrom            iso9660  defaults        0 0 
```
第 2 步：删除逻辑卷设备，需要输入 y 来确认操作。
```shell
root@linuxprobe:~# lvremove /dev/storage/vo 
Do you really want to remove active logical volume storage/vo? [y/n]: y     
  Logical volume "vo" successfully removed.
```
第 3 步：删除卷组，此处只写卷组名称即可，不需要设备的绝对路径。
```shell
root@linuxprobe:~# vgremove storage
  Volume group "storage" successfully removed
```
第 4 步：删除物理卷设备。
```shell
root@linuxprobe:~# pvremove /dev/sdb /dev/sdc
  Labels on physical volume "/dev/sdb" successfully wiped.
  Labels on physical volume "/dev/sdc" successfully wiped.
```
在上述操作执行完毕之后，再执行 lvdisplay、vgdisplay、pvdisplay 命令查看LVM 的信息时，就不会再看到相关信息了（前提是上述步骤的操作是正确的）。干净利落！