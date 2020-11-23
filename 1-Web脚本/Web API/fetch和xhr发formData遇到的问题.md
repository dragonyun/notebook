## fetch和xhr发formData遇到的问题



### 问题描述

---

react项目，在fetch下使用FormData对form表单元素进行数据封装后进行post提交至服务器，其格式被转为了WebKitFormBoundary模式。

就是会出现`------WebKitFormBoundarys`对内容进行分割。

代码如下：

```js
let data = new FormData(document.getElementById('admin-edit__form'));
fetch('/func', {
    method: "POST",
    headers: {
      'Content-Type': 'application/x-www-form-urlencoded;charset=utf-8'
             },
    body: data
})
```

若直接使用浏览器原生form表单submit按钮提交，数据格式正常。

就是FormData部分会以键值对形式出现。

其它问题：

如果使用new XHR也会有同样的问题。



### 解决方案

---

- 方案一，URLSearchParams

  ```js
  var searchParams = new URLSearchParams()
  searchParams.set('method','next')
  fetch('https://www.example.com/', {
    method: 'POST',
    body: searchParams
  }).then(function(response){
    console.log(response.json())
  })
  ```

- 方案二，键值对

  ```js
  // 在headers对象中设置"Content-Type": "application/x-www-form-urlencoded; charset=UTF-8",
  fetch('https://www.example.com/', {
    method: 'POST',
    headers: {
      "Content-Type": "application/x-www-form-urlencoded; charset=UTF-8"
    },
    body: "method=next"
  }).then(function(response){
    console.log(response.json())
  })
  ```

  





### 参考网站

---

- https://segmentfault.com/q/1010000004930012
- https://blog.csdn.net/stpeace/article/details/82810272