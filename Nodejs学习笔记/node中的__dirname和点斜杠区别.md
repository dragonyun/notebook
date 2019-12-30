## node中的__dirname和./区别



### 结论

Node.js 中，`__dirname` 总是指向被执行 js 文件的绝对路径，所以当你在 `/d1/d2/myscript.js` 文件中写了 `__dirname`， 它的值就是 `/d1/d2` 。

相反，`./` 会返回你执行 node 命令的路径，例如你的工作路径。

有一个特殊情况是在 `require()` 中使用 `./` 时，这时的路径就会是含有 `require()` 的脚本文件的相对路径。



### 例子对比

例子1

> ```js
> // 目录如下
> /dir1
>   /dir2
>     pathtest.js
> 
> // 然后在 pathtest.js 中，有如下代码
> var path = require("path");
> console.log(". = %s", path.resolve("."));
> console.log("__dirname = %s", path.resolve(__dirname));
> 
> // 然后执行了下面命令
> cd /dir1/dir2
> node pathtest.js
> 
> // 将会得到
> . = /dir1/dir2
> __dirname = /dir1/dir2
> ```
>
>  `.` 是你的当前工作目录，在这个例子中就是 `/dir1/dir2` ，`__dirname` 是 `pathtest.js` 的文件路径，在这个例子中就是 `/dir1/dir2`  

例子2

> ```js
> // 如果我们的工作目录是 /dir1
> cd /dir1
> node dir2/pathtest.js
> 
> // 将会得到
> . = /dir1
> __dirname = /dir1/dir2
> ```
>
>  此时，`.` 指向我们的工作目录，即 `/dir1`， `__dirname` 还是指向 `/dir1/dir2` 。 



### 在 `require` 中使用 `.`

如果在 `dir2/pathtest.js` 中调用了 `require` 方法，去引入位于 `dir1` 目录的 js 文件，你需要写成

```
require('../thefile')1
```

因为 `require` 中的路径总是相对于包含它的文件，跟你的工作目录没有关系。