## UIWebView 和 WKWebView

仅 IOS 使用，用于访问网页



### 是什么

---

1.首先有一个疑问：UIWebView 和 WKWebView是什么？

UIWebView 是 UIKit 框架下的某一个类；WKWebView 是 WebKit 框架下的某一个类。

2.那么这两个框架是什么呢？

官网[1]给出的介绍是：**将web内容无缝集成到您的应用程序中，并自定义内容交互，以满足您的应用程序需求。** 

也就是说，苹果提供了一个框架，可以让我们把web应用嵌入到应用程序中。这就很明白了，框架是一个容器（浏览器），内部放web应用，对外和IOS应用交互。那么如果我们有一个web应用要在苹果应用S上使用，那么这个应用S需要通过框架创建一个容器，用来放我们的web应用。

3.为什么会有两个框架呢？

UIWebView 从IOS2就有，WKWebView从IOS8才有。后者是前者的下一代升级版。

后者提供了很多新的特性，优化了前者的问题，比如内存占用过多的问题。



### 如何使用

---

简单来说，分三步

1、创建webview；2、创建访问web应用的请求；3、加载页面；

其他功能就可以依赖框架提供的类和方法去调用。

我不写ios应用，所以这里不做深入了解，具体可以参考官网的说明和网上的例子。



### 对web开发的影响

---

从整体上讲，IOS应用只是提供了一个浏览器装载web，基本不影响。

1、https的问题

我司提供的WKWebView要求我们使用https加载资源，否则报错。

查阅资料之后发现，iOS9引入了新特性ATS，默认是不允许加载http请求的，对于webview，加载http网页也是不允许的。不过是可以通过修改info.plist取消http限制。

我认为我司对于WKWebView遵循了这个特性，要求我们在开发web的时候支持https。





### 参考文档

---

[1] apple官网：`https://developer.apple.com/documentation/webkit`

[2] webkit官网：`https://webkit.org/`

[3] 两者比较：`https://nshipster.cn/wkwebkit/`

[4] wkwebwiew小结： `https://github.com/ShingoFukuyama/WKWebViewTips/blob/master/README.md`