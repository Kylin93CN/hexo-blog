---
title: Linux(CentOS)安装nodejs
date: 2018-11-28 17:52:03
tags: nodejs
comments: true
categories:
  - [Linux,Nodejs]
---
## 下载安装包
可以去[nodejs官网](https://nodejs.org/en/download/) 查看最新版本或者自己想要安装的版本
该文编写时，官网最新版本为v10.14.0（2018年11月28日）
``` x86asm
cd /usr/local

wget https://nodejs.org/dist/v10.14.0/node-v10.14.0-linux-x64.tar.xz 
```
## 解压&重命名

``` crmsh
tar xf node-v10.14.0-linux-x64.tar.xz

mv node-v10.14.0-linux-x64 nodejs
```
## 设置环境变量

``` bash
vim /etc/profile	
#set for nodejs
export NODE_HOME=/usr/local/nodejs
export PATH=$NODE_HOME/bin:$PATH
```
使其生效：
``` vim
source /etc/profile
```

通过以下命令查看安装情况：

``` crmsh
node -v
npm -v
```

![nodejs](./images/nodejs.png)