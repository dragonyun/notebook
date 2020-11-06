---
typora-root-url: img
---

## CSS 伪类和伪元素

### 目的

为什么会有伪类和伪元素？

是为了补充现有的功能，比如有些内容不能通过class，id选择元素；比如元素的不同状态；比如不存在于DOM树中的虚拟元素。

### 定义

- 伪类

  > The pseudo-class concept is introduced to permit selection based on information that lies outside of the document tree or that cannot be expressed using the other simple selectors.

  **解读：**伪类用于选择DOM树之外的信息，或是不能用简单选择器进行表示的信息。前者包含那些匹配指定状态的元素，比如`:visited`，`:active`；后者包含那些满足一定逻辑条件的DOM树中的元素，比如`:first-child`，`:first-of-type`，`：target`。

- 伪元素

  > Pseudo-elements create abstractions about the document tree beyond those specified by the document language.

  **解读：**伪元素为DOM树没有定义的虚拟元素。不同于其他选择器，它不以元素为最小选择单元，它选择的是元素指定内容。比如`::before`表示选择元素内容的之前内容，也就是`""`；`::selection`表示选择元素被选中的内容。

> **总结：**
>
> 伪类是基于现有元素的。
>
> 伪元素是基于虚拟元素的，什么是虚拟元素，比如一个p标签包含了几行文字，我想让第一个字高亮加粗，我想让第一行变色等等，他们只是元素的一部分，不以元素为最小单位。



### 语法

语法上，伪类是一个冒号，伪元素是两个冒号（CSS3规定的）。



### 一览表

#### 伪类

存在DOM文档中，逻辑上存在但在文档树中却无须标识的“幽灵”分类。

![css伪类](/css伪类.png)



#### 伪元素

不存在在DOM文档中，是虚拟的元素，是创建新元素。代表某个元素的子元素，这个子元素虽然在逻辑上存在，但却并不实际存在于文档树中。

![css伪元素](/css伪元素.png)



### 参考文档

- 总结伪类与伪元素： `http://www.alloyteam.com/2016/05/summary-of-pseudo-classes-and-pseudo-elements/`
- 伪元素和伪类的区别：`https://blog.csdn.net/qq_27674439/article/details/90608220`

- 