---
layout: post
title: "RaspberryPi with PS3 EyeToy"
date: 2014-01-23 10:45
comments: true
categories: [raspberry,pi]
---


![装备](http://ww1.sinaimg.cn/mw1024/7286e2ddjw1ectb384l7sj21kw16g7wh.jpg)

安装和配置  motion package 

<http://chris.gg/2012/07/using-a-ps3-eyetoy-with-the-raspberry-pi/a>

motion的主页

<http://www.lavrsen.dk/foswiki/bin/view/Motion/WebHome>

使用 apple 的airport 将我的raspberryPI 内网ip以及端口映射到外网ip。

可以直接使用外网ip观看捕捉到的视频了。 

为了使用域名来观看 ，我在外网运行web的服务机器上用nginx的反向解析，现在可以直接通过域名来访问。
配置例子：

        server {
            listen  8081;
            server_name your.domain.net www.your.domain.net;
            location / {
            proxy_set_header   X-Real-IP            $remote_addr;
            proxy_pass http://your.snapshot.ip:8081;
            }
        }

a real effect : my Garden

![my garden](http://bilibala.net:8081)

or link here:
<http://bilibala.net/motion.html>
