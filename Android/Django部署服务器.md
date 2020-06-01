### Django部署服务器

#### 1、准备阿里云服务器

#### 2、服务器操作

1.创建一个新的用户

```linux
# 在 root 用户下运行这条命令创建一个新用户，yan 是用户名

root@server:~# # adduser yan
# 为新用户设置密码
root@server:~# passwd yan
# 把新创建的用户加入超级权限组
root@server:~# usermod -aG wheel yan
# 切换到创建的新用户
root@server:~# su - yan
# 切换成功，@符号前面已经是新用户名而不是 root 了
yan@server:$

```

2.安装python3

```django
#首先安装可能的依赖
yangxg@server:$ sudo yum install -y openssl-devel bzip2-devel expat-devel gdbm-devel readline-devel sqlite-devel

#############################################################################################
#然后下载 Python 3.6.4 的源码并解压：
yangxg@server:$ cd ~/src
yangxg@server:$ wget https://www.python.org/ftp/python/3.7.4/Python-3.7.4.tgz
yangxg@server:$ tar -zxvf Python-3.6.4.tgz
#############################################################################################


#############################################################################################
https://blog.csdn.net/lcanlup/article/details/84635625
#编译安装python3
#解决pip3不能使用，安装时添加--with--ssl
    cd Python-3.6.4
	#编译安装到指定路径
    ./configure --prefix=/usr/local/python3  --with-ssl
    make
    sudo make install


#建立软链接
ln -s /usr/local/python3/bin/python3.7 /usr/bin/python3
ln -s /usr/local/python3/bin/pip3.7 /usr/bin/pip3
#安装虚拟环境
pip3 install virtualenv
建立软链接
ln -s /usr/local/python3/bin/virtualenv /usr/bin/virtualenv
#############################################################################################


#############################################################################################
#升级pip  pip是python2版本的  pip3才是python3相关的  如果用pip 安装其他依赖项会安装到python2的下边 
#但是pip3 安装前需要升级pip3 相关命令是
		pip3 install --upgrate pip
#可能会出现权限问题 前面加sduo  
		sudo pip3 install --upgrate pip
#此时再出现问题会出现pip3不是命令这样的坑爹玩笑  这个需要创建pip3命令的连接符 即下边部分
        1.首先找到pip3的位置(在python3.6的安装包内)
        cd /usr/local/bin
        ls      (这里可以看见pip3.6的文件了)
 
        ln -s /usr/local/bin/pip3.6 /bin/pip3       #创建pip3命令的连接符
        pip3 -v
#############################################################################################


#############################################################################################
#卸载python3
      rpm -qa|grep python3|xargs rpm -ev --allmatches --nodeps       卸载pyhton3
      whereis python3 |xargs rm -frv           删除所有残余文件
      成功卸载！
      whereis   python       查看现有安装的python
#############################################################################################


#############################################################################################
#pip卸载
	 python -m pip uninstall pip

#############################################################################################

#安装 Pipenv
#yangxg@server:$ sudo pip3 install pipenv

```

```python
#安装python3后使用pip和pip3的区别是什么？
'''
1、比如安装库numpy，pip3  install  numpy或者pip  install  numpy：只是当一台电脑同时有多个版本的Python的时候，用pip3就可以自动区分用Python3来安装库。是为了避免和制Python2发生冲突的。
如果你的电脑只安装了Python3，那么不管用pip还是pip3都一样的。
2、安装了python3之后，会有pip3

使用pip install XXX ：
新安装的库会放在这个目录下面：python2.7/site-packages；

使用pip3 install XXX ：
新安装的库会放在这个目录下面：python3.6/site-packages；

如果使用python3执行程序，那么就不能import python2.7/site-packages中的库。
'''
```

#### 3、从GitHub 上clone项目

```
#项目配置文件

./settings.py 文件

ALLOWED_HOSTS = ['127.0.0.1', 'localhost ', '.zmrenwu.com']

STATIC_URL = '/static/'
# 加入下面的配置
STATIC_ROOT = os.path.join(BASE_DIR, 'static')


#首先要传到github上然后才能clone
#安装 git：
yangxg@server:$ sudo  yum install -y git

# 在用户目录下创建 apps 目录并进入
yangxg@server:$ mkdir -p ~/apps
yangxg@server:$ cd ~/apps
# 拉取博客代码
yangxg@server:$ git clone https://github.com/Ext-xiaoming/demo01.git

```

#### 4、[Django项目部署到云服务器上，项目依赖包可以通过命令安装](https://www.cnblogs.com/xuecl/p/12260178.html)

```
#在项目根目录下，运行命令：
		pip  freeze > package.txt
#然后项目文件上传到云服务器后，在项目根目录下，运行命令：
		pip  install -r package.txt
#安装相关依赖包，其中package.txt，是在客户端时，通过pip freeze > package.txt获得	
```

#### 5、

### 初步计划

```
1、卸载原装python2.7
2、安装 python3.7 Uwsgi 和 Nginx
3、检查pip 更新pip
4、安装 虚拟环境
5、设置目录
6、拉取项目源代码
7、进入项目 下载依赖包
8、配置 python3.7的文件
9、配置Uwsgi和Nginx
10、启动
```

