## js数据类型检测常用方法



### js数据类型介绍

有两种数据类型，分别是 基本数据类型 和 引用数据类型。

- 基本数据类型： Number，String，Undefined，Null，Boolean，Symbol（ES6新增，表示独一无二的值，不怎么用到）

- 引用数据类型：对象，数组，函数



### 1、typeof

typeof 返回一个表示数据类型的字符串，有两种写法

**typeof(表达式)、typeof 值，返回结果有7种。**

```js
typeof undefined; 		// undefined 表示这个值未定义
typeof true; 			// boolean 表示这个值是布尔值
typeof ''; 				// string 表示这个值是字符串
typeof 1; 				// number 表示这个值是数字
typeof null; 			// object 表示这个值是对象或者null
typeof [] ; 			// object 表示这个值是对象或者null
typeof new Date(); 		// object 表示这个值是对象或者null 
typeof new Function();  // function 表示这个值是函数
typeof Symbol(); 		// symbol 表示这个值是symbol
```

优点：简洁

局限：**数组，对象，null返回都是object，无法区分**



### 2、instanceof

instanceof 运算符用来测试一个对象在其原型链中是否存在一个构造函数的 prototype 属性。

```js
[] instanceof Array // true 
{} instanceof Object // true 
new Date() instanceof Date // true
new RegExp() instanceof RegExp // true 
Array.isArray([]); // true
```

优点：可以区分数组和对象，还有原型链情况

缺点：

1、对于基本数据类型来说，字面量方式创建出来的结果和实例方式创建出来的是有区别的

```js
console.log( 1 instanceof Number) //false 
console.log( new Number(1) instanceof Number) //true
```

其实就是字面量创建的 基本数据类型 用 typeof 可以检测到对应的类型，用 instanceof 需要创建对象才能用。

从严格意义上来讲，只有实例创建出来的结果才是标准的对象数据类型值，也是标准的 Number 这个类的一个实例；对于字面量方式创建出来的结果是基本的数据类型值，不是严谨的实例，但是由于 JS 的松散特点，导致了可以使用 Number.prototype 上提供的方法。

2、只要在当前实例的原型链上，我们用其检测出来的结果都是 true。在类的原型继承中，我们最后检测出来的结果未必准确。

```js
var arr = [1, 2, 3]; 
console.log(arr instanceof Array) // true 
console.log(arr instanceof Object); // true 
function fn() {} 
console.log(fn instanceof Function) // true 
console.log(fn instanceof Object) // true
```

3、对于特殊的数据类型 null 和 undefined，他们的所属类是 Null 和 Undefined，但是浏览器把这两个类保护起来了，不允许我们在外面访问使用。

```js
null instanceof Null // 报错
undefined instanceof undefined // 报错
```



总结

instanceof适用于原型，原型链这样的情况，其他情况下局限性很大



### 3、严格运算符 ===

只能用于判断 null 和 undefined，因为这两种类型的值都是唯一的。

```js
var a = null 
typeof a // "object" 
a === null // true 
// undefined 还可以用 typeof 来判断 
var b = undefined; 
typeof b === "undefined" // true 
b === undefined // true
```

局限性也很大



### 4、constructor（构造函数的构造器）

constructor 作用和 instanceof 非常相似。但 constructor 检测 Object 与 instanceof 不一样，还可以处理基本数据类型的检测。

```js
var aa=[ 1, 2]; 
console.log(aa.constructor=== Array); //true 
console.log(aa.constructor=== RegExp); //false console.log((1).constructor=== Number); //true 
var reg= /^$/; 
console.log(reg.constructor=== RegExp); //true console.log(reg.constructor=== Object); //false
```

缺陷：

1、null 和 undefined 是无效的对象，因此是不会有 constructor 存在的，这两种类型的数据需要通过其他方式来判断。

2、函数的 constructor 不稳定，主要是类的原型进行重写，在重写的过程中有可能把之前的constructor给覆盖了，这样检测出来的结果就是不准确的

```js
function Fn() {} 
Fn.prototype = new Array() 
var f = new Fn() 
console.log(f.constructor) // Array
```

总结

和instanceof半斤八两，原型链适用



### 5、Object.prototype.toString.call()

Object.prototype.toString.call() 是检测类型最准确的常用方法。

首先获取到 Object 原型上的 toString()方法，让方法执行， 让 toString 方法中的 this 指向第一个参数的值。

toString说明

- 本意是转换为字符串，但是某些 toString 不仅仅是转换成字符串
- 对于 Number，String，Boolean，Array，RegExp，Date，Function 原型上的 toString 方法都是把当前的数据类型转换为字符串的类型（它们的作用仅仅是用来转换为字符串的）
- Object 上的 toString 并不是用来转换成字符串的

Obect 上的 toString()方法，它的作用是返回当前方法执行的主体（方法中的 this ）所属类的详细信息即 “[object Object]”， 其中第一个 object 代表当前实例是对象数据类型的（这个是固定死的，不会改变），第二个 Object 代表的是 this 所属的类是 Object。

```js
Object.prototype.toString.call('') ; // [object String] 
Object.prototype.toString.call(1) ; // [object Number] 
Object.prototype.toString.call(true) ; // [object Boolean] Object.prototype.toString.call(undefined) ; // [object Undefined] Object.prototype.toString.call(null) ; // [object Null] 
Object.prototype.toString.call(new Function()) ; // [object Function] Object.prototype.toString.call(new Date()) ; // [object Date] 
Object.prototype.toString.call([]) ; // [object Array] 
Object.prototype.toString.call(new RegExp()) ; // [object RegExp]
Object.prototype.toString.call( newError()) ; // [object Error]
Object.prototype.toString.call( document) ; // [object HTMLDocument]
Object.prototype.toString.call( window) ; //[object global] window是全局对象global的引用
```

