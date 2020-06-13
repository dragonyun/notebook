## react学习笔记之元素组件

### 元素

#### 元素是什么

> 元素是构成React应用的最小砖块。
>
> 元素描述了你在屏幕上想看到的内容。

```js
const element = <h1>Hello, world</h1>;
```

> 与浏览器的 DOM 元素不同，React 元素是创建开销极小的普通对象。
>
> React DOM 会负责更新 DOM 来与 React 元素保持一致。

- 注意：**组件是由元素构成的。**

#### 元素渲染为DOM

> 假设你的 HTML 文件某处有一个 `<div>`：
>
> ```js
> <div id="root"></div>
> ```
>
> 我们将其称为“根” DOM 节点，因为该节点内的所有内容都将由 React DOM 管理。

- ReactDOM.render

> 想要将一个 React 元素渲染到根 DOM 节点中，只需把它们一起传入 `ReactDOM.render()`：
>
> ```js
> const element = <h1>Hello, world</h1>;
> ReactDOM.render(element, document.getElementById('root'));
> ```
>
> 这里只是举个例子，其实通用一点的做法是使用**ReactDOM.render**将某个**元素**渲染进某个**DOM**中。
>
> ```js
> const element = <h1>Hello, world</h1>;
> ReactDOM.render(element, mountNode);
> ```

#### 元素更新

- React元素是**不可变对象**

> 一旦被创建，你就无法更改它的子元素或者属性。一个元素就像电影的单帧：它代表了某个特定时刻的 UI。

> 根据我们已有的知识，更新 UI 唯一的方式是创建一个全新的元素，并将其传入 `ReactDOM.render()`。

```js
// 例子
function tick() {
  const element = (
    <div>
      <h1>Hello, world!</h1>
      <h2>It is {new Date().toLocaleTimeString()}.</h2>
    </div>
  );
  ReactDOM.render(element, document.getElementById('root'));
}

setInterval(tick, 1000);
```

#### React元素更新原则

- React DOM 会将元素和它的子元素与它们之前的状态进行比较，并只会进行必要的更新来使 DOM 达到预期的状态。

> 以上面的计时器更新时间的例子来说明：
>
> 尽管每一秒我们都会新建一个描述整个 UI 树的元素，React DOM 只会更新实际改变了的内容，也就是例子中的文本节点。

### 组件

#### 函数组件与class组件

> react定义组件有两种方式：**函数组件与class组件**

```js
// 定义组件最简单的方式就是编写JavaScript函数
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}
```

> 该函数是一个有效的 React 组件，因为它**接收唯一带有数据的 “props”（代表属性）对象**与并**返回一个 React 元素**。这类组件被称为“函数组件”，因为它本质上就是 JavaScript 函数。

```js
// 使用 ES6 的 class 来定义组件
class Welcome extends React.Component {
  render() {
    return <h1>Hello, {this.props.name}</h1>;
  }
}
```

> 通过类定义一个React组件，需要**继承React**并且**render返回一个React元素**

> 在 `React.Component` 的子类中有个**必须定义**的 `render()`函数。其他方法均为可选。

#### 渲染组件

> 组件不仅仅是DOM标签，还可以是用户自定义的组件

```js
// DOM标签
const element = <div />;
// 自定义
const element = <Welcome name="Sara" />;
```

> 当 React 元素为用户自定义组件时，它会将 JSX 所接收的属性（attributes）转换为单个对象传递给组件，这个对象被称之为 “props”。

```js
function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

> 这个应该算是props的定义，**即在使用自定义组件时，组件的属性就是传递给组件内部的props**

> 让我们来回顾一下这个例子中发生了什么：
>
> 1. 我们调用 `ReactDOM.render()` 函数，并传入 `<Welcome name="Sara" />` 作为参数。
> 2. React 调用 `Welcome` 组件，并将 `{name: 'Sara'}` 作为 props 传入。
> 3. `Welcome` 组件将 `<h1>Hello, Sara</h1>` 元素作为返回值。
> 4. React DOM 将 DOM 高效地更新为 `<h1>Hello, Sara</h1>`。

- **注意**：自定义组件必须以大写字母开头。

> React 会将以小写字母开头的组件视为原生 DOM 标签。例如，`<div />` 代表 HTML 的 div 标签，而 `<Welcome />` 则代表一个组件，并且需在作用域内使用 `Welcome`。