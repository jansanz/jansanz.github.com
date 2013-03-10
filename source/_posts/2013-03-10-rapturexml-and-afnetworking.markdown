---
layout: post
title: "RaptureXML and AFNetworking"
date: 2013-03-10 17:26
comments: true
categories: [AFNetworking, RaptureXML, XML, iOS]
description: Using RaptureXML together with AFNetworking has never been easier.
---
If you are like me, who loves to use [RaptureXML](https://github.com/ZaBlanc/RaptureXML) for parsing XML documents, and to use [AFNetworking](http://afnetworking.com/)
when playing with REST APIs, then this post is for you.

I [created an AFNetworking extension](https://github.com/jansanz/AFRaptureXMLRequestOperation) to make it easy to use RaptureXML.

Here is a sample on how to use it:

{% codeblock lang:objc %}
AFRaptureXMLRequestOperation *operation = [AFRaptureXMLRequestOperation XMLParserRequestOperationWithRequest:[NSURLRequest requestWithURL:[NSURL URLWithString:@"http://legalindexes.indoff.com/sitemap.xml"]] success:^(NSURLRequest *request, NSHTTPURLResponse *response, RXMLElement *XMLElement) {
   // Do something with XMLElement 
} failure:^(NSURLRequest *request, NSHTTPURLResponse *response, NSError *error, RXMLElement *XMLElement) {
    // Handle the error
}];

[operation start];
{% endcodeblock %}

If subclassing AFHTTPClient then we just need to use registerHTTPOperationClass to register it with AFRaptureXMLRequestOperation
{% codeblock lang:objc %}
- (id)initWithBaseURL:(NSURL *)url {
    self = [super initWithBaseURL:url];
    if (!self) {
        return nil;
    }
    
    [self registerHTTPOperationClass:[AFRaptureXMLRequestOperation class]];
    
    // Accept HTTP Header; see http://www.w3.org/Protocols/rfc2616/rfc2616-sec14.html#sec14.1
    [self setDefaultHeader:@"Accept" value:@"text/xml;level=1,application/xml;level=2"];
    
    return self;
}
{% endcodeblock %}

I created this extension several months ago, but recently updated it to use ARC and included some
minor tweaks based on some official extensions.
Feel free to play around with it, fork it and send any pull requests.