---
layout: post
title: Things I Learned from XCGLogger
category: Swift
tags: source_code, swift
---

> [DaveWoodCom/XCGLogger](https://github.com/DaveWoodCom/XCGLogger)

{% highlight swift %}
public func verbose(@autoclosure closure: () -> String?, functionName: String = __FUNCTION__, fileName: String = __FILE__, lineNumber: Int = __LINE__) {
    self.logln(.Verbose, functionName: functionName, fileName: fileName, lineNumber: lineNumber, closure: closure)
}

public func verbose(functionName: String = __FUNCTION__, fileName: String = __FILE__, lineNumber: Int = __LINE__, @noescape closure: () -> String?) {
    self.logln(.Verbose, functionName: functionName, fileName: fileName, lineNumber: lineNumber, closure: closure)
}

public func logln(logLevel: LogLevel = .Debug, functionName: String = __FUNCTION__, fileName: String = __FILE__, lineNumber: Int = __LINE__, @noescape closure: () -> String?) {
    var logDetails: XCGLogDetails? = nil
    for logDestination in self.logDestinations {
        if (logDestination.isEnabledForLogLevel(logLevel)) {
            if logDetails == nil {
                if let logMessage = closure() {
                    logDetails = XCGLogDetails(logLevel: logLevel, date: NSDate(), logMessage: logMessage, functionName: functionName, fileName: fileName, lineNumber: lineNumber)
                }
                else {
                    break
                }
            }

            logDestination.processLogDetails(logDetails!)
        }
    }
}
{% endhighlight %}

* ``@autoclosure``
  * You need ``@autoclojure`` to wrap normal expression into clojure, a very useful tip to keep API pretty.
* ``@noescape``
  * To avoid use ``self.`` explicitly
* Two ``verbose`` method for one thing
  * To support ``Trailing Closures`` and ``@autoclojure`` at the same time.

_I have already read the source code a few times, but I never noticed the details that I mentioned above.
Until recently I want to build some log api for myself, it's very hard to make something pretty without get your hands dirty._

And some userful infomation from offical documentation.

> [The Swift Programming Language (Swift 2.1): Closures](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html)  
> As an optimization, Swift may instead capture and store a copy of a value if that value is not mutated by or outside a closure.

{% highlight swift %}
// example from
// https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html
func someFunctionWithNoescapeClosure(@noescape closure: () -> Void) {
    closure()
}

var completionHandlers: [() -> Void] = []
func someFunctionWithEscapingClosure(completionHandler: () -> Void) {
    completionHandlers.append(completionHandler)
}

class SomeClass {
    var x = 10
    func doSomething() {
        someFunctionWithEscapingClosure { self.x = 100 }
        someFunctionWithNoescapeClosure { x = 200 }
    }
}

let instance = SomeClass()
instance.doSomething()
print(instance.x)
// prints "200"

completionHandlers.first?()
print(instance.x)
// prints "100"
{% endhighlight %}

## References
[The Swift Programming Language (Swift 2.1): Closures](https://developer.apple.com/library/ios/documentation/Swift/Conceptual/Swift_Programming_Language/Closures.html#//apple_ref/doc/uid/TP40014097-CH11-ID103)

