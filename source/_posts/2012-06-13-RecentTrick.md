---
layout: post
title: "一些备用的小技巧汇总"
categories: [Tips, Notes, Mac]
comments: true
---
######在finder里面打开terminal+cd to
[https://code.google.com/p/cdto/](https://code.google.com/p/cdto/)

######用sublime打开
[https://github.com/fallroot/open-with-sublime-text-2](https://github.com/fallroot/open-with-sublime-text-2)

######我比较常用的快捷键:  
Ctrl + Command + d 查询鼠标悬停时候的单词 (词库可以拓展, 放在/Library/Dictionaries)  
Ctrl + h        退格删除一个字符，相当于通常的Backspace键  
Ctrl + u        删除光标之前到行首的字符  
Ctrl + k        删除光标之前到行尾的字符  
Ctrl + c        取消当前行输入的命令，相当于Ctrl + Break  
Ctrl + a        光标移动到行首（Ahead of line），相当于通常的Home键  
Ctrl + e        光标移动到行尾（End of line）  
Ctrl + f        光标向前(Forward)移动一个字符位置  
Ctrl + b        光标往回(Backward)移动一个字符位置  
Ctrl + l        清屏，相当于执行clear命令  
Ctrl + p        调出命令历史中的前一条（Previous）命令，相当于通常的上箭头  
Ctrl + n        调出命令历史中的下一条（Next）命令，相当于通常的上箭头  
Ctrl + r        显示：号提示，根据用户输入查找相关历史命令（reverse-i-search）  

次常用快捷键：  
Alt + f         光标向前（Forward）移动到下一个单词  
Alt + b         光标往回（Backward）移动到前一个单词  
Ctrl + w        删除从光标位置前到当前所处单词（Word）的开头  
Alt + d         删除从光标位置到当前所处单词的末尾  
Ctrl + y        粘贴最后一次被删除的单词  

######开发用得到的小工具
* Network Link Conditioner
[https://developer.apple.com/downloads/index.action](https://developer.apple.com/downloads/index.action)

######trick
	history | awk '{CMD[$2]++;count++;} END { for (a in CMD )print CMD[ a ]" " CMD[ a ]/count*100 "% " a }' | 	grep -v "./" | column -c3 -s " " -t |sort -nr | nl | head -n10 		
	
######TO BE CONTINUED