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
>   return new Promise(function (resolve, reject) {
>     fs.readFile(fileName, function(error, data) {
>       if (error) return reject(error);
>       resolve(data);
>     });
>   });
> };
> 
> /// generator的写法
> const gen = function* () {
>   const f1 = yield readFile('/etc/fstab');
>   const f2 = yield readFile('/etc/shells');
>   console.log(f1.toString());
>   console.log(f2.toString());
> };
> /// async的写法
> const asyncReadFile = async function () {
>   const f1 = await readFile('/etc/fstab');
>   const f2 = await readFile('/etc/shells');
>   console.log(f1.toString());
>   console.log(f2.toString());
> };
> // 一比较就会发现，async函数就是将 Generator 函数的星号（*）替换成async，将yield替换成await，仅此而已。
> ```
>
>  `async`函数对 Generator 函数的改进，体现在以下四点。 
>
> -  内置执行器 
>
>    Generator 函数的执行必须靠执行器，所以才有了`co`模块，而`async`函数自带执行器。也就是说，`async`函数的执行，与普通函数一模一样，只要一行。 
>
> -  更好的语义 
>
>    `async`和`await`，比起星号和`yield`，语义更清楚了。`async`表示函数里有异步操作，`await`表示紧跟在后面的表达式需要等待结果。
>
> -  更广的适用性 
>
>    `co`模块约定，`yield`命令后面只能是 Thunk 函数或 Promise 对象，而`async`函数的`await`命令后面，可以是 Promise 对象和原始类型的值（数值、字符串和布尔值，但这时会自动转成立即 resolved 的 Promise 对象）。 
>
> -  返回值是 Promise 
>
>   `async`函数的返回值是 Promise 对象，这比 Generator 函数的返回值是 Iterator 对象方便多了。你可以用`then`方法指定下一步的操作。
>
>   进一步说，`async`函数完全可以看作多个异步操作，包装成的一个 Promise 对象，而`await`命令就是内部`then`命令的语法糖。