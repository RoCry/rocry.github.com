---
layout: post
title: "iOS读书笔记_2012-12-11"
date: 2012-12-11 10:18
comments: true
categories: [iOS, GCD, Tips, Notes, OpenGL, CoreData, Reading]
---
*   Multithreading and Memory Management for iOS and OS X
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
*   Pro Core Data
    *   Entity也是有继承关系的, 可以定义一个Abstract的Person供Player等Entity继承
    *   终于觉得NSCoding有用了
    *   第7-8章(性能和合并)还没看.. 暂时的项目还没有用到这么复杂的条件(只用到了简单合并) // TODO: 以后再回过来看
    *   关于多线程CoreData配合看看这篇文章: [http://www.cocoanetics.com/2012/07/multi-context-coredata/](http://www.cocoanetics.com/2012/07/multi-context-coredata/)

---
*   iOS6 Cookbook
    *   初/中级iOS Developer必备...
    *   以前看过半本iOS5 Cookbook大部分都会了, 其他的用到的时候再查阅

---
*   iOS 6 In Practice
    *   跟上面那本书定位一样, 不过没Cookbook写的好...
    *   随便翻翻就扔掉了, 有时间看这个不如看官方文档/官方Sample Code/WWDC/iOS6 Cookbook

---
*   Pro OpenGL ES for iOS
    *   这本书前面都是讲1.1的, 比较详细, 能了解个大概
    *   建议结合以下部分一起看:
        *   raywenderlich的OpenGL部分
            *   [http://www.raywenderlich.com/tutorials](http://www.raywenderlich.com/tutorials)
        *   这里有个中文的教程貌似是翻译的(但是找不到原文), 写的很好, 也是讲1.*的
            *   [http://www.ityran.com/portal.php?mod=list&catid=3](http://www.ityran.com/portal.php?mod=list&catid=3)
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













