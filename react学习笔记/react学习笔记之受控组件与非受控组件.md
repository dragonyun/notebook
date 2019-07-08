## React学习笔记之受控组件与非受控组件

> 受控组件中，表单数据是由 React 组件来管理的。
>
> 非受控组件，这时表单数据将交由 DOM 节点来处理。

### 受控组件

> 在 HTML 中，表单元素（如`<input>`、 `<textarea>` 和 `<select>`）之类的表单元素通常自己维护 state，并根据用户输入进行更新。而在 React 中，可变状态（mutable state）通常保存在组件的 state 属性中，并且只能通过使用 [`setState()`](https://react.docschina.org/docs/react-component.html#setstate)来更新。

> 就是说在HTML里面，表单元素比较特殊，能自己维护元素本身的state。
>
> ```js
> <form>
>   <label>
>     名字:
>     <input type="text" name="name" />
>   </label>
>   <input type="submit" value="提交" />
> </form>
> ```

> 我们可以把两者结合起来，使 React 的 state 成为“唯一数据源”。渲染表单的 React 组件还控制着用户输入过程中表单发生的操作。被 React 以这种方式控制取值的表单输入元素就叫做“受控组件”。
>
> 下面的例子就是一个受控组件
>
> ```js
> class NameForm extends React.Component {
>   constructor(props) {
>     super(props);
>     this.state = {value: ''};
> 
>     this.handleChange = this.handleChange.bind(this);
>     this.handleSubmit = this.handleSubmit.bind(this);
>   }
> 
>   handleChange(event) {
>     this.setState({value: event.target.value});
>   }
> 
>   handleSubmit(event) {
>     alert('提交的名字: ' + this.state.value);
>     event.preventDefault();
>   }
> 
>   render() {
>     return (
>       <form onSubmit={this.handleSubmit}>
>         <label>
>           名字:
>           <input type="text" value={this.state.value} onChange={this.handleChange} />
>         </label>
>         <input type="submit" value="提交" />
>       </form>
>     );
>   }
> }
> ```
>
> 由于在表单元素上设置了 `value` 属性，因此显示的值将始终为 `this.state.value`，这使得 React 的 state 成为唯一数据源。由于 `handlechange` 在每次按键时都会执行并更新 React 的 state，因此显示的值将随着用户输入而更新。
>
> 当然，`handleChange`函数也可以根据你的意愿来修改，执行对应的操作。

> - ## textarea 标签
>
> 在 HTML 中, `<textarea>` 元素通过其子元素定义其文本:
>
> ```js
> <textarea>
>   你好， 这是在 text area 里的文本
> </textarea>
> ```
>
> 而在 React 中，`<textarea>` 使用 `value` 属性代替。这样，可以使得使用 `<textarea>` 的表单和使用单行 input 的表单非常类似：
>
> ```js
> class EssayForm extends React.Component {
>   constructor(props) {
>     super(props);
>     this.state = {
>       value: '请撰写一篇关于你喜欢的 DOM 元素的文章.'
>     };
> 
>     this.handleChange = this.handleChange.bind(this);
>     this.handleSubmit = this.handleSubmit.bind(this);
>   }
> 
>   handleChange(event) {
>     this.setState({value: event.target.value});
>   }
> 
>   handleSubmit(event) {
>     alert('提交的文章: ' + this.state.value);
>     event.preventDefault();
>   }
> 
>   render() {
>     return (
>       <form onSubmit={this.handleSubmit}>
>         <label>
>           文章:
>           <textarea value={this.state.value} onChange={this.handleChange} />
>         </label>
>         <input type="submit" value="提交" />
>       </form>
>     );
>   }
> }
> ```
>
> 请注意，`this.state.value` 初始化于构造函数中，因此文本区域默认有初值。
>
> **这，也没什么好说的，都是相同的配方，都是相同的效果。**

> - ## select 标签
>
> 在 HTML 中，`<select>` 创建下拉列表标签。例如，如下 HTML 创建了水果相关的下拉列表：
>
> ```js
> <select>
>   <option value="grapefruit">葡萄柚</option>
>   <option value="lime">柠檬</option>
>   <option selected value="coconut">椰子</option>
>   <option value="mango">芒果</option>
> </select>
> ```
>
> 请注意，由于 `selected` 属性的缘故，椰子选项默认被选中。React 并不会使用 `selected` 属性，而是在根 `select` 标签上使用 `value` 属性。这在受控组件中更便捷，因为您只需要在根标签中更新它。例如：
>
> ```js
> class FlavorForm extends React.Component {
>   constructor(props) {
>     super(props);
>     this.state = {value: 'coconut'};
> 
>     this.handleChange = this.handleChange.bind(this);
>     this.handleSubmit = this.handleSubmit.bind(this);
>   }
> 
>   handleChange(event) {
>     this.setState({value: event.target.value});
>   }
> 
>   handleSubmit(event) {
>     alert('你喜欢的风味是: ' + this.state.value);
>     event.preventDefault();
>   }
> 
>   render() {
>     return (
>       <form onSubmit={this.handleSubmit}>
>         <label>
>           选择你喜欢的风味:
>           <select value={this.state.value} onChange={this.handleChange}>
>             <option value="grapefruit">葡萄柚</option>
>             <option value="lime">柠檬</option>
>             <option value="coconut">椰子</option>
>             <option value="mango">芒果</option>
>           </select>
>         </label>
>         <input type="submit" value="提交" />
>       </form>
>     );
>   }
> }
> ```
>
> 总的来说，这使得 `<input type="text">`, `<textarea>` 和 `<select>` 之类的标签都非常相似—它们都接受一个 `value` 属性，你可以使用它来实现受控组件。

- ## 处理多个输入

> 这个很重要，就是一个组件里面用了多个同类的标签，它们能复用同一个函数。

> 当需要处理多个 `input` 元素时，我们可以给每个元素添加 `name` 属性，并让处理函数根据 `event.target.name` 的值选择要执行的操作。
>
> 例如：
>
> ```js
> class Reservation extends React.Component {
>   constructor(props) {
>     super(props);
>     this.state = {
>       isGoing: true,
>       numberOfGuests: 2
>     };
> 
>     this.handleInputChange = this.handleInputChange.bind(this);
>   }
> 
>   handleInputChange(event) {
>     const target = event.target;
>     const value = target.type === 'checkbox' ? target.checked : target.value;
>     const name = target.name;
> 
>     this.setState({
>       [name]: value
>     });
>   }
> 
>   render() {
>     return (
>       <form>
>         <label>
>           参与:
>           <input
>             name="isGoing"
>             type="checkbox"
>             checked={this.state.isGoing}
>             onChange={this.handleInputChange} />
>         </label>
>         <br />
>         <label>
>           来宾人数:
>           <input
>             name="numberOfGuests"
>             type="number"
>             value={this.state.numberOfGuests}
>             onChange={this.handleInputChange} />
>         </label>
>       </form>
>     );
>   }
> }
> ```
>
> 这里使用了 ES6 [计算属性名称](https://developer.mozilla.org/en/docs/Web/JavaScript/Reference/Operators/Object_initializer#Computed_property_names)的语法更新给定输入名称对应的 state 值：
>
> ```js
> this.setState({
>   [name]: value
> });
> 
> // 等同于ES5
> var partialState = {};
> partialState[name] = value;
> this.setState(partialState);
> ```

- ## 受控输入空值

> 在[受控组件](https://react.docschina.org/docs/forms.html#controlled-components)上指定 value 的 prop 可以防止用户更改输入。如果指定了 `value`，但输入仍可编辑，则可能是意外地将`value` 设置为 `undefined` 或 `null`。
>
> ```js
> ReactDOM.render(<input value="hi" />, mountNode);
> 
> setTimeout(function() {
>   ReactDOM.render(<input value={null} />, mountNode);
> }, 1000);
> ```
>
> 

---

### 非受控组件

> 非受控组件，这时表单数据将交由 DOM 节点来处理。
>
> 要编写一个非受控组件，而不是为每个状态更新都编写数据处理函数，你可以 [使用 ref](https://react.docschina.org/docs/refs-and-the-dom.html) 来从 DOM 节点中获取表单数据。
>
> 例如，下面的代码使用非受控组件接受一个表单的值:
>
> ```js
> class NameForm extends React.Component {
>   constructor(props) {
>     super(props);
>     this.handleSubmit = this.handleSubmit.bind(this);
>     this.input = React.createRef();
>   }
> 
>   handleSubmit(event) {
>     alert('A name was submitted: ' + this.input.current.value);
>     event.preventDefault();
>   }
> 
>   render() {
>     return (
>       <form onSubmit={this.handleSubmit}>
>         <label>
>           Name:
>           <input type="text" ref={this.input} />
>         </label>
>         <input type="submit" value="Submit" />
>       </form>
>     );
>   }
> }
> ```
>
> 因为非受控组件将真实数据储存在 DOM 节点中，所以再使用非受控组件时，有时候反而更容易同时集成 React 和非 React 代码。如果你不介意代码美观性，并且希望快速编写代码，使用非受控组件往往可以减少你的代码量。否则，你应该使用受控组件。

- ### 默认值

> 在 React 渲染生命周期时，表单元素上的 `value` 将会覆盖 DOM 节点中的值，在非受控组件中，你经常希望 React 能赋予组件一个初始值，但是不去控制后续的更新。 在这种情况下, 你可以指定一个 `defaultValue` 属性，而不是 `value`。
>
> ```js
> render() {
>   return (
>     <form onSubmit={this.handleSubmit}>
>       <label>
>         Name:
>         <input
>           defaultValue="Bob"
>           type="text"
>           ref={this.input} />
>       </label>
>       <input type="submit" value="Submit" />
>     </form>
>   );
> }
> ```
>
> 同样，`<input type="checkbox">` 和 `<input type="radio">` 支持 `defaultChecked`，`<select>` 和 `<textarea>` 支持 `defaultValue`。

- ## 文件输入

> 在 HTML 中，`<input type="file">` 可以让用户选择一个或多个文件上传到服务器，或者通过使用 [File API](https://developer.mozilla.org/en-US/docs/Web/API/File/Using_files_from_web_applications) 进行操作。
>
> ```js
> <input type="file" />
> ```
>
> 在 React 中，`<input type="file" />` 始终是一个非受控组件，因为它的值只能由用户设置，而不能通过代码控制。
>
> 您应该使用 File API 与文件进行交互。下面的例子显示了如何创建一个 [DOM 节点的 ref](https://react.docschina.org/docs/refs-and-the-dom.html) 从而在提交表单时获取文件的信息。
>
> ```js
> class FileInput extends React.Component {
>   constructor(props) {
>     super(props);
>     this.handleSubmit = this.handleSubmit.bind(this);
>     this.fileInput = React.createRef();
>   }
>   handleSubmit(event) {
>     event.preventDefault();
>     alert(
>       `Selected file - ${
>         this.fileInput.current.files[0].name
>       }`
>     );
>   }
> 
>   render() {
>     return (
>       <form onSubmit={this.handleSubmit}>
>         <label>
>           Upload file:
>           <input type="file" ref={this.fileInput} />
>         </label>
>         <br />
>         <button type="submit">Submit</button>
>       </form>
>     );
>   }
> }
> 
> ReactDOM.render(
>   <FileInput />,
>   document.getElementById('root')
> );
> ```
>
> 

