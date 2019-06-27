## `ES6`之Module语法

### 简介

- 目的：做文件拆分，把大型，复杂的项目拆分成相互依赖的小文件。方便管理，维护。

- 历史技术：
  - 模块加载方案（`CommonJS`）用于服务器端
  - 模块加载方案（`AMD`）用于浏览器
  - 现在（`ES6模块`）是服务器和浏览器的通用模块解决方案

- 特点与差异
  - 特点：尽量静态化， 编译时确定模块依赖关系，及输入输出变量
  - 差异：`CommonJS`和`AMD`是在运行时确定

- ES6加载的是代码不是对象

  > `ES6` 模块不是对象，而是通过`export`命令显式指定输出的代码，再通过`import`命令输入。
  >
  > ```js
  > // ES6模块
  > import { stat, exists, readFile } from 'fs';
  > ```
  >
  > 上面代码的实质是从`fs`模块加载 3 个方法，其他方法不加载。这种加载称为“编译时加载”或者静态加载，即 `ES6` 可以在编译时就完成模块加载，效率要比 `CommonJS` 模块的加载方式高。当然，这也导致了没法引用 `ES6` 模块本身，因为它不是对象。

- 优点
  - 静态加载
  - 不需要UMD模块格式了
  - 将来浏览器的新 `API` 就能用模块格式提供，不再必须做成全局变量或者`navigator`对象的属性
  - 不再需要对象作为命名空间（比如`Math`对象），未来这些功能可以通过模块提供。

- `ES6` 的模块自动采用严格模式，不管你有没有在模块头部加上`"use strict";`

### export

- 功能：用于规定模块对外的接口

- 一个模块就是一个独立文件。文件内的所有变量，外部无法访问，所以需要提供对外接口

- 举例：

  ```js
  // profile.js  写法一
  export var firstName = 'Michael';
  export var lastName = 'Jackson';
  export var year = 1958;
  
  // profile.js  写法二
  var firstName = 'Michael';
  var lastName = 'Jackson';
  var year = 1958;
  
  export { firstName, lastName, year };
  ```

  > 说明一下，文件`profile.js`是一个模块文件，保存了一些信息，通过export对外部输出了三个变量.
  >
  > 第二种写法更为常见，将需要输出的变量放在一起，比较清楚

  ---

  ```js
  // 输出方法
  export function multiply(x, y) {
    return x * y;
  };
  ```

  > `export`命令除了输出变量，还可以输出函数或类（class）

  ---

  ```js
  // 输出变量重命名
  function v1() { ... }
  function v2() { ... }
  
  export {
    v1 as streamV1,
    v2 as streamV2,
    v2 as streamLatestVersion
  };
  ```

  > `export`输出的变量就是本来的名字，但是可以使用`as`关键字重命名。
  >
  > 并且，重命名后，`v2`可以用不同的名字输出两次（多次重命名输出）。

  ---

- 写法规范：

  ```js
  // 报错
  export 1;
  
  // 报错
  var m = 1;
  export m;
  ```

  > 需要特别注意的是，`export`命令规定的是对外的接口，必须与模块内部的变量建立一一对应关系.
  >
  > 上面两种写法都会报错，因为没有提供对外的接口。
  >
  > 第一种写法直接输出 1，第二种写法通过变量`m`，还是直接输出 1。`1`只是一个值，不是接口。

  ```js
  // 正确写法
  // 写法一
  export var m = 1;
  
  // 写法二
  var m = 1;
  export {m};
  
  // 写法三
  var n = 1;
  export {n as m};
  ```

  > 上面三种写法都是正确的，规定了对外的接口`m`。其他脚本可以通过这个接口，取到值`1`。它们的实质是，在接口名与模块内部变量之间，建立了一一对应的关系。
  >
  > <font color="red">这个接口为什么要这样写，我还是不懂，等等再看看</font>

  ---

  ```js
  // 报错
  function f() {}
  export f;
  
  // 正确
  export function f() {};
  
  // 正确
  function f() {}
  export {f};
  ```

  > 同样的，`function`和`class`的输出，也必须遵守这样的写法。

- 动态绑定

  > 通过该接口，可以取到模块内部实时的值。

  ```js
  // 动态绑定，实时取值
  export var foo = 'bar';
  setTimeout(() => foo = 'baz', 500);
  ```

  > 上面代码输出变量`foo`，值为`bar`，500 毫秒之后变成`baz`。

- export只要处于顶层即可，可以出现在模块的任何位置。

  ```js
  // 报错
  function foo() {
    export default 'bar' // SyntaxError
  }
  foo()
  ```

  > export不可以放在块级作用域内，要放在顶层

  

### import

- 功能：通过import命令加载模块，和export配合使用

- 举例：

  ```js
  // 基本用法
  // main.js
  import { firstName, lastName, year } from './profile.js';
  
  function setName(element) {
    element.textContent = firstName + ' ' + lastName;
  }
  ```

  > 加载`profile.js`文件，并从中输入变量。
  >
  > `import`命令接受一对大括号，里面指定要从其他模块导入的变量名。
  >
  > 大括号里面的变量名，必须与被导入模块（`profile.js`）对外接口的名称相同。

  ---

  ```js
  // 重命名
  import { lastName as surname } from './profile.js';
  ```

  > `import`命令要使用`as`关键字，将输入的变量重命名。

  ---

  ```js
  // import输入的变量是只读的
  // 报错
  import {a} from './xxx.js'
  
  a = {}; // Syntax Error : 'a' is read-only;
  //正确
  import {a} from './xxx.js'
  
  a.foo = 'hello'; // 合法操作
  ```

  > 脚本加载了变量`a`，对其重新赋值就会报错，因为`a`是一个只读的接口。
  >
  > 但是，如果`a`是一个对象，改写`a`的属性是允许的。
  >
  > 上面代码中，`a`的属性可以成功改写，并且其他模块也可以读到改写后的值。
  >
  > 不过，这样的话出错很难查，建议完全作为只读。

  ---

  ```js
  // from 后面指定了模块的位置
  import {myMethod} from 'util';
  ```

  > 位置，可以是相对路径，也可以是绝对路径，`.js`后缀可以省略。
  >
  > 如果只是模块名，不带有路径，那么必须有配置文件，告诉 JavaScript 引擎该模块的位置。

  ---

  ```js
  // import 命令提升
  foo();
  
  import { foo } from 'my_module';
  ```

  > 会提升到整个模块的头部，首先执行。
  >
  > 本质是，`import`命令是编译阶段执行的，在代码运行之前。

  ---

  ```js
  // 由于import是静态执行，所以不能使用表达式和变量
  // 报错
  import { 'f' + 'oo' } from 'my_module';
  
  // 报错
  let module = 'my_module';
  import { foo } from module;
  
  // 报错
  if (x === 1) {
    import { foo } from 'module1';
  } else {
    import { foo } from 'module2';
  }
  ```

  > 由于`import`是静态执行，所以不能使用表达式和变量，这些只有在运行时才能得到结果的语法结构。
  >
  > 上面三种写法都会报错，因为它们用到了表达式、变量和`if`结构。在静态分析阶段，这些语法都是没法得到值的。

  ---

  ```js
  // 仅仅执行lodash模块，但是不输入任何值。
  import 'lodash';
  ```

  ---

  ```js
  // 不重复
  import 'lodash';
  import 'lodash';
  ```

  > 如果多次重复执行同一句`import`语句，那么只会执行一次，而不会执行多次。

- 模块整体加载

  > 除了指定加载某个输出值，还可以使用整体加载，即用星号（`*`）指定一个对象，所有输出值都加载在这个对象上面。

  ```js
  // circle.js
  export function area(radius) {
    return Math.PI * radius * radius;
  }
  export function circumference(radius) {
    return 2 * Math.PI * radius;
  }
  
  // main.js 加载circle.js模块（逐一指定）
  import { area, circumference } from './circle';
  
  console.log('圆面积：' + area(4));
  console.log('圆周长：' + circumference(14));
  
  // main.js 加载circle.js模块（整体加载）
  import * as circle from './circle';
  
  console.log('圆面积：' + circle.area(4));
  console.log('圆周长：' + circle.circumference(14));
  ```

### export default

- 功能：模块指定默认输出

  > 使用`import`命令的时候，用户需要知道所要加载的**变量名或函数名**，否则无法加载。
  >
  > 为了让用户能直接加载模块，就要用到`export default`命令，为模块指定默认输出。

- 举例：

  ```js
  // export-default.js
  export default function () {
    console.log('foo');
  }
  // 上面代码是一个模块文件export-default.js，它的默认输出是一个函数。
  
  // 其他模块加载该模块时，import命令可以为该匿名函数指定任意名字。
  // import-default.js
  import customName from './export-default';
  customName(); // 'foo'
  ```

  > 这样我们就可以用任意名称指向模块输出的方法，无需关心原模块中输出的函数名。
  >
  > 注意：通过这种方式我们就不再使用大括号。

  ---

  ```js
  // 使用在非匿名函数中
  // export-default.js
  export default function foo() {
    console.log('foo');
  }
  
  // 或者写成
  function foo() {
    console.log('foo');
  }
  export default foo;
  ```

  > 上面代码中，`foo`函数的函数名`foo`，在模块外部是无效的。加载的时候，视同匿名函数加载。

- 默认输出和正常输出的比较

  ```js
  // 第一组
  export default function crc32() { // 输出
    // ...
  }
  import crc32 from 'crc32'; // 输入
  
  // 第二组
  export function crc32() { // 输出
    // ...
  };
  import {crc32} from 'crc32'; // 输入
  ```

  > 第一组是使用`export default`时，对应的`import`语句不需要使用大括号；
  >
  > 第二组是不使用`export default`时，对应的`import`语句需要使用大括号。

  > `export default`命令用于指定模块的默认输出。
  >
  > 显然，一个模块只能有一个默认输出，因此`export default`命令只能使用一次。
  >
  > 所以，import命令后面才不用加大括号，因为只可能唯一对应`export default`命令。

- 换一种理解方式

  > 本质上，`export default`就是输出一个叫做`default`的变量或方法，然后系统允许你为它取任意名字。

  ```js
  // modules.js
  function add(x, y) {
    return x * y;
  }
  export {add as default};
  // 等同于
  // export default add;
  
  // app.js
  import { default as foo } from 'modules';
  // 等同于
  // import foo from 'modules';
  ```

  > 正是因为`export default`命令其实只是输出一个叫做`default`的变量，
  >
  > 所以它后面不能跟变量声明语句。

  ```js
  // 正确
  export var a = 1;
  
  //正确
  var a = 1；
  export { a }；
  
  // 正确
  var a = 1;
  export default a;// 等同于 export {a as default}
  
  // 错误
  export default var a = 1;
  ```

  > 上面代码中，`export default a`的含义是将变量`a`的值赋给变量`default`。
  >
  > 所以，最后一种写法会报错。

  ---

  ```js
  // 正确
  export default 42;
  
  // 报错
  export 42;
  ```

  > 因为`export default`命令的本质是将后面的值，赋给`default`变量，所以可以直接将一个值写在`export default`之后。
  >
  > 上面代码中，后一句报错是因为没有指定对外的接口，而前一句指定对外接口为`default`。

- 混合输出和输入

  > 在一条`import`语句中，同时输入默认方法和其他接口

  ```js
  // lodash.js
  export default function (obj) {
    // ···
  }
  
  export function each(obj, iterator, context) {
    // ···
  }
  
  export { each as forEach };
  
  // main.js
  import _, { each, forEach } from 'lodash';
  ```

  > 同一个方法可以用不同接口输出

- 可以用来输出类

  ```js
  // MyClass.js
  export default class { ... }
  
  // main.js
  import MyClass from 'MyClass';
  let o = new MyClass();
  ```

### export 与 import 复合写法

- 功能：在一个模块之中，先输入后输出同一个模块

- 举例：

  ```js
  export { foo, bar } from 'my_module';
  
  // 可以简单理解为
  import { foo, bar } from 'my_module';
  export { foo, bar };
  ```

  > 但是：当前模块不能用`foo`和`bar`这两个接口。
  >
  > 写成一行以后，`foo`和`bar`实际上并没有被导入当前模块，只是相当于对外转发了这两个接口，导致当前模块不能直接使用`foo`和`bar`。

  ---

  ```js
  // 接口改名
  export { foo as myFoo } from 'my_module';
  
  // 整体输出
  export * from 'my_module';
  
  // 默认接口
  export { default } from 'foo';
  
  // 具名接口改为默认接口
  export { es6 as default } from './someModule';
  // 等同于
  import { es6 } from './someModule';
  export default es6;
  
  // 默认接口也可以改名为具名接口
  export { default as es6 } from './someModule';
  ```

### 模块继承

- 功能：就是模块之间的继承

- 举例：

  ```js
  // 假设有一个circleplus模块，继承了circle模块。
  // circleplus.js
  export * from 'circle';
  export var e = 2.71828182846;
  export default function(x) {
    return Math.exp(x);
  }
  ```

  > 上面代码中的`export *`，表示再输出`circle`模块的所有属性和方法。
  >
  > 注意，`export *`命令会忽略`circle`模块的`default`方法。
  >
  > 然后，上面代码又输出了自定义的`e`变量和默认方法。

  ```js
  // 继承时改名
  // circleplus.js
  export { area as circleArea } from 'circle';
  ```

  ```js
  // 加载上面模块的写法如下
  // main.js
  import * as math from 'circleplus';
  import exp from 'circleplus';
  console.log(exp(math.e));
  ```

  > 上面代码中的`import exp`表示，将`circleplus`模块的默认方法加载为`exp`方法。

### 跨模块常量

- 功能：设置跨模块的常量，或者说一个值要被多个模块共享

- 举例：

  ```js
  // constants.js 模块
  export const A = 1;
  export const B = 3;
  export const C = 4;
  
  // test1.js 模块
  import * as constants from './constants';
  console.log(constants.A); // 1
  console.log(constants.B); // 3
  
  // test2.js 模块
  import {A, B} from './constants';
  console.log(A); // 1
  console.log(B); // 3
  ```

- 常量过多的推荐做法

  > 如果要使用的常量非常多，可以建一个专门的`constants`目录，将各种常量写在不同的文件里面，保存在该目录下。

  ```js
  // constants/db.js
  export const db = {
    url: 'http://my.couchdbserver.local:5984',
    admin_username: 'admin',
    admin_password: 'admin password'
  };
  
  // constants/user.js
  export const users = ['root', 'admin', 'staff', 'ceo', 'chief', 'moderator'];
  ```

  > 然后，将这些文件输出的常量，合并在`index.js`里面。

  ```js
  // constants/index.js
  export {db} from './db';
  export {users} from './users';
  ```

  > 使用的时候，直接加载`index.js`就可以了。

  ```js
  // script.js
  import {db, users} from './constants/index';
  ```

### import()

- 功能：运行时加载模块
- 举例
- 场合
  - 按需加载
  - 条件加载
  - 动态的模块路径