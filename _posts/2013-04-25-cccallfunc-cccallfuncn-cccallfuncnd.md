---
layout: post
title: "CCCallFunc CCCallFuncN CCCallFuncND 三者区别"
date: 2013-04-25 11:54
comments: true
categories: [cocos2d-iphone] 
---


CCCallFunc CCCallFuncN CCCallFuncND ， 简单理解，三者都是Action 的回调函数， 也可以看作是一个Action来使用。

区别就是是否有带参数，和所带参数形式的区别。
<!-- more -->
1, CCCallFunc , 对应的回调函数，不带任何参数
在动作序列中间或者结束调用某个函数,执行任何需要执行的任务:动作、状态修改等。代码如下:

	-(void) OnCallFunc:(id) sender {
		id ac1 = [CCMoveBy actionWithDuration:2 position:ccp(200, 200)];
		id ac2 = [ac1 reverse];
		id acf = [CCCallFunc actionWithTarget:self selector:@selector(CallBack1)];
		[sprite runAction:[CCSequence actions:ac1, acf, ac2, nil]];
	}

//对应的函数为:(再做一个动作,这就实现了动作、动作序列任意扩展和连接)

	-(void) CallBack1 {
		[sprite runAction:[CCTintBy actionWithDuration:0.5 red:255 green:0 blue:255]];
	}

2, CCCallFuncN 对应的回调函数，带对象参数 ，调用自定义函数时,传递当前对象。

	-(void) OnCallFuncN:(id) sender {
		id ac1 = [CCMoveBy actionWithDuration:2 position:ccp(200, 200)];
		id ac2 = [ac1 reverse];
		id acf = [CCCallFuncN actionWithTarget:self selector:@selector(CallBack2:)];
		[sprite runAction:[CCSequence actions:ac1, acf, ac2, nil]];
	}

//对应的自定义函数:(这里,我们直接使用了该对象)

	- (void) CallBack2:(id)sender {
		[sender runAction:[CCTintBy actionWithDuration:1 red:255 green:0 blue:255]]; 
	}

3, CCCallFuncND 对应的回调函数，带对象、数据参数 调用自定义函数时,传递当前对象和一个常量(也可以是指针)

	-(void) OnCallFuncND:(id) sender {
		id ac1 = [CCMoveBy actionWithDuration:2 position:ccp(200, 200)];
		id ac2 = [ac1 reverse];
		id acf = [CCCallFuncND actionWithTarget:self selector:@selector(CallBack3:data:) data:(void*)2];
		[sprite runAction:[CCSequence actions:ac1, acf, ac2, nil]];
	}

//对应的自定义函数,我们使用了传递的对象和数据:
//注意这里 casting 了 (NSinteger)的类型转换

	-(void) CallBack3:(id)sender data:(void*)data {
		[sender runAction:[CCTintBy actionWithDuration:(NSInteger)data red:255 green:0 blue:255]]; 
	}

着重注意 CCCallFuncN 与 CCCallFunc 的区别：
前者的回调函数名为:
	- (void) CallBack2:(id)sender
sender 表示执行动作序列的 CCSprite 对象~
(欲执行完一系列动作之后自动将sprite移除, 就可以使用这个函数，当然CCCallFuncND也可以)

而后者的回调函数名为：
	- (void) CallBack1
注意这个是不带任何参数的。

后记，顺便学习@selector的运用。

