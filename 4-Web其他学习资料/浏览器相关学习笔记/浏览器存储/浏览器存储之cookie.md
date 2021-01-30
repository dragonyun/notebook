## cookie学习笔记

### cookie是什么

---

​	cookie是服务器发送到用户浏览器并保存在本地的一小块数据，它会在浏览器下次向同一服务器再发起请求时被携带并发送到服务器上。通常，它用于告知服务器端两个请求是否来自同一浏览器，如保持用户的登录状态。cookie使基于无状态的HTTP协议记录文档的状态信息成为了可能。

查看路径：`浏览器控制台->Application->Storage->Cookies`



### cookie的用途

---

- 会话状态管理（如用户登录状态、购物车、游戏分数或其他需要记录的信息等）
- 个性化设置（如用户自定义设置、主题等）
- 浏览器行为跟踪（如跟踪分析用户行为等）

早期的cookie作为唯一的存储方式，用于携带各种数据，但是，浏览器的每次请求都会携带cookie数据，会带来额外的性能开销。在新的浏览器存储方式（storage，indexedDB）出现之后，cookie便只用于少数场景。



### 服务端如何创建cookie

---

​	当服务器收到 HTTP 请求时，服务器可以在响应头里面添加一个 [`Set-Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Set-Cookie) 选项。浏览器收到响应后通常会保存下 Cookie，之后对该服务器每一次请求中都通过 [`Cookie`](https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Headers/Cookie) 请求头部将 Cookie 信息发送给服务器。另外，Cookie 的过期时间、域、路径、有效期、适用站点都可以根据需要来指定。

比如，下面是服务器通过该头部告知客户端保存 Cookie 信息。

```js
HTTP/1.0 200 OK
Content-type: text/html
Set-Cookie: yummy_cookie=choco
Set-Cookie: tasty_cookie=strawberry
```

那么，浏览器会存储这两个cookie信息，在之后的每次请求时都会通过HTTP头部cookie发回服务器。

```js
GET /sample_page.html HTTP/1.1
Host: www.example.org
Cookie: yummy_cookie=choco; tasty_cookie=strawberry
```



### cookie存储限制（大小等）

---

​	cookie是遵守同源策略的，即它是与特定域绑定的。设置cookie之后，它会与请求一起发送到创建它的域。这个限制能保证cookie中存储的信息只对被认可的接受者开放，不被其他域访问。

​	不同的浏览器对于cookie的存储限制是有差异的，这里有一个大致的推荐：

- 不超过300个cookie；这里的每个是指一条数据，比如name=value。
- 每个cookie不超过4096字节；长度是name+value，比如name=lili，那就是8个字节。一般情况下一个字符占用1个字节，但是如果使用多字节字符（如UTF-8 Unicode），那么每个字符最多可以占用4个字节。
- 每个域不超过20个cookie；遵守同源策略，如果超过单个域上限，浏览器会删除之前设置的cookie。
- 每个域不超过81920字节。

提醒一下：我们平时说的cookie不超过4k，指的是单个域单个cookie大小不超过4k。

> 创建cookie的域不等于存储它的域，a.com页面可以存储来自b.com的cookie。这里的每个域指的是创建域。



### cookie的构成属性

---

cookie在浏览器中由名称，值，域，路径，过期时间，安全标志等参数组成。

`Name、Value、Domain、Path、Expires/Max-Age、Size、HttpOnly、Secure、SameSite、Priority`



#### 1.名称和值

---

- 名称Name：唯一标识 cookie 的名称。cookie 名不区分大小写，因此 myCookie 和 MyCookie 是同一个名称。不过，实践中最好将 cookie 名当成区分大小写来对待，因为一些服务器软件可能这样对待它们。cookie 名必须经过 **URL 编码**。
- 值Value：存储在 cookie 里的字符串值。这个值必须经过 **URL 编码**。
- 大小Size：cookie大小。

名称`Name`和值`Value`的长度之和代表了cookie的大小`Size`。



#### 2.cookie的作用域

---

即允许cookie应该发给哪些URL。

- 域Domain：cookie有效的域。如果不指定，默认为origin，不包含子域名。如果指定了，则一般包含子域名。例如，如果设置 `Domain=mozilla.org`，则 Cookie 也包含在子域名中（如`developer.mozilla.org`）。

- 路径path：指定了主机下的哪些路径可以接受cookie（该URL路径必须存在于请求URL中）。子路径也会被匹配。比如，设置`Path=/docs`，则`/docs、/docs/Web/、/docs/Web/HTTP`都会被匹配到。只有被匹配到才会发送cookie。

- SameSite：cookie允许服务器要求某个cookie在跨站请求时不会被发送，（其中site由可注册域定义），从而可以阻止跨站请求伪造攻击（CSRF）。**跨站**是指当前页面URL和请求的URL不一致。

  有三个可选值：

  - None：浏览器会在同站请求、跨站请求下继续发送cookie，不区分大小写。
  - Strict：浏览器将只在访问相同站点时发送cookie。换言之，只有当前页面的URL与请求目标一致，才会带上Cookie。（过于严格），比如，当前网页有一个 GitHub 链接，用户点击跳转就不会带有 GitHub 的 Cookie，跳转过去总是未登陆状态。
  - Lax：与Strict类似，但用户从外部站点导航至目标网址的Get请求除外。导航到目标网址的 GET 请求，只包括三种情况：链接，预加载请求，GET 表单。



#### 3.cookie的生命周期

---

- 过期时间Expires/Max-Age：表示何时会删除cookie的时间戳。一种是会话期cookie，浏览器关闭之后它会被自动删除，这种cookie不注意指定过期时间。另一种是持久性cookie，这一种生命周期取决于过期时间（`Expires`）或有效期（`Max-Age`）指定的一段时间。

这个值是GMT格式（Wdy, DD-Mon-YYYY HH:MM:SS GMT）

```js
Set-Cookie: id=a3fWa; Expires=Wed, 21 Oct 2015 07:28:00 GMT;
```



#### 4.安全标识

---

- HttpOnly：该cookie只有服务端可以访问，可防止JavaScript访问cookie的值。
- Secure：只有使用SSL安全连接的情况下才会把cookie发送到服务器。
- priority：优先级。



### js访问cookie

---

通过 [`Document.cookie`](https://developer.mozilla.org/zh-CN/docs/Web/API/Document/cookie) 属性可创建新的 Cookie，也可通过该属性访问非`HttpOnly`标记的Cookie。可读可写。

```js
document.cookie = "yummy_cookie=choco";
document.cookie = "tasty_cookie=strawberry";
console.log(document.cookie);
```

通过 JavaScript 创建的 Cookie 不能包含 HttpOnly 标志。

因为需要URL编码，最好写成

```js
document.cookie = encodeURIComponent("name") + "=" +
encodeURIComponent("Nicholas") + "; domain=.wrox.com; path=/";
```

这里涉及到一个**子cookie**的概念：为了绕过浏览器对每个域cookie数量的限制，单个cookie存储多个名/值对，比如

```js
name=name1=value1&name2=value2&name3=value3&name4=value4&name5=value5
```







### 安全危险

---

#### 1.CSRF跨站请求伪造

----

Cookie 往往用来存储用户的身份信息，恶意网站可以设法伪造带有正确 Cookie 的 HTTP 请求，这就是 CSRF 攻击。

比如，首先，用户登录了银行网站`bank.com`，银行服务器发来了一个cookie。

```js
Set-Cookie:id=a3fWa;
```

然后，用户访问了恶意网站`malicious.com`，上面有一个表单。

```js
<form action="bank.com/transfer" method="POST">
  ...
</form>
```

用户一旦被诱骗发送这个表单，银行网站会收到带有正确cookie的请求。

这就是CSRF攻击。

这种第三方网站引导发出的cookie，就称为第三方cookie。

这个还可以用于**用户追踪**。

比如，Facebook 在第三方网站插入一张看不见的图片。

`<img src="facebook.com" style="visibility:hidden;">`

浏览器加载上面代码时，就会向 Facebook 发出带有 Cookie 的请求，从而 Facebook 就会知道你是谁，访问了什么网站。

> 因为你带了cookie，所以知道你是谁，因为请求头里面带有当前URL的信息，所以知道你访问了什么网站。



#### 2.XSS 跨域脚本攻击

---

XSS（Cross Site Scripting）通过在页面中注入脚本，然后触发执行，获取相应的权限。

1.获取权限主要是指获取cookie，不过httponly可以一定程度上缓解这个攻击。

2.还有就是构造特殊字符进行攻击，需要进行XSS过滤脚本。

3.也可以通过CSP设置白名单来控制页面可以访问的资源。



### 参考资料

---

1. JavaScript高级程序设计第四版第25章
2. MDN:`https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Cookies`
3. cookie-samesite：`http://www.ruanyifeng.com/blog/2019/09/cookie-samesite.html`