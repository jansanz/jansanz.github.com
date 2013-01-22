---
layout: post
title: "State of iOS XML Libraries in 2013"
date: 2013-01-22 19:41
comments: true
categories: iOS XML
---
When it comes to XML parsing in iOS, developers usually first stumble upon NSXMLParser. NSXMLParser is a [SAX](http://en.wikipedia.org/wiki/Simple_API_for_XML) Parser included on the iOS SDK. While it works fine, developers usually end up writing tons of lines to parse XML documents.

There are better and faster alternatives to work with. I personally enjoy working with [RaptureXML](https://github.com/ZaBlanc/RaptureXML). There are different factors when deciding which XML libary to choose. Often, the most common is speed.

Currently, there is no much data on speed tests. The usual source for this data is this dated (March 2010) [excellent blog post](http://www.raywenderlich.com/553/how-to-chose-the-best-xml-parser-for-your-iphone-project). On it, the author checks various libraries and performs tests based on [Apple's source](http://developer.apple.com/library/ios/#samplecode/XMLPerformance/Introduction/Intro.html#//apple_ref/doc/uid/DTS40008094-Intro-DontLinkElementID_2).

Since then, several of these libraries have been updated and some new libraries have become available as well. Not to say, that we have now multi-core devices such as the iPhone 5. Therefore, I updated the source code to include the latest versions of the XML libraries and included a few more.

#### Libraries tested
- NSXMLParser
- libxml2
- [TBXML](https://github.com/71squared/TBXML)
- [TouchXML](https://github.com/TouchCode/TouchXML)
- [KissXML](https://github.com/robbiehanson/KissXML)
- [GDataXML](https://code.google.com/p/gdata-objectivec-client/)
- [RaptureXML](https://github.com/ZaBlanc/RaptureXML)
- [TinyXML](http://sourceforge.net/projects/tinyxml/)
- [TinyXML2](https://github.com/leethomason/tinyxml2)

All the tests were performed on an iPhone 5, each of them ran 10 times. Here's a chart with the results:

{% img /images/xmlBenchmarksChart.png %}

The fastest library was TBXML and most of the libraries finished within 0.01 seconds. It was a surprise that TinyXML2 came last, taking double the time of TinyXML.

From this tests one can conclude, that under modern hardware, the choice of speed does not matter that much as most of the libraries perform very similarly. The choice breaks down to a matter of preference, to which library you enjoy the most working with.

The code for this test can be found on [my repo](https://github.com/jansanz/iOSXMLPerformance). Feel free to fork it and improve any of the tests, specially if they will help making the parsing faster.

I am planning on revamping these benchmarks based on a [similar test but for JSON Parsing](https://github.com/bontoJR/iOS-JSON-Performance). It is currently a [work in progress](https://github.com/jansanz/iOS-XML-Performance).
