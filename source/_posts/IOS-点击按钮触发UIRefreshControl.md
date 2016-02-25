title: IOS-点击按钮触发UIRefreshControl
date: 2015-12-10 02:05:15
categories: 
- Objective-c
tags: 
- Objective-c
---

	//
	//  ViewController.m
	//  chat-view
	//
	//  Created by yonglang on 14/12/4.
	//  Copyright (c) 2014年 yonglang. All rights reserved.
	//

	#import "ViewController.h"

	@interface ViewController ()

	@property (nonatomic, strong) UIRefreshControl *refreshControl;

	@end

	@implementation ViewController

	- (void)viewDidLoad {
	    [super viewDidLoad];
	    
	    self.navigationItem.rightBarButtonItem = [[UIBarButtonItem alloc] initWithTitle:@"刷新" style:UIBarButtonItemStyleDone target:self action:@selector(refreshButton: )];
	    
	    self.refreshControl = [[UIRefreshControl alloc] init];
	    self.refreshControl.tintColor = [UIColor blueColor];
	    [self.refreshControl addTarget:self
	                            action:@selector(controlEventValueChanged:)
	                  forControlEvents:UIControlEventValueChanged];
	}

	- (void)controlEventValueChanged:(id)sender {
	    if (self.refreshControl.refreshing) {
	        self.refreshControl.attributedTitle = [[NSAttributedString alloc] initWithString:@"刷新中"];
	        
	        [self performSelector:@selector(refreshData) withObject:nil afterDelay:0.5];
	    }
	}

	- (void)refreshButton: (id)sender {
	    [UIView beginAnimations:nil context:nil];
	    [UIView setAnimationDuration:1.0];
	    self.tableView.contentOffset = CGPointMake(0.0, -200.0);
	    [UIView commitAnimations];
	    
	    [self.refreshControl beginRefreshing];
	    self.refreshControl.attributedTitle = [[NSAttributedString alloc]initWithString:@"刷新中"];
	    
	    [self performSelector:@selector(refreshData) withObject:nil afterDelay:2.0];
	}

	- (void)refreshData {
	    [self.refreshControl endRefreshing];
	    self.refreshControl.attributedTitle = [[NSAttributedString alloc]initWithString:@"下拉刷新"];
	}

	- (NSInteger)tableView:(UITableView *)tableView numberOfRowsInSection:(NSInteger)section {
	    return 10;
	}

	- (UITableViewCell *)tableView:(UITableView *)tableView cellForRowAtIndexPath:(NSIndexPath *)indexPath {
	    static NSString *str = @"MyCell";
	    
	    UITableViewCell *cell = [tableView dequeueReusableCellWithIdentifier:str];
	    
	    if (cell == nil) {
	        cell = [[UITableViewCell alloc] initWithStyle:UITableViewCellStyleDefault reuseIdentifier:str];
	    }
	    
	    cell.textLabel.text = @"sdfsdfds";
	    cell.imageView.image = [UIImage imageNamed:@"w.jpg"];
	    cell.detailTextLabel.text = @"23432432";
	    
	    return cell;
	}

	@end


