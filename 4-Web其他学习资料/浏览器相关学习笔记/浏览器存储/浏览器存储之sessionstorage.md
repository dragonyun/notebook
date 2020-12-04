## 浏览器存储之 `sessionstorage`

`sessionstorage`的定位是页面会话。为每个给定的源维护一个单独的存储区域（即同源限制）。只要浏览器打开（包括页面刷新加载和恢复），该页面的数据可用。



### 几大特性

---

- **同步操作**
- **同源限制** 受到同源限制，每一个session对应创建它的域名。网页只能访问自身域名下的session数据，而不能访问跨域的session数据。
- **无法跨页签** 在新标签或窗口打开一个页面时会复制顶级浏览会话的上下文作为新会话的上下文。打开多个相同URL的Tabs页面，会创建各自的`sessionstorage`
- **单页签会话** 页面会话在浏览器打开期间一直保持，页面的刷新加载恢复都不会使数据丢失。
- **临时保存** 数据在页面会话结束时会被清除。关闭对应浏览器窗口(window)/tab，会清除对应的`sessionstorage`。
- **键值对存储** 
- **容量有限** 大约5M（不同浏览器不一样），这个5M是单个源的存储大小，如果有多个源，每个源都有5M。





### 用法

---

> `myStorage = sessionStorage` ;
>
> 或者 `myStorage = window.localStorage`;

获取到的是一个可被用于访问当前源（origin）的本地存储空间`Storage`对象。



- `getItem(key)` 取值
- `setItem(key, value)` 设置
- `removeItem(key) `删除
- `clear()` 清空
- `key(keyIndex) `根据index获取key
- `length `数量



### 兼容性

---

大于等于 `ie8`



### 文档参考

---

- `MDN`：`https://developer.mozilla.org/zh-CN/docs/Web/API/Window/sessionStorage`