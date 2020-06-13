## JSX

> react对js的依赖很强，就像下面的语法一样，标签语言和js语言的结合；
>
> JSX是js的语法扩展，很基础，也很常用；

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

## 深入JSX

> 就是提到了JSX的更多用法，也没觉得有多深入。
>
> 不过，这里提到了JSX的本质是什么，有必要了解一下。

---

> 本质上：**JSX 仅仅只是 `React.createElement(component, props, ...children)` 函数的语法糖。**

> 如下 JSX 代码：
>
> ```js
> <MyButton color="blue" shadowSize={2}>
>   Click Me
> </MyButton>
> ```
>
> 会编译为：
>
> ```js
> React.createElement(
>   MyButton,
>   {color: 'blue', shadowSize: 2},
>   'Click Me'
> )
> ```
>
> ---
>
> 而没有子节点的自闭合标签代码：
>
> ```js
> <div className="sidebar" />
> ```
>
> 会编译为:
>
> ```js
> React.createElement(
>   'div',
>   {className: 'sidebar'},
>   null
> )
> ```
>
> 看到了吧，其实你理解的简单一点，JSX就是js和标签语言的联合使用，大胆用就可以，报错再解决；
>
> 本质上，JSX是React.createElement语法的语法糖，
>
> 包含三个参数：component（组件，标签）, props（属性，传值）, ...children（内容，子集）

### 指定 React 元素类型

> JSX 标签的第一部分指定了 React 元素的类型。
>
> 大写字母开头的 JSX 标签意味着它们是 React 组件。这些标签会被编译为对命名变量的直接引用，所以，当你使用 JSX `<Foo />` 表达式时，`Foo` 必须包含在作用域内。
>
> 就是说：
>
> 大写开头才被认为是React组件；
>
> 用之前要先引入，保证它是存在的。

- ### React 必须在作用域内

> 由于 JSX 会编译为 `React.createElement` 调用形式，所以 `React` 库也必须包含在 JSX 代码作用域内。
>
> 例如，在如下代码中，虽然 `React` 和 `CustomButton` 并没有被直接使用，但还是需要导入：
>
> ```js
> import React from 'react';
> import CustomButton from './CustomButton';
> 
> function WarningButton() {
>   // return React.createElement(CustomButton, {color: 'red'}, null);
>   return <CustomButton color="red" />;
> }
> ```
>
> 如果你不使用 JavaScript 打包工具而是直接通过 `<script>` 标签加载 React，则必须将 `React`挂载到全局变量中。

- ### 在 JSX 类型中使用点语法

> 在 JSX 中，你也可以使用点语法来引用一个 React 组件。当你在一个模块中导出许多 React 组件时，这会非常方便。例如，如果 `MyComponents.DatePicker` 是一个组件，你可以在 JSX 中直接使用：
>
> ```js
> import React from 'react';
> 
> const MyComponents = {
>   DatePicker: function DatePicker(props) {
>     return <div>Imagine a {props.color} datepicker here.</div>;
>   }
> }
> 
> function BlueDatePicker() {
>   return <MyComponents.DatePicker color="blue" />;
> }
> ```
>
> 这种挺少见的，我反正没见过，不过js还是那么灵活，也不奇怪。

- ### 用户定义的组件必须以大写字母开头

> 以小写字母开头的元素代表一个 HTML 内置组件，比如 `<div>` 或者 `<span>` 会生成相应的字符串 `'div'` 或者 `'span'` 传递给 `React.createElement`（作为参数）。大写字母开头的元素则对应着在 JavaScript 引入或自定义的组件，如 `<Foo />` 会编译为 `React.createElement(Foo)`。
>
> **大写小写导致了编译方式的不同，所以。。。**

> 建议使用大写字母开头命名自定义组件。如果你确实需要一个以小写字母开头的组件，则在 JSX 中使用它之前，必须将它赋值给一个大写字母开头的变量。

- ### 在运行时选择类型

> 你不能将通用表达式作为 React 元素类型。如果你想通过通用表达式来（动态）决定元素类型，你需要首先将它赋值给大写字母开头的变量。这通常用于根据 prop 来渲染不同组件的情况下:
>
> ```js
> import React from 'react';
> import { PhotoStory, VideoStory } from './stories';
> 
> const components = {
>   photo: PhotoStory,
>   video: VideoStory
> };
> 
> function Story(props) {
>   // 错误！JSX 类型不能是一个表达式。
>   return <components[props.storyType] story={props.story} />;
> }
> ```
>
> 要解决这个问题, 需要首先将类型赋值给一个大写字母开头的变量：
>
> ```js
> import React from 'react';
> import { PhotoStory, VideoStory } from './stories';
> 
> const components = {
>   photo: PhotoStory,
>   video: VideoStory
> };
> 
> function Story(props) {
>   // 正确！JSX 类型可以是大写字母开头的变量。
>   const SpecificStory = components[props.storyType];
>   return <SpecificStory story={props.story} />;
> }
> ```
>
> 就是告诉你，标签名你不能用表达式，先赋值给一个大写字母开头的变量，还是要大写字母。

### JSX 中的 Props

> props就是渲染的第二个参数，传给子组件的数据。

- ### JavaScript 表达式作为 Props

> 你可以把包裹在 `{}` 中的 JavaScript 表达式作为一个 prop 传递给 JSX 元素。例如，如下的 JSX：
>
> ```js
> <MyComponent foo={1 + 2 + 3 + 4} />
> ```
>
> 在 `MyComponent` 中，`props.foo` 的值等于 `1 + 2 + 3 + 4` 的执行结果 `10`。
>
> ---
>
> `if` 语句以及 `for` 循环不是 JavaScript 表达式，所以不能在 JSX 中直接使用。但是，你可以用在 JSX 以外的代码中。比如：
>
> ```js
> function NumberDescriber(props) {
>   let description;
>   if (props.number % 2 == 0) {
>     description = <strong>even</strong>;
>   } else {
>     description = <i>odd</i>;
>   }
>   return <div>{props.number} is an {description} number</div>;
> }
> ```
>
> 就是告诉你，大括号里面你不能写if和for，这种逻辑判断可以写到外面去。

- ### 字符串字面量

> 你可以将字符串字面量赋值给 prop.
>
> 如下两个 JSX 表达式是等价的：
>
> ```js
> <MyComponent message="hello world" />
> 
> <MyComponent message={'hello world'} />
> ```
>
> 当你将字符串字面量赋值给 prop 时，它的值是未转义的。所以，以下两个 JSX 表达式是等价的：
>
> ```js
> <MyComponent message="&lt;3" />
> 
> <MyComponent message={'<3'} />
> ```
>
> 就是直接放字符串也可以。

- ### Props 默认值为 “True”

> 如果你没给 prop 赋值，它的默认值是 `true`。以下两个 JSX 表达式是等价的：
>
> ```js
> <MyTextBox autocomplete />
> 
> <MyTextBox autocomplete={true} />
> ```
>
> 通常，不建议这样使用，因为它可能与 **ES6 对象简写**混淆，`{foo}` 是 `{foo: foo}` 的简写，而不是 `{foo: true}`。这样实现只是为了保持和 HTML 中标签属性的行为一致。
>
> 可以这么写，但是还是别这么写。

- ### 属性展开

> 如果你已经有了一个 props 对象，你可以使用展开运算符 `...` 来在 JSX 中传递整个 props 对象。以下两个组件是等价的：
>
> ```js
> function App1() {
>   return <Greeting firstName="Ben" lastName="Hector" />;
> }
> 
> function App2() {
>   const props = {firstName: 'Ben', lastName: 'Hector'};
>   return <Greeting {...props} />;
> }
> ```
>
> 你还可以选择只保留当前组件需要接收的 props，并使用展开运算符将其他 props 传递下去。
>
> ```js
> const Button = props => {
>   const { kind, ...other } = props;
>   const className = kind === "primary" ? "PrimaryButton" : "SecondaryButton";
>   return <button className={className} {...other} />;
> };
> 
> const App = () => {
>   return (
>     <div>
>       <Button kind="primary" onClick={() => console.log("clicked!")}>
>         Hello World!
>       </Button>
>     </div>
>   );
> };
> ```
>
> 这种，没什么好解释的，看你具体的需求是什么；
>
> 不过，这种写法还是很好用的，我经常见到，也经常用到，是一个很实用的写法。

### JSX 中的子元素

> 包含在开始和结束标签之间的 JSX 表达式内容将作为特定属性 `props.children` 传递给外层组件。有几种不同的方法来传递子元素：

- ### 字符串字面量

> 你可以将字符串放在开始和结束标签之间，此时 `props.children` 就只是该字符串。这对于很多内置的 HTML 元素很有用。例如：
>
> ```js
> <MyComponent>Hello world!</MyComponent>
> ```
>
> 这是一个合法的 JSX，`MyComponent` 中的 `props.children` 是一个简单的未转义字符串 `"Hello world!"`。因此你可以采用编写写 HTML 的方式来编写写 JSX。如下所示：
>
> ```js
> <div>This is valid HTML &amp; JSX at the same time.</div>
> ```
>
> JSX 会移除行首尾的空格以及空行。与标签相邻的空行均会被删除，文本字符串之间的新行会被压缩为一个空格。因此以下的几种方式都是等价的：
>
> ```js
> <div>Hello World</div>
> 
> <div>
>   Hello World
> </div>
> 
> <div>
>   Hello
>   World
> </div>
> 
> <div>
> 
>   Hello World
> </div>
> ```

- ### JSX 子元素

> 子元素允许由多个 JSX 元素组成。这对于嵌套组件非常有用：
>
> ```js
> <MyContainer>
>   <MyFirstComponent />
>   <MySecondComponent />
> </MyContainer>
> ```
>
> React 组件也能够返回存储在数组中的一组元素：
>
> ```js
> render() {
>   // 不需要用额外的元素包裹列表元素！
>   return [
>     // 不要忘记设置 key :)
>     <li key="A">First item</li>,
>     <li key="B">Second item</li>,
>     <li key="C">Third item</li>,
>   ];
> }
> ```
>
> 

- ### JavaScript 表达式作为子元素

> JavaScript 表达式可以被包裹在 `{}` 中作为子元素。例如，以下表达式是等价的：
>
> ```js
> <MyComponent>foo</MyComponent>
> 
> <MyComponent>{'foo'}</MyComponent>
> ```
>
> 这对于展示任意长度的列表非常有用。例如，渲染 HTML 列表：
>
> ```js
> function Item(props) {
>   return <li>{props.message}</li>;
> }
> 
> function TodoList() {
>   const todos = ['finish doc', 'submit pr', 'nag dan to review'];
>   return (
>     <ul>
>       {todos.map((message) => <Item key={message} message={message} />)}
>     </ul>
>   );
> }
> ```

- ### 函数作为子元素

> 通常，JSX 中的 JavaScript 表达式将会被计算为字符串、React 元素或者是列表。不过，`props.children` 和其他 prop 一样，它可以传递任意类型的数据，而不仅仅是 React 已知的可渲染类型。例如，如果你有一个自定义组件，你可以把回调函数作为 `props.children` 进行传递：
>
> ```js
> // 调用子元素回调 numTimes 次，来重复生成组件
> function Repeat(props) {
>   let items = [];
>   for (let i = 0; i < props.numTimes; i++) {
>     items.push(props.children(i));
>   }
>   return <div>{items}</div>;
> }
> 
> function ListOfTenThings() {
>   return (
>     <Repeat numTimes={10}>
>       {(index) => <div key={index}>This is item {index} in the list</div>}
>     </Repeat>
>   );
> }
> ```
>
> 你可以将任何东西作为子元素传递给自定义组件，只要确保在该组件渲染之前能够被转换成 React 理解的对象。这种用法并不常见，但可以用于扩展 JSX。

- ### 布尔类型、Null 以及 Undefined 将会忽略

> `false`, `null`, `undefined`, and `true` 是合法的子元素。但它们并不会被渲染。以下的 JSX 表达式渲染结果相同：
>
> ```js
> <div />
> 
> <div></div>
> 
> <div>{false}</div>
> 
> <div>{null}</div>
> 
> <div>{undefined}</div>
> 
> <div>{true}</div>
> ```
>
> 反之，如果你想渲染 `false`、`true`、`null`、`undefined` 等值，你需要先将它们转换为字符串：
>
> ```js
> <div>
>   My JavaScript variable is {String(myVariable)}.
> </div>
> ```
>
> 这个特性我一般会用在条件渲染中，如果条件不成立，那么就不渲染，不过我一般写的是空字符串。
>
> ```js
> // 类似这样，语法不要细究
> <div>
>   {
>     flag>0 ? <p>123</p> : ""
>   }
> </div>
> ```
>
> 

