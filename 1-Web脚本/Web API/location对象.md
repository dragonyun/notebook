## location对象

​	location对象是BOM对象中最有用的对象之一，它提供了与当前窗口中加载的文档有关的信息，还提供了一些导航功能。

​	location对象的用处不只表现在它保存着当前文档的信息，还表现在它将URL解析成独立的片段，让开发人员可以通过不同的属性访问到这些片段。

```js
// location既是window的属性，也是document的属性
// 两者引用的是同一个对象
window.location = document.location
```



### 属性说明

location对象的属性说明

| 属性名   | 例子                  | 说明                                                         |
| -------- | --------------------- | ------------------------------------------------------------ |
| hash     | "#contents"           | 返回URL中的hash（#号后跟零或多个字符），如果URL中不包含散列，则返回空字符串 |
| host     | "www.wrox.com:80"     | 返回服务器名称和端口号（如果有）                             |
| hostname | "www.wrox.com"        | 返回不带端口号的服务器名称                                   |
| href     | "http://www.wrox.com" | 返回当前加载页面的完整URL。而location对象的toString()方法也返回这个值 |
| pathname | "/WileyCDA"           | 返回URL中的目录和（或）文件名                                |
| port     | "8080"                | 返回URL中指定的端口号。如果URL中不包含端口号，则这个属性返回空字符串 |
| protocol | "https:"              | 返回页面使用的协议。通常是http:或https:                      |
| search   | "?q=zly"              | 返回URL的查询字符串。这个字符串以问号开头                    |

> 不全，看浏览器的实现，还有例如origin这样的属性

有了上述属性，再实现一些处理的方法，就可以很方便的访问到URL里面的信息。有一些依赖包，通过自我封装，实现了更多的功能。



**改变当前加载的页面**

通过改变属性可以改变当前加载的页面。

```js
//假设初始 URL 为 http://www.wrox.com/WileyCDA/ 
//将 URL 修改为"http://www.wrox.com/WileyCDA/#section1" 
location.hash = "#section1"; 
//将 URL 修改为"http://www.wrox.com/WileyCDA/?q=javascript" 
location.search = "?q=javascript"; 
//将 URL 修改为"http://www.yahoo.com/WileyCDA/" 
location.hostname = "www.yahoo.com"; 
//将 URL 修改为"http://www.yahoo.com/mydir/" 
location.pathname = "mydir"; 
//将 URL 修改为"http://www.yahoo.com:8080/WileyCDA/" 
location.port = 8080; 
```

每次修改 location 的属性（hash 除外），页面都会以新 URL 重新加载。

当通过上述任何一种方式修改 URL 之后，浏览器的历史记录中就会生成一条新记录，因此用户通过单击“后退”按钮都会导航到前一个页面。



### 方法

- toString()

  上面已经说过了，返回当前加载页面的完整URL，等于location.href

- assign()

  立即打开新URL并在浏览器的历史记录中生成一条记录。

  ```js
  location.assign("http://www.wrox.com");
  // 等价于
  window.location = "http://www.wrox.com";
  location.href = "http://www.wrox.com";
  ```

  **最常用的是 location.href**

- replace()

  只接受一个参数，即要导航到的URL；结果虽然会导致浏览器位置改变，但不会在历史记录中生成新记录。

  这个与修改属性变更浏览器地址表现不同，可以自由选择

- reload()

  重新加载当前显示的页面。如果调用reload()时不传递任何参数，页面就会以最有效的方式重新加载。也就是说，如果页面自上次请求以来并没有改变过，页面就会从浏览器缓存中重新加载。如果要强制从服务器重新加载，则需要像下面这样为该方法传递参数 true。

  ```js
  location.reload(); //重新加载（有可能从缓存中加载
  location.reload(true); //重新加载（从服务器重新加载）
  ```

  位于 reload()调用之后的代码可能会也可能不会执行，这要取决于网络延迟或系统资源等因素。
  为此，最好将 reload()放在代码的最后一行

