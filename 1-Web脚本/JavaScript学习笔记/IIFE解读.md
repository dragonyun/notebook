## IIFE解读

参考<https://www.cnblogs.com/yiven/p/8462666.html>

说明：

谈到这个问题主要是因为看webpack打包的时候看到的，忽然茅塞顿开，想通了很多东西。

问题：

以前在浏览器中运行 JavaScript 有两种方法。第一种方式，引用一些脚本来存放每个功能；此解决方案很难扩展，因为加载太多脚本会导致网络瓶颈。第二种方式，使用一个包含所有项目代码的大型 `.js` 文件，但是这会导致作用域、文件大小、可读性和可维护性方面的问题。

结论：

IIFE是为了处理作用域的问题，而JavaScript模块也能处理作用域问题，并且更适合工程。



### **1. 定义**
IIFE: Immediately Invoked Function Expression，意为立即调用的函数表达式，也就是说，声明函数的同时立即调用这个函数。
对比一下，这是不采用IIFE时的函数声明和函数调用：

```js
function foo(){
  var a = 10;
  console.log(a);
}
foo();
```

下面是IIFE形式的函数调用：

```js
(function foo(){
  var a = 10;
  console.log(a);
})();
```

函数的声明和IIFE的区别在于，在函数的声明中，我们首先看到的是function关键字，而IIFE我们首先看到的是左边的（。也就是说，使用一对（）将函数的声明括起来，使得JS编译器不再认为这是一个函数声明，而是一个IIFE，即需要立刻执行声明的函数。
两者达到的目的是相同的，都是声明了一个函数foo并且随后调用函数foo。



### **2. 为什么需要IIFE？**

​	如果只是为了立即执行一个函数，显然IIFE所带来的好处有限。实际上，IIFE的出现是为了弥补JS在scope方面的缺陷：JS只有全局作用域（global scope）、函数作用域（function scope），从ES6开始才有块级作用域（block scope）。对比现在流行的其他面向对象的语言可以看出，JS在访问控制这方面是多么的脆弱！那么如何实现作用域的隔离呢？在JS中，只有function，只有function，只有function才能实现作用域隔离，因此如果要将一段代码中的变量、函数等的定义隔离出来，只能将这段代码封装到一个函数中。
​	在我们通常的理解中，将代码封装到函数中的目的是为了复用。在JS中，当然声明函数的目的在大多数情况下也是为了复用，但是JS迫于作用域控制手段的贫乏，我们也经常看到只使用一次的函数：这通常的目的是为了隔离作用域了！既然只使用一次，那么立即执行好了！既然只使用一次，函数的名字也省掉了！这就是IIFE的由来。



### **3. IIFE的常见形式**

```js
// 第一种写法
(function foo(){
  var a = 10;
  console.log(a);
})();
// 第二种写法
(function foo(){
  var a = 10;
  console.log(a);
}());
```

两种都对，第一种更通用，更常见



### 4.**IIFE构造单例模式**

JS的模块就是函数，最常见的模块定义如下：

```js
function myModule(){
  var someThing = "123";
  var otherThing = [1,2,3];

  function doSomeThing(){
    console.log(someThing);
  }

  function doOtherThing(){
    console.log(otherThing);
  }

  return {
    doSomeThing:doSomeThing,
    doOtherThing:doOtherThing
  }
}

var foo = myModule();
foo.doSomeThing();
foo.doOtherThing();

var foo1 = myModule();
foo1.doSomeThing();
```

如果需要一个单例模式的模块，那么可以利用IIFE：

```js
var myModule = (function module(){
  var someThing = "123";
  var otherThing = [1,2,3];

  function doSomeThing(){
    console.log(someThing);
  }

  function doOtherThing(){
    console.log(otherThing);
  }

  return {
    doSomeThing:doSomeThing,
    doOtherThing:doOtherThing
  }
})();

myModule.doSomeThing();
myModule.doOtherThing();
```



### **5. 小结**
IIFE的目的是为了隔离作用域，防止污染全局命名空间。

---

以前的做法很多都是这么引入脚本文件的，比较经典的是jQuery，引入之后也是$作为全局变量。

**所以现在的玩法都是用模块，你需要什么就require什么，在你的文件里面，变量冲突？不存在的。**

