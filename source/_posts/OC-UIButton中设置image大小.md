title: OC-UIButton中设置image大小
date: 2015-12-10 02:08:49
categories: Objective-c
tags: Objective-c
---

	#import <UIKit/UIKit.h>

	@interface UIImage (Category)

	- (UIImage*)transformWidth:(CGFloat)width
	                    height:(CGFloat)height;

	@end
	
	
	#import "UIImage+Category.h"

	@implementation UIImage (Category)

	- (UIImage*)transformWidth:(CGFloat)width
	               height:(CGFloat)height {
	    
		CGFloat destW = width;
		CGFloat destH = height;
		CGFloat sourceW = width;
		CGFloat sourceH = height;
		    
		CGImageRef imageRef = self.CGImage;
		CGContextRef bitmap = CGBitmapContextCreate(NULL,
		                                           destW,
		                                           destH,
		                                           CGImageGetBitsPerComponent(imageRef),
		                                           4*destW,
		                                           CGImageGetColorSpace(imageRef),
		                                           (kCGBitmapByteOrder32Little | kCGImageAlphaPremultipliedFirst));
		    
		CGContextDrawImage(bitmap, CGRectMake(0, 0, sourceW, sourceH), imageRef);
		    
		CGImageRef ref = CGBitmapContextCreateImage(bitmap);
		UIImage *resultImage = [UIImage imageWithCGImage:ref];
		CGContextRelease(bitmap);
		CGImageRelease(ref);
		    
		return resultImage;
	}

	@end


