---
layout: post
title: "ssh agent(key) forwarding on mountain lion"
date: 2013-05-05 18:35
comments: true
categories: [octopress,ssh]
---

连上服务器，想rake deploy一下。结果发现rsync failed ,permissions denied (public key)
<!-- more -->
百思不得其解， 编辑Rakefile文件，添加-v参数到rsync部分。 才发现不是rsync的问题，是ssh的private key没有传递过去。 但为什么呢？
方才想起这机器是新的，应该是没有打开agent forwarding的原因。
爬了下文 ， 只要改 /etc/ssh_config 就好了。
把原来的
	#Host *
	#ForwardAgent no

改成
	Host *
	ForwardAgent yes

一切都好了。 

