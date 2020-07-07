## git 两种连接方式 https 和 ssh

git连接方式连接什么，远程仓库（例如github），就是在github，我们经常看到的下载代码有两种方式，就是对应这两种。



### SSH

---

本地Git仓库和GitHub仓库之间的传输是通过SSH加密。

那么，需要一些设置：



**1、创建SSH Key**

在c盘，用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有`id_rsa`和`id_rsa.pub`这两个文件，如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：

```bash
$ ssh-keygen -t rsa -C "youremail@example.com"
```

你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可，由于这个Key也不是用于军事目的，所以也无需设置密码。

如果一切顺利的话，可以在用户主目录里找到`.ssh`目录，里面有`id_rsa`和`id_rsa.pub`两个文件，这两个就是SSH Key的秘钥对，`id_rsa`是私钥，不能泄露出去，`id_rsa.pub`是公钥，可以放心地告诉任何人。

> SSH本质上是加密传输协议，通过密钥对，公钥，密钥。
>
> `id_rsa`是私钥，放在本地，`id_rsa.pub`是公钥，与github公用
>
> 它们是基于你的邮件地址生成的
>
> 放在c盘，用户主目录下，.ssh目录中
>
> 密钥是SSH协议的前提



**2、github设置**

在github上，导入SSH的公钥，下面是步骤

登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

点“Add Key”，你就应该看到已经添加的Key：

为什么GitHub需要SSH Key呢？因为GitHub需要识别出你推送的提交确实是你推送的，而不是别人冒充的，而Git支持SSH协议，所以，GitHub只要知道了你的公钥，就可以确认只有你自己才能推送。

当然，GitHub允许你添加多个Key。假定你有若干电脑，你一会儿在公司提交，一会儿在家里提交，只要把每台电脑的Key都添加到GitHub，就可以在每台电脑上往GitHub推送了。

> 这里就是在github上配置SSH的公钥，让github认识你
>
> 本地拥有公钥私钥，github有公钥，配对完成即可进行SSH通信



**其它问题**

使用SSH克隆项目代码的话，要求你拥有该项目，即需要你对于这个项目有权限



### https

---

这种方式要求project在创建的时候只能选择“Public”公开状态，Private和Internal私有模式下不能使用https方式进行连接。（ssh方式在三种模式下都可以）。

> 说是https，有些时候是http



使用https url克隆对初学者来说会比较方便，复制https url然后到git Bash里面直接用clone命令克隆到本地就好了，但是每次fetch和push代码都需要输入账号和密码，这也是https方式的麻烦之处。

> 这种方式不需要配置什么，只不过需要经常输入账号密码



如果本地系统的 **凭据管理器 **中已经配置了账号密码，那么就不用每一次都输入账号密码了，但是，如果账号密码换了，记得来改一下这个，要不然https这种方式就一点会报错，告诉你没有权限。。。。

> https这种方式依赖 账号密码 进行数据通信
>
> 而SSH这种方式依赖 密钥对  即可进行数据通信