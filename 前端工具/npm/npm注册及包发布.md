## `npm`注册以及包发布

> 其实官网以及写的很清楚了，所以我很多都是简要叙述，主要记录问题。

### `npm`注册

> 有两种方法，官网注册账号，或者命令行注册账号
>
> - 官网注册
>
>   `https://www.npmjs.com/`，用户名，密码，邮箱必须
>
> - 命令行注册
>
>   `https://docs.npmjs.com/cli/adduser.html`
>
>   这里官网写的比较复杂，简单点就是命令行键入`npm adduser`，然后填用户名，密码，邮箱，后面需要邮箱验证。
>
> ---
>
> - 问题一：仓库地址，可能你用的是淘宝的镜像
>
>   淘宝的镜像地址
>
>   ` npm config set registry https://registry.npm.taobao.org `
>
>   要换成官方的地址
>
>   ` npm config set registry https://registry.npmjs.org `
>
>   查仓库地址的命令
>
>   `npm config get registry`
>
> - 问题二：如何查看自己的主页
>
>   地址：`https://www.npmjs.com/~zlingyun`我的用户名就是`zlingyun`

### `npm`包发布

> 网址：`https://www.npmjs.cn/getting-started/publishing-npm-packages/`
>
> 需要一个`npm`的账号，命令行操作，先登录，再发布
>
> ---
>
> - 发布
>
> 通过`npm init`得到`package.json`文件
>
>  发布的包的名字、版本就是项目目录中`package.json`里面的`name`和`version` 
>
> 输入`npm publish`即可发布，然后到官网搜索你的发布包
>
> ----
>
> - 更新
>
> 更新的话，文件改好之后，修改`package.json`中的版本号，然后再发布
>
> 也可以通过命令改变版本号`npm patch/minor/major`，分别代表补丁，功能，大版本
>
> 然后再发布`npm publish`
>
> ---
>
> - 删除
>
>  终端打开包项目，输入`npm unpublish --force` 
>
> 然后去`npm`网站确认一下