## react学习笔记之Refs and DOM

### 总体说明

> Refs 提供了一种方式，允许我们访问 DOM 节点或在 render 方法中创建的 React 元素。

> 在典型的 React 数据流中，props 是父组件与子组件交互的唯一方式。要修改一个子组件，你需要使用新的 props 来重新渲染它。但是，在某些情况下，你需要在典型数据流之外强制修改子组件。被修改的子组件可能是一个 React 组件的实例，也可能是一个 DOM 元素。对于这两种情况，React 都提供了解决办法。

> 总结：
>
> 说白了，ref就是用来挂DOM节点或者React元素的。
>
> 这样就提供了一种独特的方式来控制DOM节点或者React元素。
>
> 还是挺有用的。

### 何时使用Refs

> 下面是几个适合使用 refs 的情况：
>
> - 管理焦点，文本选择或媒体播放。
> - 触发强制动画。
> - 集成第三方 DOM 库。
>
> 避免使用 refs 来做任何可以通过声明式实现来完成的事情。
>
> 举个例子，避免在 `Dialog` 组件里暴露 `open()` 和 `close()` 方法，最好传递 `isOpen` 属性。
>
> ---
>
> 不过我用就是另外一种用法了，我是用来提取当前节点或者元素的属性，比如宽高等。

### 不要过度使用Refs

> 你可能首先会想到使用 refs 在你的 app 中“让事情发生”。如果是这种情况，请花一点时间，认真再考虑一下 state 属性应该被安排在哪个组件层中。通常你会想明白，让更高的组件层级拥有这个 state，是更恰当的。查看 [状态提升](https://react.docschina.org/docs/lifting-state-up.html) 以获取更多有关示例。

### 创建 Refs

> Refs 是使用 `React.createRef()` 创建的，并通过 `ref` 属性附加到 React 元素。在构造组件时，通常将 Refs 分配给实例属性，以便可以在整个组件中引用它们。
>
> ```js
> class MyComponent extends React.Component {
>   constructor(props) {
>     super(props);
>     this.myRef = React.createRef();
>   }
>   render() {
>     return <div ref={this.myRef} />;
>   }
> }
> ```
>
> 注意:
>
> 上面的例子已经更新为使用在 React 16.3 版本引入的 `React.createRef()` API。如果你正在使用一个较早版本的 React，我们推荐你使用 回调形式的 refs。
>
> ----
>
> 我理解的是：
>
> 使用react自带函数或者回调函数，是两种不同的实现方式，实际上效果是相似的，
>
> 我更倾向于使用react新版自带函数，这是新版特性，省去了一些麻烦。

### 访问 Refs

> 当 ref 被传递给 `render` 中的元素时，对该节点的引用可以在 ref 的 `current` 属性中被访问。
>
> ```js
> const node = this.myRef.current;
> ```
>
> ref 的值根据节点的类型而有所不同：
>
> - 当 `ref` 属性用于 HTML 元素时，构造函数中使用 `React.createRef()` 创建的 `ref` 接收底层 DOM 元素作为其 `current` 属性。
> - 当 `ref` 属性用于自定义 class 组件时，`ref` 对象接收组件的挂载实例作为其 `current` 属性。
> - **你不能在函数组件上使用 ref 属性**，因为他们没有实例。

#### 为 DOM 元素添加 ref

> 以下代码使用 `ref` 去存储 DOM 节点的引用：
>
> ```js
> class CustomTextInput extends React.Component {
>   constructor(props) {
>     super(props);
>     // 创建一个 ref 来存储 textInput 的 DOM 元素
>     this.textInput = React.createRef();
>     this.focusTextInput = this.focusTextInput.bind(this);
>   }
> 
>   focusTextInput() {
>     // 直接使用原生 API 使 text 输入框获得焦点
>     // 注意：我们通过 "current" 来访问 DOM 节点
>     this.textInput.current.focus();
>   }
> 
>   render() {
>     // 告诉 React 我们想把 <input> ref 关联到
>     // 构造器里创建的 `textInput` 上
>     return (
>       <div>
>         <input
>           type="text"
>           ref={this.textInput} />
>         <input
>           type="button"
>           value="Focus the text input"
>           onClick={this.focusTextInput}
>         />
>       </div>
>     );
>   }
> }
> ```
>
> React 会在组件挂载时给 `current` 属性传入 DOM 元素，并在组件卸载时传入 `null` 值。
>
> `ref`会在 `componentDidMount` 或 `componentDidUpdate` 生命周期钩子触发前更新。
>
> #### <font color="red">划重点：</font>
>
> ref在挂载时传入DOM，卸载时传null，回调函数也遵守这个规则。
>
> 并且在Did生命周期函数前更新数据。

#### 为 class 组件添加 Ref

> 如果我们想包装上面的 `CustomTextInput`，来模拟它挂载之后立即被点击的操作，我们可以使用 ref 来获取这个自定义的 input 组件并手动调用它的 `focusTextInput` 方法：
>
> ```js
> class AutoFocusTextInput extends React.Component {
>   constructor(props) {
>     super(props);
>     this.textInput = React.createRef();
>   }
> 
>   componentDidMount() {
>     this.textInput.current.focusTextInput();
>   }
> 
>   render() {
>     return (
>       <CustomTextInput ref={this.textInput} />
>     );
>   }
> }
> ```
>
> 请注意，这仅在 `CustomTextInput` 声明为 class 时才有效：
>
> ```js
> class CustomTextInput extends React.Component {
>   // ...
> }
> ```
>
> #### 函数组件不能添加ref

#### Refs 与函数组件

> **你不能在函数组件上使用 ref 属性**，因为它们没有实例：
>
> ```js
> function MyFunctionComponent() {
>   return <input />;
> }
> 
> class Parent extends React.Component {
>   constructor(props) {
>     super(props);
>     this.textInput = React.createRef();
>   }
>   render() {
>     // This will *not* work!
>     return (
>       <MyFunctionComponent ref={this.textInput} />
>     );
>   }
> }
> ```
>
> 如果你需要使用 ref，你应该将组件转化为一个 class，就像当你需要使用生命周期钩子或 state 时一样。
>
> 不管怎样，你可以**在函数组件内部使用 ref 属性**，只要它指向一个 DOM 元素或 class 组件：
>
> ```js
> function CustomTextInput(props) {
>   // 这里必须声明 textInput，这样 ref 才可以引用它
>   let textInput = React.createRef();
> 
>   function handleClick() {
>     textInput.current.focus();
>   }
> 
>   return (
>     <div>
>       <input
>         type="text"
>         ref={textInput} />
>       <input
>         type="button"
>         value="Focus the text input"
>         onClick={handleClick}
>       />
>     </div>
>   );
> }
> ```
>
> ### ref还是要用在DOM元素或者class组件上。

---

### 将 DOM Refs 暴露给父组件

> 在极少数情况下，你可能希望在父组件中引用子节点的 DOM 节点。通常不建议这样做，因为它会打破组件的封装，但它偶尔可用于触发焦点或测量子 DOM 节点的大小或位置。
>
> 如果你使用 16.3 或更高版本的 React, 这种情况下我们推荐使用 [ref 转发](https://react.docschina.org/docs/forwarding-refs.html)。**Ref 转发使组件可以像暴露自己的 ref 一样暴露子组件的 ref**。关于怎样对父组件暴露子组件的 DOM 节点，在 [ref 转发文档](https://react.docschina.org/docs/forwarding-refs.html#forwarding-refs-to-dom-components)中有一个详细的例子。
>
> 这种需求建议使用ref转发吧。

### 回调 Refs

> React 也支持另一种设置 refs 的方式，称为“回调 refs”。它能助你更精细地控制何时 refs 被设置和解除。
>
> 不同于传递 `createRef()` 创建的 `ref` 属性，你会传递一个函数。这个函数中接受 React 组件实例或 HTML DOM 元素作为参数，以使它们能在其他地方被存储和访问。
>
> ---
>
> React 将在组件挂载时，会调用 `ref` 回调函数并传入 DOM 元素，当卸载时调用它并传入 `null`。在 `componentDidMount` 或 `componentDidUpdate` 触发前，React 会保证 refs 一定是最新的。
>
> ---
>
> 你可以在组件间传递回调形式的 refs，就像你可以传递通过 `React.createRef()` 创建的对象 refs 一样。
>
> ---
>
> 回调是老版本的做法， `React.createRef()` 是新版的做法，回调了解一下，知道有这么回事就好了。

### 关于回调 refs 的说明

> 如果 `ref` 回调函数是以内联函数的方式定义的，在更新过程中它会被执行两次，第一次传入参数 `null`，然后第二次会传入参数 DOM 元素。这是因为在每次渲染时会创建一个新的函数实例，所以 React 清空旧的 ref 并且设置新的。通过将 ref 的回调函数定义成 class 的绑定函数的方式可以避免上述问题，但是大多数情况下它是无关紧要的。