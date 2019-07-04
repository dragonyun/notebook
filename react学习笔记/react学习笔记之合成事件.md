## react学习笔记之合成事件

> React 事件系统一部分的 `SyntheticEvent` 包装器

> 这个本来要写到事件处理那一部分，但是我一直以来都困惑这个问题，所以单独拿出来总结了

> 就是页面上的事件event，鼠标，键盘，等等各种在页面上发生的事件，这个我是知道的，
>
> 我一直的困惑是每次写的事件函数带了一个参数e/event，然后这个e里面都有什么？
>
> 我每次都在用，但是就是搞不清参数还有相互之间的关系

> 我先总结一下：
>
> 页面上发生的事件event很多，都是希望通过事件来触发函数，还涉及到捕获和冒泡的概念，先不说；
>
> 调用的函数可以带参数e，这个e参数里面既包含每个事件都会带的共用的属性方法，也有拥有自己的属性方法。
>
> 那么这里在react里面，又包了更多的属性和方法，当然是为了自己的效率更高，视情况使用

### 概览

> `SyntheticEvent` 实例将被传递给你的事件处理函数，它是浏览器的原生事件的跨浏览器包装器。
>
> 除兼容所有浏览器外，它还拥有和浏览器原生事件相同的接口，包括 `stopPropagation()`和 `preventDefault()`。

> 如果因为某些原因，当你需要使用浏览器的底层事件时，只需要使用 `nativeEvent` 属性来获取即可。每个 `SyntheticEvent` 对象都包含以下属性：

> ```js
> // 公共的属性和方法
> boolean bubbles
> boolean cancelable
> DOMEventTarget currentTarget
> boolean defaultPrevented
> number eventPhase
> boolean isTrusted
> DOMEvent nativeEvent
> void preventDefault()
> boolean isDefaultPrevented()
> void stopPropagation()
> boolean isPropagationStopped()
> DOMEventTarget target
> number timeStamp
> string type
> ```
>
> 截止 `v0.14`，当事件处理函数返回 `false` 时，不再阻止事件冒泡。你可以选择使用 `e.stopPropagation()` 或者 `e.preventDefault()` 替代。

### 事件池

> `SyntheticEvent` 是合并而来。这意味着 `SyntheticEvent` 对象可能会被重用，而且在事件回调函数被调用后，所有的属性都会无效。出于性能考虑，你不能通过异步访问事件。

> ```js
> function onClick(event) {
>   console.log(event); // => nullified object.
>   console.log(event.type); // => "click"
>   const eventType = event.type; // => "click"
> 
>   setTimeout(function() {
>     console.log(event.type); // => null
>     console.log(eventType); // => "click"
>   }, 0);
> 
>   // 不起作用，this.state.clickEvent 的值将会只包含 null
>   this.setState({clickEvent: event});
> 
>   // 你仍然可以导出事件属性
>   this.setState({eventType: event.type});
> }
> ```
>
> 如果你想异步访问事件属性，你需在事件上调用 `event.persist()`，此方法会从池中移除合成事件，允许用户代码保留对事件的引用。

> 我没看懂这一块在说什么，主要是希望在使用事件属性时，最好不要直接异步使用；
>
> 异步使用事件属性可能有问题，要谨慎使用

### 支持的事件

> React 通过将事件 normalize 以让他们在不同浏览器中拥有一致的属性。
>
> 以下的事件处理函数在冒泡阶段被触发。如需注册捕获阶段的事件处理函数，则应为事件名添加 `Capture`。例如，处理捕获阶段的点击事件请使用 `onClickCapture`，而不是 `onClick`。

> 事件有很多，我不再一一贴出来了，具体每个事件包含了哪些更详细的事件，并且事件又包含了什么属性，请参考下面的链接。
>
> https://react.docschina.org/docs/events.html#clipboard-events

- Clipboard Events 剪贴板事件
- Composition Events 复合事件
- Keyboard Events 键盘事件
- Focus Events 焦点事件
- Form Events 表单事件
- Mouse Events 鼠标事件
- Pointer Events 指针事件
- Selection Events 选择事件
- Touch Events 触摸事件
- UI Events UI 事件
- Wheel Events 滚轮事件
- Media Events 媒体事件
- Image Events 图像事件
- Animation Events 动画事件
- Transition Events 过渡事件
- Other Events 其他事件