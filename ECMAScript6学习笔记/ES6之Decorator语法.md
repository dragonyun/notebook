## ES6之Decorator修饰器

> 装饰器（Decorator）是一种与类（class）相关的语法，用来注释或修改类和类方法。许多面向对象的语言都有这项功能。
>
> 严格来说，这个是加在ES7里面的，不过是很有用的语法。

### 举个例子

> 装饰器是一种函数，写成`@ + 函数名`。它可以放在类和类方法的定义前面。
>
> ```jsjavascript
> @frozen class Foo {
>   @configurable(false)
>   @enumerable(true)
>   method() {}
> 
>   @throttle(500)
>   expensiveMethod() {}
> }
> ```
>
> 上面代码一共使用了四个装饰器，一个用在类本身，另外三个用在类方法。它们不仅增加了代码的可读性，清晰地表达了意图，而且提供一种方便的手段，增加或修改类的功能。
>
> **这个东西就很牛了，你写了一个类，但是你想给这个类增量的加一些方法之类的，修饰器很好用，也很清晰，不同的功能放在不同的 函数 中，通过修饰器集中在一起，共同使用。**

### 类的修饰

> 装饰器可以用来装饰整个类。
>
> ```javascript
> @testable
> class MyTestableClass {
>   // ...
> }
> 
> function testable(target) {
>   target.isTestable = true;
> }
> 
> MyTestableClass.isTestable // true
> ```
>
> 上面代码中，`@testable`就是一个装饰器。它修改了`MyTestableClass`这个类的行为，为它加上了静态属性`isTestable`。`testable`函数的参数`target`是`MyTestableClass`类本身。
>
> ----
>
> 基本上，装饰器的行为就是下面这样。
>
> ```javascript
> @decorator
> class A {}
> 
> // 等同于
> 
> class A {}
> A = decorator(A) || A;
> ```
>
> 也就是说，装饰器是一个对类进行处理的函数。装饰器函数的第一个参数，就是所要装饰的目标类。
>
> ---
>
> 如果觉得一个参数不够用，可以在装饰器外面再封装一层函数。
>
> ```javascript
> function testable(isTestable) {
>   return function(target) {
>     target.isTestable = isTestable;
>   }
> }
> 
> @testable(true)
> class MyTestableClass {}
> MyTestableClass.isTestable // true
> 
> @testable(false)
> class MyClass {}
> MyClass.isTestable // false
> ```
>
> 上面代码中，装饰器`testable`可以接受参数，这就等于可以修改装饰器的行为。
>
> 注意，装饰器对类的行为的改变，是代码编译时发生的，而不是在运行时。这意味着，装饰器能在编译阶段运行代码。也就是说，装饰器本质就是编译时执行的函数。

---

> 前面的例子是为类添加一个静态属性，如果想添加实例属性，可以通过目标类的`prototype`对象操作。
>
> ```javascript
> function testable(target) {
>   target.prototype.isTestable = true;
> }
> 
> @testable
> class MyTestableClass {}
> 
> let obj = new MyTestableClass();
> obj.isTestable // true
> ```
>
> 上面代码中，装饰器函数`testable`是在目标类的`prototype`对象上添加属性，因此就可以在实例上调用。

---

> 类的修饰在react里面还挺好用的，最常见的是react和redux结合的时候用起来很直观并且好用。
>
> ```javascript
> // 不使用修饰器的写法
> class MyReactComponent extends React.Component {}
> 
> export default connect(mapStateToProps, mapDispatchToProps)(MyReactComponent);
> ```
>
> ```javascript
> // 使用修饰器的写法
> @connect(mapStateToProps, mapDispatchToProps)
> export default class MyReactComponent extends React.Component {}
> ```

### 待续。。。

