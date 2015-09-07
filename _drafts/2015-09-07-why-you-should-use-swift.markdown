---
layout: post
title: Why You should use Swift
tags: swift
---

## 

## Type safe



Don’t need to check hundred times, don’t need&nbsp;NSParameterAssert in most cases.



{% highlight swift %}
if (username.length > 0) {
    [[MWEDaemon daemon].currentInterfaceController didReceivedYoCount:count fromUsername:username type:type];
}
{% endhighlight %}

{% highlight swift %}
func didReceivedYoCount(count: UInt, fromUsername username: String, type: Int) {
    presentControllerWithName("MWEYoController", context: ["username": username, "count": count, "type": type])
}
{% endhighlight %}

{% highlight swift %}

- (void)didReceivedYoCount:(NSUInteger)count fromUsername:(NSString *)username type:(int)type {
    // already check nil parameters when call
    // NSParameterAssert(username);
    [self presentControllerWithName:@"MWEYoController" context:@{@"username": username, @"count": @(count), @"type": @(type)}];
}
{% endhighlight %}

## Generics


{% highlight swift %}
struct IntStack {
    var items = [Int]()
    mutating func push(item: Int) {
        items.append(item)
    }
    mutating func pop() -> Int {
        return items.removeLast()
    }
}
{% endhighlight %}

{% highlight swift %}
struct Stack<T> {
    var items = [T]()
    mutating func push(item: T) {
        items.append(item)
    }
    mutating func pop() -> T {
        return items.removeLast()
    }
}
{% endhighlight %}

## Enum


{% highlight swift %}
enum Barcode {
    case UPCA(Int, Int, Int, Int)
    case QRCode(String)
}

var productBarcode = Barcode.UPCA(8, 85909, 51226, 3)
productBarcode = .QRCode("ABCDEFGHIJKLMNOP")

switch productBarcode {
case let .UPCA(numberSystem, manufacturer, product, check):
    print("UPC-A: \(numberSystem), \(manufacturer), \(product), \(check).")
case let .QRCode(productCode):
    print("QR code: \(productCode).")
}
// prints "QR code: ABCDEFGHIJKLMNOP."
{% endhighlight %}

There is another a very popular use case about check result, here is a example in [Alamofire/Result.swift](https://github.com/Alamofire/Alamofire/blob/swift-2.0/Source/Result.swift)

## Default Prameter Values


Function definition will be much more&nbsp;graceful.



{% highlight swift %}
func someFunction(parameterWithDefault: Int = 22) {
    // function body goes here
}

// call
someFunction()
someFunction(1)
{% endhighlight %}

## Functions with Multiple Return Values (Tuples)


{% highlight swift %}
func minMax(array: [Int]) -> (min: Int, max: Int)? {
    // function body goes here
}
{% endhighlight %}

## Function Types as Parameter Types


{% highlight swift %}
func printMathResult(mathFunction: (Int, Int) -> Int, _ a: Int, _ b: Int) {
    print("Result: \(mathFunction(a, b))")
}
printMathResult(addTwoInts, 3, 5)
{% endhighlight %}

## Closure Expressions


{% highlight swift %}
let names = ["Chris", "Alex", "Ewa", "Barry", "Daniella"]
{% endhighlight %}

All of the codes below do the same thing:



{% highlight swift %}
func backwards(s1: String, s2: String) -> Bool {
    return s1 > s2
}
var reversed = names.sort(backwards)
{% endhighlight %}

{% highlight swift %}
reversed = names.sort({ (s1: String, s2: String) -> Bool in
    return s1 > s2
})
{% endhighlight %}

{% highlight swift %}
reversed = names.sort({ s1, s2 in return s1 > s2 })
{% endhighlight %}

{% highlight swift %}
reversed = names.sort({ s1, s2 in s1 > s2 })
{% endhighlight %}

{% highlight swift %}
reversed = names.sort({ $0 > $1 })
{% endhighlight %}

# guard


Before:



{% highlight swift %}
func somethingDidChanged(notification: NSNotification) {
    if let contact = notification.userInfo?["contact"] as? MWEContact {
        if let message = notification.userInfo?["message"] as? MWEMessage {
            // do something
        }
    }
}
{% endhighlight %}

![](/assets/quiver_export/A87CE7E7-F9EE-480A-9A2F-8C52E9FF1641.jpg)

After:



{% highlight swift %}
func somethingDidChanged(notification: NSNotification) {
    guard let contact = notification.userInfo?["contact"] as? MWEContact else {
        return
    }
    
    guard let message = notification.userInfo?["contact"] as? MWEMessage else {
        return
    }
    
    // do something
}
{% endhighlight %}

Could be better:



{% highlight swift %}
func somethingDidChanged(notification: NSNotification) {
    guard let contact = notification.userInfo?["contact"] as? MWEContact,
        message = notification.userInfo?["message"] as? MWEMessage else {
            return
    }
    
    // do something
}
{% endhighlight %}

# defer


{% highlight swift %}
func processFile(filename: String) throws {
    if exists(filename) {
        let file = open(filename)
        defer {
            close(file)
        }
        while let line = try file.readline() {
            // Work with the file.
        }
        // close(file) is called here, at the end of the scope.
    }
}
{% endhighlight %}

## Protocol Extensions


- WWDC 2015 - Session 408 - Protocol-Oriented   Programming in Swift  


{% highlight swift %}
class PresentErrorViewController: UIViewController {
    var errorViewIsShowing: Bool = false
    func presentError(message: String = “Error!", withArrow shouldShowArrow: Bool = false, backgroundColor: UIColor = ColorSalmon, withSize size: CGSize = CGSizeZero, canDismissByTappingAnywhere canDismiss: Bool = true) {
        //do complicated, fragile logic
    }
} 

//Over 100 classes inherited from this class, by the way.
class EveryViewControllerInApp: PresentErrorViewController {}
{% endhighlight %}

* * *


{% highlight swift %}
protocol ErrorPopoverRenderer {
    func presentError(message: String, withArrow shouldShowArrow: Bool, backgroundColor: UIColor, withSize size: CGSize, canDismissByTappingAnywhere canDismiss: Bool)
} 

extension UIViewController: ErrorPopoverRenderer { //Make all the UIViewControllers that conform to ErrorPopoverRenderer have a default implementation of presentError
    func presentError(message: String, withArrow shouldShowArrow: Bool, backgroundColor: UIColor, withSize size: CGSize, canDismissByTappingAnywhere canDismiss: Bool) {
        //add default implementation of present error view
    }
} 

class KrakenViewController: UIViewController, ErrorPopoverRenderer { //Drop the God class and make KrakenViewController conform to the new ErrorPopoverRenderer Protocol.
    func methodThatHasAnError() {
        //…
        //Throw error because the Kraken sucks at eating Humans today.
        presentError(/*blah blah blah much parameters*/)
    }
}
{% endhighlight %}

the Swift runtime calls the&nbsp;<font color="#363636" face="monospace, serif"><span style="font-size: 17px; line-height: 27.2000007629395px; widows: 1; background-color: rgb(255, 255, 255);">presentError()</span></font>&nbsp;through static dispatch instead of through dynamic dispatch.

# Some code snippets


{% highlight swift %}
let b = a ?? "default"
{% endhighlight %}

{% highlight swift %}
if #available(iOS 9, OSX 10.10, *) {
    // Use iOS9 APIs on iOS， and use OS X v10.10 APIs on OS X
} else {
    // Fall back to earlier iOS and OS X APIs
}
{% endhighlight %}

{% highlight swift %}
prepareSomething() // NOT use 'self.' unless it's necessary
// self.prepareSomething() // don't do this
contact.getHeadImageThumbnail { [weak self] (object, error) -> Void in
    // blabla
    self.updateUI()
}
{% endhighlight %}

# Tips


Use Playground to test something when coding.



![](/assets/quiver_export/FBFD2C46-8E02-4A18-B5D1-A9420B54D958.jpg)

# Summay


![](/assets/quiver_export/8524433B-4ACF-42CB-B977-76D9EB701ADA.jpg)

## Related

- [https://developer.apple.com/swift/resources/](https://developer.apple.com/swift/resources/)  
- [https://developer.apple.com/library/prerelease/ios/documentation/Swift](https://developer.apple.com/library/prerelease/ios/documentation/Swift)



