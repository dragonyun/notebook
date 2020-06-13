## npm生命周期钩子

挺冷门的知识，算了，应该说会了不难，难了不会。

参考网站：`https://www.cnblogs.com/163yun/p/9881906.html`



### 我的理解

其实我在学习vue和react的时候接触过生命周期函数，钩子这样的概念。

npm这个，我官网也没找到，是最近遇到问题才查到这么一个知识点的。

解释起来也很容易，就是你写脚本的时候，它本身会按照顺序执行。

```bash
npm run premock -->  npm run mock  -->  npm run postmock
```

这样就看明白了，还是很有用的，在你想要执行的脚本之前或者之后执行必要的脚本程序。



### 生命周期钩子

这里借用很多框架的生命周期钩子的概念，其实npm也在不同的生命周期 [提供了一些钩子](https://docs.npmjs.com/misc/scripts)，可以方便你在项目运行的不同时间点进行一些脚本的编写。

它的钩子分为两类：pre- 和 post- ，前者是在脚本运行前，后者是在脚本运行后执行。所有的命令脚本都可以使用钩子（包括自定义的脚本）。

例如：运行npm run build，会按以下顺序执行：

**npm run prebuild -->  npm run build -->  npm run postbuild**

pre脚本和post脚本也是出口代码敏感(exit-code-sensitive) 的，这意味着如果您的pre脚本以非零出口代码退出，那么NPM将立即停止，并且不运行后续脚本。

通常你可以在pre脚本上执行一些准备工作，在post脚本上执行一些后续操作。

```bash
"clean": "rimraf ./dist && mkdir dist",
"prebuild": "npm run clean",
"build": "cross-env NODE_ENV=production webpack"
```

另外，还有很多额外的生命周期钩子，可以方便使用，例如[husky](https://www.npmjs.com/package/husky) 和 [pre-commit](https://www.npmjs.com/package/pre-commit) 包提供了有关git的commit的生命周期钩子。