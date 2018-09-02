# Table of Contents

* [文件管理](#文件管理)
* [用户管理](#用户管理)
* [服务管理](#服务管理)
* [进程管理](#进程管理)
* [网络管理](#网络管理)
* [日志管理](#日志管理)
* [安全管理](#安全管理)
* [脚本编写](#脚本编写)
* [Linux容器和Docker](#Linux容器和Docker)

# 文件管理
linux中具有单个根（/目录）的目录树结构，层次结构可随时扩展，只需添加包含支持的文件系统的新磁盘或分区（存储设备划分为更小的块），即可在文件系统树的任何位置增加磁盘空间。
* 文件系统：接口，物理存储设备--电子文档、文件夹
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
## 1 文件系统
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

* **fdisk磁盘分区（物理设备之一）--> pvcreate创建PV（物理卷-用于注册基础物理设备，LVM自动将PV划分为物理区块PE） --> vgcreate创建VG（卷组） --> lvcreate创建LV(逻辑卷）--> mkfs格式化文件系统 --> 挂载到具体目录 --> 存储使用 --> 扩展LV**
       
*  文件链接  
   * 文件都有文件名与数据，这在 Linux 上被分成两个部分：用户数据 (user data) 与元数据 (metadata)。用户数据，即文件数据块 (data block)，数据块是记录文件真实内容的地方；而元数据则是文件的附加属性，如文件大小、创建时间、所有者等信息。在 Linux 中，元数据中的 inode 号（inode 是文件元数据的一部分但其并不包含文件名，inode 号即索引节点号）才是文件的唯一标识而非文件名。文件名仅是为了方便人们的记忆和使用，系统或程序通过 inode 号寻找正确的文件数据块。  
   * 硬链接：一个 inode 号对应多个文件名。换言之，硬链接就是同一个文件使用了多个别名，硬链接可由命令 link 或 ln 创建  
   * 软链接：文件用户数据块中存放的内容是另一文件的路径名的指向，则该文件就是软连接。软链接就是一个普通文件，只是数据块内容有点特殊。软链接有着自己的 inode 号以及用户数据块  
## 2 文件系统挂载
* 两种挂载方式：
  * mount  \[要挂载的文件系统（块设备）\]  \[文件系统在挂载后的目标目录\]  
  * mount UUID \[目标目录\]  UUID 文件系统的通用唯一识别符
######
    mount /dev/vda1  /
    
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
* NFS网络文件系统标准协议，RHEL7默认支持NFSv4，v4不可用的情况下自动回退到v3和v2；NFSv4使用TCP协议通信。
* NFS服务器导出共享，NFS客户端将导出的共享挂载到本地挂载点（目录）。

* NFS共享的文件访问权限保护：  
  * NFS服务器指定保护方法（身份验证等），客户端必须使用该方法（证书等）连接到该共享。
  * 安全性方法：none、sys、krb5、krb5i、krb5p等
　* Kerberos安全性：NFS使用nfs-secure服务（nfs-utils软件包的一部分）：协商和管理与服务器之间的通信。
### 通过NFS手动挂载网络存储
* 服务端
# 
    yum -y install nfs-utils 
    systemctl start nfs-secure
    systemctl enable nfs-secure
    mkdir /share >创建共享目录
    vim /etc/exports   >导出共享
    /share  192.168.52.101/24(rw,sync,all_squash,anonuid=1000,anongid=1000)
    
    
* 客户端
# 
    yum -y install nfs-utils
    systemctl start nfs-secure
    systemctl enable nfs-secure
    showmount -e 192.168.52.101  >查看远程共享信息
    mkdir /mounttest  >客户端新建挂载点
    mount serverX:/share /mounttest  或vi /etc/fstab  >客户端挂载NFS
    su - nfsuser
    cd /mounttest
    touch test.txt  >客户端新建文件，服务端查看验证
    
### 通过NFS自动挂载网络存储
* 自动挂载器：autofs服务
* 优点：Autofs与Mount/Umount的不同之处在于，它是一种看守程序。如果它检测到用户正试图访问一个尚未挂接的文件系统，它就会自动检测该文件系统，如果存在，        那么Autofs会自动将其挂接。另一方面，如果它检测到某个已挂接的文件系统在一段时间内没有被使用，那么Autofs会自动将其卸载，释放网络资源。因此一旦        运行了Autofs后，用户就不再需要手动完成文件系统的挂接和卸载。

## 4 具有 SMB 的 NFS 挂载
* SMB(Server Message Block)协议是微软（Microsoft）和英特尔(Intel)在1987年制定的协议，主要是作为Microsoft网络的通讯协议
* CIFS（Common Internet File System）通用的互联网文件系统，是基于SMB协议开发的文件共享协议。可以看做是增强SMB协议跨平台，通用性的协议。
* samba是在Linux上实现SMB协议的自由软件

[共享实例](http://blog.sina.com.cn/s/blog_76a30acd0102y92w.html)
* NFS与SMB的区别：
  * NFS: Network File System 是已故的Sun公司制定的用于分布式访问的文件系统，它的本质是文件系统。主要在Unix系列操作系统上使用，基于TCP/IP协议层，可以将远程的计算机磁盘挂载到本地，像本地磁盘一样操作。
  * samba是Unix系统下实现的 Windows文件共享协议-CIFS，由于Windows共享是基于NetBios协议，是基于Ethernet的广播协议，在没有透明网桥的情况下（如VPN）是不能跨网段使用的。它主要用于unix和windows系统进行文件和打印机共享，也可以通过samba套件中的程序挂载到本地使用

## 5 查找文件
* locate 根据locate数据库中的文件名或路径返回搜索结果；locate数据库每日自动更新，也可手动更新updatedb
* find 实时搜索--用户必须具有查看其内容的目录的读取r和执行权限x
  * 选项：-user -group -type f|d|l|b -name -perm -size -mmin 60(1个小时以前)
* whereis/which  查找执行程序的路径

## 6 归档文件
* tar -[c|t|x]  vf    -z(gzip) j(bzip2) J(xz)
## 7 传输文件
* ftp sftp scp(复制所有内容) rsync(仅复制差异部分)
  * scp /home/mcbadm/.profile serverX:/home/yangjian/
  * scp serverX:/home/mcbadm/.profile  /home/yangjian/ 

# 用户管理
* 用户和组
id username
UID GID(系统组0-999，用户专用组1000以上)
su 身份验证：对尝试访问的账户密码验证
su以当前环境设置启动no-login shell，而su - 则启动login shell（shell环境是切换后的用户身份环境）
sudo 身份验证：对执行sudo用户自己的密码进行验证  日志记录：/var/log/secure
* 小测试：  
1、新建用户mcbadm具有root权限  
2、新建用户组mcb,使组内用户具有root权限  
#  
    vi /etc/sudoers
*　用户密码管理 
  * /etc/passwd
  * /etc/shadow  RHEL7默认采用SHA-512加密  
# 
    yangjian:$1$/htSp2Qr$99awsw99oBTbsrEjpar810:17729:0:99999:7:::
    $1 哈希算法
    $/htSp2Qr 用户salt
    $99awsw99oBTbsrEjpar810 已加密哈希
    哈希算法（用户salt+用户密码）=已加密哈希
  * /etc/login.defs配置用户密码策略 -如强制使用户每30天修改一次密码
  * change命令设置密码过期策略
  * usermod命令锁定账户-防止离开公司的员工登陆  解锁：usermod -U username
* 文件权限管理
  * 更改权限 
    * 符号法chmod u/g/o/a  +/-/= r/w/x  file/directory 
    * 数值法chmod 421  file/directory
    * -R选项  递归修改
  * 更改用户及组
    * chown user:group file/directory
    * chgrp group file/directory
    * -R选项  递归修改
  * 特殊权限-第四位
    * setuid(4)：u+s 以拥有文件的用户身份，而不是以允许文件的用户身份执行文件，对目录无影响 --修改进程的属主
    * setgid(2)：g+s 对文件无影响，对目录中的文件继承该目录的组所属关系 
    * sticky(1)：o+s 对文件无影响，对目录具有写入权限的用户仅可以删除其所拥有的文件
      * chmod u+s sqlplus >当其他用户执行oracle的sqlplus这个程序时，他的身份因这个程序暂时变成oracle
      * chmod 7554 send.sh >数值法设置-第四位是特权位
  * 默认权限 umask
* 高级文件权限管理
  * ACL访问控制列表   getfacl/setfacl -m设置权限   -x删除权限
  * SELinux上下文  基于服务的权限控制  目标：防止已遭泄露的系统服务访问用户数据。
    * SELinux上下文：用户、角色、类型和敏感度
    * SELinux开关：getenforce/setenforce或者/etc/selinux/config  默认关闭
    * 查看目录的SELinux上下文ls -Zd /var/www/html/
    * restorecon设置默认文件上下文的规则
    * semanage fcontext用于显示或修改上下文
# 服务管理
## 1 systemctl和systemd单元
 * 系统启动和服务器进程由systemd系统和服务管理器进行管理
 * 守护进程 是在执行各种任务的后台等待或运行的进程。守护进程名称一般以d结束。为了监听连接，守护进程使用套接字。systemd 进程ID 1
 * 服务 通常指的是一个或多个守护进程
 * systemctl命令用于管理各种类型的systemd对象，即单元units：systemctl -t help查看所有单元类型列表
   * 服务单元.service 代表系统服务
   * 套接字单元.socket 代表进程间通信（IPS）套接字
   * 路径单元.path 用于将服务的激活推迟到特定文件系统更改发生之后，如打印服务
######    
    yum -y install service
    systemctl   >列出所有系统服务
    systemctl list-units >列出所有启动units
    systemctl status service
    systemctl is-active service
    systemctl is-enable service
    systemctl start service
    systemctl restart service
    systemctl reload service  >重新加载配置，进程ID不会变
    systemctl enable service  >使服务（守护进程）在系统启动时启动
    systemctl disable service
    systemctl stop service
    systemctl mask/unmask network >屏蔽服务
 * systemd目标：一组在启动后达到所需状态的systemd单元
   * graphical.target 支持多用户、图形和基于文本的登录
   * multi-user.target 支持多用户、基于文本的登录
   * rescue.target  
   * emergency.target
######
    systemctl list-unit-files --type=target --all
    systemctl isolate multi-user.target 切换到其他目标
    systemctl get-default  查看当前目标
    systemctl set-default graphical.target  设置默认启动目标
   * 启动过程中选择其他目标
      * 启动时，任意键中断启动加载器
      * e编辑启动条目
      * 光标移动至内核命令行（linux16开头的行）
      * 附加systemd.unit=desired.targe
      * Ctrl+x 使用更改进行启动
## 2 chronyd服务
 *  网络时间协议NTP是计算机通过互联网提供并获取正确时间信息的一种标准方法
 * timedatectl命令简要显示当前的时间相关系统设置
######
    root@yhost:/root>timedatectl list-timezones
    Africa/Abidjan
    Africa/Accra
    Africa/Addis_Ababa
    Africa/Algiers
    ...
    timedatectl set-timezone Asia/Shanghai
    timedatectl set-time 9:00:00
    timedatectl set-ntp true 启用NTP同步
 * chronyd服务通过与配置的NTP服务器同步，使通常不精确的本地硬件时钟（RTC）保持准确。/etc/chrony.conf,配置完需重启chronyd服务   
## 3 OpenSSH服务
 * Open Secure Shell通过公钥加密的方式保证通信安全
    * 以当前用户身份创建远程交互式shell：ssh remotehost
    * 以其他用户身份创建远程交互式shell：ssh stu@remotehost
    * 以远程用户身份在远程主机上通过将输出返回到本地：ssh stu@remotehost hostname
 * w -f命令显示当前登录到计算机的用户列表，可以查看哪些用户以何种方式登录该主机
 * SSH主机密钥:ssh客户端第一次连接ssh服务器时，登录之前服务器会向客户端发生公钥副本（存储在客户端的~/.ssh/known_hosts文件中），用于为通信渠道安全加密，并可验证客户端的服务器。   <<<   服务端公钥 --> 客户端~/.ssh/known_hosts
 * 基于ssh密钥的身份验证（免密登录） ssh-keygen生成密钥（公钥+私钥）ssh-copy-id完复制   <<<  客户端公钥  --> 服务端
 * SSH服务配置/etc/ssh/sshd_config-限制ssh登录：禁止root用户使用ssh登录、禁止使用ssh进行密码身份验证等
 
## 4 电子邮件服务
 * 电子邮件传输：邮件客户端与传出邮件服务器进行通信（通过SMTP协议），后者帮助将该邮件中继到其最终目标。
 * 通常使用/usr/sbin/sendmail的标准程序（RHEL7中由Postfix提供）发送电子邮件
 * Postfix是一个功能强大且易于配置的邮件服务器，主配置文件/etc/postfix/main.cf
 * Postfix空客户端配置
 
## 5 Apache HTTPD Web服务

## 6 数据库服务



# 进程管理
## 1 linux任务管理
  * 一次性任务：at命令  系统守护进程atd 提供a-z共26个队列，系统优先级逐次降低
#####
    查看任务 atq  = \[at -l\]
    检查任务实际执行的命令 at -c JOBNUMBER
    制定任务 at -q h test now +5min   Ctrl+D退出
    删除任务 atrm JOBNUMBER
  * 周期性任务 crontab crond守护进程 分时日月周  */5 9-16 * * 1-5
#####
    crontab -l[r|e]
    crontab cronjobfilename

## 2 进程优先级
  * 进程（已启动的可执行程序的运行中实例）的组成部分：已分配的内存地址空间、安全属性、一个或多个执行线程、进程状态
  * 进程的环境：本地和全局变量、当前调度的上下文、分配的系统资源（文件描述符和网络端口等）
  * 第一个系统进程是systemd，所有进程都是第一个系统进程的后代
  * 进程状态：运行中 R  睡眠S|D|K 停止T  僵停Z|X
  * 进程调度 多核 时间片
  * nice级别 -20~19 通常为0，进程默认继承父进程的nice级别，nice级别越高，优先级越低 top命令参数NI
######
     mcbadm@ghost:/home/yangjian>ps -el|tail -10
     1401 S        111 21721     1  0 168 20 e0040103fad5da00 2873 e000000a8c19dd98 ?        20:32 ChgVal
     1401 S        111 29645     1 16 168 22 e000000ae593fa00 1487 e000000c59830718 ?        12:40 PreMain
     401 S        111  5696  2490  0 154 20 e000000d32cc1080  145 e00c000509fa31e8 pts/1     0:00 ksh
     401 S        111   953 16463 244 154 22 e00c0002aa5cf100  129 e00401007ec7a772 ?        510:45 gzip
     401 S        111 23897 23892  0 158 20 e0000009f9387100  145 e000000c7b22dc80 pts/13    0:00 ksh
     401 S        111 21498 21221  0 154 20 e00c0002e9b70080  143 e00c00050bcabbe8 pts/11    0:00 ksh
     1401 R          0 16867 20849  0 152 20 e000000b6a33e080  571                - ?         0:00 sshd
     3401 S          0  1793 20849  0 154 20 e0000009fb1a6380  572 e00000049342e308 ?         0:20 sshd
  
#####
    显示所有进程ps aux[lax]
    ps axo pid,comm,nice --sort=nice
    指定nice级别启动命令 nice -n 15 startup.sh &   只有root可以设置-20~-1级别，其他只能设置正的nice级别
    修改进程的nice级别 renice -n -7 $(pgrep startup.sh)
 
## 3 进程监控管理
  * 控制作业 &后台运行  fg %作业ID（jobs命令查看） 将后台作业转至前台
  * 进程管理信号：HUP挂起、INT键盘中断Ctrl+c、QUIT键盘退出Ctrl+\、TSTP键盘停止Ctrl+z、KILL中断、TERM（可被拦截、友好的方式）、CONT继续、STOP
  * kill -l指定信号中断 PID
  * killall 命令名称或匹配的进程
  * pkill 高级选择条件
  * w命令查看当前用户及其积累的活动
  * 负载平均值
    * linux内核以负载数的指数移动平均值计算负载平均值指标，代表一段时间内感知的系统负载，具体指最近1分钟、5分钟、15分钟内系统活动数据的三个显示值的平均值
    * UNIX系统仅考虑CPU使用率和运行队列长度来指示系统负载，linux负载平均值还包含I/O的考量，所以如有负载平均值很高但是CPU活动很低，应该检查下磁盘和网络活动
    * 查看方式top、uptime、w，numbers of CPU=`cat /proc/cpuinfo|grep "model name"`,per-CPU load average=`uptime`/numbers of CPU
    
* 守护进程systemd

# 网络管理
## 1 基础概念
 * IPv4地址(网络部分+主机部分)、IPv4路由、DNS、静态网络配置、DHCP
######
    CIDR表示法：192.168.1.107/24
    主机地址 192.168.1.107
    网络前缀（子网掩码） /24 (255.255.255.0)
    网络地址 192.168.1.0  子网中可能达到的最低地址
    广播地址 192.168.1.255 子网中可能达到的最高地址
    ip addr
    ip -s link show eth0  显示eth0接口信息
    ip route
    tracepath ip|hostname
    ping -c3 ip|hostname    
 * 特殊地址localhost：127.0.0.1
 * NetworkManager 是监控和管理网络设置的守护进程，命令行nmcli和图形工具与之通信；
 * 配置：/etc/sysconfig/network-scripts/ifcfg-<name> 命令：nmcli 图形化工具：nm-connection-editor
    
######
    nmcli con show [--active] 显示连接的列表
    nmcli con add con-name "default" type ethernet ifname eth0
    nmcli con up "default" 激活更改
    nmcli con add con-name "static" ifname eth0 autoconnnet no type ethernet ip4 172.25.X.10/24 gw4 172.25.X.254
    nmcli con show "static"
    nmcli con mod "static" ipv4.dns 172.25.X.254  更换DNS服务器
    nmcli con mod "static" ipv4.addresses "172.25.X.10/24 172.25.X.254"  更换ip地址和网关
    nmcli con up "static"    
    nmcli con down "static"
    nmcli con reload
 * hostnamectl set-hostname yhost; hostname   /etc/hosts   静态主机名/etc/hostname
 * DNS配置/etc/resolv.conf
 
## 2 firewalld服务限制网络通信
 * linux内核包含一个强大的网络过滤子系统netfilter，允许内核模块通过遍历系统的每个数据包进行检查
 * firewalld守护进程 与netfilter交互，可配置和监控系统防火墙规则的系统守护进程
   * 预定义区域 默认public   internal、trusted、home、work等
   * 预定义服务 可用于方便地允许特定网络服务的流量通过防火墙。ssh-22/tcp等
 * 防火墙配置
   * 配置文件/etc/firewalld/
   * 图形化工具firewall-config
   * 命令行 firewall-cmd
######
    firewall-cmd --set-default-zone=dmz
    firewall-cmd  --permanent --zone=internal --add-source=192.168.0.0/24
    firewall-cmd --reload
## 3 DNS管理
 * 域名系统DNS：充当互联网主机和资源的目录，目录中的信息将网络名称映射到数据。以root域.开始。
 * 域domain：资源记录的集合。顶级域TLD：按主题.org\.com\.edu..按国家.cn\.us\.uk...
 * 子域subdomain：作为另一域的子树的域，lab.example.com为example.com的子域
 * 区域zone：特定名称服务器直接负责或对其具有权威的某个域的组成部分
 * DNS查询分析：先向/etc/resolv.conf中列出的服务器依次发送查询。host、dig命令可用于手动查询
 * DNS资源记录：用于指定有关该区域中某个特定名称或者对象的信息。
 * DNS资源记录组成：资源名称、TTL（记录生存时间）、Class（类）、type（A、AAAA、CNAME等等）、data（记录存储的数据）
    * A IPv4记录    主机名映射到IPv4
    * AAAA IPv6记录  主机名映射到IPv6
    * CNAME 规范名称记录  记录别名转换为另一个别名
    * PTR（指针）记录   将IPv4或IPv6映射到主机名
    * NS（名称服务器）记录 将域名映射到DNS名称服务器
    * ...
#####
    host -v -t A example.com
    example.com. 86400 IN A 172.25.254.254
    host -v -t AAAA example.com
    example.com. 86400 IN AAAA 2001:503:ba3e::2:30
 * 缓存名称服务器：在本地缓存中存储DNS查询结果，并在TTL到期后从缓存中删除资源记录，用以提高DNS性能。
 * DNSSEC验证：鉴于UDP的无状态性质，DNS事务很容易被欺骗和篡改
 * 使用unbound配置安全缓存名称服务器

## 4 IPv6网络管理
 * IPv4联网配置
####
    nmcli dev status  显示所有网络设备状态
    nmcli con show   显示所有连接列表
    nmcli con add con-name eno2 type ethernet ifname eno2 ip4 192.168.0.5/24 gw4 192.168.0.254  添加网络连接
    nmcli up eno2 激活连接
    nmcli con reload 告知NetworkManager 重新读取配置文件
    nmcli con del eno2  删除连接及其配置文件
    nmcli dev dis eth0 断开与网络接口设备的连接并将其关闭
    ip addr show eno2
    静态连接属性会持久保存：/etc/sysconfig/network-scripts/ifcfg-<name> 
* IPv6地址  16\*8组=128位
* IPv6联网
    
## 5 链路聚合和桥接
 * 网络组：是将多个网卡聚合在一起方法，从而实现冗错和提高吞吐量  --即链路聚合
 * 链路聚合：将多个物理端口捆绑在一起，成为一个逻辑端口，以实现出/ 入流量在各成员端口中的负荷分担，交换机根据用户配置的端口负荷分担策略决定报文从哪一个成员端口发送到对端的交换机。当交换机检测到其中一个成员端口的链路发生故障时，就停止在此端口上发送报文，并根据负荷分担策略在剩下链路中重新计算报文发送的端口，故障端口恢复后再次重新计算报文发送端口
 * 链路聚合的优点：
   * 1）增加网络带宽：链路聚合可以将多个链路捆绑成为一个逻辑链路，捆绑后的链路带宽是每个独立链路的带宽总和。
   * 2）提高网络连接的可靠性：链路聚合中的多个链路互为备份，当有一条链路断开，流量会自动在剩下链路间重新分配。
####
    nmcli con add type team con-name team0 ifname team0 config '{"runner":{"name":"loadbalance"}}'  1）创建组接口
           method : broadcast\roundrobin\activebackup\loadbalance
    nmcli con mod team0 ipv4.addresses 1.2.3.4/24  2）确定组接口的IPv4/IPv6属性
    nmcli con mod team0 ipv4.method manual
    nmcli con add type team-slave ifname eth1 master team0  3）分配端口接口
    nmcli con add type team-slave ifname eth2 master team0 con-name team0-eth2
    nmcli con up team0  4）启动组接口和端口接口
    nmcli dev dis eth2  关闭组接口和端口接口
    teamctl team0 state 显示网络合作状态
 * 网桥：在链路层实现局域网互连的存储转发设备。网桥从一个局域网接收MAC帧，拆封、校对、校验之后，按另一个局域网的格式重新组装，发往它的物理层。由于网桥是链路层设备，因此不处理数据链路层以上层次协议所加的报头，基于MAC地址在网络之间转发流量。
   *  网桥和路由器的区别:
      * 1.网桥是第二层的设备，而路由器是第三层的设备；
      * 2.网桥只能连接两个相同的网络，而路由器可以连接不同网络；网桥在信息保密方面相对较好，它连接的是两个对等的网络。
      * 3.网桥不隔离广播，而路由器可以隔离广播。
   * 与中继器相比，使用网桥进行互连克服了物理限制，这意味着构成LAN的数据站总数和网段数很容易扩充。
   * 配置软件网桥--桥接 /etc/sysconfig/network-scripts/ifcfg-<name> 中TYPE=Bridge
    
#####
    nmcli con add type bridge con-name br0 ifname br0  创建网桥br0
    nmcli con add type bridge-slave con-name bro-port1 ifname eth1 master br0 将接口eth1连接到br0网桥，持久存储ifcfg-bro-port1
    nmcli con add type bridge-slave con-name bro-port2 ifname eth2 master br0 将接口eth2连接到br0网桥
    brctl show  显示网桥以及连接到该网桥的接口列表
 * 附:集线器、交换机、路由器、网桥、网关之间的区别[\[1\]](http://www.cnblogs.com/imapla/archive/2013/03/12/2955931.html)  [\[2\]](https://blog.csdn.net/gongda2014306/article/details/52442981)
   |`应用层`|`应用网关`|
   |`传输层`|`传输网关`|
   * 网络层：路由器
   * 数据链路层：网桥、交换机
   * 物理层：中继器、集线器
     * 中继器：信号在传输过程中会不断衰减，仅做放大信号用，把信号传导偏远的地方。
     * 集线器HUB：集线器实际就是一种多端口的中继器。为了能够让通信“一对多”，需要将信号复制广播。由于它在网络中处于一种“中心”位置，因此集线器也叫做“HUB” 。
     * 网桥(Bridge)：有了集线器，但是这带来一个问题，多个集线器连接在一起，但是由于是广播通信，互相冲突，所以我们现在需要一种设备，能够有效隔离子网。让广播通信仅仅在于一个局部。LAN1：A、B、C、D主机<----->网桥<----->LAN2：E、F、G、H主机 
     * 交换机Switch：网桥只有两个端口。随着网络设备的发展，逐渐产生了多个端口的“网桥”，但是由于网桥是数据链路层的广播通信，A和G通信的时候，B和F就没法通信——一个桥上多个通信将产生冲突。为了能够实现多对多的通信，于是产生了交换机，交换机有一张表，记录的是port-mac。
     * 路由器(Router)：交换机工作在数据链路层次。如果现在A节点向未知节点B通信，如果A和B之间通过N（很大）个交换机才连接在一起，那么只用交换机来实现，那么A将数据包发送之后，在到达所有其他端口，如果其他端口不能识别，那么都将进行转发，这样，最终也可以到达B，但是势必产生很多的冗余数据通信。于是，我们有了路由器。
     * 网关（Gateway）：网关能在不同协议间移动数据，而路由器（router）是在不同网络间移动数据，相当于传统所说的IP网关（IP gateway）
    
## 6 网络端口安全性（防火墙和SELinux）
#### 1 防火墙配置
 * firewalld是RHEL7中用于管理主机级别的防火墙的默认方法，通过firewalld.service systemd服务来启动
   * firewalld对传入流量处理的标准规则：
      * 1）传入包源地址匹配
      * 2）传入包接口与过滤器匹配
      * 3）默认区域
 * 管理firewalld：1)命令行firewall-cmd 2)图形化工具firewall-config 3)配置文件/etc/firewalld
#####
    firewall-cmd  --set-default-zone=dmz 配置默认情况下通过dmz区域来路由所有流量
    firewall-cmd --permanent --zone=work --add-source=172.25.X.10/24 配置通过work区域来路由所有来自172.25.X.10/24的所有流量
    firewall-cmd --permanent --zone=work --add-service=https 为work区域打开传入https的流量
    firewall-cmd --permanent --zone=work --list-all  检查work区域防火墙配置
    curl -k https://serverX.example.com  -k选项跳过证书验证
 * 直接规则：除了firewalld提供的标准规则
#####
    firewall-cmd --direct --permanent --add-chain ****
 * 富规则：firewalld基本语法之外的可自定义的防火墙规则
 * 使用富规则进行记录到syslog：log  \[prefix=""\] \[level=""\] \[limit value=""\]
 * 网络地址转换NAT的两种形式：伪装、端口转发
    * 伪装：内网ip和外网ip转换，为内部网络提供Internet访问
    * 端口转发：将单个端口的流量转发到同计算机的不同端口或者不同计算机的端口
######
    firewall-cmd --permanent --zone=<ZONE> --add-masquerade  --使用常规firewalld命令实现区域配置伪装
    firewall-cmd --permanent --zone=<ZONE> --add-rich-rule='rule family=ipv4 source address=192.168.0.0/24 masquerade' --使用富规则实现伪装
```
    firewall-cmd --permanent --zone=work --add-rich-rule='rule family=ipv4 source address=192.168.0.0/24 forward-port port=80 protocol=tcp to-port=8080'  --使用富规则实现：将来自work区域的192.168.0.0/24且传入到端口80/TCP的流量转发到防火墙计算机自身上面的8080/TCP
```
    示例：启动web服务器，使得只有desktopX（172.25.X.10/32）能进行连接，并对连接进行记录，将记录限制为每秒最多3条，记录均以“NEW HTTP”作为前缀
```
    yum install httpd
    systemctl start httpd.service
    systemctl enable httpd.service
    firewall-cmd --permanent --add-rich-rule='rule family=ipv4 source address=172.25.X.10/32 service name="http" log level=notice prefix="NEW HTTP" limit value="3/s" accept'
    firewall-cmd --reload
    tail -f /var/log/messages
    curl http://serverX.example.com
```
      
#### 2 SELinux配置
 * SELinux端口标记：不仅仅进行文件和进程标记，还严格实施网络流量，SELinux用来控制网络流量的其中一种方法是标记网络端口。当某个进程希望监听端口时，SELinux将检查是否允许与该进程相关联的标签 绑定 该端口标签。可以阻止恶意服务控制本应由其他网络服务使用的端口。
#####
    semanage port -l 获取所有当前端口标签分配情况   --图形化工具system-config-selinux
    semanage port -a -t gopher_port_t -p tcp 71 允许gopher服务侦听端口71/tcp --向现有端口标签（类型）中添加端口71
    semanage port -d -t gopher_port_t -p tcp 71 删除端口71/tcp与gopher_port_t的绑定
    semanage port -m -t http_port_t -p tcp 71  将端口71/tcp从gopher_port_t修改为http_port_t
    

# 日志管理
## 1 RHEL7系统日志架构
 * systemd-journald守护进程可以收集来自内核、启动过程的早期阶段、标准输出、系统日志，以及守护进程启动和运行期间错误的消息。默认重启系统之后不保留。
 * rsyslog服务根据类型和优先级排列系统日志消息，将其写入到/var/log目录内的永久文件中
#####
    /var/log/messages  大多数系统日志消息记录
    /var/log/secure  安全与身份验证相关的日志
    /var/log/maillog 邮件服务器相关的日志文件
    /var/log/cron  定期执行任务相关的日志文件
    /var/log/boot.log  系统启动相关的消息记录
## 2 系统日志文件
 * 系统日志优先级emerg、alert、crit、err、warning、notice、info、debug
 * 系统日志文件有rsyslog服务维护，/etc/rsyslog.conf /etc/rsyslog.d/\*conf
 * 日志文件通过logrotate实用工具“轮转”，防止/var/log文件系统填满，cron作业每日运行一次logrotate程序
 * logger命令：发送消息到rsyslog服务 logger -p user.debug "Debug Message Test"
## 3 systemd日志
 * systemd日志默认存储在/run/log
 * journalctl查找事件 
 * 永久保存systemd日志：从/run/log/journal迁移到/var/log/journal
    * mkdir /var/log/journal
    * chown root:systemd-journal /var/log/journal
    * chmod 2755 /var/log/journal
    * killall -USER1 systemd-journald 或重启系统即可生效
       * USR1通常被用来告知应用程序重载配置文件；例如，向Apache HTTP服务器发送一个USR1信号将导致以下步骤的发生：停止接受新的连接，等待当前连接停止，重新载入配置文件，重新打开日志文件，重启服务器，从而实现相对平滑的不关机的更改。
#####
    journalctl -n  显示最后10个日志条目
    journalctl -n  5 最后5个日志条目
    journalctl -p err 输出过滤为仅列出err或以上级别的日志条目
    journalctl -f  显示最后10行
    journalctl --since|until YYYY-MM-DD hh:mm:ss|yesterday|today|tomorrow
    journalctl -b 显示系统上一次启动以来的日志消息

## 4 RHEL启动过程
    1、接通电源，系统固件运行开机自检（POST），并开始初始化部分硬件
    2、系统固件搜索可启动设备
    3、系统固件会从磁盘读取启动加载器，然后将系统控制权交给启动加载器
    4、启动加载器从磁盘加载其配置，然后向用户显示用于启动的可能配置的菜单
    5、用户做出选择后，启动加载器会从磁盘加载配置的内核及initramfs，并将它们置于内存中
    6、启动加载器将系统控制权交给内核
    7、对于内核可在initramfs中找到驱动程序的所有硬件，内核会初始化这些硬件，作为PID 1 从initramfs执行/sbin/init
    8、initramfs中的systemd实例会执行initrd.target目标的所有单元
    9、内核root文件系统从initramfs root文件系统切换为之前挂载于/sysroot上的系统root文件系统
    10、systemd会查找从内核命令行传递或系统中配置的默认目标，然后启动单元，以符合该目标的配置，从而自动解决单元间的依赖关系
 * systemctl poweroff 停止所有运行的服务，卸载所有文件系统，然后关闭系统   --RHEL7中 poweroff命令只是一个符号链接
 * systemctl reboot 停止所有运行的服务，卸载所有文件系统，然后重新启动系统  --RHEL7中 reboot命令只是一个符号链接
 * 恢复root密码
   * 重启系统
   * 任意键中断启动加载器
   * e编辑启动条目
   * 光标移动至内核命令行（linux16开头的行）
   * 附加rd.break(就在从initramfs向实际系统移交控制权之前，该操作会中断)
   * Ctrl+x 使用更改进行启动
   * 读写形式挂载：mount -o remount.rw /sysroot
   * 切换为chroot存放位置：chroot /sysroot
   * 设置新的root密码：passwd root
   * 确保所有未标记的文件在启动过程中都会重新获得标记：touch /.autorelabel
   * 键入exit两次，第一次退出chroot存放位置，第二次退出initramfs调试shell
# 安全管理

# 脚本
* 环境变量

* 脚本基础语法

* 脚本控制逻辑

* 脚本调试
  * #!/bin/bash -x
  * >bash -x scriptname

# Linux容器和Docker
* Linux容器

* Docker


* 帮助文档pinfo man /usr/share/doc  
* [centos和redhat的区别](https://blog.csdn.net/fd8559350/article/details/52604427) 
[回到顶部](#table of contents)
