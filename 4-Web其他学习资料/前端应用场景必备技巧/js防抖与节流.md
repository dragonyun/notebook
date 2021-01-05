---
typora-root-url: ..\img
---

## js 防抖与节流



### 针对场景

---

在进行窗口的resize、scroll，输入框内容校验等操作时，如果事件处理函数调用的频率无限制，会加重浏览器的负担，导致用户体验非常糟糕。此时我们可以采用debounce（防抖）和throttle（节流）的方式来减少调用频率，同时又不影响实际效果。 



### 防抖

---

![防抖](/防抖.png)

```js
// 防抖
function debounce(fn, wait) {    
    var timeout = null;    
    return function() {        
        if(timeout !== null)   clearTimeout(timeout);        
        timeout = setTimeout(fn, wait);    
    }
}
// 处理函数
function handle() {    
    console.log(Math.random()); 
}
// 滚动事件
window.addEventListener('scroll', debounce(handle, 1000));
```





### 节流

---

![节流](/节流.png)

节流代码



### 小结

---

函数防抖：将几次操作合并为一此操作进行。原理是维护一个计时器，规定在delay时间后触发函数，但是在delay时间内再次触发的话，就会取消之前的计时器而重新设置。这样一来，只有最后一次操作能被触发。

函数节流：使得一定时间内只触发一次函数。原理是通过判断是否到达一定时间来触发函数。

区别： 函数节流不管事件触发有多频繁，都会保证在规定时间内一定会执行一次真正的事件处理函数，而函数防抖只是在最后一次事件后才触发一次函数。 比如在页面的无限加载场景下，我们需要用户在滚动页面时，每隔一段时间发一次 Ajax 请求，而不是在用户停下滚动页面操作时才去请求数据。这样的场景，就适合用节流技术来实现。



> 防抖是等待用户无操作delay时间才会触发功能，如果用户持续操作，那就重置等待时间，直到无操作delay时间。
>
> 节流像是定时循环执行，如果用户持续操作，固定时间间隔内只执行一次。





### 参考资料

---

- js防抖与节流:`https://www.cnblogs.com/momo798/p/9177767.html`
- 同上：`https://mp.weixin.qq.com/s/Vkshf-nEDwo2ODUJhxgzVA`