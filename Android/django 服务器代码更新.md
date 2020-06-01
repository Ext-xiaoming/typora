### django 服务器代码更新

1、本地提交到github

2、github拉到服务器

```
#开启虚拟环境：
>	cd /data/env/Pywebs/bin
>	source activate
>	cd /data/wwwroot/IFace/IFace_Diango
$ git fetch origin master:temp    //从远程的origin仓库的master分支下载到本地并新建一个分支temp
$ git diff temp                   //比较master分支和temp分支的不同
$ git merge temp                  //合并temp分支到master分支
$ git branch -d temp              //删除temp
```

3、重启

```
>	cd /data/wwwroot/IFace/IFace_Diango
>	python3 manage.py runserver

#############################################################################################
---重启uwsgi
#如果想重启uwsgi,先使用下面的命令杀掉进程,再启动uwsgi
>	killall -9 uwsgi
#启动uwsgl
>	uwsgi -x IFace.xml
#uwsgi有没有启动成功,可以用下面的命令查看
>	ps -ef|grep uwsgi

#############################################################################################
---重启nginx
#查看Nginx的主进程ID
>ps -ef|grep Nginx
#杀死进程
>kill -QUIT xxxx

#强制停止
pkill -9 nginx

#进入/usr/local/nginx/sbin/目录
>	cd /usr/local/nginx/sbin/
#执行./nginx -t命令先检查配置文件是否有错，没有错就执行以下命令
>	./nginx



```

