## react学习笔记之组合继承

### 组合

> React 有十分强大的组合模式。我们推荐使用组合而非继承来实现组件间的代码重用。

> 讲道理，一开始没把组合当回事，现在再看一遍，发现有很多有用的地方是我之前没有注意到的。

> ## 包含关系
>
> 有些组件无法提前知晓它们子组件的具体内容。在 `Sidebar`（侧边栏）和 `Dialog`（对话框）等展现通用容器（box）的组件中特别容易遇到这种情况。
>
> 我们建议这些组件使用一个特殊的 `children` prop 来将他们的子组件传递到渲染结果中：
>
> ```jsx
> function FancyBorder(props) {
>   return (
>     <div className={'FancyBorder FancyBorder-' + props.color}>
>       {props.children}
>     </div>
>   );
> }
> ```
>
> 这使得别的组件可以通过 JSX 嵌套，将任意组件作为子组件传递给它们。
>
> ```jsx
> function WelcomeDialog() {
>   return (
>     <FancyBorder color="blue">
>       <h1 className="Dialog-title">
>         Welcome
>       </h1>
>       <p className="Dialog-message">
>         Thank you for visiting our spacecraft!
>       </p>
>     </FancyBorder>
>   );
> }
> ```
>
> `<FancyBorder>` JSX 标签中的所有内容都会作为一个 `children` prop 传递给 `FancyBorder` 组件。因为 `FancyBorder` 将 `{props.children}` 渲染在一个 `<div>` 中，被传递的这些子组件最终都会出现在输出结果中。
>
> 少数情况下，你可能需要在一个组件中预留出几个“洞”。这种情况下，我们可以不使用 `children`，而是自行约定：将所需内容传入 props，并使用相应的 prop。
>
> ```jsx
> function SplitPane(props) {
>   return (
>     <div className="SplitPane">
>       <div className="SplitPane-left">
>         {props.left}
>       </div>
>       <div className="SplitPane-right">
>         {props.right}
>       </div>
>     </div>
>   );
> }
> 
> function App() {
>   return (
>     <SplitPane
>       left={
>         <Contacts />
>       }
>       right={
>         <Chat />
>       } />
>   );
> }
> ```
>
> `<Contacts />` 和 `<Chat />` 之类的 React 元素本质就是对象（object），所以你可以把它们当作 props，像其他数据一样传递。这种方法可能使你想起别的库中“槽”（slot）的概念，但在 React 中没有“槽”这一概念的限制，你可以将任何东西作为 props 进行传递。

#### 总结1

> 以前没把这个事情当回事，这里有一个平时我没注意到地方。
>
> 组件或者标签中间是可以包含内容的，我平时写的组件都是直接用起来，并没有让使用者自由的在中间包含内容，就像div这个标签一样，它中间你可以随便添加内容，我们用react写组件也是一样的道理。
>
> {props.children}
>
> 这个就是组件中间包含的内容，我知道react可以传值、函数、组件等内容，但是我之前没有考虑过react组件标签  中间包含的部分  应该怎么带进组件里面，之前只是想放在属性里面，不过也算是属性，以后可以留意一下了。

### 继承

> **官方不推荐继承**
>
> 在 Facebook，我们在成百上千个组件中使用 React。我们并没有发现需要使用继承来构建组件层次的情况。
>
> Props 和组合为你提供了清晰而安全地定制组件外观和行为的灵活方式。注意：组件可以接受任意 props，包括基本数据类型，React 元素以及函数。
>
> 如果你想要在组件间复用非 UI 的功能，我们建议将其提取为一个单独的 JavaScript 模块，如函数、对象或者类。组件可以直接引入（import）而无需通过 extend 继承它们。