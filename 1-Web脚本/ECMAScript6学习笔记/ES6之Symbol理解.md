## Symbol



### 来源

---

问：为什么会有Symbol，并且把它作为js的基本数据类型

答：为了解决字符串名称冲突问题，通过Symbol生成一个独一无二的值。

举例：

1、在ES5中，对象的属性名都是字符串，现在字符串也是大多数情况。如果，你用了他人提供的对象，但是又想给对象添加新的方法，那么就很有可能和现有方法产生冲突。

2、在全局，你想定义 常量 来标识某一个特性或者标志位。但是当你的工程很大的时候，命名上很可能会有冲突，但是你又不想混用。react源码项目中对于节点type的定义就是这么实现的。

```js
//React 源码
const hasSymbol = typeof Symbol === 'function' && Symbol.for;

export const REACT_ELEMENT_TYPE = hasSymbol
  ? Symbol.for('react.element')
  : 0xeac7;
```

> Symbol引入的原因即是 **为了防止冲突，保证独一无二**



### 定义

---

Symbol是基本数据类型，通过 Symbol 函数生成，独一无二，类似于字符串的数据类型。

> 其他数据类型回顾：undefined、null、Boolean，String、Number、Object

举例：

```js
let s = Symbol();
let s1 = Symbol('foo');
let s2 = Symbol('bar');
let s3 = Symbol('bar');

typeof s // "symbol"
s1 // Symbol(foo)
s2 // Symbol(bar)

s1.toString() // "Symbol(foo)"
s2.toString() // "Symbol(bar)"
s2 === s3 // false
```

需要注意

1、原始类型不能使用 new 命令，new Symbol()会报错。

2、接收字符串作为参数，如果参数是对象，也会自动obj.toString()。

3、参数相同，Symbol函数的返回值也是不等的。

4、Symbol值不能参与运算，可以显式转为字符串和布尔值，不能转数值。

5、参数同时作为返回值的description属性对应的值，作为该Symbol的描述。



### 用途

---

#### 作为属性名

Symbol 值独一无二的特性，很适合作为标识符，用于对象的属性名，一种非私有的内部属性方法。

举例：

```js
let mySymbol = Symbol();

// 第一种写法
let a = {};
a[mySymbol] = 'Hello!';

// 第二种写法
let a = {
  [mySymbol]: 'Hello!'
};

// 第三种写法
let a = {};
Object.defineProperty(a, mySymbol, { value: 'Hello!' });

// 以上写法都得到同样结果
a[mySymbol] // "Hello!"
```

需要注意

1、Symbol 值作为对象属性名时，不能用点运算符。

2、定义属性时，Symbol 值必须放在方括号中，否则会被当做字符串处理。

3、Symbol 作为属性名，遍历对象的时候，该属性不会出现在`for...in`、`for...of`循环中，`Object.keys(x)`、`Object.getOwnPropertyNames(x)`、`JSON.stringify()`都无法获取它。

4、`Object.getOwnPropertySymbols()`方法，可以获取指定对象的所有 Symbol 属性名。`Reflect.ownKeys()`方法可以返回所有类型的键名，包括常规键名和 Symbol 键名。



#### 定义常量

保证常量的值都是不相等的。

举例

```js
const shapeType = {
  triangle: Symbol()
};

function getArea(shape, options) {
  let area = 0;
  switch (shape) {
    case shapeType.triangle:
      area = .5 * options.width * options.height;
      break;
  }
  return area;
}

getArea(shapeType.triangle, { width: 100, height: 100 });
```

还有我上面说到的React源码里面也是同样的用法。

> 魔术字符串：在代码之中多次出现、与代码形成强耦合的某一个具体的字符串或者数值。
>
> 风格良好的代码，应该尽量消除魔术字符串，改由含义清晰的变量代替。
>
> **简单点说**，就比如上面的例子，case后面写的是"mode1"，这样的具体的字符串，那么就是强耦合，不利于将来的修改和维护。



#### 模块的单例模式

Singleton 模式指的是调用一个类，任何时候返回的都是同一个实例。

待完善



### 原生属性

共有11个，我没用过，先记录一下

- Symbol.hasInstance

  当其他对象使用`instanceof`运算符，判断是否为该对象的实例时，会调用这个方法。

- Symbol.isConcatSpreadable

  布尔值，表示该对象用于`Array.prototype.concat()`时，是否可以展开。默认undefined

- Symbol.species

  指向一个构造函数。创建衍生对象时，会使用该属性。

- Symbol.match

  指向一个函数。当执行`str.match(myObject)`时，如果该属性存在，会调用它，返回该方法的返回值。

- Symbol.replace

  指向一个方法，当该对象被`String.prototype.replace`方法调用时，会返回该方法的返回值。

- Symbol.search

  指向一个方法，当该对象被`String.prototype.search`方法调用时，会返回该方法的返回值。

- Symbol.split

  指向一个方法，当该对象被`String.prototype.split`方法调用时，会返回该方法的返回值。

- Symbol.iterator

  指向该对象的默认遍历器方法。

- Symbol.toPrimitive

  指向一个方法。该对象被转为原始类型的值时，会调用这个方法，返回该对象对应的原始类型值。

- Symbol.toStringTag

  指向一个方法。在该对象上面调用`Object.prototype.toString`方法时，如果这个属性存在，它的返回值会出现在`toString`方法返回的字符串之中，表示对象的类型。也就是说，这个属性可以用来定制`[object Object]`或`[object Array]`中`object`后面的那个字符串。

- Symbol.unscopables

  指向一个对象。该对象指定了使用`with`关键字时，哪些属性会被`with`环境排除。



### 原生方法

#### Symbol.for()

**用途**：重新使用同一个 Symbol 值。

**行为**：它接受一个字符串作为参数，然后搜索有没有以该参数作为名称的 Symbol 值。如果有，就返回这个 Symbol 值，否则就新建一个以该字符串为名称的 Symbol 值，并将其注册到全局。

**举例**：

```js
let s1 = Symbol.for('foo');
let s2 = Symbol.for('foo');

s1 === s2 // true

Symbol.for("bar") === Symbol.for("bar") // true
Symbol("bar") === Symbol("bar") // false
```

需要注意

1、`Symbol.for()`会被登记在全局环境中供搜索，`Symbol()`不会。

2、`Symbol()`没有登记机制，所以每次调用都会返回一个不同的值。

3、`Symbol.for()`的这个全局登记特性，可以用在不同的 iframe 或 service worker 中取到同一个值。

> 再说一遍，React源码也是用的这个



#### Symbol.keyFor()

用途：返回一个已登记的 Symbol 类型值的`key`。

举例：

```js
let s1 = Symbol.for("foo");
Symbol.keyFor(s1) // "foo"

let s2 = Symbol("foo");
Symbol.keyFor(s2) // undefined
```

需要注意

1、这个方法可以认为是`Symbol.for()`的辅助方法。