## react学习笔记之事件处理

> 事件就是事件，鼠标，键盘，焦点都是，用在标签里，触发对应脚本程序。

### 基本说明

> React 元素的事件处理和 DOM 元素的很相似，但是有一点语法上的不同:
>
> - React 事件的命名采用小驼峰式（camelCase），而不是纯小写。
> - 使用 JSX 语法时你需要传入一个**函数**作为事件处理函数，而不是一个字符串。
>
> ```js
> // 传统的HTML
> <button onclick="activateLasers()">
>   Activate Lasers
> </button>
> 
> // React中
> <button onclick="activateLasers">
>   Activate Lasers
> </button>
> // 这里传入的是函数，而不是上面的字符串
> ```
>
> - 在 React 中另一个不同点是你不能通过返回 `false` 的方式阻止默认行为。
>
> 你必须显式的使用 `preventDefault` 。例如，传统的 HTML 中阻止链接默认打开一个新页面，你可以这样写：
>
> ```js
> // 传统的HTML
> <a href="#" onclick="console.log('The link was clicked.'); return false">
>   Click me
> </a>
> 
> // React中
> function ActionLink() {
>   function handleClick(e) {
>     e.preventDefault();
>     console.log('The link was clicked.');
>   }
> 
>   return (
>     <a href="#" onClick={handleClick}>
>       Click me
>     </a>
>   );
> }
> ```
>
> 这里的事件对应的函数里面的参数e，e是一个合成事件。
>
> 合成事件看另外一个总结，简单说就是里面包含了该触发事件相关的属性方法以及所有事件公用的属性方法。

### 事件监听

> 使用 React 时，你一般不需要使用 `addEventListener` 为已创建的 DOM 元素添加监听器。React恰恰与之相反，你仅需要在该元素初始渲染的时候添加一个监听器。

> 当你使用 [ES6 class](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Classes) 语法定义一个组件的时候，通常的做法是将事件处理函数声明为 class 中的方法。例如，下面的 `Toggle` 组件会渲染一个让用户切换开关状态的按钮：

```js
class Toggle extends React.Component {
  constructor(props) {
    super(props);
    this.state = {isToggleOn: true};

    // 为了在回调中使用 `this`，这个绑定是必不可少的
    this.handleClick = this.handleClick.bind(this);
  }

  handleClick() {
    this.setState(state => ({
      isToggleOn: !state.isToggleOn
    }));
  }

  render() {
    return (
      <button onClick={this.handleClick}>
        {this.state.isToggleOn ? 'ON' : 'OFF'}
      </button>
    );
  }
}

ReactDOM.render(
  <Toggle />,
  document.getElementById('root')
);
```

> 也就是说，我们组件里面要用到事件处理函数，需要先将该函数声明为class中的方法，
>
> 就是构建函数里面绑定了this，
>
> 不绑的话，事件处理函数里面的this指向为undefined.

> 你必须谨慎对待 JSX 回调函数中的 `this`，在 JavaScript 中，class 的方法默认不会[绑定](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Global_objects/Function/bind) `this`。如果你忘记绑定 `this.handleClick` 并把它传入了 `onClick`，当你调用这个函数的时候 `this`的值为 `undefined`。

> 这并不是 React 特有的行为；这其实与 [JavaScript 函数工作原理](https://www.smashingmagazine.com/2014/01/understanding-javascript-function-prototype-bind/)有关。通常情况下，如果你没有在方法后面添加 `()`，例如 `onClick={this.handleClick}`，你应该为这个方法绑定 `this`。

- 如果你不想用bind，有两个方法解决

> - 使用实验性的 [public class fields 语法](https://babeljs.io/docs/plugins/transform-class-properties/)，你可以使用 class fields 正确的绑定回调函数：
>
> ```js
> class LoggingButton extends React.Component {
>   // 此语法确保 `handleClick` 内的 `this` 已被绑定。
>   // 注意: 这是 *实验性* 语法。
>   handleClick = () => {
>     console.log('this is:', this);
>   }
> 
>   render() {
>     return (
>       <button onClick={this.handleClick}>
>         Click me
>       </button>
>     );
>   }
> }
> // Create React App 默认启用此语法。
> ```
>
> - 如果你没有使用 class fields 语法，你可以在回调中使用[箭头函数](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Functions/Arrow_functions)：
>
> ```js
> class LoggingButton extends React.Component {
>   handleClick() {
>     console.log('this is:', this);
>   }
> 
>   render() {
>     // 此语法确保 `handleClick` 内的 `this` 已被绑定。
>     return (
>       <button onClick={(e) => this.handleClick(e)}>
>         Click me
>       </button>
>     );
>   }
> }
> // 就是把事件处理函数写成箭头函数，
> ```
>
> 此语法问题在于每次渲染 `LoggingButton` 时都会创建不同的回调函数。在大多数情况下，这没什么问题，但如果该回调函数作为 prop 传入子组件时，这些组件可能会进行额外的重新渲染。我们通常建议在构造器中绑定或使用 class fields 语法来避免这类性能问题。

### 向事件处理程序传递参数

> 在循环中，通常我们会为事件处理函数传递额外的参数。例如，若 `id` 是你要删除那一行的 ID，以下两种方式都可以向事件处理函数传递参数：
>
> ```js
> <button onClick={(e) => this.deleteRow(id, e)}>Delete Row</button>
> <button onClick={this.deleteRow.bind(this, id)}>Delete Row</button>
> ```
>
> 上述两种方式是等价的，分别通过[箭头函数](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Functions/Arrow_functions)和 [`Function.prototype.bind`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_objects/Function/bind) 来实现。
>
> 在这两种情况下，React 的事件对象 `e` 会被作为第二个参数传递。如果通过箭头函数的方式，事件对象必须显式的进行传递，而通过 `bind` 的方式，事件对象以及更多的参数将会被隐式的进行传递。

### 我的理解

> 事件处理就是在react组件元素中写脚本函数，无论是点击，选择，还是焦点事件，绑定在元素上，写出来该事件对应的处理函数。
>
> 
>
> 关于事件处理函数的编写，官方推荐的方法是在构造里面先给函数绑定this，再调用，否则你在调用的时候this是undefined的。这样很麻烦。
>
> 
>
> 在现在的编码过程中，我们大多采用了第一种“实验性”做法，不知道现在还是不是“实验性”的；
>
> 不需要在构造函数里面去绑定this，直接写函数，不过要箭头函数，并且赋值给变量，就可以直接用。
>
> 
>
> 给的第二种方法我没用过，不过还是写箭头函数，感觉有点绕。