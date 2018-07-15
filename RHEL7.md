# Table of Contents

* [文件系统](#文件系统)
* [用户管理](#用户管理)
* [服务管理](#服务管理)
* [进程管理](#进程管理)
* [网络管理](#网络管理)
* [安全管理](#安全管理)


# 文件系统
linux中具有单个根（/目录）的目录树结构，层次结构可随时扩展，只需添加包含支持的文件系统的新磁盘或分区（存储设备划分为更小的块），即可在文件系统树的任何位置增加磁盘空间。
* 接口：物理存储设备--电子文档、文件夹
#  
    /dev  设备文件
    /boot  开机启动过程所需的文件
    /run   重启时创建，运行时数据
    /usr   安装的软件、共享的库/usr/bin、/usr/sbin、/usr/lib、/usr/lib64
    /etc   特定于此系统的配置文件 /etc/profile、/etc/passwd、/etc/groups、/etc/hosts、/etc/sudoers
    /tmp   供临时文件使用的全局可写空间  >10天内未访问的文件自动删除
    /home  普通用户主目录
    /root  超级用户root的主目录
    /var   系统可变数据（缓存目录、日志文件、网站内容、数据库等） /var/tmp >30天未访问的文件自动删除
    /bin  ->  /usr/bin     > RHEL7中/目录是/usr中对应目录的符号链接
    /sbin ->  /usr/sbin
    /lib  ->  /usr/lib
    /lib64 -> /usr/lib64
## 1 基本概念
*  文件系统驻留在物理磁盘或分区等存储设备上。
*  挂载：将新文件系统添加到现有目录树的过程
*  挂载点：挂载了新文件系统的目录
*  块设备：存储设备由一个特殊文件类型b表示,块设备存储在/dev目录中，RHEL中检测到的第一个SCSI、PATA/SATA或USB硬盘驱动器/dev/sda,第二个/dev/sdb,sda的分区依次是/dev/sda1、/dev/sda2、/dev/sda3...而虚拟机中的硬盘驱动器例外，通常为/dev/vda、/dev/vdb等，如下所示：
# 
    [root@yhost dev]# df
    Filesystem     1K-blocks    Used Available Use% Mounted on
    /dev/vda1       51474024 4709536  44458412  10% /
    [root@yhost ~]# ll /dev/vd*
    brw-rw---- 1 root disk 253, 0 Jun 12 19:04 /dev/vda
    brw-rw---- 1 root disk 253, 1 Jun 12 19:04 /dev/vda1
*  查看目录的磁盘使用情况：df -h\[H\] directory    # -h表示使用2的10次方单位iB（1024），-H使用SI单位，即10的3次方
*  磁盘分区：将硬盘驱动器划分为多个逻辑存储单元
  * MBR（主启动记录）分区方案 32位存储 最多4个主分区，最多共15个分区 单个分区最大2TiB   fdisk命令
  * GPT（GUID分区表）分区方案 64位存储 最多128个分区 单个分区最大8ZiB(B,KB,MB,GB,TB,EB,ZB,YB,BB)   gdisk命令   
*  LVM逻辑卷管理：PV-VG-LV概念

   * fdisk磁盘分区（物理设备之一）--pvcreate创建PV（物理卷-用于注册基础物理设备，LVM自动将PV划分为物理区块PE）--vgcreate创建VG（卷组）--lvcreate创建LV(逻辑卷）--mkfs格式化文件系统--挂载到具体目录--存储使用--扩展LV
   
*  文件链接  
   硬链接  
   软链接  
## 2 文件系统挂载
* 两种挂载方式：
  * mount  \[要挂载的文件系统（块设备）\]  \[文件系统在挂载后的目标目录\]  
######
    mount /dev/vda1  /
  * mount UUID \[目标目录\]  UUID 文件系统的通用唯一识别符 
######
    mount UUID="49f819fd-e56d-48a4-86d3-7ebe0a68ec88"  /      
    blkid命令：简要列出其上具有文件系统的现有分区和文件系统的UUID，以及用于格式化该分区的文件系统  
    [root@yhost ~]# blkid /dev/vda1
    /dev/vda1: UUID="49f819fd-e56d-48a4-86d3-7ebe0a68ec88" TYPE="ext3"

   注意：挂载到现有目录时，请先备份目录中已存在的文件，否则挂载后之前的文件不可访问 
    
  * 手动挂载  -临时挂载
  * 自动挂载  -永久挂载 /etc/fstab 
* 卸载文件系统  
umount \[目标目录\]  
lsof  \[目标目录\] 卸载前需确认当前目录无进程访问，否则无法卸载 ---解释了为什么平时扩容的时候让退出当前目录  
* 可移动设备的挂载和卸载  
可移动介质在插入后会由图形桌面环境自动挂载，挂载点是/run/media/\<user\>/\<label\>，移除时请先卸载再拔出，否则可能丢失数据。  

## 3 NFS(网络文件系统)挂载

## 4 具有 SMB 的 NFS 挂载

# 用户管理

su 身份验证：对尝试访问的账户密码验证
su以当前环境设置启动no-login shell，而su - 则启动login shell（shell环境是切换后的用户身份环境）
sudo 身份验证：对执行sudo用户自己的密码进行验证


# 服务管理

    yum -y install service
    systemctl status service
    systemctl start service
    systemctl enable service
    systemctl stop service

# 进程管理


# 网络管理


# 安全管理
