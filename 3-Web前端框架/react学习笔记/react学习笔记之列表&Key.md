## react学习笔记之列表&Key

### 渲染多个组件

> `map（）`函数帮助我们完成对数组的处理

> ```js
> const numbers = [1, 2, 3, 4, 5];
> const doubled = numbers.map((number) => number * 2);
> console.log(doubled);
> ```
>
> 在`React`中，把数组转化为元素列表的过程是相似的。

> 可以通过使用`{}`在JSX内构建一个元素集合。

> 例子如下，我们使用 Javascript 中的 [`map()`](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Global_Objects/Array/map) 方法来遍历 `numbers` 数组。将数组中的每个元素变成 `<li>` 标签，最后我们将得到的数组赋值给 `listItems`：
>
> ```js
> const numbers = [1, 2, 3, 4, 5];
> const listItems = numbers.map((number) =>
>   <li>{number}</li>
> );
> ```
>
> 我们把整个 `listItems` 插入到 `<ul>` 元素中，然后[渲染进 DOM](https://react.docschina.org/docs/rendering-elements.html#rendering-an-element-into-the-dom)：
>
> ```js
> ReactDOM.render(
>   <ul>{listItems}</ul>,
>   document.getElementById('root')
> );
> ```
>
> 就是说，通过js数组自带map函数可以放在React中用来渲染一个列表；
>
> 上面的代码生成了一个1到5的项目符号列表。

### key

> 列表元素需要分配一个key属性，否则会出现警告。

> key 帮助 React 识别哪些元素改变了，比如被添加或删除。因此你应当给数组中的每一个元素赋予一个确定的标识。
>
> ```js
> const numbers = [1, 2, 3, 4, 5];
> const listItems = numbers.map((number) =>
>   <li key={number.toString()}>
>     {number}
>   </li>
> );
> ```
>
> 一个元素的 key 最好是这个元素在列表中拥有的一个独一无二的字符串。通常，我们使用来自数据 id 来作为元素的 key：
>
> ```js
> const todoItems = todos.map((todo) =>
>   <li key={todo.id}>
>     {todo.text}
>   </li>
> );
> ```
>
> 当元素没有确定 id 的时候，万不得已你可以使用元素索引 index 作为 key：
>
> ```js
> const todoItems = todos.map((todo, index) =>
>   // Only do this if items have no stable IDs
>   <li key={index}>
>     {todo.text}
>   </li>
> );
> ```
>
> 如果列表项目的顺序可能会变化，我们不建议使用索引来用作 key 值，因为这样做会导致性能变差，还可能引起组件状态的问题。如果你选择不指定显式的 key 值，那么 React 将默认使用索引用作为列表项目的 key 值。

### 用 key 提取组件

> 元素的 key 只有放在就近的数组上下文中才有意义。
>
> 比方说，如果你[提取](https://react.docschina.org/docs/components-and-props.html#extracting-components) 出一个 `ListItem` 组件，你应该把 key 保留在数组中的这个 `<ListItem />` 元素上，而不是放在 `ListItem` 组件中的 `<li>` 元素上。
>
> ```js
> function ListItem(props) {
>   // 正确！这里不需要指定 key：
>   return <li>{props.value}</li>;
> }
> 
> function NumberList(props) {
>   const numbers = props.numbers;
>   const listItems = numbers.map((number) =>
>     // 正确！key 应该在数组的上下文中被指定
>     <ListItem key={number.toString()}
>               value={number} />
> 
>   );
>   return (
>     <ul>
>       {listItems}
>     </ul>
>   );
> }
> 
> const numbers = [1, 2, 3, 4, 5];
> ReactDOM.render(
>   <NumberList numbers={numbers} />,
>   document.getElementById('root')
> );
> ```
>
> 一个好的经验法则是：在 `map()` 方法中的元素需要设置 key 属性。
>
> 可以理解，因为如果你把key放在子组件内部，那如果上一级不做map，那还有什么意义

### key 只是在兄弟节点之间必须唯一

> 数组元素中使用的 key 在其兄弟节点之间应该是独一无二的。
>
> 然而，它们不需要是全局唯一的。
>
> 当我们生成两个不同的数组时，我们可以使用相同的 key 值。
>
> **这个其实很重要**
>
> 我们可以通过修改key触发重新渲染，而子组件不能读到key，这个可以用来做组件的初始化操作，就是改key。
>
> react会识别key，你也不用保证它全局唯一。

> key 会传递信息给 React ，但不会传递给你的组件。如果你的组件中需要使用 `key` 属性的值，请用其他属性名显式传递这个值：
>
> ```js
> const content = posts.map((post) =>
>   <Post
>     key={post.id}
>     id={post.id}
>     title={post.title} />
> );
> ```
>
> 上面例子中，`Post` 组件可以读出 `props.id`，但是不能读出 `props.key`。

### 在 JSX 中嵌入 map()

> 在上面的例子中，我们声明了一个单独的 `listItems` 变量并将其包含在 JSX 中：
>
> ```js
> function NumberList(props) {
>   const numbers = props.numbers;
>   const listItems = numbers.map((number) =>
>     <ListItem key={number.toString()}
>               value={number} />
> 
>   );
>   return (
>     <ul>
>       {listItems}
>     </ul>
>   );
> }
> ```
>
> JSX 允许在大括号中[嵌入任何表达式](https://react.docschina.org/docs/introducing-jsx.html#embedding-expressions-in-jsx)，所以我们可以内联 `map()` 返回的结果：
>
> ```js
> function NumberList(props) {
>   const numbers = props.numbers;
>   return (
>     <ul>
>       {numbers.map((number) =>
>         <ListItem key={number.toString()}
>                   value={number} />
> 
>       )}
>     </ul>
>   );
> }
> ```
>
> 其实我平时用这种写法还挺多的

### 有key的非可控组件

> 这里关于key有一个很牛的做法
>
> 场景是这样的：我们希望在改变用户的时候把某个组件整体初始化掉
>
> ```react
> class EmailInput extends Component {
>   state = { email: this.props.defaultEmail };
> 
>   handleChange = event => {
>     this.setState({ email: event.target.value });
>   };
> 
>   render() {
>     return <input onChange={this.handleChange} value={this.state.email} />;
>   }
> }
> ```
>
> 就是让组件自己存储临时的 email state。在这种情况下，组件仍然可以从 prop 接收“初始值”，但是更改之后的值就和 prop 没关系了，因为你更新了一个新的组件，就是因为用了key。
>
> ```react
> <EmailInput
>   defaultEmail={this.props.user.email}
>   key={this.props.user.id}
> />
> ```
>
> 为了在不同的页面切换不同的值，我们可以使用 `key` 这个特殊的 React 属性。当 `key` 变化时， React 会[创建一个新的而不是更新一个既有的组件](https://react-1251415695.cos-website.ap-chengdu.myqcloud.com/docs/reconciliation.html#keys)。 Keys 一般用来渲染动态列表，但是这里也可以使用。在这个示例里，当用户输入时，我们使用 user ID 当作 key 重新创建一个新的 email input 组件。
>
> ---
>
> 每次 ID 更改，都会重新创建 `EmailInput` ，并将其状态重置为最新的 `defaultEmail` 值。
>
> 使用此方法，不用为每次输入都添加 `key`，在整个表单上添加 `key` 更有位合理。每次 key 变化，表单里的所有组件都会用新的初始值重新创建。
>
> 大部分情况下，这是处理重置 state 的最好的办法。