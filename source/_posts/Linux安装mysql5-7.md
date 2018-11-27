---
title: Linux安装mysql5.7
date: 2018-11-27 17:18:47
tags: Mysql
comments: true
categories:
  - [Linux,Mysql]
---

本文参考：[linux 安装MySql 5.7.20 操作步骤【亲测】](https://segmentfault.com/a/1190000012703513)

由于mysql 5.7.17版本以后 support_files文件夹中无 my_default.cnf 文件，所以今天给大家详细描述一下 mysql 5.7.20版本(目前官方最新版)的安装步骤。

第一步：下载mysql最新版
``` shell
cd /usr/local
wget http://dev.mysql.com/get/Downloads/MySQL-5.7/mysql-5.7.20-linux-glibc2.12-x86_64.tar.gz
```
第二步：在/usr/local/中解压压缩包，并改名为mysql
``` shell
tar -zxvf mysql-5.7.20-linux-glibc2.12-x86_64.tar.gz
mv mysql-5.7.20-linux-glibc2.12-x86_64 mysql
```
第三步：创建用户组mysql，创建用户mysql并将其添加到用户组mysql中，并赋予读写权限
``` shell
groupadd mysql

useradd -r -g mysql mysql

chown -R mysql mysql/

chgrp -R mysql mysql/
```
第四步：创建配置文件并保存

>vim /etc/my.cnf
``` javascript
[client]
port = 3306
socket = /tmp/mysql.sock

[mysqld]
character_set_server=utf8
init_connect='SET NAMES utf8'
basedir=/usr/local/mysql
datadir=/usr/local/mysql/data
socket=/tmp/mysql.sock
log-error=/var/log/mysqld.log
pid-file=/var/run/mysqld/mysqld.pid
#不区分大小写
lower_case_table_names = 1

sql_mode=STRICT_TRANS_TABLES,NO_ZERO_IN_DATE,NO_ZERO_DATE,ERROR_FOR_DIVISION_BY_ZERO,NO_AUTO_CREATE_USER,NO_ENGINE_SUBSTITUTION

max_connections=5000

default-time_zone = '+8:00'
```
第五步：初始化数据库

``` shell
#先安装一下这个东东，要不然初始化有可能会报错
yum install libaio
#手动编辑一下日志文件，什么也不用写，直接保存退出
cd /var/log/

vim mysqld.log
：wq

chmod 777 mysqld.log
chown mysql:mysql mysqld.log

/usr/local/mysql/bin/mysqld --initialize --user=mysql --basedir=/usr/local/mysql --datadir=/usr/local/mysql/data --lc_messages_dir=/usr/local/mysql/share --lc_messages=en_US
```
第六步：查看初始密码

``` shell
cat /var/log/mysqld.log
```
执行后关注最后一点：root@localhost: 这里就是初始密码

第七步：启动服务，进入mysql，修改初始密码，运行远程连接（这里执行完后，密码将变成：你设置的新密码）

``` shell
#然后执行如下操作开启mysql服务，以及设置相关权限
cd /var/run/

mkdir mysqld

chmod 777 mysqld

cd mysqld

vim mysqld.pid

chmod 777 mysqld.pid

chown mysql:mysql mysqld.pid 

/usr/local/mysql/support-files/mysql.server start

/usr/local/mysql/bin/mysql -uroot -p 你在上面看到的初始密码

# 以下是进入数据库之后的sql语句
use mysql;
set password=password('新密码');

flush privileges;
 
UPDATE `mysql`.`user` SET `Host` = '%',  `User` = 'root'  WHERE (`Host` = 'localhost') AND (`User` = 'root');
 
flush privileges;
```

第八步：开机自启

``` shell
cd /usr/local/mysql/support-files

cp mysql.server /etc/init.d/mysqld

chkconfig --add mysqld
```
第九步：使用service mysqld命令启动/停止服务

``` crmsh
su - mysql

service mysqld start/stop/restart
```
第十步：远程用户建立
``` crmsh
grant all privileges on *.* to '新用户名'@'%' identified by '新密码';

flush privileges;
```

最后：添加系统路径
``` crmsh
vim /etc/profile

export PATH=/usr/local/mysql/bin:$PATH

source /etc/profile
```

其他：

远程连接且防火墙开启时，需注意3306端口的开启

永久开启3306端口，并重启防火墙使其生效

``` dockerfile
firewall-cmd --zone=public --add-port=3306/tcp --permanent

firewall-cmd --reload
```

日常登录，输入密码即可！

``` ebnf
mysql -uroot -p
```
