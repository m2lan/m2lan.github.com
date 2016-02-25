title: OC-检测视图退出
date: 2015-12-10 02:10:15
categories: Objective-c
tags: Objective-c
---

	-(void) viewWillDisappear:(BOOL)animated {
	    if ([self.navigationController.viewControllers indexOfObject:self]==NSNotFound) {
	       // back button was pressed.  We know this is true because self is no longer
	       // in the navigation stack.  
	    }
	    [super viewWillDisappear:animated];
	}


