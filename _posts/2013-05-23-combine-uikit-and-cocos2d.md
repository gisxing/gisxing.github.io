---
layout: post
title: "让UIKit (XIB) 和 cocos2d 一齐工作"
date: 2013-05-23 20:56
comments: true
categories: [iOS, cocos2d]
---


突然想在cocos2d 的 CCLayer中，使用xib编辑好的界面，直接覆盖在CCLayer中，可以减少界面编辑时候的工作。

这里有个传送门 <http://www.raywenderlich.com/4817/how-to-integrate-cocos2d-and-uikit>

<!--more-->

这里简单做下笔记：

1. 修改main.m , 不要调用原来的AppDelegate来构造 UIApplicaitonMain  ,改用调用 info.plist里面的Main nib base name 指向的xib文件来构造。 
2. 创建一个 xib （就是Main nib base name 的键值）文件，把诸如object , window , delegate那些东西都连接好。
3. 在原来的appDelegate.h 里面加上 


	
		@property (nonatomic, strong) IBOutlet UIWindow *window; 
		
	appDelegate.m 里把原来创建 CCGlview , director , push scene那些全部都去µ?? ，现在我们要换成 load xib , 放到 application: didFinishLaunchingWithOptions: 里面
		
		navController_ = [[UINavigationController alloc] initWithRootViewController:director_];
		navController_.navigationBarHidden = YES;
    
    	myViewController *rv = [[myViewController alloc] initWithNibName:@"myViewController" bundle:nil];
    	rv.title = @"test";
    
   		[navController_ pushViewController:rv animated:YES];
    
		// set the Navigation Controller as the root view controller
    	[window_ setRootViewController:navController_];

		// make main window visible
		[window_ makeKeyAndVisible];

4. 用 xib 创建好上面代码中的 myViewController.xib . 在myViewController中创建代码 ， load cocos2d的scene (就是把刚才删掉的那些补上去)关键代码是这句：


	
		[self.view insertSubview:glView atIndex:0];
		
		
	就是把cocos2d的view 当成一个Subview 加进去。
	
5. all done , 剩下的内容是讲如何让 view 上的UIKit的东西和 cocos2d CCLayer上的东西进行通信：方法可以有很多 。 原文里用到的是：

	view --> CCLyaer:

		- (IBAction)buttonTap:(id)sender {
		    CCScene * scene = [[CCDirector sharedDirector] runningScene];
		    PlayfieldLayer *layer = [scene.children objectAtIndex:0];
		    if([layer respondsToSelector:@selector(clickFromController)]) {
		        //绑定 layer 的 controller 
		        layer.controller = self;
		        [layer clickFromController];
		    }
		} 
		
	CCLayer --> view:
	可以在创建CScene CCLayer 的时候，把 当前view 的一个引用传入，假如scene中有多次的push切换，那么就把这个引用传递下去 。 我的做法是subClass 一下CCLayer  . 添加一个 myViewController * 的属性，那么想设置的时候自己用setter 来设置就好了。  调用的时候同样最好自省一下。
				
		if ([self.controller respondsToSelector:@selector(clickFromLayer)]) {
            [self.controller clickFromLayer];
        }
