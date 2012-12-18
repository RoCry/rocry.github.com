---
layout: post
title: "weakself的一种写法"
date: 2012-12-18 14:46
comments: true
categories: [iOS, Objective-C, ARC, LLVM]
---
###前言
在不久前看AFNetworking的源码时候发现了这么一句:
{% codeblock lang:objc %}
// 不知道这行代码的使用场景的同学你该去自习看看ARC的注意事项和Block的使用了
// AFNetworking的写法
__weak __typeof(&*self)weakSelf = self;

// 我之前一直这么写的
__weak __typeof(self) weakSelf = self;
// 或者这么写
__weak XxxViewController *weakSelf = self;
// 或者这么写
__weak id weakSelf = self;
{% endcodeblock %}
当时也没注意为什么要写成&*这种样子...  今天再想起来, 搜了一圈, 终于让我找到原因了...

###正文
其实以上的4种写法都是对的  
AFNetworking里面不写成以上这行代码的原因是因为`typeof(self)`会被解析成`XxxViewController *const __strong`(假如你的self是XxxViewController的话), 这样的话就就会报错...(没有老版本, 没有验证, 不过看网上的结论应该是这样了)

不过, 如果你写成了__typeof(self)也没什么问题... 那是因为在LLVM3.1之后已经不会出现以上的情况了... ([via](http://stackoverflow.com/a/11226768/630195))  

###总结
*   TODO: ARC的文档值得仔细看看 [http://clang.llvm.org/docs/AutomaticReferenceCounting.html](http://clang.llvm.org/docs/AutomaticReferenceCounting.html)
*   之前推荐的[Multithreading and Memory Management for iOS and OS X](http://rocry.com/2012/12/11/book-notes)是一本好书