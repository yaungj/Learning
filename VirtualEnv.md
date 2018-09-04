
# PYTHON虚拟环境
* 为方便不同项目环境切换，可以在同一个宿主机搭建不同的python环境，清理也方便。
## **python虚拟环境搭建：**
* pip install virtualenv
* pip install virtualenvwrapper
* pip install virtualenvwrapper-win　　#Windows使用该命令
* 安装完成后，在~/.bashrc写入以下内容
* 	export WORKON_HOME=~/Envs
* 	source /usr/local/bin/virtualenvwrapper.sh　
* source ~/.bashrc
## **创建虚拟环境**　
* mkvirtualenv venv　
* 这样会在WORKON_HOME变量指定的目录下新建名为venv的虚拟环境。
* 若想指定python版本，可通过"--python"指定python解释器
* mkvirtualenv --python=/usr/local/bin/python3 vpy36
## __激活虚拟环境__
* $ source venv/bin/activate　　
## __查看虚拟环境__
* [root@localhost ~]# workon
* py2
* py3
## __切换到虚拟环境__
* [root@localhost ~]# workon py3
* (py3) [root@localhost ~]# 
## __退出虚拟环境__
* (py3) [root@localhost ~]# deactivate
* [root@localhost ~]# 
## __删除虚拟环境__
* rmvirtualenv venv  或者rm -rf删除目录


# PyPI源

PyPI使用默认的pip源的速度实在无法忍受，其他一些国内的pip源，如下：

阿里云 http://mirrors.aliyun.com/pypi/simple/

中国科技大学 https://pypi.mirrors.ustc.edu.cn/simple/ 

豆瓣(douban) http://pypi.douban.com/simple/ 

清华大学 https://pypi.tuna.tsinghua.edu.cn/simple/

中国科学技术大学 http://pypi.mirrors.ustc.edu.cn/simple/

使用方法很简单，直接 -i 加 url 即可！如下：
```  pip install web.py -i http://pypi.douban.com/simple
     pip install web.py -i http://pypi.douban.com/simple --trusted-host pypi.douban.com
``` 
如果想配置成默认的源，方法如下：

需要创建或修改配置文件（一般都是创建），

linux的文件在~/.pip/pip.conf，

windows在%HOMEPATH%\pip\pip.ini），

修改内容为：

[global]
index-url = http://pypi.douban.com/simple
[install]
trusted-host=pypi.douban.com
 
这样在使用pip来安装时，会默认调用该镜像。

临时使用其他源安装软件包的python脚本如下：
######
    #!/usr/bin/python
    import os
     
    package = raw_input("Please input the package which you want to install!\n")
    command = "pip install %s -i http://pypi.mirrors.ustc.edu.cn/simple --trusted-host pypi.mirrors.ustc.edu.cn" % package
    os.system(command)
