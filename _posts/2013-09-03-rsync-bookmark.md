---
layout: post
title: "记一次rsync旅行"
date: 2013-09-03 17:56
comments: true
categories: [rsync]
---

简单配置如下:

	# Distributed under the terms of the GNU General Public License v2
	# Minimal configuration file for rsync daemon
	# See rsync(1) and rsyncd.conf(5) man pages for help

	# This line is required by the /etc/init.d/rsyncd script
	pid file = /var/run/rsyncd.pid
	port = 873
	address = 0.0.0.0
	uid = nobody
	gid = nogroup

	use chroot = no
	read only = no

	#limit access to private LANs
	hosts allow= 11.11.17.11 # 用空格分割多个ip
	hosts deny=*

	max connections = 5

	#This will give you a separate log file
	log file = /var/log/rsync.log

	#This will log every file transferred - up to 85,000+ per user, per sync
	#transfer logging = yes

	log format = %t %a %m %f %b
	syslog facility = local3
	timeout = 300

	[rsync]
	path=/home/some/path
	list=no
	#ignore errors
	#auth users = linuxsir
	#secrets file = /etc/rsyncd/rsyncd.secrets
	#comment = linuxsir home
	#exclude =   beinan/  samba/


启动命令:

/usr/bin/rsync --no-detach --daemon --config /etc/rsyncd.conf

测试rsync:

rsync -avz /some/path/ locahost::rsync
