---
layout: post
title: "Fading in UIImageView"
date: 2013-03-09 17:43
comments: true
categories: [iOS, UIImageView, animations, CAAnimation, Fade in, CATransition, CALayer, Quartz]
description: Fading in images as they load improve greatly user experience.
---
It is always better to fade in image as they load, as this leads to great user experience. It is much better than just have the images abruptly appear. In order to achieve this we can make use of [CATransition](https://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CATransition_class/Introduction/Introduction.html) which is a subclass
of [CAAnimation](https://developer.apple.com/library/ios/#documentation/graphicsimaging/reference/CAAnimation_class/Introduction/Introduction.html#//apple_ref/occ/cl/CAAnimation).

If you are fetching remote images, a great framework to use is [SDWebImage](https://github.com/rs/SDWebImage). The following
sample code assumes you are using this framework.

{% codeblock lang:objc %}
// We need this import as we'll be adding the animation to the UIView's layer
#import <QuartzCore/QuartzCore.h>

// Let's say we have UIImageView *imageView 
[imageView setImageWithURL:[NSURL URLWithString:@"http://example.com/example.png"] completed:^(UIImage *image, NSError *error, SDImageCacheType cacheType) {
    if (image) {
        CATransition *transition = [CATransition animation];
        transition.type = kCATransitionFade; // there are other types but this is the nicest
        transition.duration = 0.34; // set the duration that you like
        transition.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];
        [imageView.layer addAnimation:transition forKey:nil];
    }
}];
{% endcodeblock %}
You can also customize the direction of the transition using the *subtype* property of the CATransition.

This is a quite nice feature if you have a UITableView displaying lots of images.