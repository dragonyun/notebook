## nvm 使用指南

说明：nvm是一个node包管理工具，可以在一个环境里面使用不同版本的nodejs

我是在windows环境下，下面的说明也是针对windows进行说明



### 1、卸载现有的 node.js

如果已经安装过 nodejs 的话，需要先卸载干净，再进行 nvm 的安装。

1. 控制面板-程序-卸载
   确保node.js没有在后台运行的情况下，进入控制面板，进行卸载
2. 到文件夹中进行进一步的删除
   
   > C:\Program Files（x86）\Nodejs
   > C:\Program Files\Nodejs
   > C:\Users\Administrator\AppData\Roaming\npm
   > C:\Users\Administrator\AppData\Roaming\npm\npm-cache
   > 删除上述的几个文件夹，Administrator只是举例，每个人不一样，我的是80272564
3. 检查环境变量中Path中有没有
   有即删除
4. 重启一下



### 2、安装nvm

这里去找一下安装包，github上，执行nvm-setup.exe安装完成

> 默认nvm安装路径：C:\Users\80272564\AppData\Roaming\nvm
>
> 默认nodejs安装路径：C:\Program Files\nodejs



### 3、替换nvm文件夹下的内容（其中一种方式）

这里解压nvm多版本内容.zip，将文件夹中的内容拷贝至 nvm 安装路径下，即C:\Users\80272564\AppData\Roaming\nvm。

这里已经包含了三个可用版本 10.15.0、11.11.0、12.15.0

这里就相当于，你已经有了相关的已经安装好的文件，直接拷贝过去就好了，等于可以说是绿色版（直接拷贝结果）

----

### 3、手动安装需要的多个node（另一种方式）

通过 nvm install <version> 安装多个版本的nodejs



### 4、更改setting.txt文件（非必须）

这里只需要把 root 内容改成 nvm 的安装目录

>  // 说明
>
> root：nvm的安装目录
>
> path：nodejs的安装目录
>
> node_mirror：node的行内镜像地址
>
> npm_mirror：npm的行内镜像地址

可以了，到这里应该就可以用了

---

这里是在公司里的做法，需要改镜像地址，公司之外应该无所谓，跳过这一步就可以了



### 5、nvm使用

这里说几个常用的

> nvm version： 查 nvm 的版本号
>
> nvm ls：查当前本地已经安装，可以使用的node版本列表
>
> nvm ls available：查远程厂库里面可以使用的 node版本列表
>
> nvm use <version>：切换到指定版本，如 nvm use 12.15.0
>
> nvm install <version>：安装指定版本到本地
>
> nvm install node：安装最新版node（稳定版）
>



## 补充说明（公司内的做法）

如果我们手边没有可以直接使用的多版本文件，即这里提到的nvm多版本内容.zip，那么在安装好nvm之后，需要修改settings.txt，默认里面只有root和path的配置，需要手动添加node和npm的镜像地址，然后应该就可以了（推论，待实践），下面这个是我的例子，可以作为参考

```txt
root: C:\Users\80272564\AppData\Roaming\nvm
path: C:\Program Files\nodejs
node_mirror: http://central.jaf.cmbchina.cn/artifactory/taobao-mirrors/node/
npm_mirror: http://central.jaf.cmbchina
```