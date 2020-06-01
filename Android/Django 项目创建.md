### Django 项目创建

1、使用Virtualenv 安装Django

```python
https://www.django.cn/article/show-5.html
https://www.django.cn/course/show-4.html
#创建虚拟环境 
virtualenv D:\PyProject\venv

#直接输入（相当于执行了activate文件）  激活虚拟环境
#进入D盘 后进入D:\PyProject\Scripts\
>D:
> cd D:\PyProject\venv\Scripts\
> activate
结果开启成功： (venv) D:\PyProject\venv\Scripts>

#退出虚拟环境  
deactivate

#进入虚拟环境    安装django（2.2）  (激活)
#先在D:\PyProject\venv\目录下新建项目的工程目录  Project文件夹

#然后切换到我们要安装项目的目录
>cd D:\PyProject\venv\Project

#下载django2.2
pip install -i https://pypi.douban.com/simple django==2.2
pip3 install -i https://pypi.douban.com/simple django==2.2
#Django安装之后，我们来验证一下Django的版本 虚拟环境中运行
pip list


#创建项目之前，先把你虚拟环境下的目录Scripts目录将它加入操作系统的环境变量中
path = D:\PyProject\venv\Scripts

#创建名字为 IFace的项目
django-admin startproject IFace

>cd IFace
>tree /F
   结果：   文件夹 PATH 列表
            卷序列号为 0F78-116B
            D:.
            │  manage.py
            │
            └─IFace
                    settings.py
                    urls.py
                    wsgi.py
                    __init__.py
#ok django2.2项目创建成功！

#接着用Pycharm打开即可
#在项目下创建APP
python manage.py startapp app


完成！

#连接mysql 
--#在App _init_ 中添加 
import pymysql
pymysql.install_as_MySQLdb()

--#在sittings中配置


DATABASES = {
    'default': {
        'ENGINE': 'django.db.backends.mysql',
        'NAME': 'iface',
        'HOST': '0.0.0.0',
        'PORT': 3306,
        'USER': 'root',
        'PASSWORD': '123456',
    }
}

--#反向生成models 
python .\manage.py inspectdb
'''
可能出现的问题一
报错django.core.exceptions.ImproperlyConfigured: mysqlclient 1.3.13 or newer is required; you have 0.9.3.
https://blog.csdn.net/weixin_45476498/article/details/100098297
解决方案
步骤一：
C:\Python37\Lib\site-packages\django\db\backends\mysql（python安装目录）打开base.py
将---注释掉
	if version < (1, 3, 13): 　　　　　　　　　　
		raise ImproperlyConfigured(‘mysqlclient 1.3.13 or newer is required; you have %s.’ % 				Database.version) 　
		
步骤二：
打开同样目录下的·  .\operations.py
将--- 
	146行的decode修改为encode



可能出现的问题二
报错django.core.exceptions.ImproperlyConfigured: mysqlclient 1.3.13 or newer is required; you have 0.9.3.
https://blog.csdn.net/qq_37232731/article/details/89684409

解决方案：
打开python 目录下的 Lib\site-packages\django\views\debug.py
将---332 行处
	with Path(CURRENT_DIR, 'templates', 'technical_500.html').open() as fh:
	修改为
	with Path(CURRENT_DIR, 'templates', 'technical_500.html').open(encoding='utf-8') as fh:

over!
'''

#配置pyCharm启动django 
1/点击edit configurations 编辑配置参数
2/在scrip parameters 对应的对话框中输入配置参数 runserver 127.0.0.1:8000. 即可

#命令行
python manage.py runserver 0.0.0.0:8000
    

    
```

https://github.com/Ext-xiaoming/IFace.git