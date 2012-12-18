---
layout: post
title: "Xcode不太常见又实用的小技巧 UPDATE:2012-12-18 09:55"
date: 2012-12-17 14:46
comments: true
categories: [iOS, Tips, Notes, Xcode]
---
###让代码中的TODO和FIXME变成Warning
选中某个Target > Build Phase > Add Build Phase > Add Run Script  
然后输入  
{% codeblock lang:bash %}
KEYWORDS="TODO:|FIXME:|\?\?\?:|\!\!\!:"
find ${SRCROOT} \( -name "*.h" -or -name "*.m" \) -print0 | \
    xargs -0 egrep --with-filename --line-number --only-matching "($KEYWORDS).*\$" | \
    perl -p -e "s/($KEYWORDS)/ warning: \$1/"
{% endcodeblock %}
From: [http://www.benzado.com/blog/post/329/make-xcode-nag-you-about-unfinished-todos](http://www.benzado.com/blog/post/329/make-xcode-nag-you-about-unfinished-todos)

###Ctrl + NUM
*   Ctrl + 1 : Standard Editor > **Show Related Items**
*   Ctrl + 2/3/4/5/6/7/8 试试就知道了... 就那一排的按钮, 还比较实用
*   顺便说一下按住Command再用鼠标点可以以字母顺序排序

###Cmd + Shift + O
很多时候这么跳转都比鼠标点的要快点

###把 CodeSnippet 放到Dropbox多终端同步
{% codeblock lang:bash %}
mv ~/Library/Developer/Xcode/UserData/CodeSnippets ~/Library/Developer/Xcode/UserData/CodeSnippets_bak
ln -s ~/Dropbox/appdata/CodeSnippets ~/Library/Developer/Xcode/UserData/CodeSnippets
{% endcodeblock %}
P.S. 比如 **NSFetchedResultsControllerDelegate** 实现模板放在CodeSnippet里面就挺实用的

###总结
*   多看看Preference > Key Bindings里面的快捷键, 可以按需自己定制
*   可以总结一套合适自己的Behaviors+Tabs
*   WWDC2012 402 - Working Efficiently with Xcode里面讲得很详细