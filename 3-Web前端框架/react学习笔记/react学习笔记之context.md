## Context

**Context 提供了一个无需为每层组件手动添加 props，就能在组件树间进行数据传递的方法。**

> 在一个典型的 React 应用中，数据是通过 props 属性自上而下（由父及子）进行传递的，但这种做法对于某些类型的属性而言是极其繁琐的（例如：地区偏好，UI 主题），这些属性是应用程序中许多组件都需要的。Context 提供了一种在组件之间共享此类值的方式，而不必显式地通过组件树的逐层传递 props。



### 背景和场景

---

Context 为一个典型应用场景提供了解决方案，那就是**全局式**的props。

最直观的例子就是**地区偏好（语言）**，**UI主题（颜色风格）**，**组件尺寸（大小）**。

更倾向于**全局式**的使用场景，而不是作为几个或者几层组件的**数据共享**。



### API一览

---

- `React.createContext`
- `Context.Provider`
- `Class.contextType`
- `Context.Consumer`
- `Context.displayName`



### 基本使用规则

---

1. 首先通过`React.createContext`创建一个context对象。
2. 通过默认初值或者`Context.Provider`来控制这个全局变量。
3. 下级组件通过`Class.contextType`或者`Context.Consumer`获取该变量。
4. 根据变量重新渲染组件，触发相关逻辑。



### 注意点

----

1. `React.createContext`

   ```js
   const MyContext = React.createContext(defaultValue);
   ```

   这里是在创建一个Context对象。`defaultValue`是默认值，如果没有匹配到provider的话，就使用这个默认值。并且，这个默认值可以作为一个对象，包含多个子项。

   例如antd的样式前缀就是这样实现的。

2. `Context.Provider`

   ```js
   <MyContext.Provider value={/* 某个值 */}>
   ```

   从Context对象中可以得到provider组件和customer组件。

   使用上是如果不想使用默认值，可以用provider来变化这个数据。

   那么这个provider组件和customer组件是成对使用的，它们构成订阅发布模式。

3. `Class.contextType`

   在class组件中通过它接收context对象，然后在组件内可以使用`this.context`。

4. class和函数式组件

   函数式组件只能通过订阅的方式，而不能通过class属性获取context。

   class组件两种都可以实现。

5. 嵌套组件中的更新context

   就是在很深的组件中去更新顶层的context，很有必要

   ```js
   // 确保传递给 createContext 的默认值数据结构是调用的组件（consumers）所能匹配的！
   export const ThemeContext = React.createContext({
     theme: themes.dark,
     toggleTheme: () => {},
   });
   ```

   可以通过在创建时传递方法进去，让深处的组件直接调用修改context。

6. 有个坑，provider的重新渲染可能会触发customer的意外渲染

   下面这个例子里value属性总是会被赋值为新的对象，根据参考标识原则，会触发更新。

   ```js
   class App extends React.Component {
     render() {
       return (
         <MyContext.Provider value={{something: 'something'}}>
           <Toolbar />
         </MyContext.Provider>
       );
     }
   }
   // 为了防止这种情况，将 value 状态提升到父节点的 state 里：
   class App extends React.Component {
     constructor(props) {
       super(props);
       this.state = {
         value: {something: 'something'},
       };
     }
   
     render() {
       return (
         <Provider value={this.state.value}>
           <Toolbar />
         </Provider>
       );
     }
   }
   ```

   



### 参考文档

- 官方文档：`https://react.docschina.org/docs/context.html`