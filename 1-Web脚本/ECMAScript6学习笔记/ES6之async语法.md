## ES6之async语法

> Node.js 是一个异步的世界，官方 API 支持的都是 callback 形式的异步编程模型，这会带来许多问题，例如
>
> - [callback hell](http://callbackhell.com/): 最臭名昭著的 callback 嵌套问题。
> - [release zalgo](https://oren.github.io/#/articles/zalgo/): 异步函数中可能同步调用 callback 返回数据，带来不一致性。
>
> 因此社区提供了各种异步的解决方案，最终胜出的是 Promise，它也内置到了 ECMAScript 2015 中。而在 Promise 的基础上，结合 Generator 提供的切换上下文能力，出现了 [co](https://github.com/tj/co) 等第三方类库来让我们用同步写法编写异步代码。同时，[async function](https://github.com/tc39/ecmascript-asyncawait) 这个官方解决方案也于 ECMAScript 2017 中发布，并在 Node.js 8 中实现。
>
> ---
>
> 以上是egg框架提到的异步问题的解决方案解释。
>
> 简单说，大概是三个阶段，首先是promise，我看过，没用过；
>
> 然后是generator，就是*和yield，在非标项目里用过，很初级；
>
> 到最后是用async处理异步问题，async和await，高级语法糖。



---

### 含义

> ES2017标准引入了async函数，异步操作更方便了。
>
> 一句话解释，它就是generator函数的语法糖。
>
> 先看一个例子
>
> ```js
> const fs = require('fs');
> 
> const readFile = function (fileName) {
> return new Promise(function (resolve, reject) {
>  fs.readFile(fileName, function(error, data) {
>    if (error) return reject(error);
>    resolve(data);
>  });
> });
> };
> 
> /// generator的写法
> const gen = function* () {
> const f1 = yield readFile('/etc/fstab');
> const f2 = yield readFile('/etc/shells');
> console.log(f1.toString());
> console.log(f2.toString());
> };
> /// async的写法
> const asyncReadFile = async function () {
> const f1 = await readFile('/etc/fstab');
> const f2 = await readFile('/etc/shells');
> console.log(f1.toString());
> console.log(f2.toString());
> };
> // 一比较就会发现，async函数就是将 Generator 函数的星号（*）替换成async，将yield替换成await，仅此而已。
> ```
>
> `async`函数对 Generator 函数的改进，体现在以下四点。 
>
> （1）内置执行器 
>
> Generator 函数的执行必须靠执行器，所以才有了`co`模块，而`async`函数自带执行器。也就是说，`async`函数的执行，与普通函数一模一样，只要一行。 
>
> （2）更好的语义 
>
> `async`和`await`，比起星号和`yield`，语义更清楚了。`async`表示函数里有异步操作，`await`表示紧跟在后面的表达式需要等待结果。
>
> （3）更广的适用性 
>
> `co`模块约定，`yield`命令后面只能是 Thunk 函数或 Promise 对象，而`async`函数的`await`命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。 
>
> （4）返回值是 Promise 
>
> `async`函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用`then`方法指定下一步的操作。
>
> 进一步说，`async`函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而`await`命令就是内部`then`命令的语法糖。



---

### 基本用法

> `async`函数返回一个 Promise 对象，可以使用`then`方法添加回调函数。当函数执行的时候，一旦遇到`await`就会先返回，等到异步操作完成，再接着执行函数体内后面的语句。
>
> 例子如下
>
> ```js
> async function getStockPriceByName(name) {
>   const symbol = await getStockSymbol(name);
>   const stockPrice = await getStockPrice(symbol);
>   return stockPrice;
> }
> 
> getStockPriceByName('goog').then(function (result) {
>   console.log(result);
> });
> ```
>
> 上面代码是一个获取股票报价的函数，函数前面的`async`关键字，表明该函数内部有异步操作。调用该函数时，会立即返回一个`Promise`对象。
>
> 第二个例子
>
> ```js
> function timeout(ms) {
>   return new Promise((resolve) => {
>     setTimeout(resolve, ms);
>   });
> }
> 
> async function asyncPrint(value, ms) {
>   await timeout(ms);
>   console.log(value);
> }
> 
> asyncPrint('hello world', 50);
> ```
>
> 上面代码指定 50 毫秒以后，输出`hello world`。
>
> 由于`async`函数返回的是 Promise 对象，可以作为`await`命令的参数。所以，上面的例子也可以写成下面的形式。
>
> ```js
> async function timeout(ms) {
>   await new Promise((resolve) => {
>     setTimeout(resolve, ms);
>   });
> }
> 
> async function asyncPrint(value, ms) {
>   await timeout(ms);
>   console.log(value);
> }
> 
> asyncPrint('hello world', 50);
> ```
>
> ---
>
> async函数有很多种使用形式
>
> ```js
> // 函数声明
> async function foo() {}
> 
> // 函数表达式
> const foo = async function () {};
> 
> // 对象的方法
> let obj = { async foo() {} };
> obj.foo().then(...)
> 
> // Class 的方法
> class Storage {
>   constructor() {
>     this.cachePromise = caches.open('avatars');
>   }
> 
>   async getAvatar(name) {
>     const cache = await this.cachePromise;
>     return cache.match(`/avatars/${name}.jpg`);
>   }
> }
> 
> const storage = new Storage();
> storage.getAvatar('jake').then(…);
> 
> // 箭头函数
> const foo = async () => {};
> ```
>
> 



---

### 语法

> async语法算是简单多了，毕竟迭代的产物
>
> ---
>
> ### 返回 Promise 对象
>
> `async`函数返回一个 Promise 对象。
>
> `async`函数内部`return`语句返回的值，会成为`then`方法回调函数的参数。
>
> ```js
> async function f() {
>   return 'hello world';
> }
> 
> f().then(v => console.log(v))
> // "hello world"
> ```
>
> 上面代码中，函数`f`内部`return`命令返回的值，会被`then`方法回调函数接收到。
>
> `async`函数内部抛出错误，会导致返回的 Promise 对象变为`reject`状态。抛出的错误对象会被`catch`方法回调函数接收到。
>
> ```javascript
> async function f() {
>   throw new Error('出错了');
> }
> 
> f().then(
>   v => console.log(v),
>   e => console.log(e)
> )
> // Error: 出错了
> ```
>
> ----
>
> ### Promise 对象的状态变化
>
> `async`函数返回的 Promise 对象，必须等到内部所有`await`命令后面的 Promise 对象执行完，才会发生状态改变，除非遇到`return`语句或者抛出错误。也就是说，只有`async`函数内部的异步操作执行完，才会执行`then`方法指定的回调函数。
>
> 例子如下
>
> ```javascript
> async function getTitle(url) {
>   let response = await fetch(url);
>   let html = await response.text();
>   return html.match(/<title>([\s\S]+)<\/title>/i)[1];
> }
> getTitle('https://tc39.github.io/ecma262/').then(console.log)
> // "ECMAScript 2017 Language Specification"
> ```
>
> 上面代码中，函数`getTitle`内部有三个操作：抓取网页、取出文本、匹配页面标题。只有这三个操作全部完成，才会执行`then`方法里面的`console.log`。
>
> ---
>
> ### await 命令
>
> 正常情况下，`await`命令后面是一个 Promise 对象，返回该对象的结果。如果不是 Promise 对象，就直接返回对应的值。
>
> ```javascript
> async function f() {
>   // 等同于
>   // return 123;
>   return await 123;
> }
> 
> f().then(v => console.log(v))
> // 123
> ```
>
> 上面代码中，`await`命令的参数是数值`123`，这时等同于`return 123`。
>
> 另一种情况是，`await`命令后面是一个`thenable`对象（即定义`then`方法的对象），那么`await`会将其等同于 Promise 对象。
>
> 