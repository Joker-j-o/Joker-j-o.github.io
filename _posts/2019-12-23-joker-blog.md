---
layout:     post                    # 使用的布局（不需要改）
title:      project_deploy_uwsgi             # 标题 
subtitle:   Joker #副标题
date:       2019-12-23              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
tags:                               #标签
	- project_delopy
categories: [python, django,project_delopy]
---

## uWSGI Deploy when nginx is not used
### uWSGI configuration information uwsgi.ini

```yaml
[uwsgi]
; 监听的端口
http = :8000 
socket = 0.0.0.0:9000 

; 项目所在目录，和manage.py同级
chdir = /root/xhs

; 虚拟环境所在目录 一般使用绝对路径
;home = /Users/joker/joker_works/django1.11.11_py3/
;PYTHONHOME = /Users/joker/joker_works/django1.11.11_py3/bin/

; 主应用中的wsgi文件 当访问时 出现异常 internet server error 1. 或许使用的pip(python2的) 改为pip3 使用pip3 报错 使用绝对路径
wsgi-file = /root/xhs/xiaohongshu_django/wsgi.py

; 启动一个master进程，来管理其余的子进程
master = True
processes = 4
threads = 2

; 代理静态资源：路径映射
static-map = /static=/root/xhs/static

; 保存主进程的pid，用来控制uwsgi服务
pidfile = uwsgi.pid

; 启动项目  uwsgi -ini uwsgi.ini
; uwsgi --stop/reload xxx.pid  停止/重启uwsgi

; 设置后台运行，保存日志
daemonize = uwsgi.log
```

## 将项目拉取到xshell 会使用 docker 配置MySQL

```yaml
1. 将项目 拉取到xshell
2. 查看docker是否安装好
	docker images
	2.1 如果没有 那么安装
		apt install docker.io

3. 安装好docker之后 下载mysql:5.7 我们会使用mysql5.7 部署
	docker pull mysql:5.7
	启动: docker run -itd -e MYSQL_ROOT_PASSWORD=password -p 3307:3306 mysql:5.7
	注意: -itd 后台交互 -p 端口映射 注意此时先把安全组的端口通过,然后将settings.py里面的改为当前的

4. 安装redis:latest 我们会使用redis:latest 部署
	docker pull redis:latest
	启动: docker run -itd -p 6399:6379 redis:latest
	注意: redis默认端口6379 映射后 安全组端口通过 然后将settings.py里面修改为当前的

5. 配置uwsgi
	使用pip3 安装 如果项目中已经有 requirements.txt
	pip3 install -r requirenemts.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

6. 根据上面的提示 uwsgi 所需配置完善
	启动: uwsgi --ini uwsgi.ini
	停止: uwsgi --stop uwsgi.pid
	重启: uwsgi --reload uwsgi.pid
	# 可根据绝对路径开启
		uwsgi --ini /home/ubuntu/shop_mall/uwsgi.ini

7. 查询uwsgi的开启情况
	查询进程: ps -aux|grep uwsgi
	杀死进程: kill -9 ...

8. 使用域名部署
	8.1 首先 域名解析
	8.2 域名映射 跨域
	8.3 开启域名 80 端口 权限sudo
		sudo nohup python3 manage.py runserver 0.0.0.0:80
```

### **pip加速源**
> -i https://pypi.tuna.tsinghua.edu.cn/simple # pip加速源

hey miss liu this rose is for you
