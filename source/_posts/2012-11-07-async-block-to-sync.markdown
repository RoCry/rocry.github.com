---
layout: post
title: "异步改同步block"
date: 2012-11-07 12:38
comments: true
categories: [iOS, GCD, Tips, Notes]
---
###场景
一款视频类的软件(比如[光影DV](itunes.apple.com/us/app/guang-yingdv/id552718710?ls=1&mt=8))在某个时间需要后台保存一些视频到媒体库, 查官方文档之后只发现这么一个异步的方法
{% codeblock lang:objc %}
- (void)writeVideoAtPathToSavedPhotosAlbum:(NSURL *)videoPathURL completionBlock:(ALAssetsLibraryWriteVideoCompletionBlock)completionBlock
{% endcodeblock %}
因为本来就放在后台线程了, 所以我希望的是每个视频写到媒体库的时候是线性往下走的, 这样也比较好控制, 然后保存完了之后也好提示用户
###结论
所以, 万能的GCD还有这么一招:
{% codeblock lang:objc %}
for (NSString *path in videoPaths) {
    dispatch_semaphore_t sema = dispatch_semaphore_create(0);
                        
    [library writeVideoAtPathToSavedPhotosAlbum:[NSURL fileURLWithPath:path] completionBlock:^(NSURL *assetURL, NSError *error) {
        // 为了简明, 我把此处的错误判断神马的去掉了
        dispatch_semaphore_signal(sema);
    }];

    dispatch_semaphore_wait(sema, DISPATCH_TIME_FOREVER);
    dispatch_release(sema);
}
// Done
{% endcodeblock %}

