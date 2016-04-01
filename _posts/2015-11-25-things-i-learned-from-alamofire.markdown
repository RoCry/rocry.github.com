---
layout: post
title: Things I Learned from Alamofire
category: Swift
tags: source_code, swift
---

# The steps of a simple http request


{% highlight swift %}
Alamofire.request(.GET, "https://httpbin.org/get").responseJSON { response in
    debugPrint(reponse)
}
{% endhighlight %}

``NSURLSession`` in ``Manager`` create a ``NSURLSessionDataTask`` inside a ``Request``, then resume the task.
Once the server responsed, it will call the session's delegate, which is the ``SessionDelegate`` in manager, the global delegate will try find the subDelegate for the task by identifier. The subDelegate(``TaskDelegate``) in ``Request`` will handle the response, and disable ``queue.suspended``, after some of the response serializer, the completionBlock will be called.

# Use protocol and extension to improve API


{% highlight swift %}
public protocol URLStringConvertible {
    var URLString: String { get }
}
extension String: URLStringConvertible {
    public var URLString: String {
        return self
    }
}
extension NSURLRequest: URLStringConvertible {
    public var URLString: String {
        return URL!.URLString
    }
}

public func request(
    method: Method,
    _ URLString: URLStringConvertible, // you could pass NSURL/String/NSURLRequest
    parameters: [String: AnyObject]? = nil,
    encoding: ParameterEncoding = .URL,
    headers: [String: String]? = nil)
    -> Request
{
    return Manager.sharedInstance.request(
        method,
        URLString,
        parameters: parameters,
        encoding: encoding,
        headers: headers
    )
}
{% endhighlight %}

# Use subscript and dispatch\_barrier to implement subdelegates


{% highlight swift %}
public final class SessionDelegate: NSObject, NSURLSessionDelegate, NSURLSessionTaskDelegate, NSURLSessionDataDelegate, NSURLSessionDownloadDelegate {
        private var subdelegates: [Int: Request.TaskDelegate] = [:]
        private let subdelegateQueue = dispatch_queue_create(nil, DISPATCH_QUEUE_CONCURRENT)

        subscript(task: NSURLSessionTask) -> Request.TaskDelegate? {
            get {
                var subdelegate: Request.TaskDelegate?
                dispatch_sync(subdelegateQueue) {
                    subdelegate = self.subdelegates[task.taskIdentifier]
                }

                return subdelegate
            }

            set {
                dispatch_barrier_async(subdelegateQueue) {
                    self.subdelegates[task.taskIdentifier] = newValue
                }
            }
        }
        
// blablabla

}
{% endhighlight %}

Block style API instead of delegate



# weakify and strongify


{% highlight swift %}
self.delegate.sessionDidFinishEventsForBackgroundURLSession = { [weak self] session in
    if let strongSelf = self {
        strongSelf.backgroundCompletionHandler?()
    }
}
{% endhighlight %}

# Response


There is a ``NSOperationQueue`` in ``Request``, this queue won't excute untill the request completed.

{% highlight swift %}
func URLSession(session: NSURLSession, task: NSURLSessionTask, didCompleteWithError error: NSError?) {
    if let taskDidCompleteWithError = taskDidCompleteWithError {
        taskDidCompleteWithError(session, task, error)
    } else {
        if let error = error {
            self.error = error

            if let
                downloadDelegate = self as? DownloadTaskDelegate,
                userInfo = error.userInfo as? [String: AnyObject],
                resumeData = userInfo[NSURLSessionDownloadTaskResumeData] as? NSData
            {
                downloadDelegate.resumeData = resumeData
            }
        }

        queue.suspended = false
    }
}
{% endhighlight %}

# Use ``Enum`` to simplify API


{% highlight swift %}
public enum ParameterEncoding {
    case URL
    case URLEncodedInURL
    case JSON
    case PropertyList(NSPropertyListFormat, NSPropertyListWriteOptions)
    case Custom((URLRequestConvertible, [String : AnyObject]?) -> (NSMutableURLRequest, NSError?))
    public func encode(URLRequest: URLRequestConvertible, parameters: [String : AnyObject]?) -> (NSMutableURLRequest, NSError?)
    ...
}

public func request(
    method: Method,
    _ URLString: URLStringConvertible,
    parameters: [String: AnyObject]? = nil,
    encoding: ParameterEncoding = .URL,
    headers: [String: String]? = nil)
    -> Request
{
    let mutableURLRequest = URLRequest(method, URLString, headers: headers)
    let encodedURLRequest = encoding.encode(mutableURLRequest, parameters: parameters).0
    return request(encodedURLRequest)
}
{% endhighlight %}

# Result\<T, E\> for Error Handling


{% highlight swift %}
public enum Result<Value, Error : ErrorType> {
    case Success(Value)
    case Failure(Error)
    ...
    public var value: Value? { get }
    public var error: Error? { get }
}

Alamofire.request(.GET, "https://httpbin.org/get").responseJSON { response in
    switch response.result {
    case .Success(let value):
        print(value)
    case .Failure(let error):
        print(error)
    }
}
{% endhighlight %}

# ResponseSerializer with Generic to support custom serializer


{% highlight swift %}
public struct ResponseSerializer<Value, Error : ErrorType> : ResponseSerializerType {
    public typealias SerializedObject = Value
    public typealias ErrorObject = Error

    public var serializeResponse: (NSURLRequest?, NSHTTPURLResponse?, NSData?, NSError?) -> Result<Value, Error>
    public init(serializeResponse: (NSURLRequest?, NSHTTPURLResponse?, NSData?, NSError?) -> Result<Value, Error>)
}

extension Request {
    ...
    public func response<T: ResponseSerializerType>(
        queue queue: dispatch_queue_t? = nil,
        responseSerializer: T,
        completionHandler: Response<T.SerializedObject, T.ErrorObject> -> Void)
        -> Self
    {
        delegate.queue.addOperationWithBlock {
            let result = responseSerializer.serializeResponse(
                self.request,
                self.response,
                self.delegate.data,
                self.delegate.error
            )

            dispatch_async(queue ?? dispatch_get_main_queue()) {
                let response = Response<T.SerializedObject, T.ErrorObject>(
                    request: self.request,
                    response: self.response,
                    data: self.delegate.data,
                    result: result
                )

                completionHandler(response)
            }
        }

        return self
    }
}


// JSONResponseSerializer as example
extension Request {
    public static func JSONResponseSerializer(
        options options: NSJSONReadingOptions = .AllowFragments)
        -> ResponseSerializer<AnyObject, NSError>
    {
        return ResponseSerializer { _, _, data, error in
            guard error == nil else { return .Failure(error!) }

            guard let validData = data where validData.length > 0 else {
                let failureReason = "JSON could not be serialized. Input data was nil or zero length."
                let error = Error.errorWithCode(.JSONSerializationFailed, failureReason: failureReason)
                return .Failure(error)
            }

            do {
                let JSON = try NSJSONSerialization.JSONObjectWithData(validData, options: options)
                return .Success(JSON)
            } catch {
                return .Failure(error as NSError)
            }
        }
    }


    public func responseJSON(
        options options: NSJSONReadingOptions = .AllowFragments,
        completionHandler: Response<AnyObject, NSError> -> Void)
        -> Self
    {
        return response(
            responseSerializer: Request.JSONResponseSerializer(options: options),
            completionHandler: completionHandler
        )
    }
}
{% endhighlight %}

# ServerTrustPolicy, Stream didn’t looked much.. -\_-


# Validation Chain


{% highlight swift %}
Alamofire.request(.GET, "http://httpbin.org/get", parameters: ["foo": "bar"])
         .validate(statusCode: 200..<300)
         .validate(contentType: ["application/json"])
         .response { response in
             print(response)
         }
{% endhighlight %}

{% highlight swift %}
extension Request {
    public enum ValidationResult {
        case Success
        case Failure(NSError)
    }
    public typealias Validation = (NSURLRequest?, NSHTTPURLResponse) -> ValidationResult
    public func validate(validation: Validation) -> Self {
        delegate.queue.addOperationWithBlock {
            if let response = self.response where self.delegate.error == nil,
                case let .Failure(error) = validation(self.request, response)
            {
                self.delegate.error = error
            }
        }

        return self
    }
    ...
}
{% endhighlight %}

# One more thing


The UnitTest cases in Alamofire is very clean and tidy, what’s importand is that it didn’t use any third party code. Very easy to learn how ``XCTest`` works.

# At last, here is the mind map for Alamofire, made by iThoughtX


 ![](/assets/quiver_export/235091F5C3A6A70F5BF777A688F50E3C.jpg)  



