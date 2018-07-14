


# 1、文件系统
具有单个根（/目录）的目录树结构，层次结构可随时扩展，只需添加包含支持的文件系统的新磁盘或分区（存储设备划分为更小的块），即可在文件系统树的任何位置增加磁盘空间。
*  文件系统驻留在物理磁盘或分区等存储设备上。
*  挂载：将新文件系统添加到现有目录树的过程
*  挂载点：挂载了新文件系统的目录
*  块设备：存储设备由一个特殊文件类型b表示,块设备存储在/dev目录中，RHEL中检测到的第一个硬盘驱动器/dev/sda,第二个/dev/sdb,sda的分区依次是/dev/sda1、/dev/sda2、/dev/sda3...而虚拟机中的硬盘驱动器例外，通常为/dev/vda、/dev/vdb等，如下所示：
# 
    [root@yhost dev]# df
    Filesystem     1K-blocks    Used Available Use% Mounted on
    /dev/vda1       51474024 4709536  44458412  10% /
    [root@yhost ~]# ll /dev/vd*
    brw-rw---- 1 root disk 253, 0 Jun 12 19:04 /dev/vda
    brw-rw---- 1 root disk 253, 1 Jun 12 19:04 /dev/vda1
*  df -h\[H\]
*  LVM逻辑卷管理：PV-VG-LV概念

# 2、用户和组
