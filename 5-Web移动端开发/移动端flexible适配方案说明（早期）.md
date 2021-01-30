## 移动端flexible适配方案（早期）

该方案最早由前端大佬 大漠 提出，时间是2015年底，用于处理移动端不同手机的适配。主要是借助rem，em来实现的。（现已废弃）



### 大致思路

---

1、设置html的font-size为屏幕宽度（设备独立像素）的十分之一。

2、HTML的font-size 和 rem 相等。

3、页面上的css单位使用rem（文字除外）。

4、相当于是等比缩放的一个方案。



对于开发实现来说

1、假设设计图宽度是750px，即假设html的font-size为75px。

2、页面上其他css单位，通过px转换为rem。





具体文档看下面的参考文章。



### 参考文章

---

- 大漠的《使用Flexible实现手淘H5页面的终端适配》：`https://www.w3cplus.com/mobile/lib-flexible-for-html5-layout.html`
- 和第一个是同一篇文章，上面的需要会员才能看：`https://www.cnblogs.com/doseoer/p/5545546.html`