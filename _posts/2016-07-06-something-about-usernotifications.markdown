---
layout: post
title: Something about UserNotifications
category: AppleDev
tags: apple
---

> Before reading this, if you don't know what [UserNotifications](https://developer.apple.com/reference/usernotifications) is, please check [https://developer.apple.com/videos/wwdc2016/](https://developer.apple.com/videos/wwdc2016/)

> Tested on iOS10 beta1/2

Let's say you have a instagram-like app, you want to display the image directly in the notification, there is a way on iOS 10 now, in fact, there are several ways to do it: 

 ![](/assets/quiver_export/1AA6A5A45C355F554FFC72165AB78283.jpg) ![](/assets/quiver_export/4AEA6DCFF5152AE28F60E247F3046B92.jpg)

1. Put the image url in notification payload, download and display it in Notification Content Extension.
  * You won't get the thumbnail on notification, and the image won't download untill you expand the notification, and will see nothing but placeholder before the image downloaded.
2. Put the image url in notification payload, download it in Notification Service Extension, then add the image as `UNNotifcationAttachment`
  * You have less than **30s** to do what you want before you actually change the notification data. After that, the system will present the notification
  * You could use `ProcessInfo.performActivity` to keep the code running longger, but the notification will still be present at 29-30s even you didn't callback.
  * So the best we could do using this is, try to finish downloading in 30s, if didn't make it, use `ProcessInfo.performActivity` to keep downloading, once finished, we could modify the notification that already showed to add attachment for it.
  * But notice that the system will suspend our code at anytime, so this only works in most situation.
3. Partly similar as the solution 2, instead of download and modify it in Notification Service Extension, we use silent notification(`content-available: 1` in payload), handle the downloading job to background session, then assemble the local notification with attachments in the callback.
  * This will be the most reliable way, but notice that the background session on beta1 have a bug, so remember to test this after beta2.

