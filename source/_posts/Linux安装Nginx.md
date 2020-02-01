---
title: Linux安装Nginx
date: 2018-11-27 10:20:59
tags: Nginx
comments: true
categories:
  - [Linux,Nginx]
---

本文参考：
[centOS7.4安装nginx](https://blog.csdn.net/ky1in93/article/details/80131489)


### 1.添加Nginx存储库

要添加CentOS 7 EPEL仓库，请打开终端并使用以下命令：
``` javascript
sudo yum install epel-release
```
### 2.添加Nginx存储库

现在Nginx存储库已经安装在您的服务器上，使用以下yum命令安装Nginx ：

``` javascript
sudo yum install nginx
```
### 3.启动Nginx
Nginx不会自行启动。要运行Nginx，请输入：
``` javascript
sudo systemctl start nginx
```
如果您正在运行防火墙，请运行以下命令以允许HTTP和HTTPS通信：
``` javascript
sudo firewall-cmd --permanent --zone=public --add-service=http 


sudo firewall-cmd --permanent --zone=public --add-service=https


sudo firewall-cmd --reload
```

如果提示

> FirewallD is not running

需要开启防火墙
``` javascript
systemctl start firewalld
```

### 4.Nginx常用命令：
``` javascript
启动：sudo systemctl start nginx
关闭：sudo systemctl stop nginx
重启：sudo systemctl restart nginx
```

### 5.Nginx常用配置：
``` javascript
启动：sudo systemctl start nginx
关闭：sudo systemctl stop nginx
重启：sudo systemctl restart nginx
```

通过
``` javascript
find  / -name nginx.conf

路径为：/etc/nginx/nginx.conf
```

查找nginx的配置文件路径，然后修改配置文件

{% asset_img nginx1.png nginx主配置 %}


{% asset_img nginx2.png hexo配置 %}



### 6.其他：

如果修改nginx的端口，并且防火墙开启的情况下，需要添加端口的配置

``` javascript
firewall-cmd --zone=public --add-port=端口号/tcp --permanent

eg：
firewall-cmd --zone=public --add-port=8089/tcp --permanent

重启防火墙：
firewall-cmd --reload
```






