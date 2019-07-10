## react学习笔记之Fragments

> React 中的一个常见模式是一个组件返回多个元素。
>
> Fragments 允许你将子列表分组，而无需向 DOM 添加额外节点。

- 我认为它等价于vue里面的template，不会被渲染为标签，又可以去包装标签



### 场景

> 一种常见模式是组件返回一个子元素列表。以此 React 代码片段为例：
>
> ```js
> class Table extends React.Component {
>   render() {
>     return (
>       <table>
>         <tr>
>           <Columns />
>         </tr>
>       </table>
>     );
>   }
> }
> ```
>
> `<Columns />` 需要返回多个 `<td>` 元素以使渲染的 HTML 有效。如果在 `<Columns />` 的 `render()` 中使用了父 div，则生成的 HTML 将无效。
>
> ```js
> class Columns extends React.Component {
>   render() {
>     return (
>       <div>
>         <td>Hello</td>
>         <td>World</td>
>       </div>
>     );
>   }
> }
> ```
>
> 得到一个 `<Table />` 输出：
>
> ```js
> <table>
>   <tr>
>     <div>
>       <td>Hello</td>
>       <td>World</td>
>     </div>
>   </tr>
> </table>
> ```
>
> 可以很明显看出来，这样就不对了，tr标签包含了div。
>
> 所以：我们的需求是希望div不包含在里面，但是子组件又需要一个标签来包含多个元素。
>
> 然后就有了Fragments，用来解决这个问题。

### 用法

> ```js
> class Columns extends React.Component {
>   render() {
>     return (
>       <React.Fragment>
>         <td>Hello</td>
>         <td>World</td>
>       </React.Fragment>
>     );
>   }
> }
> ```
>
> 这样可以正确的输出 `<Table />`：
>
> ```js
> <table>
>   <tr>
>     <td>Hello</td>
>     <td>World</td>
>   </tr>
> </table>
> ```
>
> 这里的Fragment这样引入
>
> ```js
> import React from 'react'
> 然后就可以用<React.Fragment>
>     
> 或者
> import { Fragment } from 'react'
> 然后用<Fragment>
> ```

### 短语法

> Fragments的简短语法
>
> ```js
> class Columns extends React.Component {
>   render() {
>     return (
>       <>
>         <td>Hello</td>
>         <td>World</td>
>       </>
>     );
>   }
> }
> ```
>
> 不过它不支持key或属性；
>
> 注意：目前很多工具不支持该短语法，so，先别用。

### 带key的Fragments

> 使用显式 `<React.Fragment>` 语法声明的片段可能具有 key。一个使用场景是将一个集合映射到一个 Fragments 数组 - 举个例子，创建一个描述列表：
>
> ```js
> function Glossary(props) {
>   return (
>     <dl>
>       {props.items.map(item => (
>         // 没有`key`，React 会发出一个关键警告
>         <React.Fragment key={item.id}>
>           <dt>{item.term}</dt>
>           <dd>{item.description}</dd>
>         </React.Fragment>
>       ))}
>     </dl>
>   );
> }
> ```
>
> `key` 是唯一可以传递给 `Fragment` 的属性。

### 我的总结

> 和vue里面的template很相似，用过vue可以类比一下；
>
> 比较有用，用来包含不同的元素，替换div；
>
> 其本身不会渲染到html上面；