
---
layout: post
title: "很隐晦的一个错误"
date: 2013-04-02 16:30
comments: true
categories: [Note, Debug, Tips, Block, iOS, Objective-C]
---
比如说你有一个这么一个需求, 已经有这么一个函数
{% codeblock lang:objc %}
- (void)saveWithBlock:(void (^)(BOOL succeeded, NSError *error))block {
    ...
}
{% endcodeblock %}
你要在这个函数的基础上封装一个同步的函数, 虽然不建议在主线程执行同步的函数(会卡住UI线程), 但是你不能排除使用这个API的人有这个觉悟, 所以像[这篇文章](http://rocry.com/2012/11/07/async-block-to-sync/)用dispatch_semaphore_signal的方法就不行了   
然后我就这么弄了一下
{% codeblock lang:objc %}
// 这段代码有个小问题 请继续往下看
- (BOOL)save:(NSError **)theError
{
    BOOL __block theResult = NO;
    BOOL __block hasCalledBack = NO;
    [self saveWithBlock:^(BOOL succeeded, NSError *error) {
            if (theError != NULL) *theError = error;
            theResult = (error == nil);
            hasCalledBack = YES;
    }];
        
    NSTimeInterval checkEveryInterval = 0.1;
    while(!hasCalledBack) {
        @autoreleasepool {
            if (![[NSRunLoop currentRunLoop] runMode:NSDefaultRunLoopMode beforeDate:[NSDate dateWithTimeIntervalSinceNow:checkEveryInterval]])
                [NSThread sleepForTimeInterval:checkEveryInterval];
        }
    }
    return theResult;
}
{% endcodeblock %}

其实以上的有点小问题, 请仔细思考一下再看下面的正确的代码~~~
{% codeblock lang:objc %}
// 如果有更好的解决方案欢迎交流...
- (BOOL)save:(NSError **)theError
{
    BOOL __block theResult = NO;
    BOOL __block hasCalledBack = NO;
    NSError __block *blockError = nil;
    
    [self saveWithBlock:^(BOOL succeeded, NSError *error) {
        blockError = error;
        theResult = (error == nil);
        hasCalledBack = YES;
    }];
    
    NSTimeInterval checkEveryInterval = 0.1;
    while(!hasCalledBack) {
        @autoreleasepool {
            if (![[NSRunLoop currentRunLoop] runMode:NSDefaultRunLoopMode beforeDate:[NSDate dateWithTimeIntervalSinceNow:checkEveryInterval]])
                [NSThread sleepForTimeInterval:checkEveryInterval];
        }
    }
    if (theError != NULL) *theError = blockError;
    return theResult;
}
{% endcodeblock %}

同理还有这么一个sample:  
![http://ww1.sinaimg.cn/large/57361f0cjw1e36pys52zij.jpg](http://ww1.sinaimg.cn/large/57361f0cjw1e36pys52zij.jpg)