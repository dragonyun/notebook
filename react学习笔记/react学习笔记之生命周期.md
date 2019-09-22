---
typora-root-url: img
---

## react学习笔记之生命周期

- 参考网上总结：https://www.jianshu.com/p/514fe21b9914

  > 大体一致，不过我还是自己写一遍，加一些自己的想法。



### 生命周期图

#### v16版本之后的生命周期图

- 生命周期图官方网址：**http://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/**

- `react v16.4`之后的生命周期图（中文）

![react生命周期图cn](/react生命周期图cn.png)

- `react v16.4`之后的生命周期图（英文）

![react生命周期图en](/react生命周期图en.png)

#### v16版本之前的生命周期图

- 生命周期图

![react旧生命周期图en](/react旧生命周期图en.png)

---

### 旧版生命周期图解读



#### 组件初始化(initialization)阶段

> 也就是以下代码中类的构造方法( constructor() ),Test类继承了React.Component这个基类，也就继承这个react的基类，才能有render(),生命周期等方法可以使用，这也说明为什么**函数组件不能使用这些方法**的原因。

> `super(props)`用来调用基类的构造方法( constructor() ), 也将父组件的props注入给子组件，供子组件读取(组件中props只读不可变，state可变)。
>
> <font color="red">注意</font>：`super(props)`应放在其他语句之前调用，否则在构造函数中`this.props`可能会出现未定义的bug。
>
> 而`constructor()`用来做一些组件的初始化工作，如定义this.state的初始内容。

```js
import React, { Component } from 'react';

class Test extends Component {
  constructor(props) {
    super(props);
  }
}
```

> 初始化阶段=构造方法`constructor()`
>
> 继承让自定义组件有了render，生命周期等，函数组件不讨论生命周期。
>

> <font color="red">**用途**</font>：构造函数仅用于以下两种情况
>
> - 通过给 `this.state` 赋值对象来初始化**内部 state**。
> - 为**事件处理函数**绑定实例
>
> ```js
> constructor(props) {
>   super(props);
>   // 不要在这里调用 this.setState()
>   this.state = { counter: 0 };
>   this.handleClick = this.handleClick.bind(this);
> }
> ```
>
> 只能在构造函数中直接为 `this.state` 赋值。如需在其他方法中赋值，你应使用 `this.setState()` 替代。

> <font color="red">注意</font>：
>
> **如果不初始化state或不进行方法绑定，则不需要为React组件实现构造函数。**
>
> 注：这句话可能在旧版里面不适用，因为旧版把生命周期分为四个阶段，新版只有三个阶段。

> <font color="red">注意：</font>
>
> **不允许在构造函数中使用`this.setState()`**
>
> **避免将 props 的值复制给 state！这是一个常见的错误：**
>
> ```js
> constructor(props) {
>  super(props);
>  // 不要这样做
>  this.state = { color: props.color };
> }
> ```
>
> 如此做毫无必要（你可以直接使用 `this.props.color`），同时还产生了 bug（更新 prop 中的 `color` 时，并不会影响 state）。
>
> **只有在你刻意忽略 prop 更新的情况下使用。**



#### 组件的挂载(Mounting)阶段

> 此阶段分为`componentWillMount`，`render`，`componentDidMount`三个时期。

- `componentWillMount()`

  > 在组件挂载到DOM前调用，且只会被调用一次，在这边调用`this.setState`不会引起组件重新渲染，也可以把写在这边的内容提前到constructor()中，所以项目中很少用。
  >
  > 新版已不推荐使用该函数，在v17之后将被弃用。

- `render()`

  > **`render()`方法是class组件中唯一必须实现的方法。**
  >
  > 根据组件的props和state（无两者的重传递和重赋值，论值是否有变化，都可以引起组件重新render） ，return 一个React元素（描述组件，即`UI`），不负责组件实际渲染工作，之后由React自身根据此元素去渲染出页面DOM。
  >
  > render是**纯函数**（Pure function：函数的返回结果只依赖于它的参数；函数执行过程里面没有副作用），不能在里面执行`this.setState`，会有改变组件状态的副作用。
  >
  > ---
  >
  > `render()`函数返回以下五种类型之一：
  >
  > - **React元素**。通常通过 JSX 创建。例如，`<div />` 会被 React 渲染为 DOM 节点，`<MyComponent />` 会被 React 渲染为自定义组件，无论是 `<div />` 还是 `<MyComponent />` 均为 React 元素。
  > - **数组或fragments**。使得 render 方法可以返回多个元素。
  > - **Portals**。可以渲染子节点到不同的 DOM 子树中。
  > - **字符串或数值类型**。它们在 DOM 中会被渲染为文本节点。
  > - **布尔类型或者null**。什么都不渲染。（主要用于支持返回 `test && <Child />` 的模式，其中 test 为布尔类型。)

- `componentDidMount()`

  > 组件挂载到DOM后调用，且只会被调用一次。

  > <font color="red">用途：</font>
  >
  > 1.依赖DOM节点的初始化。比如网络请求数据
  >
  > 2.订阅。记得在卸载时取消订阅
  >
  > 3.可直接调用`setState()`（不推荐）

#### 组件的更新(Updation)阶段

> react组件更新机制:`setState`引起的state更新或父组件重新render引起的props更新，更新后的state和props相对之前无论是否有变化，都将引起子组件的重新render。

- 造成组件更新有两类（三种）方法

  - 父组件重新render之一

    > 直接使用,**每当父组件重新render导致的重传props，子组件将直接跟着重新渲染，无论props是否有变化**。可通过`shouldComponentUpdate`方法优化。
    >
    > ```js
    > class Child extends Component {
    >    shouldComponentUpdate(nextProps){ // 应该使用这个方法，否则无论props是否有变化都将会导致组件跟着重新渲染
    >         if(nextProps.someThings === this.props.someThings){
    >           return false
    >         }
    >     }
    >     render() {
    >         return <div>{this.props.someThings}</div>
    >     }
    > }
    > ```

  - 父组件重新render之二

    > 在`componentWillReceiveProps`方法中，将props转换成自己的state
    >
    > ```js
    > class Child extends Component {
    >     constructor(props) {
    >         super(props);
    >         this.state = {
    >             someThings: props.someThings
    >         };
    >     }
    >     componentWillReceiveProps(nextProps) { // 父组件重传props时就会调用这个方法
    >         this.setState({someThings: nextProps.someThings});
    >     }
    >     render() {
    >         return <div>{this.state.someThings}</div>
    >     }
    > }
    > ```
    >
    > 在该函数(`componentWillReceiveProps`)中调用 `this.setState() `将不会引起第二次渲染。

    > 因为`componentWillReceiveProps`中判断props是否变化了，若变化了，`this.setState`将引起state变化，从而引起render，此时就没必要再做第二次因重传props引起的render了，不然重复做一样的渲染了。

  - 组件本身调用setState

    > **组件本身调用`setState`，无论state有没有变化**。可通过`shouldComponentUpdate`方法优化。
    >
    > ```js
    > class Child extends Component {
    >    constructor(props) {
    >         super(props);
    >         this.state = {
    >           someThings:1
    >         }
    >    }
    >    shouldComponentUpdate(nextStates){ // 应该使用这个方法，否则无论state是否有变化都将会导致组件重新渲染
    >         if(nextStates.someThings === this.state.someThings){
    >           return false
    >         }
    >     }
    > 
    >    handleClick = () => { // 虽然调用了setState ，但state并无变化
    >         const preSomeThings = this.state.someThings
    >          this.setState({
    >             someThings: preSomeThings
    >          })
    >    }
    > 
    >     render() {
    >         return <div onClick = {this.handleClick}>{this.state.someThings}</div>
    >     }
    > }
    > ```

---

> #### 此阶段分为`componentWillReceiveProps`，`shouldComponentUpdate`，`componentWillUpdate`，`render`，`componentDidUpdate`

- `componentWillReceiveProps(nextProps)`

  > 此方法只调用于props引起的组件更新过程中，参数`nextProps`是父组件传给当前组件的新props。但父组件render方法的调用不能保证重传给当前组件的props是有变化的，所以在此方法中根据`nextProps`和this.props来查明重传的props是否改变，以及如果改变了要执行啥，比如根据新的props调用`this.setState`出发当前组件的重新render

- `shouldComponentUpdate(nextProps,nextState)`

  > 此方法通过比较`nextProps`，`nextState`及当前组件的this.props，this.state，返回true时当前组件将继续执行更新过程，返回false则当前组件更新停止，以此可用来减少组件的不必要渲染，优化组件性能。

  > 这边也可以看出，就算`componentWillReceiveProps()`中执行了`this.setState`，更新了state，但在render前（如`shouldComponentUpdate`，`componentWillUpdate`），this.state依然指向更新前的state，不然`nextState`及当前组件的this.state的对比就一直是true了。

  > <font color="red">注意：</font>**该方法仅作为性能优化的方式而存在**。
  >
  > 后续可能会将该函数当做提示而不是指令。也就是说返回false，仍然会重新渲染

- `componentWillUpdate(nextProps, nextState)`

  > 此方法在调用render方法前执行，在这边可执行一些组件更新发生前的工作，一般较少用。

- `render`

  > render方法在上文讲过，这边只是重新调用。

- `componentDidUpdate(prevProps, prevState)`

  > 此方法在组件更新后被调用，可以操作组件更新的DOM，`prevProps`和`prevState`这两个参数指的是组件更新前的props和state
  >
  > 在新版中加入了第三个参数snapshot。
  >
  > 该函数会在更新后立即调用。首次渲染不会执行此方法。
  >
  > 用途：
  >
  > - 在此处进行DOM操作
  > - 对更新前后的props进行比较
  > - 在此处进行网络请求
  >
  > - 可直接调用setSate()函数，但是它必须放在条件语句里，否则会死循环的。

#### 卸载阶段(Unmouting)阶段

> 此阶段只有一个生命周期方法：`componentWillUnmount`

- `componentWillUnmount`

  > 此方法在组件被卸载前调用，可以在这里执行一些清理工作，比如清除组件中使用的定时器，清除`componentDidMount`中手动创建的DOM元素等，以避免引起内存泄漏。

  > 卸载组件前的收尾工作:
  >
  > 一些必要的清理操作。
  >
  > 不应该调用`setState()`，因为卸载了，所以永远不会生效

### 新版生命周期解读（v16）

> 如上图，生命周期被改动了，v16.04之后都遵循着新版的生命周期

#### 新版生命周期函数一览

> - 挂载阶段
>   - constructor()
>   - `static getDerivedStateFromProps()`
>   - `render()`
>   - `componentDidMount()`
> - 更新阶段
>   - `static gitDerivedStateFromProps()`
>   - `shouldComponentUpdate()`
>   - `render()`
>   - `getSnapshotBeforeUpdate()`
>   - `componentDidUpdate()`
> - 卸载阶段
>   - `componentWillUnmount()`
> - 错误处理
>   - `static getDerivedStateFromError()`
>   - `componentDidCatch()`

#### 变更缘由

> 原来（`React v16.0`前）的生命周期在`React v16`推出的[Fiber](https://zhuanlan.zhihu.com/p/26027085)之后就不合适了，因为如果要开启`async rendering`，在render函数之前的所有函数，都有可能被执行多次。

> 原来（React v16.0前）的生命周期有哪些是在render前执行的呢？
>
> - `componentWillMount`
> - `componentWillReceiveProps`
> - `shouldComponentUpdate`
> - `componentWillUpdate`

> 如果开发者开了`async rendering`，而且又在以上这些render前执行的生命周期方法做AJAX请求的话，那AJAX将被无谓地多次调用。。。明显不是我们期望的结果。而且在`componentWillMount`里发起AJAX，不管多快得到结果也赶不上首次render，而且`componentWillMount`服务器端渲染也会被调用到（当然，也许这是预期的结果），这样的IO操作放在`componentDidMount`里更合适。

> 禁止不能用比劝导开发者不要这样用的效果更好，所以除了`shouldComponentUpdate`，其他在render函数之前的所有函数（`componentWillMount`，`componentWillReceiveProps`，`componentWillUpdate`）都被`getDerivedStateFromProps`替代。

> 也就是用一个静态函数`getDerivedStateFromProps`来取代被deprecate的几个生命周期函数，就是强制开发者在render之前只做无副作用的操作，而且能做的操作局限在根据props和state决定新的state.

> <font color="red">就是因为React提供了异步的功能，所以为了保证渲染的时候数据不出错。把render之前的生命周期函数合并为一个静态函数。这样挺好的，与其告诉开发者这里要注意那里要注意，不如就官方默认规定好，用不了就是不允许。</font>

----

#### 新加入的生命周期

- `getDerivedStateFromProps`

  > **`static` `getDerivedStateFromProps(props, state)`** 在组件创建时和更新时的render方法之前调用，它应该返回一个对象来更新状态state，或者返回null来不更新任何内容。

  > <font color="red">注意：</font>
  >
  > - 此方法适用于罕见的用例，即 state 的值在任何时候都取决于 props。
  > - 此方法无权访问组件实例。

  > 所以这个函数用的不多，推荐使用别的函数去处理你的逻辑。

- `getSnapshotBeforeUpdate`

  > **`getSnapshotBeforeUpdate(prevProps, prevState)`** 被调用于render之后，可以读取但无法使用DOM的时候。它使您的组件可以在可能更改之前从DOM捕获一些信息（例如滚动位置）。此生命周期返回的任何值都将作为参数传递给`componentDidUpdate（）`，作为它的第三个参数。

  > <font color='red'>注意</font>：此用法并不常见，但它可能出现在 UI 处理中，如需要以特殊方式处理滚动位置的聊天线程等。
  >
  > 应返回 snapshot 的值（或 `null`）。

  ```js
  // 官方例子
  class ScrollingList extends React.Component {
    constructor(props) {
      super(props);
      this.listRef = React.createRef();
    }
  
    getSnapshotBeforeUpdate(prevProps, prevState) {
      //我们是否要添加新的 items 到列表?
      // 捕捉滚动位置，以便我们可以稍后调整滚动.
      if (prevProps.list.length < this.props.list.length) {
        const list = this.listRef.current;
        return list.scrollHeight - list.scrollTop;
      }
      return null;
    }
  
    componentDidUpdate(prevProps, prevState, snapshot) {
      //如果我们有snapshot值, 我们已经添加了 新的items.
      // 调整滚动以至于这些新的items 不会将旧items推出视图。
      // (这边的snapshot是 getSnapshotBeforeUpdate方法的返回值)
      if (snapshot !== null) {
        const list = this.listRef.current;
        list.scrollTop = list.scrollHeight - snapshot;
      }
    }
  
    render() {
      return (
        <div ref={this.listRef}>{/* ...contents... */}</div>
      );
    }
  }
  ```


#### 去除的生命周期

- `componentWillMount`，`componentWillReceiveProps`，`componentWillUpdate`

  > 都位于render之前，被`getDerivedStateFromProps`替换掉。

#### 异常捕获

> [Error boundaries](https://react.docschina.org/docs/error-boundaries.html) 是 React 组件，它会在其子组件树中的任何位置捕获 JavaScript 错误，并记录这些错误，展示降级 UI 而不是崩溃的组件树。Error boundaries 组件会捕获在渲染期间，在生命周期方法以及其整个树的构造函数中发生的错误。

> 如果 class 组件定义了生命周期方法 `static getDerivedStateFromError()` 或 `componentDidCatch()` 中的任何一个（或两者），它就成为了 Error boundaries。通过生命周期更新 state 可让组件捕获树中未处理的 JavaScript 错误并展示降级 UI。

> 仅使用 Error boundaries 组件来从意外异常中恢复的情况；**不要将它们用于流程控制。**

> **注意：**Error boundaries 仅捕获组件树中**以下**组件中的错误。但它本身的错误无法捕获。

- 涉及到两个函数：`static getDerivedStateFromError()`和`componentDidCatch()`

- ### `static getDerivedStateFromError()`

> ```js
> static getDerivedStateFromError(error)
> ```
>
> 此生命周期会在后代组件抛出错误后被调用。 它将抛出的错误作为参数，并返回一个值以更新 state
>
> ```js
> class ErrorBoundary extends React.Component {
>   constructor(props) {
>     super(props);
>     this.state = { hasError: false };
>   }
> 
>   static getDerivedStateFromError(error) {
>     // 更新 state 使下一次渲染可以显降级 UI
>     return { hasError: true };
>   }
> 
>   render() {
>     if (this.state.hasError) {
>       // 你可以渲染任何自定义的降级  UI
>       return <h1>Something went wrong.</h1>;
>     }
> 
>     return this.props.children; 
>   }
> }
> ```
>
> **注意**
>
> `getDerivedStateFromError()` 会在`渲染`阶段调用，因此不允许出现副作用。 如遇此类情况，请改用 `componentDidCatch()`。

- ### `componentDidCatch()`

> ```js
> componentDidCatch(error, info)
> ```
>
> 此生命周期在后代组件抛出错误后被调用。 它接收两个参数：
>
> 1. `error` —— 抛出的错误。
> 2. `info` —— 带有 `componentStack` key 的对象，其中包含[有关组件引发错误的栈信息](https://react.docschina.org/docs/error-boundaries.html#component-stack-traces)。
>
> `componentDidCatch()` 会在“提交”阶段被调用，因此允许执行副作用。 它应该用于记录错误之类的情况：
>
> ```js
> class ErrorBoundary extends React.Component {
>   constructor(props) {
>     super(props);
>     this.state = { hasError: false };
>   }
> 
>   static getDerivedStateFromError(error) {
>     // 更新 state 使下一次渲染可以显示降级 UI
>     return { hasError: true };
>   }
> 
>   componentDidCatch(error, info) {
>     // "组件堆栈" 例子:
>     //   in ComponentThatThrows (created by App)
>     //   in ErrorBoundary (created by App)
>     //   in div (created by App)
>     //   in App
>     logComponentStackToMyService(info.componentStack);
>   }
> 
>   render() {
>     if (this.state.hasError) {
>       // 你可以渲染任何自定义的降级 UI
>       return <h1>Something went wrong.</h1>;
>     }
> 
>     return this.props.children; 
>   }
> }
> ```
>
> 注意
>
> 如果发生错误，你可以通过调用 `setState` 使用 `componentDidCatch()` 渲染降级 UI，但在未来的版本中将不推荐这样做。 可以使用静态 `getDerivedStateFromError()` 来处理降级渲染。

### 生命周期注意事项

#### 新老版本生命周期更换的一些解决方案的考虑

##### 初始化state

> 原先的生命周期中，初始化是放在`componentWillMount`中的
>
> ```js
> // Before
> class ExampleComponent extends React.Component {
>   state = {};
> 
>   componentWillMount() {
>     this.setState({
>       currentColor: this.props.defaultColor,
>       palette: 'rgb',
>     });
>   }
> }
> ```
>
> 现在直接放在构造函数，或者直接给值
>
> ```js
> // After
> class ExampleComponent extends React.Component {
>   state = {
>     currentColor: this.props.defaultColor,
>     palette: 'rgb',
>   };
> }
> ```
>
> 总结：
>
> 我觉得没什么说的，我反而想问，以前为什么要放在那个生命周期里呢？

##### 初始请求数据（API）

> 原先的生命周期中，请求是放在`componentWillMount`中的
>
> ```js
> // Before
> class ExampleComponent extends React.Component {
>   state = {
>     externalData: null,
>   };
> 
>   componentWillMount() {
>     this._asyncRequest = loadMyAsyncData().then(
>       externalData => {
>         this._asyncRequest = null;
>         this.setState({externalData});
>       }
>     );
>   }
> 
>   componentWillUnmount() {
>     if (this._asyncRequest) {
>       this._asyncRequest.cancel();
>     }
>   }
> 
>   render() {
>     if (this.state.externalData === null) {
>       // Render loading state ...
>     } else {
>       // Render real UI ...
>     }
>   }
> }
> ```
>
> 现在是放在`componentDidMount`中的
>
> ```js
> // After
> class ExampleComponent extends React.Component {
>   state = {
>     externalData: null,
>   };
> 
>   componentDidMount() {
>     this._asyncRequest = loadMyAsyncData().then(
>       externalData => {
>         this._asyncRequest = null;
>         this.setState({externalData});
>       }
>     );
>   }
> 
>   componentWillUnmount() {
>     if (this._asyncRequest) {
>       this._asyncRequest.cancel();
>     }
>   }
> 
>   render() {
>     if (this.state.externalData === null) {
>       // Render loading state ...
>     } else {
>       // Render real UI ...
>     }
>   }
> }
> ```
>
> 总结：
>
> 一个是处于SSR的考虑，会跳过`componentWillMount`函数；
>
> 一个是开启Async Rendering后，就可能会发多个请求；
>
> 这里有一个考量，我们都希望先有数据再渲染，但是你就算是把请求放在`componentWillMount`中，依然不能保证数据早于`render`，所以请求放在挂载之后，没问题的。

##### 订阅监听

> 原先的生命周期中，订阅监听是放在`componentWillMount`中的
>
> ```js
> // Before
> class ExampleComponent extends React.Component {
>   componentWillMount() {
>     this.setState({
>       subscribedValue: this.props.dataSource.value,
>     });
> 
>     // This is not safe; it can leak!
>     this.props.dataSource.subscribe(
>       this.handleSubscriptionChange
>     );
>   }
> 
>   componentWillUnmount() {
>     this.props.dataSource.unsubscribe(
>       this.handleSubscriptionChange
>     );
>   }
> 
>   handleSubscriptionChange = dataSource => {
>     this.setState({
>       subscribedValue: dataSource.value,
>     });
>   };
> }
> ```
>
> 现在的，是放在`componentDidMount`函数中的
>
> ```js
> // After
> class ExampleComponent extends React.Component {
>   state = {
>     subscribedValue: this.props.dataSource.value,
>   };
> 
>   componentDidMount() {
>     // Event listeners are only safe to add after mount,
>     // So they won't leak if mount is interrupted or errors.
>     this.props.dataSource.subscribe(
>       this.handleSubscriptionChange
>     );
> 
>     // External values could change between render and mount,
>     // In some cases it may be important to handle this case.
>     if (
>       this.state.subscribedValue !==
>       this.props.dataSource.value
>     ) {
>       this.setState({
>         subscribedValue: this.props.dataSource.value,
>       });
>     }
>   }
> 
>   componentWillUnmount() {
>     this.props.dataSource.unsubscribe(
>       this.handleSubscriptionChange
>     );
>   }
> 
>   handleSubscriptionChange = dataSource => {
>     this.setState({
>       subscribedValue: dataSource.value,
>     });
>   };
> }
> ```
>
> 总结：
>
> 主要一个原因是`compoonentDidMount`函数不一定执行（在Async Rendering环境下），如果它不执行，那么就永远也没法执行`componentWillUnmount`函数，订阅和监听就没法取消掉。为了保证这个，也要移过去。

##### 基于props更新state

> 原先的生命周期中，更新是放在`componentWillReceiveProps`中的
>
> ```js
> // Before
> class ExampleComponent extends React.Component {
>   state = {
>     isScrollingDown: false,
>   };
> 
>   componentWillReceiveProps(nextProps) {
>     if (this.props.currentRow !== nextProps.currentRow) {
>       this.setState({
>         isScrollingDown:
>           nextProps.currentRow > this.props.currentRow,
>       });
>     }
>   }
> }
> ```
>
> 现在
>
> ```js
> // After
> class ExampleComponent extends React.Component {
>   // Initialize state in constructor,
>   // Or with a property initializer.
>   state = {
>     isScrollingDown: false,
>     lastRow: null,
>   };
> 
>   static getDerivedStateFromProps(props, state) {
>     if (props.currentRow !== state.lastRow) {
>       return {
>         isScrollingDown: props.currentRow > state.lastRow,
>         lastRow: props.currentRow,
>       };
>     }
> 
>     // Return null to indicate no change to state.
>     return null;
>   }
> }
> ```
>
> 总结：
>
> 官方怕开发者用的不对，或者说以前的做法容易出错，所以换新的。
>
> 新的做法可以看到在做判断的时候是`if (props.currentRow !== state.lastRow) `这里是获取不到prevProps的，因为在最开始第一次这个值将会是null，不方便处理。
>
> 还有一个考虑是未来的内存优化问题，暂时还不理解。

##### 调用外部回调

> 旧的做法是放在`componentWillUpdate`里面
>
> ```js
> // Before
> class ExampleComponent extends React.Component {
>   componentWillUpdate(nextProps, nextState) {
>     if (
>       this.state.someStatefulValue !==
>       nextState.someStatefulValue
>     ) {
>       nextProps.onChange(nextState.someStatefulValue);
>     }
>   }
> }
> ```
>
> 新的做法是放在`componentDidUpdate`里面
>
> ```js
> // After
> class ExampleComponent extends React.Component {
>   componentDidUpdate(prevProps, prevState) {
>     if (
>       this.state.someStatefulValue !==
>       prevState.someStatefulValue
>     ) {
>       this.props.onChange(this.state.someStatefulValue);
>     }
>   }
> }
> 
> ```
>
> 总结：
>
> 其实，并不是说新的一定比旧方法更正确，可以理解为官方在整体调整生命周期之后的解决方案。
>
> 这里旧版在异步模式下可能会多次触发回调函数。

##### props改变的副作用

> ```js
> // Before
> class ExampleComponent extends React.Component {
>   componentWillReceiveProps(nextProps) {
>     if (this.props.isVisible !== nextProps.isVisible) {
>       logVisibleChange(nextProps.isVisible);
>     }
>   }
> }
> ```
>
> ```js
> // After
> class ExampleComponent extends React.Component {
>   componentDidUpdate(prevProps, prevState) {
>     if (this.props.isVisible !== prevProps.isVisible) {
>       logVisibleChange(this.props.isVisible);
>     }
>   }
> }
> ```
>
> 总结：
>
> 其实是一个问题就是`componentWillUpdate`, `componentWillReceiveProps`这样的生命周期函数可能会多次触发导致问题，所以很多情况下，推荐使用`componentDidUpdate`，它可以保证只执行一次。

##### props改变时的请求数据（API）

> ```js
> // Before
> class ExampleComponent extends React.Component {
>   state = {
>     externalData: null,
>   };
> 
>   componentDidMount() {
>     this._loadAsyncData(this.props.id);
>   }
> 
>   componentWillReceiveProps(nextProps) {
>     if (nextProps.id !== this.props.id) {
>       this.setState({externalData: null});
>       this._loadAsyncData(nextProps.id);
>     }
>   }
> 
>   componentWillUnmount() {
>     if (this._asyncRequest) {
>       this._asyncRequest.cancel();
>     }
>   }
> 
>   render() {
>     if (this.state.externalData === null) {
>       // Render loading state ...
>     } else {
>       // Render real UI ...
>     }
>   }
> 
>   _loadAsyncData(id) {
>     this._asyncRequest = loadMyAsyncData(id).then(
>       externalData => {
>         this._asyncRequest = null;
>         this.setState({externalData});
>       }
>     );
>   }
> }
> ```
>
> 新的做法是拆分开到两个生命周期中
>
> ```js
> // After
> class ExampleComponent extends React.Component {
>   state = {
>     externalData: null,
>   };
> 
>   static getDerivedStateFromProps(props, state) {
>     // Store prevId in state so we can compare when props change.
>     // Clear out previously-loaded data (so we don't render stale stuff).
>     if (props.id !== state.prevId) {
>       return {
>         externalData: null,
>         prevId: props.id,
>       };
>     }
> 
>     // No state update necessary
>     return null;
>   }
> 
>   componentDidMount() {
>     this._loadAsyncData(this.props.id);
>   }
> 
>   componentDidUpdate(prevProps, prevState) {
>     if (this.state.externalData === null) {
>       this._loadAsyncData(this.props.id);
>     }
>   }
> 
>   componentWillUnmount() {
>     if (this._asyncRequest) {
>       this._asyncRequest.cancel();
>     }
>   }
> 
>   render() {
>     if (this.state.externalData === null) {
>       // Render loading state ...
>     } else {
>       // Render real UI ...
>     }
>   }
> 
>   _loadAsyncData(id) {
>     this._asyncRequest = loadMyAsyncData(id).then(
>       externalData => {
>         this._asyncRequest = null;
>         this.setState({externalData});
>       }
>     );
>   }
> }
> ```
>
> 总结：
>
>  不论是初始化时的请求，还是更新过程中的请求，
>
> 一方面是为了不会多次请求，保证唯一性；
>
> 另一方面把功能拆分到了`static getDerivedStateFromProps`和`componentDidUpdate`，与原生命周期的做法等价。

##### 在更新之前读取DOM属性

> ```js
> class ScrollingList extends React.Component {
>   listRef = null;
>   previousScrollOffset = null;
> 
>   componentWillUpdate(nextProps, nextState) {
>     // Are we adding new items to the list?
>     // Capture the scroll position so we can adjust scroll later.
>     if (this.props.list.length < nextProps.list.length) {
>       this.previousScrollOffset =
>         this.listRef.scrollHeight - this.listRef.scrollTop;
>     }
>   }
> 
>   componentDidUpdate(prevProps, prevState) {
>     // If previousScrollOffset is set, we've just added new items.
>     // Adjust scroll so these new items don't push the old ones out of view.
>     if (this.previousScrollOffset !== null) {
>       this.listRef.scrollTop =
>         this.listRef.scrollHeight -
>         this.previousScrollOffset;
>       this.previousScrollOffset = null;
>     }
>   }
> 
>   render() {
>     return (
>       <div ref={this.setListRef}>
>         {/* ...contents... */}
>       </div>
>     );
>   }
> 
>   setListRef = ref => {
>     this.listRef = ref;
>   };
> }
> ```
>
> ```js
> class ScrollingList extends React.Component {
>   listRef = null;
>   previousScrollOffset = null;
> 
>   componentWillUpdate(nextProps, nextState) {
>     // Are we adding new items to the list?
>     // Capture the scroll position so we can adjust scroll later.
>     if (this.props.list.length < nextProps.list.length) {
>       this.previousScrollOffset =
>         this.listRef.scrollHeight - this.listRef.scrollTop;
>     }
>   }
> 
>   componentDidUpdate(prevProps, prevState) {
>     // If previousScrollOffset is set, we've just added new items.
>     // Adjust scroll so these new items don't push the old ones out of view.
>     if (this.previousScrollOffset !== null) {
>       this.listRef.scrollTop =
>         this.listRef.scrollHeight -
>         this.previousScrollOffset;
>       this.previousScrollOffset = null;
>     }
>   }
> 
>   render() {
>     return (
>       <div ref={this.setListRef}>
>         {/* ...contents... */}
>       </div>
>     );
>   }
> 
>   setListRef = ref => {
>     this.listRef = ref;
>   };
> }
> ```
>
> ```js
> class ScrollingList extends React.Component {
>   listRef = null;
>   previousScrollOffset = null;
> 
>   componentWillUpdate(nextProps, nextState) {
>     // Are we adding new items to the list?
>     // Capture the scroll position so we can adjust scroll later.
>     if (this.props.list.length < nextProps.list.length) {
>       this.previousScrollOffset =
>         this.listRef.scrollHeight - this.listRef.scrollTop;
>     }
>   }
> 
>   componentDidUpdate(prevProps, prevState) {
>     // If previousScrollOffset is set, we've just added new items.
>     // Adjust scroll so these new items don't push the old ones out of view.
>     if (this.previousScrollOffset !== null) {
>       this.listRef.scrollTop =
>         this.listRef.scrollHeight -
>         this.previousScrollOffset;
>       this.previousScrollOffset = null;
>     }
>   }
> 
>   render() {
>     return (
>       <div ref={this.setListRef}>
>         {/* ...contents... */}
>       </div>
>     );
>   }
> 
>   setListRef = ref => {
>     this.listRef = ref;
>   };
> }
> ```
>
> 希望在更新前后保留滚动条位置，这个场景在Async Rendering下 *比较特殊* ,因为 `componentWillUpdate `属于第1阶段，实际DOM更新在第2阶段，两个阶段之间允许其它任务及用户交互，如果 `componentWillUpdate `之后，用户 `resize `窗口或者滚动列表（ `scrollHeight `和 `scrollTop `发生变化），就会导致DOM更新阶段应用旧值.
>
> 可以通过 `getSnapshotBeforeUpdate + componentDidUpdate `来解：
>
> ```js
> class ScrollingList extends React.Component {
>   listRef = null;
> 
>   getSnapshotBeforeUpdate(prevProps, prevState) {
>     // Are we adding new items to the list?
>     // Capture the scroll position so we can adjust scroll later.
>     if (prevProps.list.length < this.props.list.length) {
>       return (
>         this.listRef.scrollHeight - this.listRef.scrollTop
>       );
>     }
>     return null;
>   }
> 
>   componentDidUpdate(prevProps, prevState, snapshot) {
>     // If we have a snapshot value, we've just added new items.
>     // Adjust scroll so these new items don't push the old ones out of view.
>     // (snapshot here is the value returned from getSnapshotBeforeUpdate)
>     if (snapshot !== null) {
>       this.listRef.scrollTop =
>         this.listRef.scrollHeight - snapshot;
>     }
>   }
> 
>   render() {
>     return (
>       <div ref={this.setListRef}>
>         {/* ...contents... */}
>       </div>
>     );
>   }
> 
>   setListRef = ref => {
>     this.listRef = ref;
>   };
> }
> ```
>
> 总结：
>
> `getSnapshotBeforeUpdate `是在第2阶段更新实际DOM之前调用，从这里到实际DOM更新之间不会被打断。
>
> 这里更多的提到了`Async Rendering`这个概念，很多问题都是由它引发的，这里的生命周期的更替都还好理解，主要是这个概念是干什么用的，异步渲染？？？