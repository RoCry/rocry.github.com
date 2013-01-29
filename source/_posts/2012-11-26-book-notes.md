---
layout: post
title: "iOS读书笔记 update:2012-12-17 17:40"
date: 2012-12-11 10:18
comments: true
categories: [iOS, GCD, Tips, Notes, OpenGL, CoreData, Reading]
---
###Multithreading and Memory Management for iOS and OS X
*   这本书太无敌了... 必须强烈推荐
*   每个做iOS/Mac开发的都应该仔细研读几遍 // TODO: 隔阵子再温习一遍
*   它能够解决你的疑惑:
    *   isa是神马
    *   block为什么要用copy
    *   weak和assign在底层有什么不同
    *   还有曾经被__block+ARC坑过的同学, 叫你们不看这本书!
        *   这里的被坑狭义的解释请注意**__block** + **ARC**
        *   广义的就有很多坑了...
    *   还有很多GCD的特性的demo
    *   ...  
        
---
###Pro Core Data
*   Entity也是有继承关系的, 可以定义一个Abstract的Person供Player等Entity继承
*   终于觉得NSCoding有用了
*   第7-8章(性能和合并)还没看.. 暂时的项目还没有用到这么复杂的条件(只用到了简单合并) // TODO: 以后再回过来看
*   关于多线程CoreData配合看看这篇文章: [http://www.cocoanetics.com/2012/07/multi-context-coredata/](http://www.cocoanetics.com/2012/07/multi-context-coredata/)

---
###iOS6 Cookbook
*   初/中级iOS Developer必备...
*   以前看过半本iOS5 Cookbook大部分都会了, 其他的用到的时候再查阅

---
###iOS 6 In Practice
*   跟上面那本书定位一样, 不过没Cookbook写的好...
*   随便翻翻就扔掉了, 有时间看这个不如看官方文档/官方Sample Code/WWDC/iOS6 Cookbook

---
###Pro OpenGL ES for iOS
*   这本书前面都是讲1.1的, 比较详细, 能了解个大概
*   建议结合以下部分一起看:
    *   raywenderlich的OpenGL部分
        *   [http://www.raywenderlich.com/tutorials](http://www.raywenderlich.com/tutorials)
    *   这里有个中文的教程貌似是翻译的(但是找不到原文), 写的很好, 也是讲1.*的
        *   [http://www.ityran.com/portal.php?mod=list&catid=3](http://www.ityran.com/portal.php?mod=list&catid=3)
    *   这里有个中文的OpenGLES 2.0的教程, 写得挺好 [http://blog.csdn.net/kesalin/article/category/1288827](http://blog.csdn.net/kesalin/article/category/1288827)
    *   线性代数跟我一样忘完了的同学可以补习一下:
        *   孟岩的理解矩阵系列
            *   [理解矩阵（一）](http://blog.csdn.net/myan/article/details/647511)
            *   [理解矩阵（二）](http://blog.csdn.net/myan/article/details/649018)
            *   [理解矩阵（三）](http://blog.csdn.net/myan/article/details/1865397)
        *   MIT的线性代数网易公开课翻译版
            *   [http://v.163.com/special/opencourse/daishu.html](http://v.163.com/special/opencourse/daishu.html)
    *   [IOS中二维坐标变换](http://www.cnblogs.com/delonchen/archive/2011/08/03/iostransform.html)
*   以上都看了可以再看看OpenGL的应用(游戏的话基本都用到了, 就不说了):
    *   一个OpenGL封装成Objective-C的库, 我在人人写的软件[光影DV](http://itunes.apple.com/us/app/guang-yingdv/id552718710?ls=1&mt=8)就是用到了这套库...
        *   [https://github.com/BradLarson/GPUImage](https://github.com/BradLarson/GPUImage)
    *  [Learning OpenGL with GPUImage](http://indieambitions.com/idevblogaday/learning-opengl-gpuimage/)

---
###Wrox.Professional.iOS.Network.Programming.Oct.2012
*   有经验的同学可以直接从第6章开始看... 老外的书就是这么个特点, 循序渐进
*   前五章 比较基础, 跟apple的文档差不多, 略读了一遍
*   第六章 讲安全/加密/认证什么的, 读了一遍不是太清楚, 主要是暂时没用得上的地方, 以后用的时候还得再看看
*   第七章 讲了一些优化的技巧, 除了cache部分其他都没感到共鸣... -_- 难道我悲剧了...
*   第八章 是重头戏了.. 
    *   因为BSD的socket没法用一些特性, 比如VPN/启动socket时自动连wifi什么的, 所以苹果基于BSD的socket做了一层很轻的封装:CFNetwork
    *   然后有BSD socket和CFNetwork socket的简单demo, 被AFNetworking惯坏了... 这么效率低(指人写代码效率低)的方法先了解了解, 不到得已就不用了
    *   可以配合一些[CFNetwork](http://rocry.com/2012/12/11/cfnetwork-notes/)的文档看
*   第九章 讲了一些调试的技巧和相关工具的使用... P.S. wireshark长的真丑啊...
*   第十章 讲APN由于我之前看过, 直接略读了... 不过貌似讲得很详细的样子~~ 值得参考
*   第十一章 的程序间相互调用, 之前也在别的地方看过了~~ 没看过的可以看看
*   第十二章 Gamekit没什么兴趣, 跳过了
*   第十三章 







