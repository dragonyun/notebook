## CSS哪些样式属性可以继承



### 来源

CSS样式是可以上下级影响的，但是实际情况又会很复杂。

继承是其中的一种，就是上一层写了，下面的子树就没有必要再写了。

`visibility和cursor`就是一个很好的例子，父节点隐藏了，子节点默认一定会隐藏。



### 列表

#### 不可继承的：

`display、margin、border、padding、background、height、min-height、max- height、width、min-width、max-width、overflow、position、left、right、top、 bottom、z-index、float、clear、table-layout、vertical-align、page-break-after、 page-bread-before和unicode-bidi`

#### 所有元素可继承：

`visibility和cursor`

#### 内联元素可继承：

`letter-spacing、word-spacing、white-space、line-height、color、font、 font-family、font-size、font-style、font-variant、font-weight、text- decoration、text-transform、direction`

#### 块状元素可继承：

`text-indent和text-align`

#### 列表元素可继承：

`list-style、list-style-type、list-style-position、list-style-image`

#### 表格元素可继承：

`border-collapse`