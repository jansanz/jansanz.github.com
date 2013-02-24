---
layout: post
title: "UITextField and attributedPlaceholder"
date: 2013-02-24 14:04
comments: true
categories: [iOS, UITextField, attributedPlaceholder]
description: Using attributedPlaceholder in UITextField is broken. This blog post shows how to work around the issue
---
I wanted to do a custom format placeholder text in a *UITextField* and found out about the *attributedPlaceholder* property [on the documentation](http://developer.apple.com/library/ios/#documentation/uikit/reference/UITextField_Class/Reference/UITextField.html#//apple_ref/occ/instp/UITextField/attributedPlaceholder).

Great! That should be easy.

{% codeblock lang:objc %}
NSDictionary *textAttributes = 
		@{ NSFontAttributeName : [UIFont italicSystemFontOfSize:15.f] };
NSAttributedString *attributedPlaceholder = 
		[[NSAttributedString alloc] initWithString:@"Placeholder" 
						 	            attributes:textAttributes];
// Assume we already have created UITextField *textField
[textField setAttributedPlaceholder:attributedPlaceholder];
{% endcodeblock %}

So I ran the build and the placeholder should be italic as I wanted, right? **WRONG!**

Apparently, this is a bug. And if you had read until this line, please go and file a bug report to Apple so they can fix this.

Now, there are some workarounds for this problem. The one I ended up using is to subclass *UITextField* and override *drawPlaceholderInRect:*

{% codeblock lang:objc %}
- (void)drawPlaceholderInRect:(CGRect)rect {
	// Set to any color of your preference
	[[UIColor lightGrayColor] setFill];
	// We use self.font.pointSize in order to match the input text's font size
	[self.placeholder drawInRect:rect 
	                    withFont:[UIFont italicSystemFontOfSize:self.font.pointSize]];
}
{% endcodeblock %}

It is possible to make this subclass more general and use *NSAttributedString's* *drawInRect:* if attributedPlaceholder is set. But for my requirements the above snippet was enough.