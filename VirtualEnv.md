* 为方便不同项目环境切换，可以在同一个宿主机搭建不同的python环境，清理也方便。
# **python虚拟环境搭建：**
* pip install virtualenv
* pip install virtualenvwrapper
* pip install virtualenvwrapper-win　　#Windows使用该命令
* 安装完成后，在~/.bashrc写入以下内容
* 	export WORKON_HOME=~/Envs
* 	source /usr/local/bin/virtualenvwrapper.sh　
* source ~/.bashrc
**创建虚拟环境**　mkvirtualenv venv　
	这样会在WORKON_HOME变量指定的目录下新建名为venv的虚拟环境。
	若想指定python版本，可通过"--python"指定python解释器
	mkvirtualenv --python=/usr/local/python3.5.3/bin/python venv
__激活虚拟环境__
	$ source venv/bin/activate　　
__查看当前的虚拟环境目录_
	[root@localhost ~]# workon
	py2
	py3
__切换到虚拟环境__
	[root@localhost ~]# workon py3
	(py3) [root@localhost ~]# 
__退出虚拟环境__
	(py3) [root@localhost ~]# deactivate
	[root@localhost ~]# 
__删除虚拟环境__
	rmvirtualenv venv  或者rm -rf删除目录
