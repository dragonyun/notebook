## aggrid功能大纲

- aggird是一个表格组件
- 功能丰富
- 现在用在我们公司的项目上，需要熟练掌握它的内容



---

### Getting Started 开始

#### 使用

​	npm可以直接安装

```js
// 安装普通版或者企业版
npm install --save ag-grid-community
npm install --save ag-grid-enterprise

// 然后就是调用，还需要引入css文件
// ECMA 5 - using nodes require() method
var AgGrid = require('ag-grid-enterprise');

// ECMA 6 - using the system import method
import {Grid} from 'ag-grid-enterprise';
```



#### react-aggrid

```jsx
// 还需要安装一个ag-grid-react
npm install --save ag-grid-community ag-grid-react

// 这样是为了在react中使用，下面就是调用里面对应的组件
import { AgGridReact } from 'ag-grid-react';
```



---

### Basic 基础

#### Interface / API

**接口分为** Gird接口（指aggrid接口）和列接口（Column）

**初始化grid之后，通过onGridReady等可以获取到grid实例和Column实例**



##### Grid Interface

- Grid Properties ：用于配置grid的属性

- Grid API：创建grid之后用于与网格进行交互的方法

  记录一个很常见的需求

  refreshCell(params) 不带参数即对所有单元格进行刷新，带参数指定单元格刷新

- Events：grid发布的事件，用于通知应用程序状态变化。

  事件就是发生的事件，比如单击，双击，拖拽等，在aggrid里面，cellClicked事件就是 单元格单击事件，对应的回调方法是onCellClicked。其实就类似button的onClick

- Grid Callbacks：回调方法可以从应用程序中检索所需的信息

  **这个和上面那个事件回调是不一样的。**

  它更多是在组件执行过程中，插入的用户的意图。比如发送请求之前的参数预处理方法，这些不是通过属性配置进来的，而是执行过程中动态获取到的。

- Row Node：网格中的每一行均由行节点对象表示，该对象又具有对应用程序提供行数据的引用。行节点包装行数据项。行节点具有用于与特定行进行交互的属性，方法和事件，例如rowNode.setSelected(true)



##### Column Interface

- Column Properties：通过列定义配置列。列定义包含列属性
- Column API：列API提供了与列相关的方法
- Column Object：aggrid中的每一列都由一个列对象表示，该对象又引用了应用程序提供的列定义。列包装列定义。Clolumn对象具有用于与特定列进行交互的属性，方法和事件



#### Columns



#### Rows



#### Build & Tooling



## 



### Core Features 核心特性

#### Layout & Styling



#### Server-Side Data



#### Client-Side Data



#### Server-Side Data



#### Selection



#### Filtering



#### Rendering



#### Editing



#### Group & Pivot



#### Import & Export

## 



### Extending the grid grid更多功能（企业版）

#### Accessories



#### Components



#### Integrated Charts



#### Standalone Charts



## 



### Other 其它

#### Scrolling



#### Interactivity



#### Testing & Security



## 