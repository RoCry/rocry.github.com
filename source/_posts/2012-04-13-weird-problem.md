---
layout: post
title: "记一个诡异的问题"
categories: [iOS]
comments: true
---
####问题描述:  
我一个view controller 里面操作core data  
因为是在viewDidLoad里面问服务器获取再刷新数据库的   
然后老在保存的地方报错..  
controllerWillChangeContent:]: message sent to deallocated instance.  
大致就是这个错误信息  
没办法啊 一点点的排查  
中间的过程我都不想说了… 想起来就心痛

####总结下整个过程和错误发生的地方   
1. 从navigation controller push到这个view controller  
2. 然后pop 这期间这个view controller 没有销毁!!  
3. 然后我再push进去的时候又出现了另外一个实例了.. 导致上述报错的出现  太诡异了

虽然知道问题出在哪里 但是还是不太明白为什么会有这样情况出现…   
网上找到一个2010年的帖子, 也是这个问题.. 也是没有找到具体的原因..  
然后帖子地址还被我弄丢了   
iOS就是这点不好, 要是android把你惹火了, 你还可以去翻源代码 看到底是为什么出现这种情况   
iOS就算了吧..

####解决方式
最后又是以蛋疼的方式解决这个问题:push过去的时候用这个view controller的单例(singleton)   
保证不出现两个实例...

##UPDATE:
这个地方是由于内存没释放掉导致pop的时候ViewController没有销毁...
大家千万不要向我学习...