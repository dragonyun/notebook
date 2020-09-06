## URL 编码转换函数总结



### 函数列表

- escape()

  ​	采用ISO Latin字符集对指定的字符串进行编码。所有的空格符、标点符号、特殊字符以及其他非ASCII字符都会转化成%xx格式的字符编码（xx代表此字符在字符集表里编码的16进制数字）。

  ​	比如，空格符的对应编码是%20。不会对ASCII字符和数字进行编码。不会被此方法编码的字符：@ * / +，反向编码函数：**unescape()**。

- encodeURI()

  ​	把URI字符串采用UTF-8编码格式转化成escape格式的字符串。不会被此方法编码的字符：! @ # $ & ( ) = ： / ; ? + '，反向编码函数：**decodeURI()**。

- encodeURIComponent()

  ​	把URI字符串采用URF-8编码格式转化成escape格式的字符串。与encodeURI相比，这个函数会将更多的字符进行编码，比如"/"等字符。所以如果字符串里面包含了URI的几个部分的话，不能用这个来进行编码。否则“/”字符被编码后将URL显示错误。不会被此方法编码的字符：! * ( )，反向编码函数：**decodeURIComp****onent()**。

> escape 用处不大。
>
> encodeURI 部分字符不会被转换，其他都会被转换。
>
> encodeURIComponent 更少的字符不会被转换，用于处理参数。



### 实例

- 编码

```js
var url = "http://localhost:8080/pro?a=1&b=张三&c=aaa";
var url1 = escape(url);                                           	  http%3A//localhost%3A8080/pro%3Fa%3D1%26b%3D%u5F20%u4E09%26c%3Daaa         
var url2 = encodeURI(url);                                      http://localhost:8080/pro?a=1&b=%E5%BC%A0%E4%B8%89&c=aaa
var url3 = encodeURIComponent(url);                  http%3A%2F%2Flocalhost%3A8080%2Fpro%3Fa%3D1%26b%3D%E5%BC%A0%E4%B8%89%26c%3Daaa   
```

- 反向编码

```js
var vurl2 = unescape(url2);                                    http://localhost:8080/pro?a=1&b=张三&c=aaa
var vurl3 = decodeURI(url3);                                  http://localhost:8080/pro?a=1&b=张三&c=aaa
var vurl1 = decodeURIComponent(url1);              http://localhost:8080/pro?a=1&b=张三&c=aaa     
```



### 使用场景

- escape()

  用于js对**字符串进行编码**。不常用

 

- encodeURI()

  用于**整个url跳转**，比如：

​       转化前： location.href = "[http://localhost:8080/pro?a=1&b=张三&c=aaa";](http://localhost:8080/pro?a=1&b=%E5%BC%A0%E4%B8%89&c=aaa%22;)

​       转化后： location.href = "http://localhost:8080/pro?a=1&b=%E5%BC%A0%E4%B8%89&c=aaa"

​        本例中只是将中文转成%...，传过去再解码就可以拿到中文

 

- encodeURIComponent()

  用于**参数的传递**，参数包含特殊字符可能会造成间断。比如：

  ```js
  var paramUrl = "http://localhost:8080/aa?a=1&b=2&c=3";
  
  var url = "http://localhost:8080/pp?a=1&b="+ paramUrl;
  ```

​       应该使用encodeURIComponent()进行转码，

​       结果：`http://localhost:8080/pp?a=1&b=http%3A%2F%2Flocalhost%3A8080%2Faa%3Fa%3D1%26b%3D2%23%26c%3D3`



### 参考文档

- URL编码： <https://www.cnblogs.com/douJiangYouTiao888/p/6473874.html>