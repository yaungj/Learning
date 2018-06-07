## **多主机ssh免密登陆配置**
    ##auto_ssh.sh
    #!/usr/bin/expect  
    set timeout 10  
    set username [lindex $argv 0]  
    set password [lindex $argv 1]  
    set hostname [lindex $argv 2]  
    spawn ssh-copy-id -i /root/.ssh/id_rsa.pub $username@$hostname
    expect {
                #first connect, no public key in ~/.ssh/known_hosts
                "Are you sure you want to continue connecting (yes/no)?" {
                send "yes\r"
                expect "password:"
                    send "$password\r"
                }
                #already has public key in ~/.ssh/known_hosts
                "password:" {
                    send "$password\r"
                }
                "Now try logging into the machine" {
                    #it has authorized, do nothing!
                }
            }
    expect eof
    
    ./auto_ssh.sh root abcd1234 redis01
    
    ./auto_ssh.sh mcbadm abcd1234 zookeeper01
    ./auto_ssh.sh mcbadm abcd1234 zookeeper02


* 在使用ansible中的时候，默认的模块是-m command，从而模块的参数不需要填写，直接使用即可。
  * ansible ZOOKEEPER -m copy -a "src=/home/mcbadm/zookeeper-3.4.5.tar.gz dest=/app/zookeeper-3.4.5.tar.gz"
  * ansible ZOOKEEPER -a "chdir=/app tar -zxvf zookeeper-3.4.5.tar.gz"


     [root@redis01 src]# ./redis-trib.rb create --replicas 1 192.168.133.73:6379 192.168.133.74:6379 192.168.133.75:6379 192.168.133.76:6379 192.168.133.77:6379 192.168.133.78:6379
     >>> Creating cluster
     [ERR] Sorry, can't connect to node 192.168.133.74:6379
     ansible REDIS -m shell  -a "ps -ef|grep redis-ser|grep -v grep|awk '{print $2}'|xargs kill"
     ansible REDIS -a "chdir=/usr/local/bin ./redis-server ./redis.conf"
     ansible REDIS -a "yum -y install telnet telnet-server"
     ansible REDIS -a "systemctl status xinetd"
     ansible REDIS -a "yum -y install xinetd"
     ansible REDIS -a "systemctl enable xinetd"
     ansible REDIS -a "systemctl start xinetd"
     ansible REDIS -a "firewall-cmd --add-port=6379/tcp --permanent"
     ansible REDIS -a "firewall-cmd --add-port=16379/tcp --permanent"
     ansible REDIS -a "firewall-cmd --add-port=23/tcp --permanent"
     ansible REDIS -a "systemctl status telnet.socket"
     ansible REDIS -a "systemctl enable telnet.socket"
     ansible REDIS -a "systemctl strat telnet.socket"
     ansible REDIS -a "systemctl start telnet.socket"
     ansible REDIS -a "iptables -F"


## **keepalived安装**
### 1、安装依赖包
* yum -y install ipvsadm
* yum -y install popt-devel  #否则会提示configure: error: Popt libraries is required
* yum -y install openssl-devel    #否则会提示!!! OpenSSL is not properly installed on your system. 
### 2、编译安装keepalived
* shell> wget http://www.keepalived.org/software/keepalived-1.2.24.tar.gz
* shell> tar -zxvf keepalived-1.2.24.tar.gz
* shell> cd keepalived-1.2.24
* shell> ./configure --prefix=/usr/local/keepalived
* shell> make && make instal

