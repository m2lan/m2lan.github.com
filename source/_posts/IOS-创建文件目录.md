title: IOS-创建文件目录
date: 2015-12-10 02:07:32
categories: Objective-c
tags: Objective-c
---
	//
	//  ViewController.m
	//  chat-view
	//
	//  Created by yonglang on 14/12/4.
	//  Copyright (c) 2014年 yonglang. All rights reserved.
	//

	#import "ViewController.h"

	static NSString *homeDir = nil;

	@interface ViewController ()

	@end

	@implementation ViewController

	- (void)viewDidLoad {
	    [super viewDidLoad];
	    
	    NSLog(@"audioDir: %@", [self createFileDirectory]);
	}

	- (NSString *)createFileDirectory {
	    if (!homeDir) {
	        NSArray *paths = NSSearchPathForDirectoriesInDomains(NSDocumentDirectory, NSUserDomainMask, YES);
	        homeDir = [[paths objectAtIndex:0] stringByAppendingPathComponent:[NSString stringWithFormat:@"1321312321/home"]];
	        
	        NSFileManager *fileManager = [NSFileManager defaultManager];
	        if (![fileManager fileExistsAtPath:homeDir]) {
	            [fileManager createDirectoryAtPath:homeDir withIntermediateDirectories:YES attributes:nil error:nil];
	        }
	    }
	    return homeDir;
	}

	@end


