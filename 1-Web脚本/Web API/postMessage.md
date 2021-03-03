## window.postMessage

本质是通过获得窗口的引用，然后调用`postMessage`分发`MessageEvent`消息。



### 用途

---

跨源通信

通常，只有两个同源（协议，端口，主机）的页面才可以脚本通信。window.postMessage()提供了一种受控机制来规避该限制。

> 跨源，跨域指的是类似的问题。根据不同的场景有不同的解决方案。
>
> 该方案，用于前端有关联窗口直接的通信。（能获取到窗口的引用，就可以有关联，就可以通信）



### 语法

---

> ```
> otherWindow.postMessage(message, targetOrigin, [transfer]);
> ```

- otherWindow

  其他窗口的一个引用，比如iframe的contentWindow属性、执行[window.open](https://developer.mozilla.org/en-US/docs/Web/API/Window/open)返回的窗口对象、或者是命名过或数值索引的[window.frames](https://developer.mozilla.org/en-US/docs/Web/API/Window/frames)。

- message

  将要发送到其他 window的数据。它将会被[结构化克隆算法](https://developer.mozilla.org/en-US/docs/DOM/The_structured_clone_algorithm)序列化。

- targetOrigin

  通过窗口的origin属性来指定哪些窗口能接收到消息事件，其值可以是字符串"*"（表示无限制）或者一个URI。在发送消息的时候，如果目标窗口的协议、主机地址或端口这三者的任意一项不匹配targetOrigin提供的值，那么消息就不会被发送；只有三者完全匹配，消息才会被发送。（该参数可以看做校验）



### 接收消息

---

postMessage是分发消息，其他window可以监听分发的message

```js
window.addEventListener("message", receiveMessage, false);

function receiveMessage(event)
{
  var origin = event.origin
  if (origin !== "http://example.org:8080")
    return;
  // ...
}
```

message属性（即回调函数的参数event）

- data

  从其他 window 中传递过来的对象。

- origin

  调用 `postMessage` 时消息发送方窗口的 [origin](https://developer.mozilla.org/en-US/docs/Origin) 

- source

  对发送消息的[窗口](https://developer.mozilla.org/en-US/docs/Web/API/Window)对象的引用; 您可以使用此来在具有不同origin的两个窗口之间建立双向通信。



### 总结

---

- 安全问题
  - 不希望接收message，就不要监听
  - **始终使用origin和source属性验证发件人的身份**
  - **使用postMessage将数据发送到其他窗口时，始终指定精确的目标origin，而不是\*。**

- 通信的双方
  - 通过窗口的引用发送信息
  - 发送方可以通过targetOrigin指定接收方
  - 接收方可以校验origin和source，指定发送方



### 参考资料

---

- 官方介绍：https://developer.mozilla.org/zh-CN/docs/Web/API/Window/postMessage