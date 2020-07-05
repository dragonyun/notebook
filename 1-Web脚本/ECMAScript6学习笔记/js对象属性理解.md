## js对象属性理解

**ECMA-262中，对象的定义是：“无序属性的集合，其属性可以包含基本值、对象或者函数”。**

​	严格来讲，这就相当于说对象是一组没有特定顺序的值。对象的每个属性或方法都有一个名字，而每个名字都映射到一个值。正因为这样（以及其他将要讨论的原因），我们可以把 ECMAScript 的对象想象成散列表：无非就是一组名值对，其中值可以是数据或函数

​	每个对象都是基于一个引用类型创建的，这个引用类型可以是原生类型，也可以是开发人员定义的类型。



### 理解对象

---

创建自定义对象的最简单方式就是创建一个 Object 的实例，然后再为它添加属性和方法，如下所示。

```js
var person = new Object();
person.name = "Nicholas";
person.age = 29;
person.job = "Software Engineer";
person.sayName = function(){
alert(this.name);
};
```

后来，对象字面量成为创建这种对象的首选模式，如下所示。

```js
var person = {
name: "Nicholas",
age: 29,
job: "Software Engineer",
sayName: function(){
alert(this.name);
}
};
```

> 没什么好说的，这两种JavaScript都支持，后面一种见的比较多。



### 对象属性类型

---

​	ECMA-262 第 5 版在定义只有内部才用的特性（attribute）时，描述了属性（property）的各种特征。ECMA-262 定义这些特性是为了实现 JavaScript 引擎用的，因此在 JavaScript 中不能直接访问它们。为了表示特性是内部值，该规范把它们放在了两对儿方括号中，例如 [[Enumerable]] 。

> 这些属性的特征是一定存在的，只不过在js里面不能直接访问到。
>
> 这些特征对我们平时编程来说，毫无感觉，但是对于js执行来说，很有用。
>
> 深层次的编码也是很有用的，比如vue的基本实现原理，依赖包打包生成的文件。

ECMAScript 中有两种属性：**数据属性和访问器属性**。



**1、数据属性**

数据属性包含一个数据值的位置。在这个位置可以读取和写入值。数据属性有 4 个描述其行为的
特性。

- [[Configurable]] ：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特
  性，或者能否把属性修改为访问器属性。像前面例子中那样直接在对象上定义的属性，它们的
  这个特性默认值为 true 。
- [[Enumerable]] ：表示能否通过 for-in 循环返回属性。像前面例子中那样直接在对象上定
  义的属性，它们的这个特性默认值为 true 。
- [[Writable]] ：表示能否修改属性的值。像前面例子中那样直接在对象上定义的属性，它们的
  这个特性默认值为 true 。
-  [[Value]] ：包含这个属性的数据值。读取属性值的时候，从这个位置读；写入属性值的时候，
  把新值保存在这个位置。这个特性的默认值为 undefined 。

> 像平时我们的写法
>
> var person = {
> 	name: "Nicholas"
> };
>
> 那么数据属性将会是默认值：[[Configurable]] 、 [[Enumerable]]和 [[Writable]] 特性都被设置为 true ，而 [[Value]] 特性被设置为"Nicholas"。
>
> 对于name的修改都会反映到 [[Value]] 特性上。

​	

​	要修改属性默认的特性，必须使用 ECMAScript 5 的 Object.defineProperty() 方法。这个方法接收三个参数：属性所在的对象、属性的名字和一个描述符对象。其中，描述符（descriptor）对象的属性必须是： configurable 、 enumerable 、 writable 和 value 。设置其中的一或多个值，可以修改对应的特性值。

```js
// 创建了name属性，只读
var person = {};
Object.defineProperty(person, "name", {
	writable: false,
	value: "Nicholas"
});
alert(person.name); //"Nicholas"
person.name = "Greg";
alert(person.name); //"Nicholas"
```

​	这个name属性的值是不可修改的，如果尝试为它指定新值，则在非严格模式下，赋值操作将被忽略；在严格模式下，赋值操作将会导致抛出错误。

```js
// 创建了name属性，不可配置
var person = {};
Object.defineProperty(person, "name", {
	configurable: false,
	value: "Nicholas"
});
alert(person.name); //"Nicholas"
delete person.name;
alert(person.name); //"Nicholas"
```

> 把 configurable 设置为 false，影响很多
>
> 1、不能从对象中删除属性
>
> 2、一旦把属性定义为不可配置的，就不能再把它变回可配置了
>
> 3、一旦把属性定义为不可配置的，只能修改 writable 特性，其它修改都会报错

> 在调用 Object.defineProperty() 方法时，如果不指定， configurable 、 enumerable 和writable 特性的默认值都是 false 。
>
> 当然，平时也不会用到这些高级功能。



**2、访问器属性**

​	访问器属性不包含数据值；它们包含一对儿 getter 和 setter 函数（不过，这两个函数都不是必需的）。在读取访问器属性时，会调用 getter 函数，这个函数负责返回有效的值；在写入访问器属性时，会调用setter 函数并传入新值，这个函数负责决定如何处理数据。访问器属性有如下 4 个特性。

- [[Configurable]] ：表示能否通过 delete 删除属性从而重新定义属性，能否修改属性的特
  性，或者能否把属性修改为数据属性。对于直接在对象上定义的属性，这个特性的默认值为
  true 。
- [[Enumerable]] ：表示能否通过 for-in 循环返回属性。对于直接在对象上定义的属性，这
  个特性的默认值为 true 。
- [[Get]] ：在读取属性时调用的函数。默认值为 undefined 。
- [[Set]] ：在写入属性时调用的函数。默认值为 undefined 。

访问器属性不能直接定义，必须使用 Object.defineProperty() 来定义。

```js
// 访问器属性 year 则包含一个getter 函数和一个 setter 函数。getter 函数返回 _year 的值，setter 函数通过计算来确定正确的版本。
// 因此，把 year 属性修改为 2005 会导致 _year 变成 2005，而 edition 变为 2。
var book = {
	_year: 2004,
	edition: 1
};
Object.defineProperty(book, "year", {
	get: function(){
		return this._year;
	},
	set: function(newValue){
		if (newValue > 2004) {
			this._year = newValue;
			this.edition += newValue - 2004;
		}
	}
});
book.year = 2005;
alert(book.edition); //2
```

这是使用访问器属性的常见方式，即设置一个属性的值会导致其他属性发生变化。

> 不一定非要同时指定 getter 和 setter。
>
> 只指定 getter 意味着属性是不能写，尝试写入属性会被忽略。在严格模式下，尝试写入只指定了 getter 函数的属性会抛出错误。
>
> 类似地
>
> 只指定 setter 函数的属性也不能读，否则在非严格模式下会返回 undefined ，而在严格模式下会抛出错误。

> 在Object.defineProperty这个方法之前，要创建访问器属性，用的是非标准方法
>
> `__defineGetter__()` 和 `__defineSetter__()`，看下面例子就明白了
>
> 知道就好了，不推荐用

```js
var book = {
	_year: 2004,
	edition: 1
};
// 定义访问器的旧有方法
book.__defineGetter__("year", function(){
	return this._year;
});
book.__defineSetter__("year", function(newValue){
	if (newValue > 2004) {
		this._year = newValue;
		this.edition += newValue - 2004;
	}
});
book.year = 2005;
alert(book.edition); //2
```



### 定义多个属性

---

ECMAScript 5 又定义了一个 Object.definePro-perties() 方法。

​	利用这个方法可以通过描述符一次定义多个属性。

​	这个方法接收两个对象参数：第一个对象是要添加和修改其属性的对象，第二个对象的属性与第一个对象中要添加或修改的属性一一对应。例如：

```js
// 在 book 对象上定义了两个数据属性（ _year 和 edition ）和一个访问器属性（ year ）。
// 和单独创建相比，唯一的区别是这里的属性都是在同一时间创建的。
var book = {};
Object.defineProperties(book, {
	_year: {
		value: 2004
	},
	edition: {
		value: 1
	},
	year: {
		get: function(){
		    return this._year;
		},
		set: function(newValue){
			if (newValue > 2004) {
				this._year = newValue;
				this.edition += newValue - 2004;
			}
		}
	}
});
```



### 读取属性的特性

---

ECMAScript 5 的 Object.getOwnPropertyDescriptor() 方法。

​	使用这个方法可以取得给定属性的描述符。

​	这个方法接收两个参数：属性所在的对象和要读取其描述符的属性名称。返回值是一个对象，如果是访问器属性，这个对象的属性有 configurable 、 enumerable 、 get 和 set ；如果是数据属性，这个对象的属性有 configurable 、 enumerable 、 writable 和 value 。例如：

```js
var book = {};
Object.defineProperties(book, {
	_year: {
		value: 2004
	},
	edition: {
		value: 1
	},
	year: {
		get: function(){
			return this._year;
		},
		set: function(newValue){
			if (newValue > 2004) {
				this._year = newValue;
				this.edition += newValue - 2004;
			}
		}
	}
});
var descriptor = Object.getOwnPropertyDescriptor(book, "_year");
alert(descriptor.value); //2004
alert(descriptor.configurable); //false
alert(typeof descriptor.get); //"undefined"
var descriptor = Object.getOwnPropertyDescriptor(book, "year");
alert(descriptor.value); //undefined
alert(descriptor.enumerable); //false
alert(typeof descriptor.get); //"function"
```

对于数据属性 _year ， value 等于最初的值， configurable 是 false ，而 get 等于 undefined 。
对于访问器属性 year ， value 等于 undefined ， enumerable 是 false ，而 get 是一个指向 getter函数的指针。

> 在 JavaScript 中，可以针对任何对象——包括 DOM 和 BOM 对象，使用 Object.getOwnProperty-
> Descriptor() 方法。
>
> 就是说，你可以在window上用。