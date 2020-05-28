## JavaScript 判断对象中是否存在某属性



### 对象和属性的关系

- 属性存在于原型链上
- 属性存在于对象自身上



### 1、点（.）或者方括号（[]）

​	通过点或者方括号可以获取对象的属性值，如果对象上不存在该属性，则会返回undefined。当然，这里的“不存在”指的是**对象自身和原型链上**都不存在，如果原型链有该属性，则会返回原型链上的属性值。

```js
// 创建对象
let test = {name : 'lei'}
// 获取对象的自身的属性
test.name   //"lei"
test["name"]   //"lei"
 
// 获取不存在的属性
test.age    //undefined
 
// 获取原型上的属性
test["toString"]  //toString() { [native code] }
 
// 新增一个值为undefined的属性
test.un = undefined
 
test.un    //undefined 不能用在属性值存在，但可能为 undefined的场景
```

所以，我们可以根据 `Obj.x !== undefined `的返回值 来判断Obj是否有x属性。

优点：

​	这种方式很简单方便

局限性：

​	不能用在x的属性值存在，但可能为 undefined的场景。方案二可以解决这个问题

​	同时查 **对象自身和原型链**，无法区分。方案三可以解决这个问题



### 2、in 运算符

如果指定的属性在指定的对象或其原型链中，则in 运算符返回true。

```js
'name' in test  //true
'un' in test    //true
'toString' in test //true
'age' in test   //false
```

示例中可以看出，值为undefined的属性也可正常判断。

优点：

​	可以解决方案一中的值为undefined的属性判断问题。

局限性：

​	同时查 **对象自身和原型链**，无法区分。方案三可以解决这个问题



### 3、hasOwnProperty()

Object的原型链上的方法

```js
test.hasOwnProperty('name')  //true 自身属性
test.hasOwnProperty('age')   //false 不存在
test.hasOwnProperty('toString') //false 原型链上属性
```

优点：

​	可以只查自身属性

局限：

​	不能查原型链