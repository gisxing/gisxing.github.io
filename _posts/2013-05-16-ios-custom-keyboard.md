---
layout: post
title: "iOS 自定义键盘"
date: 2013-05-16 09:56
comments: true
categories: [iOS]
---

关于在iOS中如何使用一个特殊键盘，主要考虑点是用户体现更佳，无论是弹出还是切换键盘都要smooth。这里说的特殊键盘不是指完全构造一个自己的键盘，而是在标准的iOS键盘上添加（覆盖）按钮等。


主要表现两点：

1. 当屏幕没有键盘显示框的时候，把特殊键盘（特殊按钮）准备好。实现底部滑出键盘的时候平滑效果，不是生硬的把（按键）添加上去。
2. 当屏幕已经有键盘显示框的时候，用户点击其他的文本输入框，跳转到其他键盘（特殊或默认的键盘），此时需要根据实际情况控制（特殊按键）的显示或者删除。
<!-- more -->


针对以上两点，主要对应手段：

1. 注册一个对UIKeyboardWillShowNotification监听的observer , 当键盘弹出的时候触发添加按钮（制作特殊键盘）的方法。
2. 通过判定button的tag，以及相应逻辑决定是否要添加、删除一些特殊的键。 注意删除或添加键时，我们主要通过button.tag 来获取是否存在这个键，再作后面的逻辑。 

okay , talk is cheap , show me the codes: 

1. 第一种情况的核心代码: （我们往标准数字键盘的左下角添加一个Done按钮）
		- (void)textFieldDidBeginEditing:(UITextField *)textField{
		    ...
		    // 为数字键盘添加 完成按钮
	    	if (self.card_sn == textField || self.card_pass == textField) {
	        	// need to hide the keyboard to get UIKeyboardDidShowNotification again !
	        	// so we will remove this observer in some where later;
	        	// 注意ios版本的不同，注册消息的名字有所不同，假如你不需要理会 3.2- 的设备，忽略这个判定
	        	if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 3.2) {
	            	[[NSNotificationCenter defaultCenter] addObserver:self
	                   		                                 selector:@selector(keyboardDidShow:)
	                	   	                                     name:UIKeyboardDidShowNotification
	                   	    	                               object:nil];
	        	} 
	        	else {
	            	[[NSNotificationCenter defaultCenter] addObserver:self
	                                                         selector:@selector(keyboardWillShow:)
	                                                             name:UIKeyboardWillShowNotification
	                                                           object:nil];
	        	}
	        	[self addDoneButtonToKeyboard];	    //直接添加按钮的函数（函数里面我们会先判定是否已经添加了键）
	    	}
	    	else
	        	[self removeKeyboardDoneButton];  //删掉这个键
	        }	
        }	
        	
		- (void)keyboardWillShow:(NSNotification *)note {
			// if clause is just an additional precaution, you could also dismiss it
			if ([[[UIDevice currentDevice] systemVersion] floatValue] < 3.2) {
				[self numberPadWithDoneButton];
			}
		}
		
		- (void)keyboardDidShow:(NSNotification *)note {
			// if clause is just an additional precaution, you could also dismiss it
			if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 3.2) {
				[self numberPadWithDoneButton];
		    }
		}
		
		-(void)numberPadDoneButtonClicked:(id) sender{
		    // 按了完成键以后的逻辑
		}
    
		- (void)numberPadWithDoneButton{
		    //键盘框准备弹出来的情况
		    // create custom button
		    UIButton *doneButton = [UIButton buttonWithType:UIButtonTypeCustom];
		    doneButton.frame = CGRectMake(0, 163, 106, 53);
		    doneButton.adjustsImageWhenHighlighted = NO;
		    [doneButton setImage:[UIImage imageNamed:@"confirm.png"] forState:UIControlStateNormal];
		    [doneButton setImage:[UIImage imageNamed:@"confirm-hover.png"] forState:UIControlStateHighlighted];
		    [doneButton addTarget:self action:@selector(numberPadDoneButtonClicked:) forControlEvents:UIControlEventTouchUpInside];
		    doneButton.tag = 1;	    //这里我们给这个button 添加一个tag的标识，稍后的逻辑可以使用
		    
		    UIWindow* tempWindow = [[[UIApplication sharedApplication] windows] objectAtIndex:1];
		    UIView* keyboard;
		    
		    for(int i=0; i<[tempWindow.subviews count]; i++) {
		        keyboard = [tempWindow.subviews objectAtIndex:i];
		        if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 3.2) {
		            if([[keyboard description] hasPrefix:@"<UIPeripheralHost"] == YES) {
		                [keyboard addSubview:doneButton];
		                break;
		            }
		        } else {
		            if([[keyboard description] hasPrefix:@"<UIKeyboard"] == YES) {
		                [keyboard addSubview:doneButton];
		                break;
		            }
		        }
		        
		    }
		    
		}
	
		- (BOOL)textFieldShouldReturn:(UITextField *)textField{
			//删除我们的 observer
		    [[NSNotificationCenter defaultCenter] removeObserver:self];
	    }
	    //特别地:
	    -(void) viewWillDisappear:(BOOL)animated {
		    [super viewWillDisappear:animated];
		    //删除通知检测
		    [[NSNotificationCenter defaultCenter] removeObserver:self];
		}
	
	
2. 第二种情况的核心代码:		
 		
		- (void)addDoneButtonToKeyboard{
		    //键盘框已经弹出的情况
		    if ([[[UIApplication sharedApplication] windows] count] <= 1) {
		        //还没有键盘框，直接返回
		        return;
		    }
		    // create custom button
		    UIButton *doneButton = [UIButton buttonWithType:UIButtonTypeCustom];
		    doneButton.frame = CGRectMake(0, 163, 106, 53);
		    doneButton.adjustsImageWhenHighlighted = NO;
		    [doneButton setImage:[UIImage imageNamed:@"confirm.png"] forState:UIControlStateNormal];
		    [doneButton setImage:[UIImage imageNamed:@"confirm-hover.png"] forState:UIControlStateHighlighted];
		    [doneButton addTarget:self action:@selector(numberPadDoneButtonClicked:) forControlEvents:UIControlEventTouchUpInside];
		    doneButton.tag = 1;
		    
		    UIWindow* tempWindow = [[[UIApplication sharedApplication] windows] objectAtIndex:1];
		    UIView* keyboard;
		    //注意这里也是针对不同iOS版本，需要判定不同的 [keyboard description]
		    for(int i=0; i<[tempWindow.subviews count]; i++) {
		        keyboard = [tempWindow.subviews objectAtIndex:i];
		        if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 3.2) {
		            if([[keyboard description] hasPrefix:@"<UIPeripheralHost"] == YES) {
		                if ([keyboard viewWithTag:1] != nil){
		                    return;
		                }
		                [keyboard addSubview:doneButton];
		                break;
		            }
		        } else {
		            if([[keyboard description] hasPrefix:@"<UIKeyboard"] == YES) {
		                if ([keyboard viewWithTag:1] != nil)
		                     return;
		                [keyboard addSubview:doneButton];
		                break;
		            }
		        }
		        
		    }
		    
		}
		
		#pragma mark 键盘删除完成按钮函数
		// 删除的时候同样要做好判定是否可以删除
		- (void)removeKeyboardDoneButton{
		    if ([[[UIApplication sharedApplication] windows] count] <= 1) {
		        //还没有键盘框，直接返回
		        return;
		    }
		
		    UIWindow* tempWindow = [[[UIApplication sharedApplication] windows] objectAtIndex:1];
		    UIView* keyboard;
		    
		    for(int i=0; i<[tempWindow.subviews count]; i++) {
		        keyboard = [tempWindow.subviews objectAtIndex:i];
		        if ([[[UIDevice currentDevice] systemVersion] floatValue] >= 3.2) {
		            if([[keyboard description] hasPrefix:@"<UIPeripheralHost"] == YES) {
		                // 通过 viewWithTag方法取得我们早前添加的button 
		                [[keyboard viewWithTag:1] removeFromSuperview];
		                break;
		            }
		        } else {
		            if([[keyboard description] hasPrefix:@"<UIKeyboard"] == YES) {
		                [[keyboard viewWithTag:1] removeFromSuperview];
		                break;
		            }
		        }
		        
		    }
		    
		}
		
		
Mission accomplished .
		
