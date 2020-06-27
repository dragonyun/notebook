## window对象

### 简介

`window` 对象表示一个包含DOM文档的窗口，其 `document` 属性指向窗口中载入的 [DOM文档](https://developer.mozilla.org/zh-CN/docs/Web/API/Document)。

`window`作为全局变量，代表了脚本正在运行的窗口，暴露给 Javascript 代码。

---

> 老子说：道生一，一生二，二生三，三生万物。
>
> ECMAScript 是 JavaScript的核心；
>
> 在web中使用JavaScript，那么BOM（浏览器对象模型）才是真正的核心；
>
> 那么在BOM中，最核心的对象就是window

window表示浏览器的一个实例。在浏览器中，window对象有双重角色。

- 既是通过JavaScript访问浏览器窗口的一个接口
- 又是 ECMAScript 规定的 Global对象

这意味着在网页中定义的任何一个对象、变量和函数，都以 window 作为其 Global 对象。

> window下面挂着document，location，history等等很多很多的对象，还有例如parseInt，FormData等，应该说window在浏览器中才是位于顶层的对象。
>
> 因为有了window这个全局对象，所以我们在代码里面才可以直接使用定时器，location等等的对象和方法。
>
> **在控制台打印一下 console.log(window)，展开之后就很清楚了。**



### 全局作用域

​	由于 window 对象同时扮演着 ECMAScript 中 Global 对象的角色，因此所有在全局作用域中声明的变量、函数都会变成 window 对象的属性和方法。比如

```js
var age = 29;
function sayAge(){
    alert(this.age);
}
alert(window.age); //29
sayAge();		   //29
window.sayAge();   //29
```

全局作用域里面定义的变量和函数，它们被自动归在了window对象名下。

> 我们都有经验，在控制台，随便定义的变量，就等价于在全局作用域里定义的，那么它会自动挂在window下面。

定义全局变量和在 window 对象上直接定义属性还是有一点差别的：全局变量不能通过 delete 操作费擅长，而直接在window对象上的定义的属性可以。

```js
var age = 29;
window.color = "red";

delete window.age; 	//返回false
delete window.color; //返回true

alert(window.age);	//29
alert(window.color); //undefined
```

> 使用 var 语句添加的 window 属性有一个名为 [configurable] 的特性，这个特性的值被设置为false，因此这样定义的属性不可以通过 delete 操作符删除。

尝试访问未声明的变量会抛出错误，但是通过查询window对象，可以知道某个可能未声明的变量是否存在。

```js
var value = oldValue; // 报错,oldValue未定义
var value = window.oldValue //value的值是undefined，不会报错
```

> 不仅仅是window，这是一个通用的规则：
>
> 使用未定义的变量会报错；
>
> 但是如果是对象的属性未定义，那这个属性不会报错，就是undefined。
>
> 我们经常写 if(xxx && xxx.yyy)，如果对象存在，那么判断属性有没有，就是这个意思。



### 窗口关系及框架

​	如果页面中包含框架，则每个框架都拥有自己的 window 对象，并且保存在 frames 集合中。在 frames 集合中，可以通过数值索引（从0开始，从左至右，从上到下）或者框架名称来访问相应的 window 对象。每个 window 对象都有一个 name 属性，其中包含框架的名称。

> window虽然是全局对象，但是如果存在框架，那么框架本身也可以拥有自己的window
>
> 对于这个所谓的框架，我理解的不透彻，现在理解为 iframe 这样的

```html
<html>
    <head>
        <title>Frameset Example</title>
    </head>
    <frameset>
        <frame src="frame.htm" name="topFrame" >
        <frameset cols="50%,50%">
            <frame src="anotherframe.htm" name="leftFrame" >
            <frame src="yetanotherframe.htm" name="rightFrame" >
        </frameset>
    </frameset>
</html>
```

> frameset 元素可定义一个框架集。它被用来组织多个窗口（框架）。每个框架存有独立的文档。在其最简单的应用中，frameset 元素仅仅会规定在框架集中存在多少列或多少行。您必须使用 cols 或 rows 属性。
>
> 看来我理解的不对，还真有框架这么一个标签，还以为是react这样的框架。
>
> 那就是说这个标签就是人为的去拆分了不同的窗口。

​	以上代码创建了一个框架集，其中一个框架居上，两个框架居下。

```js
// 三个框架的调用方式
//  上方的框架
window.frames[0]
windwo.frames["topFrame"]
top.frames[0]
top.frames["topFrame"]
frames[0]
frames["topFrame"]

//  左下的框架
window.frames[1]
windwo.frames["leftFrame"]
top.frames[1]
top.frames["leftFrame"]
frames[1]
frames["leftFrame"]

// 右下的框架
window.frames[2]
windwo.frames["rightFrame"]
top.frames[2]
top.frames["rightFrame"]
frames[2]
frames["rightFrame"]
```

​	这里推荐使用top而不是window，因为 top 对象始终指向最高（最外）层的框架，也就是浏览器窗口。使用它可以确保在一个框架中正确地访问另一个框架。因为对于在一个框架中编写的任何代码来说，其中的 window 对象指向的都是那个框架的特定实例，而非最高层的框架。

> 嵌套关系复杂的话，直接用window 确实不方便，不如从 最外层 的 top 开始引用。

​	与 top 相对的另一个 window 对象是parent。顾名思义，parent（父）对象始终指向当前框架的直接上层框架。在某些情况下，parent 有可能等于 top；但在没有框架的情况下，parent 一定等于top（此时它们都等于 window）。

​	与框架有关的最后一个对象是 self ，它始终指向 window；实际上，self和window对象可以互换使用。引入 self 对象的目的只是为了与 top 和parent 对象对应起来，因此它不额外包含其他值。

```js
// 这些对象都是 window 对象的属性,可以如下访问
window.parent
window.top
window.self
// 也可以把不同层次的 window 对象连起来
window.parent.parent.frames[0]
```

> 就像组件的层级关系一样，框架也有层级关系。
>
> top是顶层，根。parent是上一层。self是当前层。就这样



### 窗口位置

这里不细说了，涉及到下面几个属性 和 方法，不同的浏览器表现不同。

```js
window.screenLeft
window.screenTop
window.screenX
window.screenY

// moveTo()接收的是新位置的x和y的坐标值
// moveBy()接收的是在水平和垂直方向上移动的像素数
window.moveTo(x,y) // 将窗口移动到以屏幕左上角为原点的（x,y）坐标位置
window.moveBy(m,n) // 将窗口水平方向移动m像素，垂直方向移动n像素
```



### 窗口大小

这里不细说了，涉及到下面几个属性 和 方法，不同的浏览器表现不同。

```js
innerWidth
innerHeight
outerWidth
outerHeight
clientWidth
clientHeight

// resizeTo()接收的是浏览器窗口的新宽度和新高度
// resizeBy()接收的是新窗口与原窗口的宽度和高度之差
window.resizeTo(100,100) // 调整到 100x100
window.resizeBy(100,50) // 调整到200x150
```



### 导航和打开窗口

```window.open()```

这个方法既可以导航到一个特定的 URL，也可以打开一个新的浏览器窗口。

四个参数

- 要加载的 URL
- 窗口目标
- 一个特性字符串
- 一个表示新页面是否取代浏览器历史记录中当前加载页面的布尔值

如果传了第二个参数，而且该参数是已有窗口或框架的名称，那么就会在具有该名称的窗口或框架中加载第一个参数指定的 URL。

```js
// 等同于 <a href="http://www.wrox.com" target="topFrame" />
window.open("http://www.wrox.com", "topFrame")
```

​	这行代码，等同于用户单击了上面那个链接。如果有一个名为“topFrame”的框架或者窗口，就会在该窗口或框架加载这个 URL；否则，就会创建一个新窗口并将其命名为“topFrame”。此外，第二个参数也可以是下列任何一个特殊的窗口名称：```_self、_parent、_top或_blank```.

​	然后是第三个参数，如果第二个参数并不是一个已经存在的窗口或者框架，那么该方法就会根据在第三个参数位置上传入的字符串创建一个新窗口或新标签页。在不打开新窗口的情况下，会忽略第三个参数。

```js
// 这行代码会打开一个新的可以调整大小的窗口，窗口初始大小为400x400像素，并且距离屏幕上沿和左边各10像素。
window.open("http://www.wrox.com", "wroxWindow", "height=400,width=400,top=10,left=10,resizable=yes")
```

​	window.open() 方法会返回一个指向新窗口的引用。引用的对象与其他 window 对象大致相似，但我们可以对其进行更多控制。

```js
var wroxWin = window.open("http://www.wrox.com", "wroxWindow", "height=400,width=400,top=10,left=10,resizable=yes");

// 调整大小
wroxWin.resizeTo(500,500);
// 移动位置
wroxWin.moveTo(100,100);
// 关闭新窗口
wroxWin.close();
```

