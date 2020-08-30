## 各种width与height区分

这里不是讲DOM元素的盒子模型上的width和height，而是讲屏幕的宽高。

详细解释去看下面的引用文章，清晰明了，我抄一遍没有意义。



### 设备像素和CSS像素

---

这个必须要简单介绍一下。

设备像素是指 **正在使用的任何设备的物理像素**

CSS像素是指 **代码里面控制编写的如`width:100px`**

两者不一定相等。

> 100%缩放级别时，两者相等，一个CSS像素等于一个设备像素。

屏幕如果放大到200%，那么设备像素不会变化，CSS像素会增长到一个设备像素大小的四倍（宽度的两倍，高度的两倍，总共产生四倍）。

CSS像素还是原来的数值，只不过一个像素占用的设备像素的大小变成了原来的四倍。



### 各种width和height总结

就是平时我搞不清楚的一些获取宽高的变量

- screen.width和screen.height

  用户屏幕的总大小。测量的是设备像素。

  > 比如是电脑显示器，那就是显示器的设置的分辨率对应的宽高

- window.innerWidth和window.innerHeight

  浏览器窗口的总大小，包括滚动条。测量的是 CSS像素。

  IE不支持

  > 指浏览器中html所在的可视区域，包括滚动条。

- window.pageXOffset和window.pageYOffset

  页面滚动的偏移量。测量的是 CSS像素。

  > 浏览器页面可视区域小于实际document时，滚动之后，可视区域左上角相对于实际document左上角的偏移量

- document.documentElement.clientWidth/Height

  视口尺寸。测量的是 CSS像素。

  > 与window.innerWidth相似，不包括滚动条。

- document.documentElement.offsetWidth/Height

  <html>元素的尺寸（页面的尺寸）。测量的是CSS像素。

  > 整个html的实际尺寸，包括超出浏览器可视区域的部分。
  >
  > 如果html设置的尺寸小于可视区域，那么依然是html的宽高，html之外的不包括。

- 事件坐标

  鼠标事件，参数中包含事件相关的确切位置的信息。

  - pageX/Y	相对于 html 的 CSS 像素中元素的坐标。（IE不支持）
  - clientX/Y      以 CSS 像素给出相对于视口的坐标。
  - screenX/Y     以 设备像素为单位给出相对于屏幕的坐标。（IE和Opera用CSS像素）

- 媒体查询相关

  - `width/height`

    与document.documentElement.clientWidth/Height等价，CSS像素。

  - `device-width/device-height`

    与screen.width和screen.height等价，设备像素。



### 引用文献

- ppk大神的viewports <https://www.quirksmode.org/mobile/viewports.html>