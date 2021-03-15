## 个人博客之git梳理



### 必备概念

---

- **集中式版本控制系统**：版本库集中存放在中央服务器，每次都需要先从中央服务器取得最新的版本，干完活，再把自己的活推送给中央服务器。比如**CVS和SVN**。
- **分布式版本控制系统**：每个人电脑里都有完整的版本库。如**git**
- **github**：免费的远程仓库。可以充当“中央服务器”。



### 集中式和分布式对比

---

- 网络要求：集中式必须要联网，如果遇到网速慢，很影响效率。分布式大部分时候不需要联网，每人都有一个完整的版本库。
- 安全：集中式如果中央服务器出问题，所有人无法工作。分布式没有中央服务器，每人都有一个完整的版本库，更安全。
- 分支管理：集中式没有。分布式有。



### git软件的本质

---

- git软件本质上是一个软件工具
- git软件大多数时候通过shell等终端操作，包括软件配置，功能执行
- git命令的执行依赖于环境变量，name，email等配置



### git关键设计

---

- 工作区和暂存区

  **工作区**：就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区。

  **版本库**：工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

  **暂存区**：位于版本库中。执行git add之后，会把对于文件放在暂存区。

- 文件比较

  **文件流向**：工作区=>版本库的暂存区=>版本库的当前分支。

  **git add**：把文件修改从工作区添加到暂存区。

  **git commit**：把暂存区的所有内容提交到当前分支。

  **文件变更**：工作区和版本库的当前分支的比较。

- Git跟踪并管理修改

  Git跟踪并管理的是修改，而非文件，可以很方便的管理修改的内容，以及版本回退。

  ```bash
  git reset --hard HEAD^
  git log
  git reset --hard 1094a
  ```

- 分支管理

  git的分支控制速度特别快，处理bug，开发功能，多人协作都很便利。



### git常用命令

---

#### 创建版本库

```bash
git init # 创建一个git管理的版本库，会生成.git目录
```



#### 添加到版本库

```bash
git add . 或者 git add xxx.tsx  # 从工作区=>版本库的暂存区
git commit -m "" # 版本库的暂存区=>版本库的当前分支
```



#### 历史状态

```bash
git status # 当前项目状态，文件状态
git log # 历史提交
git relog # 回退之后，重返未来，未来提交
```



#### 撤销修改与版本跳转

```bash
# 撤销修改
git checkout -- file # 工作区撤销某个文件的修改
git reset HEAD <file> # 从暂存区撤销，回到工作区
# 版本跳转
git reset --hard commit_id # 指定版本
git reset --hard HEAD^  # 上一个版本
git reset --hard # 放弃所有变更，因为是跳转到当前版本，正在改动的内容就丢失了
```



#### 远程仓库操作

```bash
# 关联一个远程库
git remote add origin git@server-name:path/repo-name.git
# 第一次推送master分支
git push -u origin master
# 非第一次推送master分支
git push origin master
# 从远程抓取代码
git pull
# 解除本地与远程关联
git remote rm origin
# 克隆仓库
git clone （https/ssh）# 支持多种协议
git clone -b master # 指定分支
# 本地创建和远程分支对应的分支，本地和远程分支的名称最好一致
git checkout -b branch-name origin/branch-name
# 建立本地分支和远程分支的关联
git branch --set-upstream branch-name origin/branch-name
```



#### 分支管理

```bash
git branch # 查看分支
git branch <name> # 创建分支
git checkout <name> # 切换分支
git checkout -b <name> # 创建+切换分支
git merge <name> # 合并某分支到当前分支
git branch -d <name> # 删除分支
```



#### 隐藏与恢复现场

```bash
# 为了处理别的问题，先把当前修改隐藏起来
git stash # 隐藏当前变更内容，工作区状态是干净的
git stash list # 隐藏列表，可多次隐藏
git stash apply # 恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除
git stash apply stash@{0} # 恢复指定的stash
git stash pop # 恢复现场，恢复的同时把stash内容也删了
```



#### 标签

```bash
git tag <tagname> # 新建一个标签，默认为HEAD，也可以指定一个commit id
git tag -a <tagname> -m "blablabla..." # 指定标签信息
git tag # 查看所有标签
git push origin <tagname> # 推送一个本地标签
git push origin --tags # 推送全部未推送过的本地标签
git tag -d <tagname> # 删除一个本地标签
git push origin :refs/tags/<tagname> # 删除一个远程标签
```

