title: UITabBarController使用
date: 2015-12-10 02:02:13
categories: Objective-c
tags: Objective-c
---

	//
	//  ViewController.m
	//  ceshi
	//
	//  Created by yonglang on 14/11/6.
	//  Copyright (c) 2014年 yonglang. All rights reserved.
	//  图片资源 https://github.com/m2lan/Resource
	//

	#import "ViewController.h"
	#import "OneViewController.h"
	#import "TwoViewController.h"
	#import "ThreeViewController.h"

	@interface MainController () {
		UITabBarController *_tabC;
	}

	@end

	@implementation MainController

	- (void)viewDidLoad {
		[super viewDidLoad];
	    
		_tabC = [[UITabBarController alloc] init];
		[_tabC.tabBar setBackgroundColor:[UIColor clearColor]];
		[_tabC.view setFrame:self.view.frame];
		[self.view addSubview:_tabC.view];
	    
		OneViewController *one = [[OneViewController alloc] init];
		TwoViewController *two = [[TwoViewController alloc] init];
		ThreeViewController *three = [[ThreeViewController alloc] init];
		_tabC.viewControllers = @[one, two, three ];
	    
		[_tabC.tabBar setBackgroundColor:[UIColor whiteColor]];
		NSArray *ar = _tabC.viewControllers;
		NSMutableArray *arD = [NSMutableArray new];
		[ar enumerateObjectsUsingBlock:^(UIViewController *viewController, NSUInteger idx, BOOL *stop)
		{
			UITabBarItem *item = nil;
			switch (idx)
			{
			   case 0:
			   {
			       item = [[UITabBarItem alloc] initWithTitle:@"消息" image:[[UIImage imageNamed:@"tab_recent_nor.png"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal] selectedImage:[[UIImage imageNamed:@"tab_recent_press.png"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]];
			       break;
			   }
			   case 1:
			   {
			       item = [[UITabBarItem alloc] initWithTitle:@"联系人" image:nil tag:1];
			       [item setImage:[[UIImage imageNamed:@"tab_buddy_nor.png"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]];
			       [item setSelectedImage:[[UIImage imageNamed:@"tab_buddy_press.png"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]];
			       break;
			   }
			   case 2:
			   {
			       item = [[UITabBarItem alloc]initWithTitle:@"动态" image:nil tag:1];
			       [item setImage:[[UIImage imageNamed:@"tab_qworld_nor.png"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]];
			       [item setSelectedImage:[[UIImage imageNamed:@"tab_qworld_press.png"] imageWithRenderingMode:UIImageRenderingModeAlwaysOriginal]];
			       break;
			   }
			}
			viewController.tabBarItem = item;
			[arD addObject:viewController];
		}];
		_tabC.viewControllers = arD;
		    
		[[UITabBarItem appearance] setTitleTextAttributes:
		[NSDictionary dictionaryWithObjectsAndKeys:[UIColor colorWithRed:96/255.f green:164/255.f blue:222/255.f alpha:1], UITextAttributeTextColor, nil]
		                                   forState:UIControlStateSelected];
		    
		[_tabC setSelectedIndex:1];
	}

	- (void)didReceiveMemoryWarning {
		[super didReceiveMemoryWarning];
		// Dispose of any resources that can be recreated.
	}

	@end


