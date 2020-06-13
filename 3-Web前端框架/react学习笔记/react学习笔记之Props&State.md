## 核心概念之Props&State

> 在数据这一块，这两个可以说是最重要的概念了；
>
> Props是父组件传给子组件用的；
>
> State是组件本身的；
>
> 我们知道React的数据流向是自上而下的；
>
> 主要需要思考的是哪些是要传下去的，哪些是自己要维护的，这个很重要！！！

### Props

#### 基本用法

> 先举个例子

```js
const element = <Welcome name="Sara" />;

function Welcome(props) {
  return <h1>Hello, {props.name}</h1>;
}

const element = <Welcome name="Sara" />;
ReactDOM.render(
  element,
  document.getElementById('root')
);
```

> 当 React 元素为用户自定义组件时，它会将 JSX 所接收的属性（attributes）转换为单个对象传递给组件，这个对象被称之为 “props”。

> 我的理解是props是父子组件之间通过属性（attributes）传值用的，下一级组件可以通过props直接读值。

#### 只读性

> 组件无论是使用**函数声明**还是通过**class 声明**，都**决不能修改自身的 props**。

> 引入一个概念：**纯函数**
>
> 定义：函数不会尝试更改传入的参数，且多次调用下相同的参数始终返回相同的结果。

- **所有 React 组件都必须像纯函数一样保护它们的 props 不被更改。**

#### 类型检查及默认值

> `defaultProps` 用于确保 `this.props.xxx` 在父组件没有指定其值时，有一个默认值。
>
> `propTypes` 类型检查发生在 `defaultProps` 赋值后，所以类型检查也适用于 `defaultProps`。

> **PropTypes**
>
> `PropTypes` 提供一系列验证器，可用于确保组件接收到的数据类型是有效的。
>
> ```js
> import PropTypes from 'prop-types';
> 
> class Greeting extends React.Component {
>   render() {
>     return (
>       <h1>Hello, {this.props.name}</h1>
>     );
>   }
> }
> 
> Greeting.propTypes = {
>   name: PropTypes.string
> };
> ```
>
> 在本例中, 我们使用了 `PropTypes.string`。当传入的 `prop` 值类型不正确时，JavaScript 控制台将会显示警告。出于性能方面的考虑，`propTypes` 仅在开发模式下进行检查。
>
> 不过，我在第三方组件antd里面很常见这个东西，至于要不要只在开发模式下用，存疑。
>
> 可以检测的类型很全面，不只是基本的类型，还有元素，枚举等，可参考官网。

> PropTypes.element
>
> 可以通过 `PropTypes.element` 来确保传递给组件的 children 中只包含一个元素。
>
> ```js
> import PropTypes from 'prop-types';
> 
> class MyComponent extends React.Component {
>   render() {
>     // 这必须只有一个元素，否则控制台会打印警告。
>     const children = this.props.children;
>     return (
>       <div>
>         {children}
>       </div>
>     );
>   }
> }
> 
> MyComponent.propTypes = {
>   children: PropTypes.element.isRequired
> };
> ```

> **defaultProps**
>
> 可以通过配置特定的 `defaultProps` 属性来定义 `props` 的默认值。
>
> ```js
> class Greeting extends React.Component {
>   render() {
>     return (
>       <h1>Hello, {this.props.name}</h1>
>     );
>   }
> }
> 
> // 指定 props 的默认值：
> Greeting.defaultProps = {
>   name: 'Stranger'
> };
> 
> // 渲染出 "Hello, Stranger"：
> ReactDOM.render(
>   <Greeting />,
>   document.getElementById('example')
> );
> ```



### State

#### 基本用法

- State用于更新UI界面，是私有的，并且完全受控于当前组件

```js
class Clock extends React.Component {
  // 构造函数
  constructor(props) {
    super(props);
    this.state = {date: new Date()};
  }
  // 当 Clock 组件第一次被渲染到 DOM 中的时候，“挂载（mount）”
  componentDidMount() {
    this.timerID = setInterval(
      () => this.tick(),
      1000
    );
  }
  // 当 DOM 中 Clock 组件被删除的时候，“卸载（umount）”
  componentWillUnmount() {
    clearInterval(this.timerID);
  }
  // 更新State
  tick() {
    this.setState({
      date: new Date()
    });
  }
  // 渲染
  render() {
    return (
      <div>
        <h1>Hello, world!</h1>
        <h2>It is {this.state.date.toLocaleTimeString()}.</h2>
      </div>
    );
  }
}

ReactDOM.render(
  <Clock />,
  document.getElementById('root')
);
```

> 上例是一个计时器
>
> 封装为class组件，继承与React.Component。
>
> 添加了构造函数，通过super(props)将props传递到父类的构造函数中，并且赋初值。
>
> reder()在每次组件更新时都会被调用
>
> 用到了两个生命周期函数，“挂载”和''卸载"

#### 使用要求

- 不要直接修改State

> ```js
> // Wrong
> this.state.comment = 'Hello';
> 
> // Correct
> this.setState({comment: 'Hello'});
> ```
>
> React中通过`setState()`函数修改State

> **构造函数是唯一可以给 `this.state` 赋值的地方**

- State的更新可能是异步的

> 因为 `this.props` 和 `this.state` 可能会异步更新，所以在更新的时候，不要依赖它们

```js
// Wrong
this.setState({
  counter: this.state.counter + this.props.increment,
});
// 这样可能会产生问题
```

```js
// Correct
this.setState((state, props) => ({
  counter: state.counter + props.increment
}));

// Correct
this.setState(function(state, props) {
  return {
    counter: state.counter + props.increment
  };
});
```

> 处理这种问题的方法是让`setState()`接收一个函数而不是一个对象。
>
> 那么第一个参数就是上一个state，第二个参数是将此次更新被应用时的props。

> **setState更新时间**
>
> 调用setState()修改的数据真正更新发生在**即将要执行下一次的render函数时**
>
> **这之间发生了什么？**
> `setState`调用后，React会执行一个事务（Transaction），在这个事务中，React将新state放进一个队列中，当事务完成后，React就会刷新队列，然后启动另一个事务，这个事务包括执行 `shouldComponentUpdate` 方法来判断是否重新渲染，如果是，React就会进行state合并（`state merge`）,生成新的state和props；如果不是，React仍然会更新`this.state`，只不过不会再`render`了。

- 数据是向下流动的

  > 组件本身的state可以作为props向下传递到它的子组件中。
  >
  > 就像瀑布一样，那么每一个组件的 state 就像是在任意一点上给瀑布增加额外的水源，但是它只能向下流动。
  >
  > 一般情况下，state用于当前组件内部，进行数据改变触发重新渲染。
  >
  > 而props来自于父组件，只读，从里面取出属性值。