## npm publish 发布文件说明

问题：npm publish 发布依赖到制品库中，都发布了哪些文件，又是如何控制选择文件的？



### 默认文件

---

**npm publish**的时候会把项目目录里面所有的文件都publish到npm仓库中。

但是往往有一部分目录和文件不想发布上去，比如项目的源码、编译脚本等等信息。



### 文件控制

---

如何发布用户需要使用的相关文件呢？

#### 方法一：使用 .gitignore 设置忽略哪些文件

***.gitignore\*** 设置的忽略文件，在git代码管理和 ***npm publish\*** 都会被忽略

#### 方法二：使用 ***.npmignore***设置忽略哪些文件

***.npmignore\***的写法跟**.gitignore** 的规则完全一样。若同时使用了***.npmignore***和**.gitignore**，只有***.npmignore***会生效，优先级比较高。

> 文件优先级。可以用一个空的.npmignore去覆盖.gitignore的配置

#### 方法三：使用***package.json\***的***files***字段选择发布哪些文件

直接在***package.json\***中***files***字段设置发布哪些文件或目录。这个优先级高于***.npmignore***和***.gitignore**。*

省略该字段将使其默认为["*"]，这意味着它将包括所有文件。

PS：选择哪种方法，根据自己的需求而定。一般情况，使用方法三。

> 配置覆盖，有优先级更高的配置，低优先级配置会被覆盖掉。



### 默认忽略和默认包含文件

---

**无论如何设置，总会忽略某些文件，总会包含某些文件。**

#### 默认忽略文件

> - `.git`
> - `CVS`
> - `.svn`
> - `.hg`
> - `.lock-wscript`
> - `.wafpickle-N`
> - `.*.swp`
> - `.DS_Store`
> - `._*`
> - `npm-debug.log`
> - `.npmrc`
> - `node_modules`
> - `config.gypi`
> - `*.orig`
> - `package-lock.json`(use shrinkwrap instead)
> - All files containing a `*` character (incompatible with Windows)

#### 默认包含文件

> - `package.json`
> - `README`
> - `CHANGES`/ `CHANGELOG`/`HISTORY`
> - `LICENSE` / `LICENCE`
> - `NOTICE`
> - `The file in the “main” field`



### 资料参考

- https://www.jianshu.com/p/6c2f5c31bfe6
- https://docs.npmjs.com/configuring-npm/package-json#files
- https://docs.npmjs.com/cli-commands/publish.html