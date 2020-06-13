## git 命令行合并分支代码

1. .git同文件夹下，右键进入git bash命令行，然后进入要合并的分支（如master分支合并到release，则进入release目录）

   git checkout release（切换分支）
   git pull（拉去最新的代码）

2. 查看分支（可跳过）

   git branch -a（查看所有分支：本地分支白色，当前分支绿色，远程分支红色）

3. 合并分支

   git merge master（如果是远程仓库，就是origin/master）

4. 查看状态

   git status（这里可以看到是否有冲突：conflict，或者修改：modify）

5. 有冲突，通过IDE/编辑器解决（通过vscode处理冲突）

6. 解决后，提交至暂存区

   git add .（最后是空格+点，不能少，这里是直接全部放到暂存）

7. 提交

   git commit -m "说点什么"（-m后面的是本次提交的信息，如果不填，直接使用git commit,系统会自动生成）

8. 推送

   git push（已提交的变动推送至远程）