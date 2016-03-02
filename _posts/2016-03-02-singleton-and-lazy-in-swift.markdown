---
layout: post
title: Singleton and Lazy in Swift
category: Swift
tags: swift
---

# Singleton in Swift


 [http://krakendev.io/blog/the-right-way-to-write-a-singleton](http://krakendev.io/blog/the-right-way-to-write-a-singleton)

{% highlight swift %}
class TheOneAndOnlyKraken {
    static let sharedInstance = TheOneAndOnlyKraken()
}
{% endhighlight %}

 [https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/AdoptingCocoaDesignPatterns.html](https://developer.apple.com/library/prerelease/ios/documentation/Swift/Conceptual/BuildingCocoaApps/AdoptingCocoaDesignPatterns.html)  


In Swift, you can simply use a static type property, which is guaranteed to be lazily initialized only once, even when accessed across multiple threads simultaneously:

{% highlight swift %}
class Singleton {
    static let sharedInstance = Singleton()
}
{% endhighlight %}

If you need to perform additional setup beyond initialization, you can assign the result of the invocation of a closure to the global constant:

{% highlight swift %}
class Singleton {
    static let sharedInstance: Singleton = {
        let instance = Singleton()
        // setup code
        return instance
    }()
}
{% endhighlight %}

{% highlight swift %}
class TestClass {
    // both of below will init once
    
    // init at first
    let a: Int = {
        print("aaa")
        return 1+2
    }()
    
    // lazy init
    lazy var b: Int = {
        print("bbb")
        return 2+3
    }()
}
{% endhighlight %}

