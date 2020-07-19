## CSS 盒子模型

所有HTML元素可以看作盒子，在CSS中，"box model"这一术语是用来设计和布局时使用。

CSS盒模型本质上是一个盒子，封装周围的HTML元素，它包括：边距，边框，填充，和实际内容。

盒模型允许我们在其它元素和周围元素边框之间的空间放置元素。

> 就是Content=》Padding=》Border=》Margin
>
> HTML元素的内容，然后是内边距，然后是边框，最后是外边距

- **Margin(外边距)** - 清除边框外的区域，外边距是透明的。
- **Border(边框)** - 围绕在内边距和内容外的边框。
- **Padding(内边距)** - 清除内容周围的区域，内边距是透明的。
- **Content(内容)** - 盒子的内容，显示文本和图像。



### 标签属性

- box-sizing

  两个选项：content-box和border-box，默认是content-box，即W3C盒模型。

> 这个属性很重要，用来区分W3C盒模型和ie盒模型。
>
> 其他属性不再记录，宽高，边距，边框之类的，有很多其他的属性。



### 标准盒子模型（W3C）

W3C盒模型，为默认模型，标签属性`box-sizing:content-box`

标签实际宽度 = 设置的宽度width + border宽度 + padding宽度

> 标签实际宽度指的是 外边框内部的部分的宽高。根据模型不同计算方式不同。



举个例子

```css
.box {
  width: 200px;
  height: 200px;
  border: 20px solid black;
  padding: 50px;
  margin: 50px;
}
```

那么，content的宽为200px，高为200px；内边距50px；边框20px；外边距50px；



### 怪异盒模型（IE盒模型）

IE盒模型，标签属性`box-sizing：border-box`

标签实际宽度 = 设置的宽度width

如果设置了padding和border，那就是从设置的实际宽高中减去，减去后才是内容的宽高。



举个例子

```css
.box {
  width: 200px;
  height: 200px;
  border: 20px solid black;
  padding: 50px;
  margin: 50px;
}
```

那么，content的宽为60px，高为60px；内边距50px；边框20px；外边距50px；

这里的 60px = 200px - 50px - 50px - 20px - 20px；即width减去padding和border。



### 区别与总结

主要区别：**对于宽高的定义不同**
**w3c盒模型：设置的宽度width就等于内容的宽度**
**怪异盒模型：内容的宽度 = 设置的宽度width - border的宽度 - padding的宽度**



总结：

> W3C规范会一统天下，其实在Edge里面已经完全使用W3C的盒子模型。
>
> 但是目前还是有很多需要兼容IE的程序，然后他们丧心病狂地在Chrome里面强制把box-sizing设置成IE盒模型，就是为了通用逻辑，我也很无语，但是应该会持续一段时间吧。
>
> 