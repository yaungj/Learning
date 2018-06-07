1.install python  
https://www.python.org/downloads/windows/  
2.install pip  可在1中添加安装  
>python get-pip.py  
 
3.install django
>pip install django
>python -c "import django; print(django.get_version())"
或python -m django --version
>django-admin startproject mysite
>python manage.py migrate
settings.py :    
     TEST_RUNNER = 'django.test.runner.DiscoverRunner'
>python manage.py runserver
>python manage.py runserver 8080
>python manage.py runserver 0.0.0.0:8000
 
>python manage.py startapp polls

Windows 安装mysql
1、	MySQL下载地址：https://www.mysql.com/downloads/
首先解释一下社区版本和企业版本的区别：
1.	MySQL Community Server 社区版本，开源免费，但不提供官方技术支持。（本文安装示例）
2.	MySQL Enterprise Edition 企业版本，需付费，可以试用30天。
3.	MySQL Cluster 集群版，开源免费。可将几个MySQL Server封装成一个Server。
4.	MySQL Cluster CGE 高级集群版，需付费。
5.	MySQL Workbench（GUITOOL）一款专为MySQL设计的ER/数据库建模工具。它是著名的数据库设计工具DBDesigner4的继任者。MySQLWorkbench又分为两个版本，分别是社区版（MySQL Workbench OSS）、商用版（MySQL WorkbenchSE）。
注：一般可以在一个系统上安装多个MySQL，只不过需要去修改端口号，这里只演示安装一个，我是把之前的数据库数据备份了，删除原来的数据库，进行的安装，之前用的是安装版本，所以用的是360卸载（当然也可以用自带的卸载器），然后删除关于MySQL的所有目录，再到注册表里删除关于MySQL的所有东西，具体方法：进行注册表，用ctrl + f 搜索所有有关MySQL的内容进行删除。
2、下载完成以后进行解压，解压到你想要的盘中：
解压后的文件没有data文件夹和my.ini文件，需要自己创建一下
 
####  my.ini文件内容：
    # MySQL Server Instance Configuration File 
    [client] 
    # 设置mysql客户端默认字符集 
    default-character-set=utf8 
    [mysqld] 
    #设置3306端口 
    port=3306 
    # 设置mysql的安装目录 
    basedir =”D:\MySQL\mysql-5.7.18-winx64” 
    # 设置mysql数据库的数据的存放目录 
    datadir =”D:\MySQL\mysql-5.7.18-winx64\data” 
    tmpdir =”D:\MySQL\mysql-5.7.18-winx64\data” 
    socket =”D:\MySQL\mysql-5.7.18-winx64\data\mysql.sock” 
    log-error=”D:\MySQL\mysql-5.7.18-winx64\data\mysql_error.log” 
    # 设置mysql服务端默认字符集 
    character-set-server=utf8 
    # 创建新表时将使用的默认存储引擎 
    default-storage-engine=INNODB 
    default-tmp-storage-engine=INNODB 
    #server_id = 2 
    #skip-locking 
    # 允许最大连接数 
    max_connections=1000 
    table_open_cache=256 
    query_cache_size=32M 
    tmp_table_size=32M 
    thread_cache_size=8 
    innodb_data_home_dir=”D:\MySQL\mysql-5.7.18-winx64\data\” 
    innodb_flush_log_at_trx_commit =1 
    innodb_log_buffer_size=128M 
    innodb_buffer_pool_size=128M 
    innodb_log_file_size=10M 
    innodb_thread_concurrency=16 
    innodb-autoextend-increment=1000 
    join_buffer_size = 128M 
    sort_buffer_size = 32M 
    read_rnd_buffer_size = 32M 
    max_allowed_packet = 32M 
    explicit_defaults_for_timestamp=true 
    sql-mode=”STRICT_TRANS_TABLES,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION” 
    skip-grant-tables 
    #sql_mode=NO_ENGINE_SUBSTITUTION,STRICT_TRANS_TABLES
 
3、配置MySQL环境变量
3.1右击 我的电脑 –> 属性，进入高级系统设置，进行配置MySQL_HOME 
我的路径是在：D:\MySQL\mysql-5.7.18-winx64，因此： 
MySQL_HOME 
D:\MySQL\mysql-5.7.18-winx64 
3.2点击确定之后，停在这个画面，进行配置path路径 
我是在path后面加的，你也可以在中间或者最前面加，都一样，只要注意分号问题就可以了，在最后加前面要有一个分号，在最前面加，后面得有一个分号。 
如图： 
；%MySQL_HOME%\bin
4、将MySQL注册成windows服务
4.1进入c:\Windows\System32，以管理员身份运行cmd.exe
4.2进入黑窗口，敲如下命令，进入你的MySQL的bin目录（根据你放的文件位置）
D:
cd MySQL\mysql-5.7.18-winx64\bin
进入即可    
输入增加服务命令：mysqld install MySQL --defaults-file="D:\MySQL\mysql-5.7.18-winx64\bin"
mysqld install MySQL –defaults-file=” D:\programdata\mysql-5.7.21-winx64\bin”
移除服务的命令是：mysqld remove
5、成功后，初始化data目录
初始化data文件夹非常重要，如图：
mysqld --initialize
6、打开系统服务管理
开启mysql服务：net start mysql
关闭mysql服务：net stop mysql
7、修改MySQL的root密码
7.1这时候创建的是无root密码的，可以直接通过mysql -uroot 直接进入
C:\Users\Administrator>mysql –uroot

mysql>show databases;
mysql>use mysql;
mysql>update mysql.user set authentication_string=password('123456') where user='root' and Host = 'localhost';
mysql>FLUSH PRIVILEGES;
mysql>quit

*****************************************************************************
注意：新版的mysql数据库下的user表中已经没有Password字段了，而是将加密后的用户密码存储于authentication_string字段
7.2在这里我遇到了一个问题, 就是使用123456的时候可以登录mysql，但是使用nvicat链接时报了错误，需要重新修改一下密码，这里提供四种修改密码的方式，我利用的是第二种[3]：
 

8、设置远程访问MySQL数据库操作
不同的权限设置： 格式：grant 权限 on 数据库教程名.表名 to 用户@登录主机 identified by “用户密码”; @ 后面是访问MySQL的客户端ip地址（或是 主机名） % 代表任意的客户端，如果填写 localhost 为 
本地访问（那此用户就不能远程访问该mysql数据库了）。同时也可以为现有的用户设置是否具有远程访问权限。
第一：是所有用户都能远程访问：(开发可以用这种方式，方便其他人访问你的数据库) 
进入mysql 
GRANT ALL PRIVILEGES ON . TO ‘root’@’%’ IDENTIFIED BY ‘123456’ WITH GRANT OPTION; 
mysql>FLUSH PRIVILEGES; 
mysql>quit
第二：限定某个ip访问[4]：（真实上线可以限定访问的ip地址） 
GRANT ALL PRIVILEGES ON *.* TO ‘root’@’192.168.188.123’ IDENTIFIED BY ‘123456’ WITH GRANT OPTION; 
mysql>FLUSH PRIVILEGES; 
mysql>quit
后记：    安装过程中遇到了很多问题，也参考了很多人的安装方法，将自己的安装过程总结到这。  如果，博客中有什么不对的地方，希望能够提醒我一下，谢谢。
我的邮箱地址是：it_chang@126.com

Show databases;看不到mysql数据库
放开权限检测：
D:\programdata\mysql-5.7.21-winx64\bin>mysqld --skip-grant-tables
在开窗口进数据库。

另外一个原因是，数据库安装之后初始化没有成功：
关闭数据库，删除data目录内容，重新初始化:
Mysqld –initialize



