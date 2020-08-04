## react学习笔记之Refs and DOM



### 来源

---

官方文档是这样说的

> Refs 提供了一种方式，允许我们访问 DOM 节点或在 render 方法中创建的 React 元素。

我的认识是：

现在父子组件的数据交互，主要通过**props或者回调方法**。

修改props可以看做状态转换，0变成1,0和1两种状态的切换，回调是某一状态（0变1）变化的时刻，类似电子专业**上升沿**或者**下降沿**的概念。

缺少**父组件对子组件一次性的触发动作**。比如父组件控制子组件管理焦点，媒体播放，强制执行子组件的一个具体方法。这些都是一次性操作，不代表状态的切换，比如visible，可见不可见是状态的切换。

那么ref就是这样一个方式，可以指向React组件的实例，或者DOM元素，进而控制它。

> 这个还是很有用的，在组件的使用过程中，属性和方法可以满足大部分的需求，antd也是如此。
>
> 但是主动控制组件，比如手动触发一次刷新，手动指定某一个特定的内容变更，比如aggrid。
>
> 可以认为ref是对现有功能的补充。



### 使用场景

---

首先需要注意，ref可以用在 **React组件的实例，或者DOM元素** 上。

- 管理焦点，文本选择或媒体播放。
- 触发强制动画。
- 集成第三方 DOM 库。

以上是官方说明

还有就是

- 调用组件的内部方法
- 对DOM的操作

> 用在React组件实例上，当然就是对组件的操作，调用内部方法，访问内部属性内容等等。
>
> 用在DOM元素上，聚焦，DOM操作等等。



### 创建方式

---

创建方式有三种，是为了适用不同的场景

#### 1.React.createRef

Refs 是使用 `React.createRef()` 创建的，并通过 `ref` 属性附加到 React 元素。

```js
class MyComponent extends React.Component {
  constructor(props) {
    super(props);
    this.myRef = React.createRef();
  }
  render() {
    return <div ref={this.myRef} />;
  }
}
```

> 本质上是创建了一个对象，一个方便React源代码识别的对象。
>
> 最好用这个，也方便React底层处理相关逻辑（看过源码之后的感触）



#### 2.回调Ref

React 也支持另一种设置 refs 的方式，称为“回调 refs”。它能助你更精细地控制何时 refs 被设置和解除。

不同于传递 `createRef()` 创建的 `ref` 属性，你会传递一个函数。这个函数中接受 React 组件实例或 HTML DOM 元素作为参数，以使它们能在其他地方被存储和访问。

```js
class CustomTextInput extends React.Component {
  constructor(props) {
    super(props);

    this.textInput = null;

    this.setTextInputRef = element => {
      this.textInput = element;
    };

    this.focusTextInput = () => {
      // 使用原生 DOM API 使 text 输入框获得焦点
      if (this.textInput) this.textInput.focus();
    };
  }

  componentDidMount() {
    // 组件挂载后，让文本框自动获得焦点
    this.focusTextInput();
  }

  render() {
    // 使用 `ref` 的回调函数将 text 输入框 DOM 节点的引用存储到 React
    // 实例上（比如 this.textInput）
    return (
      <div>
        <input
          type="text"
          ref={this.setTextInputRef}
        />
        <input
          type="button"
          value="Focus the text input"
          onClick={this.focusTextInput}
        />
      </div>
    );
  }
}
```

> 本质上是创建了一个方法，一个方便React源代码识别的function。
>
> React底层能方便识别就是第一种里的object和第二种里的function，视使用场景使用。



#### 3.string类型的Refs（即将被废弃）

给ref赋值字符串，通过this.refs 再访问的方式

```js
class MyComponent extends React.Component {
  
  componentDidMount() {
      setTimeout(()=>{
          this.refs.stringRef.textContent = "string ref"
      }, 1000)
  }
  render() {
    return <div ref="stringRef">string1</div>
  }
}
```

> 这种用法有三个问题
>
> 1、影响React性能，因为要跟踪组件状态
>
> 2、无法通过回调方式使用
>
> 3、无法组合。如果库在传递的子组件上放了ref，那么用户不能在其上面放另一个ref
>
> 总之，问题很多，马上废弃了，别用。



### 访问 Refs

当 ref 被传递给 `render` 中的元素时，对该节点的引用可以在 ref 的 `current` 属性中被访问。

```js
const node = this.myRef.current;
```

ref 的值根据节点的类型而有所不同：

- 当 `ref` 属性用于 HTML 元素时，构造函数中使用 `React.createRef()` 创建的 `ref` 接收底层 DOM 元素作为其 `current` 属性。
- 当 `ref` 属性用于自定义 class 组件时，`ref` 对象接收组件的挂载实例作为其 `current` 属性。
- **你不能在函数组件上使用 ref 属性**，因为他们没有实例。

也可以换一个角度看

- 使用createRef创建，那么通过current属性获取实例或者DOM元素。
- 使用回调Ref创建，那么参数即实例或者DOM元素。
- 函数组件上不能使用ref，函数组件内可以，用useRef





