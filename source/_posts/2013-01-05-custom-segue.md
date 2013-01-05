---
layout: post
title: "自定义 UIStoryboardSegue"
date: 2013-01-05 16:30
comments: true
categories: [iOS, UIStoryboardSegue, UI, UIStoryboard]
---
###使用场景
举个栗子:   
比如我在现在很流行的这种侧边栏是Menu的交互情况下   
![side_panel_demo_0](https://raw.github.com/RoCry/rocry.github.com/master/assets/pics/side_panel_demo_0.png)    
现在的需求就是点左边Menu里面对应的条目可以跳转到相应的ViewController  
{% codeblock lang:objc %}
// 如果不用的话, 新建MenuViewController.h 和.m
// 然后在 MenuViewController.m 里面分开写逻辑
- (void)tableView:(UITableView *)tableView didSelectRowAtIndexPath:(NSIndexPath *)indexPath
{
    UIViewController *destinationViewController;
    switch (indexPath.row) {
        case 0:
            // Selected Home
            destinationViewController = [[MainViewController alloc] init];
            break;
        case 1:
            // Selected Setting
            destinationViewController = [[SettingViewController alloc] init];
            break;
        default:
            NSLog(@"error");
            break;
    }
    self.sidePanelController.centerPanel = destinationViewController;    
}
{% endcodeblock %}

{% codeblock lang:objc %}
// 如果用的话根本就不需要 MenuViewController 这个类!!
// 只需要在自己定义的CustomSegue里面处理跳转的逻辑就好了
- (void)perform {
    UIViewController *vc = self.sourceViewController;
    vc.sidePanelController.centerPanel = self.destinationViewController;
}
// 剩下的你只需要在界面上按住Control拖几条线出来就可以了
{% endcodeblock %}

###优点
这个方式的优点就是逻辑结构很清晰, 特别是如果你的Menu很多的时候, 你要写一大堆跳转的逻辑, 但是你用Segue的方式来实现的话, 只需要上面的两行代码, 以后每增加一个Menu的话, 只需要在界面上新建一个cell, 然后拖到相应的目的ViewController上面选择自定义的Segue就好了    

###结尾
最后的结果如下图:  
![side_panel_demo_1](https://raw.github.com/RoCry/rocry.github.com/master/assets/pics/side_panel_demo_1.png)  
同样的, 其实在很多地方都可以用Segue(无论是自定义的还是用SDK本身的)来简化代码, 使逻辑清晰化, 这里只是一个比较实际的案例而已   
**Demo完整代码**下载地址: [https://github.com/RoCry/CustomSegueDemo/](https://github.com/RoCry/CustomSegueDemo/)  

###特别鸣谢:
*   官方文档  
    *   [http://developer.apple.com/library/ios/#documentation/uikit/reference/UIStoryboardSegue_Class/Reference/Reference.html](http://developer.apple.com/library/ios/#documentation/uikit/reference/UIStoryboardSegue_Class/Reference/Reference.html)
*   特别入门的CustomSegue教程
    *   [在Storyboard中使用自定义的segue类型](http://ryan.easymorse.com/?p=72)
*   基于OpenGL来实现类似于iBooks的效果的自定义Segue
    *   [GC3DFlipTransitionStyleSegue](https://github.com/GlennChiu/GC3DFlipTransitionStyleSegue)
*   [JASidePanels](https://github.com/gotosleep/JASidePanels)




















