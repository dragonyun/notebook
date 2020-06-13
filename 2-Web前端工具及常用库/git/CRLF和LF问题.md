## CRLF和LF问题

### **一、不同操系统下的换行符**

CR回车 LF换行
Windows/Dos CRLF \r\n
Linux/Unix LF \n
MacOS CR \r



### **二、解决方法**

打卡git bash，设置core.autocrlf和core.safecrlf（可不设置），建议设置autocrlf为input，safecrlf为true，同时设置你的Eclipse、IDEA等IDE的换行符为LF\n。

> 这里不一定设置成什么，看自己的需要，我在项目中设置core.autocrlf是true

下面为参数说明，--global表示全局设置

#### **2.1、autocrlf**

\#提交时转换为LF，检出时转换为CRLF
git config --global core.autocrlf true

\#提交时转换为LF，检出时不转换
git config --global core.autocrlf input

\#提交检出均不转换
git config --global core.autocrlf false

#### **2.2、safecrlf**

\#拒绝提交包含混合换行符的文件
git config --global core.safecrlf true

\#允许提交包含混合换行符的文件
git config --global core.safecrlf false

\#提交包含混合换行符的文件时给出警告
git config --global core.safecrlf warn



### 我的问题

我可能自己把自己给坑了，全局的core.autocrlf设置为false，我没改过，可能是默认的

然后仓库上是lf，我拉下来是lf没转换，然后在我更新子模块依赖的时候得到的子模块是lf格式的，然后我把cmb-script和ui、bff打包发布了版本，这样就导致了版本里面包含的是lf格式的ui和bff子模块代码

别人通过update scaffold更新覆盖子模块的时候，得到了格式为lf的文件，出现了大量的变更。

正确流程应该是：

仓库上是lf，我拉下来是crlf，自动转换出来的，然后我更新子模块依赖的时候得到的子模块是crlf格式的，然后我把cmb-script和ui、bff打包发布了版本，这样发布版本里面包含的是crlf格式的ui和bff子模块代码

别人通过update scaffold更新覆盖子模块的时候，得到了格式为crlf的文件，保持不变。

