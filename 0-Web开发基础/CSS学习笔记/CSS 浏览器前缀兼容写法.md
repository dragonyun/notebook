## CSS 浏览器前缀兼容写法

---

Vendor prefix---浏览器引擎前缀，是一些放在 CSS 属性前的小字符串，用来确保这种属性只在特定的浏览器渲染引擎下才能识别和生效。

> 前缀主要是在区分内核，不同内核会识别自己独有的前缀（当然也可以识别别人的）。



### 为什么会有浏览器引擎前缀

这些浏览器引擎前缀主要是各种浏览器用来实验或者测试新出现的CSS3属性特征。

- 实验一些还未成为标准的CSS属性----也许永远不会成为标准
- 对新出现的标准的CSS3属性特征做实验性的实现
- 对CSS3中一些新属性做特效语义的个性实现

> 所以，我们可以得出结论，前缀是为了实验新的功能特征，可留可删。
>
> 不同引擎对于一条语法的实现，各有各的想法。



### 内核现状

- IE基于Trident
- Edge基于Chromium（webkit分支）
- chrome、safari、Opera基于WebKit
- 火狐基于Gecko



### 前缀列表

- -moz-		火狐等使用Mozilla浏览器引擎的浏览器
- -webkit-     Safari, 谷歌浏览器等使用Webkit引擎的浏览器
- -o-               Opera浏览器(早期)
- -ms-            Internet Explorer (不一定)



### 编写要求

​	**这些前缀并非所有都是需要的，但通常你加上这些前缀不会有任何害处——只要记住一条，把不带前缀的版本放到最后一行：**

```css
-moz-border-radius: 10px; 
-webkit-border-radius: 10px; 
-o-border-radius: 10px; 
border-radius: 10px; 
```

​	**有些新的CSS3属性已经试验了很久，一些浏览器已经对这些属性不再使用前缀。`Border-radius`属性就是一个非常典型的例子。最新版的浏览器都支持不带前缀的`Border-radius`属性写法。**





### 参考文献

- https://www.cnblogs.com/duyingxuan/p/6517933.html