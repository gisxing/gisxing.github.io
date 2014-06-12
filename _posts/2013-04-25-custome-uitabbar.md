---
layout: post
title: "UITabBar 自定义"
date: 2013-04-25 22:04
comments: true
categories: [iOS] 
---

虽然苹果的UI已经是无可挑剔，不过我们总想自定义一些现有的苹果 UI 。 例如 UITabBar UITabBarItem。
先来一张图诠释 UITabBar 和 UITabBarItem 的关系：
![image](http://ww3.sinaimg.cn/large/7286e2ddjw1e427f5vpjmj20vo0kijtb.jpg)

直接上代码：
<!-- more -->

	- (BOOL)application:(UIApplication *)application didFinishLaunchingWithOptions:(NSDictionary *)launchOptions
	{
	    self.window = [[UIWindow alloc] initWithFrame:[[UIScreen mainScreen] bounds]];
		// 构造两个vierController， 作为tapBarController的两个tab 对应的 viewController
	    UIViewController *viewController1 = [[FirstViewController alloc] initWithNibName:@"FirstViewController" bundle:nil];
	    UIViewController *viewController2 = [[SecondViewController alloc] initWithNibName:@"SecondViewController" bundle:nil];
		// 构造我们的tabBarController
	    self.tabBarController = [[UITabBarController alloc] init];
		//把刚才的两个viewController装填进去
	    self.tabBarController.viewControllers = @[viewController1, viewController2];
	    

		//获取UITabBar 对象
	    UITabBar *tabbar = self.tabBarController.tabBar;
	    NSLog(@"count:%d", [tabbar.items count]);
	    
		//获取其中一个UITabBarItem
	    UITabBarItem *tabBarItem1 = tabbar.items[0];
		//修改title
	    tabBarItem1.title=@"中文选项";
	    
		//ios5下修改 barItem的图片 (一个是选中状态，一个是非选中状态)
	    [tabBarItem1 setFinishedSelectedImage:[UIImage imageNamed:@"cards_3_8.jpg"] withFinishedUnselectedImage:[UIImage imageNamed:@"first.png"]];
	    
		//修改tab的背景
	    UIImage* tabBarBackground = [UIImage imageNamed:@"btn-blue-long-bg"];
	    [[UITabBar appearance] setBackgroundImage:tabBarBackground];
		//选中后的标识背景
	    [[UITabBar appearance] setSelectionIndicatorImage:[UIImage imageNamed:@"btn-yellow-bg"]];

		//选中后的文字样式修改
	    [[UITabBarItem appearance] setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:
							       [UIColor whiteColor], UITextAttributeTextColor,
							       nil] forState:UIControlStateNormal];
	    UIColor *titleHighlightedColor = [UIColor colorWithRed:153/255.0 green:192/255.0 blue:48/255.0 alpha:1.0];
	    [[UITabBarItem appearance] setTitleTextAttributes:[NSDictionary dictionaryWithObjectsAndKeys:
							       titleHighlightedColor, UITextAttributeTextColor,
							       nil] forState:UIControlStateHighlighted];
	    
	    self.window.rootViewController = self.tabBarController;
	    [self.window makeKeyAndVisible];
	    return YES;
	}

如果使用了storybord来构造tabBarController ,固然可以visual的修改UITabBarItem的title和图标 （默认图标是一个没有颜色的位图，只负责显示默认的蓝色透明带高光效果图标）
不过如果使用了以上代码，将覆盖storyboard中的设定。

最后看一下效果： 这里图片我都是临时拼凑的，大概明白效果就好。 
![image](http://ww3.sinaimg.cn/bmiddle/7286e2ddjw1e4293ux06vj210408c0u8.jpg)

