---
layout:     post                    # 使用的布局（不需要改）
title:      MySQL master-slave mode            # 标题 
subtitle:   Joker #副标题
date:       2019-12-25              # 时间
author:     Joker                     # 作者
header-img: static/img/16.jpg    #这篇文章标题背景图片
catalog: true                       # 是否归档
categories: [docker]
---

# **docker delopy MySQL master-slave**
```s
    # install docker
    apt-get install docker.io
```
> 下载docker

### **Add docker to the user group**
```yaml
    sudo groupadd docker # Add user group
    sudo gpasswd -a $USER docker # Add login user group
    newgrp docker # update user group
    docker ps # test
```
> 将docker 添加用户组 目的就是不想输入 sudo...

### **Download MySQL:5.7 mirror Using MySQL:5.7 deploy MySQL master slave**
```s
    # install mysql5.7
    docker pull mysql:5.7
```
> 下载MySQL:5.7镜像 使用MySQL:5.7部署MySQL主从

### **Make a copy of mysql. Conf.d**
```s
    # 新建一个文件夹
    mkdir mysql_master_slave
    # 将docker里面的/etc/mysql/mysql.conf.d文件夹拷贝
    docker cp (mysql5.7容器id):/etc/mysql/mysql.conf.d ./
```
> 获取mysql:5.7容器的id --> docker ps

### **Get master and slave folders**
```s
    # 拷贝一份主的配置文件
    cp -r mysql.conf.d mysql.conf.d-master
    # 拷贝一份从的配置文件
    cp -r mysql.conf.d mysql.conf.d-slave
```
> 得到主从的配置文件夹

### **Modify the master configuration**
```s
    # 在mysql.conf.d-master文件夹下 修改mysqld.cnf
    vim mysqld.cnf
    # 在最后面 添加
    server-id=1
    log-bin=/var/log/mysql/mysql-bin.log
```
> 修改主的配置文件

### **Modify the slave configuration**
```s
    # 在mysql.conf.d-slave文件夹下 修改mysqld.cnf
    vim mysqld.cnf
    # 在最后面 添加
    server-id=2
    general_log = 0
```
> 修改从的配置文件

### **Create two folders to share files**
```s
    # 创建 主 共享文件 mysql-master-db
    mkdir mysql-master-db
    # 创建 从 共享文件 mysql-slave-db
    mkdir mysql-slave-db
```
> 创建共享文件 目的就是当退出后 数据不会丢失

### **Run docker to open mysql master**
```s
    # 执行开启 主 MySQL的 共享文件 端口映射3307
    docker run -itd --rm -v /home/ubuntu/mysql_master_slave/mysql.conf.d-master/:/etc/mysql/mysql.conf.d -v /home/ubuntu/mysql_master_slave/mysql-master-db/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=youpassword -p 3307:3306 mysql:5.7
    # 执行开启 主 MySQL的 共享文件 端口映射3308
    docker run -itd --rm -v /home/ubuntu/mysql_master_slave/mysql.conf.d-slave/:/etc/mysql/mysql.conf.d -v /home/ubuntu/mysql_master_slave/mysql-slave-db/:/var/lib/mysql -e MYSQL_ROOT_PASSWORD=youpassword -p 3308:3306 mysql:5.7
```
> 测试连接 mysql -uroot -pyoupasword -h 127.0.0.1 --port=3307 主从端口不一样 自己换即可

### **master configuration**
```s
    -- 创建从机账号
    GRANT REPLICATION SLAVE ON *.* TO 'slave'@'%' identified by 'slave';
    -- 刷新权限
    FLUSH PRIVILEGES;
    -- 查看主机的二进制信息;
    SHOW MASTER STATUS;
```

### **The slave machine is connected to the main machine**
```s
    # 注意 当前的 master_host='172.....' 是要先获取的
    change master to master_host='172.18.0.4', master_port=3306, master_user='slave', master_password='slave',master_log_file='mysql-bin.000003', master_log_pos=582;
        
    -- 开启从机服务
    start slave;
    -- 展示从机服务状态
    show slave status \G
```
> 获取master_host方式: docker network ls 会有一个是bridge的id
>> 然后 使用 docker network inspect (bridge的id)
>>> 最后 检查从服务器的容器id 开头部分一样的 注意观察

### ok
```s
    # 当出现以下 说明成功
    Slave_IO_state: Waiting for master to send event
    Slave_IO_Running: Yes
    Slave_SQL_Running: Yes
```

~~Hey Miss Liu This Rose Is For You~~