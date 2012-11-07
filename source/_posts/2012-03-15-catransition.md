---
layout: post
title: "蛋疼的方式完成CATransition的动画效果"
categories: [iOS]
comments: true
---
## 前言
昨天被**[iiiyu](http://iiiyu.com/)**同学推荐了一个好东西:[StudyiOS](https://github.com/iimgal/StudyiOS) 

然后今天决定学习一下里面的动画效果, 然后发现里面都是针对同一个viewController里面切换不同的view的~ 

明显的我们需要默认的那几个稍微华丽一点的viewController之间切换的效果

##思路
把nextViewController.view贴到现在的viewController里面, 然后动画效果结束之后push到nextViewController

这样看起来就像是切换两个viewcontroller的动画了

##代码

以下是设置动画效果
{% highlight objectivec %}

            UIStoryboard *board=[UIStoryboard storyboardWithName:@"MainStoryboard" bundle:nil];
            nextViewController =[board instantiateViewControllerWithIdentifier:@"NextView"];
            
            // 把要加入进来的view设置提高一点~~ 不然自动留出一个status bar的高度
            CGAffineTransform t = CGAffineTransformMakeTranslation(0.0, -20.0);  
            [nextViewController.view setTransform:t]; 
            
            CATransition *animation = [CATransition animation];
            animation.delegate = self; 
            animation.duration = 1;
            animation.timingFunction = [CAMediaTimingFunction functionWithName:kCAMediaTimingFunctionEaseInEaseOut];
            // 设定动画类型
            // kCATransitionFade 淡化
            // kCATransitionPush 推挤
            // kCATransitionReveal 揭开
            // kCATransitionMoveIn 覆盖
            // @"cube" 立方体
            // @"suckEffect" 吸收
            // @"oglFlip" 翻转
            // @"rippleEffect" 波纹
            // @"pageCurl" 翻页
            // @"pageUnCurl" 反翻页
            // @"cameraIrisHollowOpen" 镜头开
            // @"cameraIrisHollowClose" 镜头关
            animation.type = @"suckEffect"; 
            animation.subtype = kCATransitionFromRight;

            [self.view addSubview:nextViewController.view];
            [[self.view layer] addAnimation:animation forKey:@"animation"];
{% endhighlight %}

以下是动画结束之后的动作
{% highlight objectivec %}

// 动画结束时的回调方法
-(void)animationDidStop:(CAAnimation *)anim finished:(BOOL)flag {
    [self.navigationController pushViewController:nextViewController animated:NO];
    [nextViewController.view removeFromSuperview];    
}
{% endhighlight %}
