## react学习笔记之条件渲染

> 条件渲染，顾名思义，就是通过判断条件渲染不同的内容，虽然我现在已经在用了，
>
> 常见的有if，？：三目运算符等等，不过还是总结一下吧，查缺补漏。

> React 中的条件渲染和 JavaScript 中的一样，使用 JavaScript 运算符 [`if`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/if...else) 或者[条件运算符](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)去创建元素来表现当前的状态，然后让 React 根据它们来更新 `UI`。

### 一个例子

> ```js
> // 这里是两个基本组件
> function UserGreeting(props) {
>   return <h1>Welcome back!</h1>;
> }
> 
> function GuestGreeting(props) {
>   return <h1>Please sign up.</h1>;
> }
> ```
>
> 再创建一个 `Greeting` 组件，它会根据用户是否登录来决定显示上面的哪一个组件。（条件渲染）
>
> ```js
> function Greeting(props) {
>   const isLoggedIn = props.isLoggedIn;
>   if (isLoggedIn) {
>     return <UserGreeting />;
>   }
>   return <GuestGreeting />;
> }
> 
> ReactDOM.render(
>   // Try changing to isLoggedIn={true}:
>   <Greeting isLoggedIn={false} />,
>   document.getElementById('root')
> );
> ```
>
> 根据 `isLoggedIn` 的值来渲染不同的问候语

### 元素变量

> 可以使用变量来储存元素。 它可以帮助你有条件地渲染组件的一部分，而其他的渲染部分并不会因此而改变。
>
> 例子就不贴了，和上面是一样的。
>
> 通过声明一个变量并使用 `if` 语句进行条件渲染是不错的方式，也很常见。

### 与运算符&&

> 通过花括号包裹代码，你可以[在 JSX 中嵌入任何表达式](https://react.docschina.org/docs/introducing-jsx.html#embedding-expressions-in-jsx)。这也包括 JavaScript 中的逻辑与 (&&) 运算符。它可以很方便地进行元素的条件渲染。
>
> ```js
> function Mailbox(props) {
>   const unreadMessages = props.unreadMessages;
>   return (
>     <div>
>       <h1>Hello!</h1>
>       {unreadMessages.length > 0 &&
>         <h2>
>           You have {unreadMessages.length} unread messages.
>         </h2>
>       }
>     </div>
>   );
> }
> 
> const messages = ['React', 'Re: React', 'Re:Re: React'];
> ReactDOM.render(
>   <Mailbox unreadMessages={messages} />,
>   document.getElementById('root')
> );
> ```
>
> 之所以能这样做，是因为在 JavaScript 中，`true && expression` 总是会返回 `expression`, 而 `false && expression` 总是会返回 `false`。
>
> 因此，如果条件是 `true`，`&&` 右侧的元素就会被渲染，如果是 `false`，React 会忽略并跳过它。
>
> 对对对，想起来了这是与运算符的基本逻辑，前者为false返回false，前者为true返回后者的结果。
>
> 挺好的用法。

### 三目运算符

> 另一种内联条件渲染的方法是使用 JavaScript 中的三目运算符 [`condition ? true : false`](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Conditional_Operator)。
>
> ```js
> render() {
>   const isLoggedIn = this.state.isLoggedIn;
>   return (
>     <div>
>       The user is <b>{isLoggedIn ? 'currently' : 'not'}</b> logged in.
>     </div>
>   );
> }
> ```
>
> 同样的，它也可以用于较为复杂的表达式中，虽然看起来不是很直观：
>
> ```js
> render() {
>   const isLoggedIn = this.state.isLoggedIn;
>   return (
>     <div>
>       {isLoggedIn ? (
>         <LogoutButton onClick={this.handleLogoutClick} />
>       ) : (
>         <LoginButton onClick={this.handleLoginClick} />
>       )}
>     </div>
>   );
> }
> ```
>
> 好像也不复杂，已经在用了，这个很常用的。
>
> 如果再复杂一点就考虑再提取一个组件出来好了。

### 阻止组件渲染

> 在极少数情况下，你可能希望能隐藏组件，即使它已经被其他组件渲染。若要完成此操作，你可以让 `render` 方法直接返回 `null`，而不进行任何渲染。

> 下面的示例中，`<WarningBanner />` 会根据 prop 中 `warn` 的值来进行条件渲染。如果 `warn`的值是 `false`，那么组件则不会渲染:
>
> ```js
> function WarningBanner(props) {
>   if (!props.warn) {
>     return null;
>   }
> 
>   return (
>     <div className="warning">
>       Warning!
>     </div>
>   );
> }
> 
> class Page extends React.Component {
>   constructor(props) {
>     super(props);
>     this.state = {showWarning: true};
>     this.handleToggleClick = this.handleToggleClick.bind(this);
>   }
> 
>   handleToggleClick() {
>     this.setState(state => ({
>       showWarning: !state.showWarning
>     }));
>   }
> 
>   render() {
>     return (
>       <div>
>         <WarningBanner warn={this.state.showWarning} />
>         <button onClick={this.handleToggleClick}>
>           {this.state.showWarning ? 'Hide' : 'Show'}
>         </button>
>       </div>
>     );
>   }
> }
> 
> ReactDOM.render(
>   <Page />,
>   document.getElementById('root')
> );
> ```
>
> 在组件的 `render` 方法中返回 `null` 并不会影响组件的生命周期。例如，上面这个示例中，`componentDidUpdate` 依然会被调用。

> 第一次见这种写法和用法，
>
> 不过这个例子应该是用在函数组件里的，因为它是函数，所以它顺序执行，所以先返回null。
>
> 如果是class组件，那就不一样了，大概要写到生命周期里面吧。
>
> 阻止渲染，也挺好的，我先前的做法都是用三目运算符判断然后给空字符串  （判断?成立结果:""）
>
> 不过我是用在标签内部，这种是用在返回上面的，不一样。