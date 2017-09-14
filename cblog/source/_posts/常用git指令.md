---
title: 常用git指令
date: 2017-09-14 14:25:20
tags: git
---
# 99%的时间在使用的Git命令
Git是目前最流行的分布式版本控制系统，它是Linus献给软件行业的两件礼物之一，另外一件礼物是目前最大的服务器系统软件Linux。

# 基本命令
Git是一个非常强大的工具，各种命令组合上千种。不过，我们90%的时间估计都在用这4条命令。
{% codeblock %}
$ git status # 查看工作区和缓冲区状态
$ git add --all # 将工作区修改暂存到缓冲区
$ git commit -m"<comment>" # 提交到仓库
$ git push origin master # 推送到远程分支
{% endcodeblock %}
为了合作与规范，这还远远不够

## 关联远程仓库
如果还没有Git仓库，你需要
{% codeblock %}
$ git init
{% endcodeblock %}
如果你想关联远程仓库
{% codeblock %}
$ git remote add <name> <git-repo-url>
# 例如 git remote add origin https://github.com/xxxxxx
{% endcodeblock %}
如果你想关联多个远程仓库
{% codeblock %}
$ git remote add <name> <another-git-repo-url>
# 例如 git remote add coding https://coding.net/xxxxxx
{% endcodeblock %}
是远程仓库的名称，通常为 origin
忘了关联了哪些仓库或者地址
{% codeblock %}
$ blog-react git:(master) git remote -v
# origin https://github.com/gzdaijie/koa-react-server-render-blog.git (fetch)
# origin https://github.com/gzdaijie/koa-react-server-render-blog.git (push)
{% endcodeblock %}
如果远程有仓库，你需要clone到本地
{% codeblock %}
$ git clone <git-repo-url>
{% endcodeblock %}
关联的远程仓库将被命名为origin，这是默认的。
如果你想把别人仓库的地址改为自己的
{% codeblock %}
$ git remote set-url origin <your-git-url>
{% endcodeblock %}
切换分支
新建仓库后，默认生成了master分支
如果你想新建分支并切换
{% codeblock %}
$ git checkout -b <new-branch-name>
# 例如 git checkout -b dev
# 如果仅新建，不切换，则去掉参数 -b
{% endcodeblock %}
看看当前有哪些分支
{% codeblock %}
$ git branch
# * dev
#   master
{% endcodeblock %}
标*号的代表当前所在的分支
看看当前本地&远程有哪些分支
{% codeblock %}
$ git branch -a
# * dev
#   master
#   remotes/origin/master
{% endcodeblock %}
切换到现有的分支
{% codeblock %}
$ git checkout master
{% endcodeblock %}
你想把dev分支合并到master分支
{% codeblock %}
$ git merge <branch-name>
# 例如 git merge dev
{% endcodeblock %}
你想把本地master分支推送到远程去
{% codeblock %}
$ git push origin master
{% endcodeblock %}
你可以使用git push -u origin master将本地分支与远程分支关联，之后仅需要使用git push即可。
远程分支被别人更新了，你需要更新代码
{% codeblock %}
$ git pull origin <branch-name>
{% endcodeblock %}
之前如果push时使用过-u，那么就可以省略为git pull
本地有修改，能不能先git pull
{% codeblock %}
$ git stash # 工作区修改暂存
$ git pull  # 更新分支
$ git stash pop # 暂存修改恢复到工作区
{% endcodeblock %}
撤销操作
恢复暂存区文件到工作区
{% codeblock %}
$ git checkout <file-name>
{% endcodeblock %}
恢复暂存区的所有文件到工作区
{% codeblock %}
$ git checkout .
{% endcodeblock %}
重置暂存区的某文件，与上一次commit保持一致，但工作区不变
{% codeblock %}
$ git reset <file-name>
{% endcodeblock %}
重置暂存区与工作区，与上一次commit保持一致
{% codeblock %}
$ git reset --hard <file-name>
{% endcodeblock %}
如果是回退版本(commit)，那么file，变成commit的hash码就好了。
去掉某个commit
{% codeblock %}
$ git revert <commit-hash>
{% endcodeblock %}
实质是新建了一个与原来完全相反的commit，抵消了原来commit的效果
版本回退与前进
查看历史版本
{% codeblock %}
$ git log
{% endcodeblock %}
你可能觉得这样的log不好看，试试这个
{% codeblock %}
$ git log --graph --decorate --abbrev-commit --all
{% endcodeblock %}
检出到任意版本
{% codeblock %}
$ git checkout a5d88ea
{% endcodeblock %}
hash码很长，通常6-7位就够了
远程仓库的版本很新，但是你还是想用老版本覆盖
{% codeblock %}
$ git push origin master --force
# 或者 git push -f origin master
{% endcodeblock %}
觉得commit太多了？多个commit合并为1个
{% codeblock %}
$ git rebase -i HEAD~4
{% endcodeblock %}
这个命令，将最近4个commit合并为1个，HEAD代表当前版本。将进入VIM界面，你可以修改提交信息。推送到远程分支的commit，不建议这样做，多人合作时，通常不建议修改历史。
想回退到某一个版本
{% codeblock %}
$ git reset --hard <hash>
# 例如 git reset --hard a3hd73r
{% endcodeblock %}
--hard代表丢弃工作区的修改，让工作区与版本代码一模一样，与之对应，--soft参数代表保留工作区的修改。
想回退到上一个版本，有没有简便方法？
{% codeblock %}
$ git reset --hard HEAD^
{% endcodeblock %}
回退到上上个版本呢？
{% codeblock %}
$ git reset --hard HEAD^^
{% endcodeblock %}
HEAD^^可以换作具体版本hash值。
回退错了，能不能前进呀
{% codeblock %}
$ git reflog
{% endcodeblock %}
这个命令保留了最近执行的操作及所处的版本，每条命令前的hash值，则是对应版本的hash值。使用上述的git checkout 或者 git reset命令 则可以检出或回退到对应版本。
刚才commit信息写错了，可以修改吗
{% codeblock %}
$ git commit --amend
{% endcodeblock %}
看看当前状态吧
{% codeblock %}
$ git status
{% endcodeblock %}
## 配置属于你的Git
看看当前的配置
{% codeblock %}
$ git config --list
{% endcodeblock %}
估计你需要配置你的名字
{% codeblock %}
$ git config --global user.name "<name>"
{% endcodeblock %}
--global为可选参数，该参数表示配置全局信息
希望别人看到你的commit可以联系到你
{% codeblock %}
$ git config --global user.email "<email address>"
{% endcodeblock %}