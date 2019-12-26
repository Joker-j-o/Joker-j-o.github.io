---
layout:     post                    # 使用的布局（不需要改）
title:      project_deploy_nginx             # 标题 
subtitle:   Joker #副标题
date:       2019-12-24              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [python, django]
---

# **uWSGI and Nginx delopy**

## **uWSGI configuration information uwsgi.ini**

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

## **Nginx.conf configuration information**

```yaml
server {
        listen 80;
        server_name ice.qjnubk.club;
        location / {
        # 后端的Web服务器可以通过X-Forwarded-For获取用户真实IP
        proxy_next_upstream error timeout invalid_header http_500 http_502 http_503 http_504;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header Host $http_host;
        proxy_set_header X_Nginx_Proxy true;
        proxy_pass http://ice;
        proxy_redirect off;
        }
}

user www-data;
worker_processes auto;
pid /run/nginx.pid;

events {
	worker_connections 768;
	# multi_accept on;
}

http {
	server {
        listen       80;       #监听端口   <-------------注意 过会在使用浏览器查看效果时，就用这个端口访问
        server_name  0.0.0.0;  #这里写自己的域名（没有就用localhost）
 
        #charset koi8-r;
 
        #access_log  logs/host.access.log  main;
 
        location / {
            root   html;
            index  index.html index.htm;
            uwsgi_pass 127.0.0.1:9000;     
            include uwsgi_params;
            }
        location /static/ {
	    alias  静态资源路径;
	}         
    }

	##
	# Basic Settings
	##

	sendfile on;
	tcp_nopush on;
	tcp_nodelay on;
	keepalive_timeout 65;
	types_hash_max_size 2048;
	# server_tokens off;

	# server_names_hash_bucket_size 64;
	# server_name_in_redirect off;

	include /etc/nginx/mime.types;
	default_type application/octet-stream;

	##
	# SSL Settings
	##

	ssl_protocols TLSv1 TLSv1.1 TLSv1.2; # Dropping SSLv3, ref: POODLE
	ssl_prefer_server_ciphers on;

	##
	# Logging Settings
	##

	access_log /var/log/nginx/access.log;
	error_log /var/log/nginx/error.log;

	##
	# Gzip Settings
	##

	gzip on;
	gzip_disable "msie6";

	# gzip_vary on;
	# gzip_proxied any;
	# gzip_comp_level 6;
	# gzip_buffers 16 8k;
	# gzip_http_version 1.1;
	# gzip_types text/plain text/css application/json application/javascript text/xml application/xml application/xml+rss text/javascript;

	##
	# Virtual Host Configs
	##

	include /etc/nginx/conf.d/*.conf;
	include /etc/nginx/sites-enabled/*;
}


#mail {
#	# See sample authentication script at:
#	# http://wiki.nginx.org/ImapAuthenticateWithApachePhpScript
# 
#	# auth_http localhost/auth.php;
#	# pop3_capabilities "TOP" "USER";
#	# imap_capabilities "IMAP4rev1" "UIDPLUS";
# 
#	server {
#		listen     localhost:110;
#		protocol   pop3;
#		proxy      on;
#	}
# 
#	server {
#		listen     localhost:143;
#		protocol   imap;
#		proxy      on;
#	}
#}
```

> 注意 测试nginx是否开启的时候 关闭uwsgi 在浏览器 输入你的公网ip 默认端口为 80

## **将项目拉取到xshell 会使用 docker 配置MySQL 配置redis**
```python
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
> -i https://pypi.tuna.tsinghua.edu.cn/simple

## **docker MySQL配置 使用的是MySQL:5.7**
```yaml
docker pull mysql:5.7
```
> docker run -itd -e MYSQL_ROOT_PASSWORD=youpassword -p 3307:3306 mysql:5.7
>> 注意: -itd 后台交互, -e 默认用户名 root pwd=youpassword, -p 端口映射 为了远程连接 

## **docker redis配置 使用的是最新版**
```yaml
docker pull redis:latest
```
> docker run -itd -p 6380:6379 redis:latest
>> 注意: -itd 后台交互 -p 端口映射关系

## **Nginx的开启与重启**

```yaml
> 开启: nginx
> 开启: service nginx start
> 重启: nginx -s reload
```

### **pip导出所有的依赖**
```yaml
> pip freeze > requirenemts.txt
```

### **安装所有依赖**
```yaml
> pip install -r requirenemts.txt -i https://pypi.tuna.tsinghua.edu.cn/simple
```

~~hey miss liu this rose is for you too~~
