## 浏览器存储之localstorage



### 场景

----

同步操作，前端存储少量数据，永久保存



### 特性

---

- **同源限制** 受到同源限制，每一个`localstorage`对应创建它的域名。网页只能访问自身域名下的`localstorage`，而不能访问跨域的`localstorage`。

- **同步操作** 
- **字符串键值对** 键值对总是以字符串的形式存储。
- **长久保存** 即使关闭浏览器，数据也会保存在浏览器中，只有主动清除才会清除数据。
- **容量有限** 大约5M（不同浏览器不一样），这个5M是单个源的存储大小，如果有多个源，每个源都有5M。



### 语法

---

> myStorage = localStorage;
>
> 或者 myStorage = window.localStorage;

获取到的是一个可被用于访问当前源（origin）的本地存储空间`Storage`对象。



### 接口

---

- getItem(key) 取值
- setItem(key, value) 设置
- removeItem(key) 删除
- clear() 清空
- key(keyIndex) 根据index获取key
- length 数量



### 兼容性

---

大于等于 ie8





### 文档参考

----

- MDN：`https://developer.mozilla.org/zh-CN/docs/Web/API/Window/localStorage`