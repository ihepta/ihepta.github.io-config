---
title: Git commands used commonly
thumbnailImagePosition: right
date: 2017-06-09 10:52:08
thumbnailImage: git.jpg
coverImage:
tags: 
- git
- commands
categories:
---

总结记录一些常用的git命令。
<!--excerpt-->

<!--toc-->

### 创建版本库
`$ git clone <url>` #从远程仓库克隆

`$ git init` #初始化本地版本库
### 修改和提交
`$ git status` #查看状态

`$ git diff` #查看变更内容

`$ git add .` #将所有文件添加至暂存区

`$ git add <file>` #将指定文件添加至暂存区

`$ git rm <file>` #删除指定文件

`$ git commit -m <msg>` #将所有更改从暂存区提交至版本库
### 查看提交历史
`$ git log` #查看提交历史

`$ git log -p <file>` #查看指定文件的提交历史

`$ git blame <file>` #以列表方式查看指定文件的提交历史

`$ git reflog` #查看命令历史
### 撤销
`$ git checkout -- <file>` #撤销在工作区的修改

`$ git reset HEAD <file>` #将在暂存区的修改重新撤回工作区

`$ git reset --hard <commit_id>` #回退到指定版本

`$ git revert <commit_id>` #撤销指定的提交
### 分支
`$ git branch` #查看分支

`$ git branch <name>` #创建分支

`$ git checkout <name>` #切换分支

`$ git checkout -b <name>` #创建并切换到新分支

`$ git merge <name>` #合并指定分支到当前分支

`$ git merge --no-ff -m <msg> <name>` #保留合并历史并合并指定分支到当前分支

`$ git branch -d <name>` #删除分支

`git branch --set-upstream <branch-name> origin/<branch-name>` # 建立本地分支和远程分支的关联
### 标签
`$ git tag` #查看标签

`$ git tag <tagname>` #给当前版本打上标签

`$ git tag -d <tagname>` #删除本地标签

`$ git tag -a <tagname> -m <msg>` #新建标签同时添加标签信息

`$ git push origin <tagname>` #推送一个本地标签到远程仓库

`$ git push origin --tags` #推送所有还未推送至远程的标签

`$ git push origin :refs/tags/<tagname>` #删除远程标签
### 远程
`$ git remote -v` #查看远程版本库信息

`$ git remote add <remote> <url>` #添加远程版本库

`$ git fetch <remote>` #从远程库获取代码

`$ git push <remote> <branch>` #上传代码及快速合并
