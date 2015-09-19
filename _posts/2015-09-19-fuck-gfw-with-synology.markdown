---
layout: post
title: Fuck GFW with Synology
category: Lifehacks
---

First of all, you need to SSH to your Synology which is the easiest part.  
*If you have some problems, please figure out them by Google*

&nbsp;&nbsp;



# Install ShadowSocks

* Install Python from Package Center
* SSH to your Synology

* Install pip and shadowsocks

{% highlight sh %}
su
cd /tmp
wget https://bootstrap.pypa.io/get-pip.py
python get-pip.py
pip install shadowsocks
{% endhighlight %}

* Create shadowsocks config **/etc/shadowsocks.json**

{% highlight json %}
{
  "server": "SERVER",
  "server_port": 8888,
  "local_address": "127.0.0.1",
  "local_port": 1080,
  "password": "PASSWORD",
  "timeout": 300,
  "method": "aes-256-cfb",
  "fast_open": false
}
{% endhighlight %}

* start shadowsocks in background

{% highlight sh %}
sslocal -c /etc/shadowsocks.json -d start --pid-file /tmp/sslocal.pid --log-file /tmp/sslocal.log
{% endhighlight %}

# Convert Shadowsocks into HTTP proxy

* Install Privoxy

The tool is [Privoxy](http://www.privoxy.org/), but you may need install ipkg first.
If you are using DS214, you could follow [https://gist.github.com/marlun78/9349792](https://gist.github.com/marlun78/9349792), otherwise your could google it. 

*By the way, install ipkg for DS214 is not easy, honestly, this is the hardest part of this article. It maybe easier if you are using x86 or other artitecture instead of ARM.*

Once you are ready, you could install [Privoxy](http://www.privoxy.org/) by typing `ipkg install privoxy`

* Config Privoxy

Edit **etc/privoxy/config**

{% highlight sh %}
forward-socks5   /               127.0.0.1:1080 .
# change the ip to your nas ip
listen-address  10.0.0.6:8118

#local network do not use proxy
forward         192.168.*.*/     .
forward            10.*.*.*/     .
forward           127.*.*.*/     .
{% endhighlight %}

* Start Privoxy with `privoxy /etc/privoxy/config`

&nbsp;&nbsp;



For now, you could use http proxy from your other devices.

And, of course we can always make it better.

Host a pac file to identify which sites need proxy and which don't is a good idea. For me, I just export a pac file from my SwitchyOmega for Chrome. Works for me.

One more thing, if you are using Apple TV, you can add http proxy with Apple Configuarator. 

[How to install a configuration profile on Apple TV - Apple Support](https://support.apple.com/en-us/HT202577)

Enjoy~~



&nbsp;&nbsp;


Some Reference

* [ubuntu上使用shadowsocks + polipo配置socks5和http代理 - 为程序员服务](http://ju.outofmemory.cn/entry/144696)
* [在群晖系统上安装shadowsocks - 长微博](http://weibo.com/p/2304185410e9070102vila)
* [Privoxy - Home Page](http://www.privoxy.org/)
* [在命令行使用Shadowsocks翻墙 - CXH.ME](http://cxh.me/2015/01/30/use-shadowsocks-in-terminal/)
* [ds214-ipkg-install](https://gist.github.com/marlun78/9349792)
* [How to install a configuration profile on Apple TV - Apple Support](https://support.apple.com/en-us/HT202577)

