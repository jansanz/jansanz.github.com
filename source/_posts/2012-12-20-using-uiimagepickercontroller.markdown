---
layout: post
title: "Using UIImagePickerController"
date: 2012-12-20 16:50
comments: true
categories: [iOS, UIImagePickerController]
description: Quick Tutorial on how to use UIImagePickerController
---
Have you ever found the need in your app to let the user take a Picture or choose one from the Camera Roll? Follow and find
how easy is to add this feature to your app.

The iOS SDK provides a native controller that handles the hard bits for you. This controller is *[UIImagePickerController](http://developer.apple.com/library/ios/#documentation/uikit/reference/UIImagePickerController_Class/UIImagePickerController/UIImagePickerController.html)*.

{% codeblock lang:objc %}
UIImagePickerController *pickerController = [[UIImagePickerController alloc] init];
pickerController.sourceType = UIImagePickerControllerSourceTypeCamera;
pickerController.delegate = self;
[self presentViewController:imagePickerController 
                   animated:YES 
                 completion:NULL];
{% endcodeblock %}

In this particular example it will bring the camera. There are other options as well.

{% codeblock lang:objc %}
// This will bring the Camera
pickerController.sourceType = UIImagePickerControllerSourceTypeCamera;
// Option for showing the photo library
pickerController.sourceType = UIImagePickerControllerSourceTypePhotoLibrary;
// This will just show the photos / videos view
pickerController.sourceType = UIImagePickerControllerSourceTypeSavedPhotosAlbum;
{% endcodeblock %}

Note that the last two options require users to allow the app to access their photos / videos.

Don't forget to check if the source type is available:

{% codeblock lang:objc %}
if ([UIImagePickerController isSourceTypeAvailable:UIImagePickerControllerSourceTypeCamera]) {
  // Set sourceType to UIImagePickerControllerSourceTypeCamera
  // and present controller
}
{% endcodeblock %}

If you do not care about movies, just still images we can set the media typpe to *[kUTTypeImage](http://developer.apple.com/library/ios/#documentation/MobileCoreServices/Reference/UTTypeRef/Reference/reference.html#//apple_ref/c/data/kUTTypeImage)*.
{% codeblock lang:objc %}
#import <MobileCoreServices/MobileCoreServices.h>
// Don't forget to add MobileCoreServices.framework to the project
pickerController.mediaTypes = @[(NSString *)kUTTypeImage];
{% endcodeblock %}

This class also supports the ability to let users crop and scale pictures. This is very useful if want to get square pics, for instance to update a social profile picture.
{% codeblock lang:objc %}
pickerController.allowsEditing = YES;
{% endcodeblock %}

To get the actual image that users picked we implement one of *[UIImagePickerControllerDelegate](http://developer.apple.com/library/ios/#documentation/uikit/reference/UIImagePickerControllerDelegate_Protocol/UIImagePickerControllerDelegate/UIImagePickerControllerDelegate.html)* methods.
{% codeblock lang:objc %}
- (void)imagePickerController:(UIImagePickerController *)picker
didFinishPickingMediaWithInfo:(NSDictionary *)info
{
  [self dismissViewControllerAnimated:YES completion:NULL];
  // UIImagePickerControllerEditedImage is useful if you allowed
  // editing of images
  UIImage *image = [info objectForKey:UIImagePickerControllerEditedImage];
  if (image == nil)
    image = [info objectForKey:UIImagePickerControllerOriginalImage];
  
  // Do something with the image

}
{% endcodeblock %}

As you can see, *UIImagePickerController* is quite easy to implement and it has lots of options that you can make use of.
It also allows you to set a custom overlay and define your own camera buttons.

This sample code can be found in my [GitHub repo](https://github.com/jansanz/UIImagePickerController-Demo).
