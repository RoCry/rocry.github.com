---
layout: post
title: "总结一下"
date: 2013-07-23 18:00
comments: true
categories: [Mac, Tools, Tips, Life, Gossip]
---

好久没写博客了, 记录点什么鞭策下自己吧...

##最近在学/用的东西
###SelfCapturing
之前为了练习写Mac程序的一个小项目功能很简单:  
每隔5秒录一帧的视频, 然后每天8点自动开始晚上8点自动结束...(当然也可以手动开关)  
这么一天下来就是14分钟的视频, 每天看着自己摇头晃脑也很好玩的...  
项目源码和可运行的程序都在: [https://github.com/RoCry/SelfCapturing](https://github.com/RoCry/SelfCapturing)

###WWDC2013
WWDC刚出来的时候正好在外面玩, 就没看直播, 后来回来之后看了几集, 然后又犯懒了..  
记录在自己的wiki上了, 以后考虑加上一点笔记

###ReactiveCocoa
其实第一次看到ReactiveCocoa是去年的事情了, 那时候没意识到有什么用, 前阵子又想起来了, 费力的学了一下  
资料记录在[https://github.com/RoCry/rocry.github.com/wiki/Project_ReactiveCocoa](https://github.com/RoCry/rocry.github.com/wiki/Project_ReactiveCocoa), 自己也有一个业余的项目尝试性的使用  
然后项目还没写完, 写完了去提个appstore然后开源出来吧…

###mogenerator
很轻量级的一个小工具, 简单来说它主要做这么一件事情:  
给CoreData对应的Entity生成一个隐藏的父类, 这样你直接用这个Entity的时候, 还可以用到它自动生成的很多好用的方法  
用CoreData必用啊... 也没什么学习成本, 以下是我用的命令  
```
mogenerator -m what.xcdatamodeld/what.xcdatamodel/ --template-var arc=true -M generated -H entities
```

###MagicalRecord
也是用CoreData必备.. 生活变得更美好啊. 不过因为我在工作中暂时还没用到, 也只是简单的用用, 之后要深入使用的话还得看看源码加深理解(因为这货文档太少了啊!! 好在源码写的挺好的, 也很好读)  
[https://github.com/magicalpanda/MagicalRecord](https://github.com/magicalpanda/MagicalRecord)

###RestKit
之前简单的用过, 最近又在公司的项目里面用了用.. 合理利用还是非常提高效率的... 有时候项目有临时的轻量级的api需求的时候, 就直接用它底下的AFNetworking这层好了  
[https://github.com/RestKit/RestKit](https://github.com/RestKit/RestKit)

###AFAmazonS3Client
[AFAmazonS3Client](https://github.com/AFNetworking/AFAmazonS3Client)是大名鼎鼎的[AFNetworking](https://github.com/AFNetworking/AFNetworking)的作者[Mattt](https://github.com/mattt)写的给Amazon S3相关功能的项目, 之前公司有需求, 但是Amazon官方的SDK太臃肿了, 就开始用这个. 过程中还发现实现验证的部分不太对, 还发了个Pull Request被Merge了... 哈哈哈...

###Workflow
*	终于有快捷键可以关屏幕了! (如果你的外接键盘有Eject键就不用这么费事了, 直接Ctrl + Shift + Eject就好了)
	*	[http://blog.joshdick.net/2013/02/27/sleep_displays_with_alfred_v2.html](http://blog.joshdick.net/2013/02/27/sleep_displays_with_alfred_v2.html)
*	在各种颜色的表达方式之间转换并预览, 太方便了...
	*	[https://github.com/TylerEich/Alfred-Extras/blob/master/Workflows/Colors.alfredworkflow](https://github.com/TylerEich/Alfred-Extras/blob/master/Workflows/Colors.alfredworkflow)
*	然后还有一些自己写的, 不太通用, 就不贴出来了, 有兴趣的同学自己写
	*	搜索项目文件夹->git log->提取自己的commits->保存到工作的日历
	*	把当前复制或者选中的url添加到迅雷离线队列

###Some Useful Code
*	[https://github.com/onevcat/VVDocumenter-Xcode](https://github.com/onevcat/VVDocumenter-Xcode) // 快速生成文档的一个Xcode插件
*	[https://github.com/applidium/ADTransitionController](https://github.com/applidium/ADTransitionController) // 很多很酷的Transition动画
*	[https://github.com/RoCry/APNS-Pusher](https://github.com/RoCry/APNS-Pusher) // 我fork的一个测试push的mac小程序, 原版在:[https://github.com/blommegard/APNS-Pusher](https://github.com/blommegard/APNS-Pusher)
*	[https://github.com/mmackh/Hacker-News-for-iOS](https://github.com/mmackh/Hacker-News-for-iOS) // 一个写的很好的Hacker news客户端, 经典, 小巧, 用来入门练手不错

##工作
来[AVOS](http://www.avos.com/)之后主要在做两个东西:[玩拍](http://wanpaiapp.com/)和[AVOS Cloud](https://cn.avoscloud.com/), 很近的最近都在做前者, 之前主要是在做后者... 具体的就不说了吧
