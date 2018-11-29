---
title: H5在线直播
date: 2018-11-28 17:52:03
tags: 
  - nodejs
  - flvjs
  - 直播
  - obs
  - Node-Media-Server
comments: true
categories:
  - [直播]
---
# watch-live-videos

###### 使用 [Node-Media-Server](https://github.com/illuspas/Node-Media-Server) + [flvjs](https://github.com/Bilibili/flv.js) 完成在线直播！

 1. 准备工作

①、[obs](https://obsproject.com/)
②、nodejs环境
③、服务器(本地亦可)

 2. 使用flvjs播放mp4和flv
 
 可见 [play-flv.html](https://github.com/Kylin93CN/watch-live-videos/blob/master/demo/other/play-flv.html)  和 [play-mp4.html](https://github.com/Kylin93CN/watch-live-videos/blob/master/demo/other/play-mp4.html)
 
 其中flv需要注意视频的编码，目前flvjs仅支持 H.264 + AAC / MP3
 非此类型的会报错:
 ![flv-video](./images/play-error-flv.png)
[详情可见 https://github.com/Bilibili/flv.js#features](https://github.com/Bilibili/flv.js#features)

转码借助于 ffmpeg，安装请自行百度。
安装之后使用：

``` stylus
ffmpeg -i 目标文件名.flv -vcodec libx264 -c:a aac   转码后文件名.flv

eg:
ffmpeg -i dy.flv -vcodec libx264 -c:a aac   dy-codec.flv
```
3. 创建直播流服务器&使用obs推流

主要借助于Node-Media-Server 这个库。

``` crmsh
mkdir nms
cd nms
git clone https://github.com/illuspas/Node-Media-Server
cd Node-Media-Server
npm i
node app.js
```

启动服务后，通过obs配置流:
设置——流——流类型选择自定义——url为rtmp://XX.XX.XX.XX/live——流名称自己定义。确定之后点击“开始推流”
服务端会有信息打印出来。说明连接成功。

 ![flv-video](./images/obs-setting.png)
 
 4.播放直播流
 
 通过本地搞个[html](https://github.com/Kylin93CN/watch-live-videos/blob/master/demo/live/local.html)
 
url: 'http://XX.XX.XX.XX:8000/live/STEAM_NAME.flv'  改为对应的ip和流地址  即可播放。

服务器端需要注意防火墙和端口的开启，以及阿里云或者其他云的端口安全规则。

CentOS 系统可以参考  [linux常用命令【centOS7.4】](https://blog.csdn.net/ky1in93/article/details/80105320)中防火墙开启关闭以及端口开启的部分。

