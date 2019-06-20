## JSX

### 简介

```js
const element = <h1>Hello, world!</h1>;
```

> 代码如上，这个语法既不是字符串也不是HTML
>
> **JSX是一个很像XML的JavaScript的语法扩展**

- JSX具有JavaScript的全部功能

### 基本用法

```js
const name = 'Josh Perez';
const element = <h1>Hello, {name}</h1>;

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

> 声明了一个名为name的变量，在JSX中使用，并用**大括号**包裹起来。
>
> 大括号是要用的，大括号里面可以放置任何有效的**JavaScript表达式**。
>
> 常见的比如：**2+2，user.firstName或者formatName(user)**，都可以用。

- 自动插入分号陷阱

```js
function formatName(user) {
  return user.firstName + ' ' + user.lastName;
}

const user = {
  firstName: 'Harper',
  lastName: 'Perez'
};

const element = (
  <h1>
    Hello, {formatName(user)}!
  </h1>
);

ReactDOM.render(
  element,
  document.getElementById('root')
);
```

> 有些时候我们会将JSX拆分成多行，最好外面加上**圆括号**，如果不加，那么遇到分号，就代表赋值操作结束了。

### JSX也是表达式

> 在编译之后，JSX 表达式会被转为普通 JavaScript 函数调用，并且对其取值后得到 JavaScript 对象。

```js
function getGreeting(user) {
  if (user) {
    return <h1>Hello, {formatName(user)}!</h1>;
  }
  return <h1>Hello, Stranger.</h1>;
}

```

> 也就是说，你可以在 `if` 语句和 `for` 循环的代码块中使用 JSX，将 JSX 赋值给变量，把 JSX 当作参数传入，以及从函数中返回 JSX：

### JSX特定属性

> 你可以通过使用引号，来将属性值指定为字符串字面量

```js
const element = <div tabIndex="0"></div>;
```

> 也可以使用大括号，来在属性值中插入一个 JavaScript 表达式：

```js
const element = <img src={user.avatarUrl}></img>;
```

> 用表达式的时候，大括号外面不要加引号。
>
> 只使用引号（对于字符串值）或大括号（对于表达式）。
>
> 不能同时使用。

### JSX子元素

> JSX 标签里能够包含很多子元素:

```js
const element = (
  <div>
    <h1>Hello!</h1>
    <h2>Good to see you here.</h2>
  </div>
);
```

> 就是说，JSX可以像写HTML一样，包含很多层，不过它更像JavaScript

### JSX防止注入攻击

> 你可以安全地在 JSX 当中插入用户输入内容：

```js
const title = response.potentiallyMaliciousInput;
// 直接使用是安全的：
const element = <h1>{title}</h1>;
```

> React DOM 在渲染所有输入内容之前，默认会进行[转义](https://stackoverflow.com/questions/7381974/which-characters-need-to-be-escaped-on-html)。它可以确保在你的应用中，永远不会注入那些并非自己明确编写的内容。所有的内容在渲染之前都被转换成了字符串。这样可以有效地防止 [XSS（cross-site-scripting, 跨站脚本）](https://en.wikipedia.org/wiki/Cross-site_scripting)攻击。

### JSX 表示对象

> Babel 会把 JSX 转译成一个名为 `React.createElement()` 函数调用。

```js
const element = (
  <h1 className="greeting">
    Hello, world!
  </h1>
);
```

```js
const element = React.createElement(
  'h1',
  {className: 'greeting'},
  'Hello, world!'
);
```

> `React.createElement()` 会预先执行一些检查，以帮助你编写无错代码，但实际上它创建了一个这样的对象：

```js
// 注意：这是简化过的结构
const element = {
  type: 'h1',
  props: {
    className: 'greeting',
    children: 'Hello, world!'
  }
};
```

> **这些对象被称为“React 元素”。**
>
> 它们描述了你希望在屏幕上看到的内容。React通过读取这些对象，然后使用它们来构建DOM以及保持随时更新。

> **这里提一下元素和组价的差别，元素是比组件更小的基本单元。**
>
> **如果说元素用一条语句可以完成，那么组件要用一个文件来构成。**
>
> **它们在不同的层次上，都是基本单元。**

