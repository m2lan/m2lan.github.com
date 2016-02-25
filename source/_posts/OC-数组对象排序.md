title: OC-数组对象排序
date: 2015-12-10 02:06:15
categories: 
- Objective-c
tags: 
- Objective-c
---
## Comparator排序

	NSArray *arr =[array sortedArrayUsingComparator:^NSComparisonResult(id obj1, id obj2) {
        
        HLMessage *msg1 = [[NSClassFromString([obj1 messageClassName]) alloc]initWithManagedObject:obj1];
        HLMessage *msg2 = [[NSClassFromString([obj2 messageClassName]) alloc]initWithManagedObject:obj2];
        
        NSComparisonResult result = [[msg1 timestamp] compare:[msg2 timestamp]];
        
        switch(result) {
            case NSOrderedAscending:
                return NSOrderedAscending;
            case NSOrderedDescending:
                return NSOrderedDescending;
            case NSOrderedSame:
                return NSOrderedSame;
            default:
                return NSOrderedSame;
        }
    }];
    return arr;
## SortDescriptor排序

	NSSortDescriptor *sortDescriptor = [[NSSortDescriptor alloc] initWithKey:@"timestamp" ascending:YES];
	NSArray *sortDescriptors = [NSArray arrayWithObjects:sortDescriptor, nil];
	NSArray *sortArray = [array sortedArrayUsingDescriptors:sortDescriptors];
	    
	return sortArray;



