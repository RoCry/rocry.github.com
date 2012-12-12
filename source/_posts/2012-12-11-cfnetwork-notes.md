---
layout: post
title: "CFNetwork 学习笔记"
date: 2012-12-11 13:46
comments: true
categories: [iOS, Notes, CFNetwork]
---

*   CFNetwork属于CoreFoundation.framework
    *   它跟Foundation的关系差不多就是一个前者是C层面的库(提供CFString等CF打头的), 后者是Objective-C的(提供NSString等)
*   CFNetwork是基于BSD sockets的一套API, 比NSNetwork系列(NSURLConnection这套东西)更加底层一些
*   粗略的看下来发现有AFNetworking用好幸福啊..., 有NSNetwork用也很幸福... 
*   好在文档写的很详细... 如果会C, 了解网络的基本原理还是比较好懂
*   粗略的看一遍, 然后有需要的时候再查查就好了.. 在没有强烈的驱动的情况下, 我也不打算看完了...
*   在以上基础上可以结合[AudioStreamer](https://github.com/mattgallagher/AudioStreamer)看看... 
    *   CFNetwork同时顺便学习一下AudioToolbox, 多好...
    *   [http://www.cocoawithlove.com/2009/06/revisiting-old-post-streaming-and.html](http://www.cocoawithlove.com/2009/06/revisiting-old-post-streaming-and.html)