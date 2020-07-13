---
layout:   post          # 使用的布局（不需要改）
title:   Win Remote connection MySQL       # 标题 
subtitle:  Bree #副标题
date:    2020-07-10       # 时间
author:   Bree           # 作者
header-img: static/img/16.jpg  #这篇文章标题背景图片
catalog: true            # 是否归档
categories: [python, django]
---





# <font color="LightGreen">windows 下 使用 navicat 远程连接MySQL</font>

#### Tip:

```s
在Ubuntu下的MySQL Windows下使用工具连接
```



##  <font color=" LightSteelBlue ">1. 登录远程MySQL</font>

```shell
mysql -uroot -p
```

##  <font color=" LightSteelBlue ">2. 授权</font>

```bash
案例:
GRANT ALL PRIVILEGES ON *.* TO '你的名字(root)'@'%' IDENTIFIED BY 'YourPassword' WITH GRANT OPTION;
示例:
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY 'Bree' WITH GRANT OPTION;
```

##  <font color=" LightSteelBlue ">3. 刷新权限</font>

```bash
FLUSH PRIVILEGES;
```

##  <font color=" LightSteelBlue ">4. 查看权限是否通过</font>

```shell
SELECT DISTINCT CONCAT('User: ''',user,'''@''',host,''';') AS query FROM mysql.user;
```

> 权限通过结果

```bash
+-------------------------------------------------------+
| query                                                 |
+-------------------------------------------------------+
| User: 'root'@'%';                                     |
| User: 'debian-sys-maint'@'localhost';                 |
| User: 'mysql.session'@'localhost';                    |
| User: 'mysql.sys'@'localhost';                        |
| User: 'root'@'localhost';                             |
+-------------------------------------------------------+
```

##  <font color=" LightSteelBlue ">5. 退出数据库</font>

```bash
exit 或者 quit
```

##  <font color=" LightSteelBlue ">6. 打开 /etc/mysql/mysql.conf.d/mysqld.cnf  ------- ubuntu16.04的位置</font>

```bash
使用 vim 打开 修改文件中的 绑定地址 注意 最开始是 127.0.0.1的 修改为 0.0.0.0
 41 # Instead of skip-networking the default is now to listen only on
 42 # localhost which is more compatible and is not less secure.
 43 # bind-address          = 127.0.0.1
 44 bind-address=0.0.0.0
 45 #
 46 # * Fine Tuning
 47 #
 在进入 vim 后 可以使用此命令 打开 行数 / 关闭行数也是此命令
 : set number
```

##  <font color=" LightSteelBlue ">7. 重启MySQL服务</font>

```bash
sudo /etc/init.d/mysql stop
[ ok ] Stopping mysql (via systemctl): mysql.service.

sudo /etc/init.d/mysql start
[ ok ] Starting mysql (via systemctl): mysql.service.
```

> 使用 Navicat 连接即可