## XMLHttpRequest对象学习

### 简介

​	`XMLHttpRequest`（XHR）对象用于与服务器交互。通过 XMLHttpRequest 可以在不刷新页面的情况下请求特定 URL，获取数据。这允许网页在不影响用户操作的情况下，更新页面的局部内容。`XMLHttpRequest` 在 [AJAX](https://developer.mozilla.org/zh-CN/docs/Glossary/AJAX) 编程中被大量使用。

​	尽管名称如此，`XMLHttpRequest` 可以用于获取任何类型的数据，而不仅仅是 XML。

> EventTarget <-- XMLHttpRequestEventTarget <-- XMLHttpRequest



### 属性及方法

#### 构造函数

[`XMLHttpRequest()`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/XMLHttpRequest)

该构造函数用于初始化一个 `XMLHttpRequest` 实例对象。在调用下列任何其他方法之前，必须先调用该构造函数，或通过其他方式，得到一个实例对象。



#### 属性

*此接口继承了 XMLHttpRequestEventTarget 和 EventTarget 的属性。*

- [`XMLHttpRequest.onreadystatechange`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/onreadystatechange)

  当 `readyState` 属性发生变化时，调用的 [`EventHandler`](https://developer.mozilla.org/zh-CN/docs/Web/API/EventHandler)。

  

- [`XMLHttpRequest.readyState`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/readyState) 只读

  返回 一个无符号短整型（`unsigned short`）数字，代表请求的状态码。

  

- [`XMLHttpRequest.response`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/response) 只读

  返回一个 [`ArrayBuffer`](https://developer.mozilla.org/zh-CN/docs/Web/API/ArrayBuffer)、[`Blob`](https://developer.mozilla.org/zh-CN/docs/Web/API/Blob)、[`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document)，或 [`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)，具体是哪种类型取决于 [`XMLHttpRequest.responseType`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/responseType) 的值。其中包含整个响应实体（response entity body）。

  

- [`XMLHttpRequest.responseText`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/responseText) 只读

  返回一个 [`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)，该 [`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString) 包含对请求的响应，如果请求未成功或尚未发送，则返回 `null`。

  

- [`XMLHttpRequest.responseType`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/responseType)

  一个用于定义响应类型的枚举值（enumerated value）。

  

- [`XMLHttpRequest.responseURL`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/responseURL) 只读

  返回经过序列化（serialized）的响应 URL，如果该 URL 为空，则返回空字符串。

  

- [`XMLHttpRequest.responseXML`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/responseXML) 只读

  返回一个 [`Document`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document)，其中包含该请求的响应，如果请求未成功、尚未发送或时不能被解析为 XML 或 HTML，则返回 `null`。

  

- [`XMLHttpRequest.status`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/status) 只读

  返回一个无符号短整型（`unsigned short`）数字，代表请求的响应状态。

  

- [`XMLHttpRequest.statusText`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/statusText) 只读

  返回一个 [`DOMString`](https://developer.mozilla.org/zh-CN/docs/Web/API/DOMString)，其中包含 HTTP 服务器返回的响应状态。与 [`XMLHTTPRequest.status`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHTTPRequest/status) 不同的是，它包含完整的响应状态文本（例如，"`200 OK`"）。

  

- [`XMLHttpRequest.timeout`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/timeout)

  一个无符号长整型（`unsigned long`）数字，表示该请求的最大请求时间（毫秒），若超出该时间，请求会自动终止。

  

- [`XMLHttpRequestEventTarget.ontimeout`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/ontimeout)

  当请求超时调用的 [`EventHandler`](https://developer.mozilla.org/zh-CN/docs/Web/API/EventHandler)。

  

- [`XMLHttpRequest.upload`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/upload) 只读

  [`XMLHttpRequestUpload`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestUpload)，代表上传进度。

  

- [`XMLHttpRequest.withCredentials`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/withCredentials)

  一个[`布尔值`](https://developer.mozilla.org/zh-CN/docs/Web/API/Boolean)，用来指定跨域 `Access-Control` 请求是否应当带有授权信息，如 cookie 或授权 header 头。



**事件处理器**

作为 `XMLHttpRequest` 实例的属性之一，所有浏览器都支持 `onreadystatechange`。

后来，许多浏览器实现了一些额外的事件（`onload`、`onerror`、`onprogress` 等）。

更多现代浏览器，包括 Firefox，除了可以设置 `on*` 属性外，也提供标准的监听器 [`addEventListener()`](https://developer.mozilla.org/zh-CN/docs/Web/API/EventTarget/addEventListener) API 来监听`XMLHttpRequest` 事件。



#### 方法

[`XMLHttpRequest.abort()`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/abort)

如果请求已被发出，则立刻中止请求。



[`XMLHttpRequest.getAllResponseHeaders()`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/getAllResponseHeaders)

以字符串的形式返回所有用 [CRLF](https://developer.mozilla.org/zh-CN/docs/Glossary/CRLF) 分隔的响应头，如果没有收到响应，则返回 `null`。



[`XMLHttpRequest.getResponseHeader()`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/getResponseHeader)

返回包含指定响应头的字符串，如果响应尚未收到或响应中不存在该报头，则返回 `null`。



[`XMLHttpRequest.open()`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/open)

初始化一个请求。该方法只能在 JavaScript 代码中使用，若要在 native code 中初始化请求，请使用 [`openRequest()`](https://developer.mozilla.org/zh-CN/docs/Mozilla/Tech/XPCOM/Reference/Interface/nsIXMLHttpRequest)。



[`XMLHttpRequest.overrideMimeType()`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/overrideMimeType)

覆写由服务器返回的 MIME 类型。



[`XMLHttpRequest.send()`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/send)

发送请求。如果请求是异步的（默认），那么该方法将在请求发送后立即返回。



[`XMLHttpRequest.setRequestHeader()`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/setRequestHeader)

设置 HTTP 请求头的值。必须在 `open()` 之后、`send()` 之前调用 `setRequestHeader()` 方法。



#### 事件

[`abort`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/abort_event)

当 request 被停止时触发，例如当程序调用 [`XMLHttpRequest.abort()`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/abort) 时。
也可以使用 [`onabort`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onabort) 属性。



[`error`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/error_event)

当 request 遭遇错误时触发。
也可以使用 [`onerror`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onerror) 属性



[`load`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/load_event)

[`XMLHttpRequest`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest)请求成功完成时触发。
也可以使用 [`onload`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onload) 属性.



[`loadend`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/loadend_event)

当请求结束时触发, 无论请求成功 ( [`load`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/load_event)) 还是失败 ([`abort`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/abort_event) 或 [`error`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/error_event))。
也可以使用 [`onloadend`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onloadend) 属性。



[`loadstart`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/loadstart_event)

接收到响应数据时触发。
也可以使用 [`onloadstart`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onloadstart) 属性。



[`progress`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/progress_event)

当请求接收到更多数据时，周期性地触发。
也可以使用 [`onprogress`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/onprogress) 属性。



[`timeout`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequest/timeout_event)

在预设时间内没有接收到响应时触发。
也可以使用 [`ontimeout`](https://developer.mozilla.org/zh-CN/docs/Web/API/XMLHttpRequestEventTarget/ontimeout) 属性。



> 以上部分摘抄自MDN，因为 XHR 是Web API，具体实现要看浏览器的支持情况。
>
> 最早是微软提出来的，IE10+已经能全部支持，放心使用。



### 使用

#### XHR 对象

XHR最早起源于微软的库，早期的开发需要考虑很多的因素，判断条件有很多，现在创建 XHR 对象只需要 new 即可。

> var xhr = new XMLHttpRequest();



#### XHR 的用法

使用XHR对象时，第一个需要知道的方法是 open()，它有三个参数：

- 要发送请求的类型（get，post等）
- 请求的 URL 
- 表示是否异步发送请求的布尔值

> // 这行代码会启动一个针对 example.php 的get请求
>
> xhr.open("get","example.php", false);
>
> // 说明
>
> // 一是 URL 相对于执行代码的当前页面（也可以使用绝对路径）
>
> // 二是调用 open() 方法并不会真正发送请求，而只是启动一个请求以备发送



发送请求，用到的方法是 send() 方法

> xhr.open("get","example.php", false);
>
> xhr.send(null);

这里的 send() 方法接收一个参数，即要作为请求主体发送的数据。

如果不需要通过请求主体发送数据，则必须传入 null，因为这个参数对有些浏览器来说是必需的。调用 send()之后，请求就会被发送到服务器。



收到响应后，响应的数据会自动填充 XHR 对象的属性，如下

- responseText：作为响应主体被返回的文本。
- responseXML：如果响应的内容类型是"text/xml"或"application/xml"，这个属性将保存包含着响应数据的 XML DOM 文档。
- status：响应的 HTTP 状态。
- statusText：HTTP 状态的说明。

```js
xhr.open("get","example.php", false);
xhr.send(null);

if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
    alert(xhr.responseText);
} else {
    alert("error:" + xhr.status);
}
```



上面的写法用于同步请求可以，但是不适用于异步请求，大多数情况下还是异步请求。此时，可以检测 XHR 对象的 readyState 属性，该属性表示请求/响应过程的当前活动阶段。readyState 属性可取的值如下：

- 0：未初始化。尚未调用 send() 方法。
- 1：启动。已经调用 open() 方法，但尚未调用 send() 方法
- 2：发送。已经调用 send() 方法，但尚未接收到响应。
- 3：接收。已经接收到部分响应数据。
- 4：完成。已经接收到全部响应数据，而且已经可以在客户端使用了。

**readyState 属性值的变化，都会触发一次 readystatechange 事件。**

```js
// 我们只对readyState值为4的阶段感兴趣
var xhr = new XMLHttpRequest();
xhr.readystatechange = function(){
    if (xhr.readyState == 4){
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
    alert(xhr.responseText);
} else {
    alert("error:" + xhr.status);
}
    }
}

xhr.open("get","example.php", false);
xhr.send(null);
```



可以调用 abort() 方法，在接收到响应之前取消异步请求，如下

```js
xhr.abort();
```

调用这个方法之后，XHR 对象会停止触发事件，而且也不再允许房屋任何与响应相关的对象属性。



#### HTTP 头部信息

​	每个 HTTP 请求和响应都会带有相应的头部信息，其中有的有用，有的没用。XHR 对象也提供了操作这两种头部（即请求头部和响应头部）信息的方法。

​	默认情况下，在发送 XHR 请求的同时，还会发送下列头部信息。

- Accept：浏览器能够处理的内容类型。
- Accept-Charset：浏览器能够显示的字符集。
- Accept-Encoding：浏览器能够处理的压缩编码。
- Accept-Language：浏览器当前设置的语言。
- Connection：浏览器与服务器之间连接的类型。
- Cookie：当前页面设置的任何Cookie。
- Host：发出请求的页面所在的域。
- Referer：发出请求的页面的URI。
- User-Agent：浏览器的用户代理字符串。



​	虽然不同浏览器实际发送的头部信息会有所不同，但以上列出的基本上是所有浏览器都会发送的。可以使用 setRequestHeader() 方法设置自定义的头部信息。这个方法有两个参数：

头部字段的名称和头部字段的值。

```js
xhr.setRequestHeader("MyHeader", "MyValue");
```

> 最好使用自定义的头部字段名称，不要使用浏览器正常发送的字段名称，否则有可能会影响服务器的响应。



可以通过方法 getResponseHeader() 和 getAllResponseHeaders() 获取到返回的头部信息

```js
// 获取指定名称的头部字段值
var myHeader = xhr.getResponseHeader("MyHeader");
// 获取所有头部字段信息的长字符串
var allHeaders = xhr.getAllResponseHeaders();
```



#### GET 请求

​	get 是最常见的请求类型，最常用于向服务器查询某些信息。必要时，可以将查询字符串参数追加到 URL 的末尾，以便将信息发送给服务器。

​	对于 XHR 来说，位于传入 open 方法的 URL 末尾的查询字符串必须经过正确的编码才行。查询字符串中每个参数的名称和值都必须使用 encodeURIComponent() 进行编码，然后才能放到 URL 的末尾；而且所有名-值对都必须由（&）分隔，如下

```js
xhr.open("get", "example.php?name1=value1&name2=value2", true);
```

下面这个函数可以向现有的 URL 的末尾加查询字符串参数：

```js
function addURLParam(url, name, value) {
    url += (url.indexOf("？") == -1 ? "?" : "&");
    url += encodeURIComponent(name) + "=" + encodeURIComponent(value);
    return url;
}
```

> 这个方法写的挺好的，也很实用，值得学习



#### POST 请求

​	post仅次于 get，通常用于向服务器发送应该被保存的数据。POST 请求应该把数据作为请求的主体提交，而 get 请求传统上不上这样。post 请求的主体可以包含非常多的数据，而且格式不限。

```js
xhr.open("post", "example.php", true);
```

然后通过 send 方法传入某些数据

```js
xhr.send(xxx);
```

传入的数据需要规定格式，服务器才能识别，假如我们要提交表单数据，那么需要首先将Content-Type 设置为 application/x-www-form-urlencoded，然后将表单数据进行合理的处理才可以。



### 进一步使用

#### FormData

​	现代 Web 应用中频繁使用的一项功能就是表单数据的序列化，XHR 2级为此定义了 FormData 类型。FormData 为序列化表单以及创建于表单格式相同的数据（用于通过 XHR 传输）提供了便利。

```js
var data = new FormData();
data.append("name", "Nicholas");
```

append 方法接收两个参数：键和值，分别对应表单字段的名称和字段中包含的值。可以添加任意多个。也可以用表单元素的数据预先向其中填入键值对：

```js
var data = new FormData(document.form[0]);
// FormData 的实例，可以将它直接传给 XHR 的 send 方法，如下
...
var from = document.getElementById("user-info");
xhr.send(new FormData(form));
```

​	使用 FormData 的方便之处体现在不必明确地在 XHR 对象上设置请求头部。 XHR 对象能够识别传入的数据类型是 FormData 的实例，并配置适当的头部信息。



#### 超时设定

​	XHR 对象有一个 timeout 属性，表示请求在等待响应多少毫秒之后就终止。在给 timeout 设置一个值之后，如果在规定的时间内浏览器还没有接收到响应，那么就会触发 timeout 事件，进而会调用 ontimeout 事件处理程序。

```js
// 使用timeout，配合 ontimeout 方法
var xhr = new XMLHttpRequest();
xhr.readystatechange = function(){
    if (xhr.readyState == 4){
        if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
    alert(xhr.responseText);
} else {
    alert("error:" + xhr.status);
}
    }
}

xhr.open("get","example.php", true);
xhr.timeout = 1000; // 1s
xhr.ontimeout = function(){
  alert("Request did not return in a second.");
};
xhr.send(null);
```



#### overrideMimeType()方法

​	该方法用于重写 XHR 响应的 MIME 类型。因为返回响应的 MIME 类型决定了 XHR 对象如何处理它，所以提供一种方法能够重写服务器返回的 MIME 类型是有用的。

​	比如，服务器返回的 MIME 类型是 text/plain，但数据中实际包含的是 XML。根据  MIME类型，即使数据是 XML，responseXML属性中仍然是null。通过调用 overrideMimeType 方法，可以保证把响应当做 XML 而非纯文本处理。

```js
var xhr = new XMLHttpRequest();
xhr.open("get", "text.php", true);
xhr.overrideMimeType("text/plain");
xhr.send(null);
```

​	这个例子强迫 XHR 对象将响应当做 XML 而非纯文本来处理。调用overrideMimeType 必须在send 方法之前，才能保证重写响应的 MIME 类型。



#### 进度事件

与客户端服务通信有关的事件。以下有6个进度事件。

- loadstart：在接收到响应数据的第一个字节时触发。
- progress：在接收响应期间持续不断地触发。
- error：在请求发送错误时触发。
- abort：在因为调用 abort() 方法而终止连接时触发。
- load：在接收到完整的响应数据时触发。
- loadend：在通信完成或者触发 error、abort或load事件后触发。

每个请求都是从触发 loadstart 事件开始，接下来是一个或多个 progress 事件，然后触发 error、abort 或 load 事件中的一个，最后以触发 loadend 事件结束。

**事件 不等于 方法**，load 事件对应的事件处理程序是 onload 方法。



**load 事件**

​	load事件的引入是为了简化交互模型，用于替代 readystatechange 事件。响应接收完毕后将触发 load 事件，因此也就没有必要去检查 readyState 属性了。而 onload 事件处理程序会接收到一个 event 对象，其 target 属性就指向 XHR 对象实例，因而可以访问到 XHR 对象的所有方法和属性。然而并非所有浏览器都为这个事件实现了适当的事件对象。结果，开发人员还是要像下面这样被迫使用 XHR 对象变量。

```js
var xhr = new XMLHttpRequest();
xhr.onload = function(){
	if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
    	alert(xhr.responseText);
	} else {
  	  alert("error:" + xhr.status);
	}  
}

xhr.open("get","example.php", true);
xhr.send(null);
```

​	只要浏览器接收到服务器的响应，不管其状态如何，都会触发 load 事件。而这意味着你必须要检查 status 属性，才能确定数据是否真的已经可用了。

> 无论成功失败，服务器只要响应了，load事件就会触发



**progress 事件**

这个事件会在浏览器接收新数据期间周期性地触发。而 onprogress 事件处理程序会接收到一个 event 对象，其 target 属性是 XHR 对象，但包含着三个额外的属性：lengthComputable、position和totalSize。其中，lengthComputable是一个表示进度信息是否可用的布尔值，position 表示已经接收的字节数，totalSize 表示根据 Content-Length 响应头部确定的预期字节数。有了这些信息，我们就可以为用户创建一个进度指示器了。

```js
var xhr = new XMLHttpRequest();
xhr.onload = function(){
	if ((xhr.status >= 200 && xhr.status < 300) || xhr.status == 304){
    	alert(xhr.responseText);
	} else {
  	  alert("error:" + xhr.status);
	}  
}
xhr.onprogress = function(event) {
    var divStatus = document.getElementById("status");
    if (event.lengthComputable){
        divStatus.innerHTML = "Received " + event.position + "of" +
            event.totalSize + "bytes";
    }
}

xhr.open("get","example.php", true);
xhr.send(null);
```

> 需要写在 send 之前
>
> 可用来获取实时信息
>
> 可用计算接收数据的百分比，upload，download等组件