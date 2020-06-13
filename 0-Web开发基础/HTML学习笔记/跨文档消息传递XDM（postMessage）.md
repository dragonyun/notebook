## 跨文档消息传递（XDM）

本质上是HTML5定义的JavaScript API，核心是postMessage方法，与iframe关系密切

> <https://www.w3cschool.cn/fetch_api/fetch_api-lx142x8t.html>



### 概念

​	跨文档消息传送（cross-document messaging），有时候简称为XDM，指的是来自不同域的页面间传递消息。

​	比如：www.wrox.com域中的页面与位于一个内嵌框架中的 p2p.wrox.com域中的页面通信，在 XDM 出现之前，这种通信比较麻烦。

​	而 XDM 就是把这种制度规范化，让我们稳妥又简单地实现跨文档通信。



### 核心

​	XDM 的核心是 postMessage() 方法。除了 XDM 部分之外的其他部分也会提到这个方法名，但都是为了同一个目的：**向另一个地方传递数据**。

​	对于 XDM 而言，**另一个地方**指的是包含在当前页面中的`<iframe>`元素，或者由当前页面弹出的窗口。



### 用法

#### 发送方

postMessage() 方法接收两个参数：一条消息和一个表示消息接收方来自哪个域的字符串。

第二个参数对保障安全通信非常重要，可以防止浏览器把消息发到不安全的地方。

```js
// 注意：所有支持 XDM 的浏览器也支持 iframe 的 contentWindow 属性
var iframeWindow = document.getElementById("myframe").contentWindow;
iframeWindow.postMessage("A select", "http://www.wrox.com");
```

最后一行代码尝试向内嵌框架中发送一条消息，并指定框架中的文档必须来源于“http://www.wrox.com”域。如果来源匹配，消息会传递到内嵌框架中；否则，postMessage() 什么也不做，这一限制可以避免窗口中的位置在你不知情的情况下发生改变。

如果传给 postMessage() 的第二个参数是“*”，则表示可以把消息发送给来自任意域的文档，但我们不推荐这样做。

> 不得不说，书里面写的言简意赅，我不想画蛇添足，加点我的笔记好了。
>
> 1、上面的例子中 iframeWindow 代表接收方，父页面传给子页面需要像例子一样先拿到子页面（iframe）的window；反过来子传父，也是先拿到父的window，再调用postMessage()方法
>
> 2、第二个参数是一种额外的校验，保证我传到的接收方是我期望的接收方



#### 接收方

接收 XDM 消息时，会触发 window 对象的message事件。

​	这个事件是以异步形式触发的，因此从发送消息到接收消息（触发接收窗口的 message 事件）可能要经过一段时间的延迟。触发 message 事件后，传递给 onmessage 处理程序的事件对象包含以下三方面的重要信息。

- data：作为 postMessage() 第一个参数传入的字符串数据
- origin：发送消息的文档所在的域，例如“http://www.wrox.com”
- source：发送消息的文档的 window 对象的代理。这个代理的对象主要用于在发送上一条消息的窗口中调用 postMessage() 方法。如果发送消息的窗口来自同一个域，那这个对象就是window

> 接收是事件，参数是event，event的构成是{data，origin，source，。。。}

​	接收到消息后验证发送窗口的来源是至关重要的。就像给 postMessage() 方法指定第二个参数，以确保浏览器不会把消息发送给未知页面一样，在onmessage处理程序中检测来源可以确保传入的消息来自已知的页面。

```js
EventUtil.addHandle(window, 'message', function(event){
    // 确保发送消息的域是已知的域
    if (event.origin == "http://www.wrox.com"){
        // 处理接收到的数据
        processMessage(event.data);
        // 可选：向来源窗口发送回执
        event.source.postMessage("received!", "http://p2p.wrox.com");
    }
})
```

​	这里 event.source 大多数情况下只是 window 对象的代理，并非实际的 window 对象。换句话说，不能通过这个代理对象访问 window 对象的其他任何信息。记住，只通过这个代理用 postMessage() 就好，这个方法永远存在，永远可以调用。

> 1、校验是双向的，发送方和接收方都可以校验 域
>
> 2、监听 message 事件完成接收
>
> 3、event.source 不可滥用



### 其他

​	XDM 还有一些怪异之处。首先，postMessage() 的第一个参数最早是作为“永远都是字符串”来实现的。但是后来这个参数的定义改了，改成允许传入任何数据结构。可是，并非所有浏览器都实现了这一变化。为了保险起见，使用 postMessage() 时，最好还是只传字符串。如果你想传入结构化的数据，最佳选择是先在要传入的数据上调用 JSON.stringify()，通过postMessage() 传入得到的字符串，然后再在onmessge 事件处理程序中调用JSON.parse()。

> postMessage()的第一个参数最好传字符串，太低版本的浏览器不一定支持其他数据结构，不过我不担心，IE10+都支持，哈哈

​	在通过内嵌框架加载其他域的内容时，使用 XDM 是非常方便的。因此，在混搭（mashup）和社交网络应用中，这种传递消息的方法极为常用。有了 XDM，包含`<iframe>`的页面可以确保自身不受恶意内容的侵扰，因为它只通过 XDM 与内嵌的框架通信。而 XDM 也可以在来自相同域的页面间使用。

