---
title: git学习笔记
date: 2017-07-15 15:57:35
categories:
- git
tags:
- git
---

> 参考：[廖雪峰的官方网站 Git教程](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000)

### 初始化

```
git init
```

### 添加文件到版本库

- 添加文件
```
git add <file>
```
> 注意，可反复多次使用，添加多个文件；

- 把文件提交到仓库
```
git commit -m "说明"
```

### 版本管理

#### 工作区状态
- 要随时掌握工作区的状态，使用
```
git status
```
- 如果git status告诉你有文件被修改过，用
```
git diff
```
可以查看修改内容。

#### 版本回退
- HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset commit_id`，其中，该命令有三个可选参数：
    - `git reset –mixed`：此为默认方式，不带任何参数的git reset，即时这种方式，它回退到某个版本，只保留源码，回退commit和index信息
    - `git reset –soft`：回退到某个版本，只回退了commit的信息，不会恢复到index file一级。如果还要提交，直接commit即可
    - `git reset –hard` ：彻底回退到某个版本，本地的源码也会变为上一个版本的内容
- 穿梭前，用
```
git log
```
可以查看提交历史，以便确定要回退到哪个版本
- 要重返未来，用
```
git reflog
```
查看命令历史，以便确定要回到未来的哪个版本。

#### 工作区（Working Directory）
就是你在电脑里当前项目的目录

#### 版本库（Repository）
工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

#### 撤销更改
- 当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令
```
git checkout -- file
```
> `git checkout`其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

- 当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令
    ```
    git reset HEAD file
    ```
    就回到了场景1，第二步按场景1操作。
- 已经提交了不合适的修改到版本库时，想要撤销本次提交，参考版本回退一节，不过前提是没有推送到远程库。

### 远程仓库

- 要关联一个远程库，使用命令
```
git remote add origin git@server-name:path/repo-name.git
```
- 关联后，使用命令
```
git push -u origin master
```
第一次推送master分支的所有内容；
- 此后，每次本地提交后，只要有必要，就可以使用命令
```
git push origin master
```
推送最新修改；
- 要克隆一个仓库，首先必须知道仓库的地址，然后使用
```
git clone
```
命令克隆。


> Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。

- 查看远程库信息，使用
```
git remote -v
```
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用
```
git push origin branch-name
```
如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用
```
git checkout -b branch-name origin/branch-name
```
本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用
```
git branch --set-upstream branch-name origin/branch-name；
```
- 从远程抓取分支，使用git pull，如果有冲突，要先处理冲突。

### 分支管理

> Git鼓励大量使用分支

- 查看分支：
```
git branch
```
- 创建分支：
```
git branch <name>
```
- 切换分支：
```
git checkout <name>
```
- 创建+切换分支：
```
git checkout -b <name>
```
- 合并某分支到当前分支：
```
git merge <name>
```
- 只合并某个commit
```
git cherry-pick <commit-id>
```
- 删除分支：
```
git branch -d <name>
```
- 当手头工作没有完成时，先把工作现场`git stash`一下（`git stash list`查看有多少工作空间被stash了），然后去修复bug，修复后，再`git stash pop`，回到工作现场。
- 修复bug时，我们会通过创建新的bug分支进行修复，然后合并，最后删除；
- 开发一个新feature，最好新建一个分支；如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

### 多人协作

- 首先，可以试图用
```
git push origin branch-name
```
推送自己的修改；
- 如果推送失败，则因为远程分支比你的本地更新，需要先用
```
git pull
```
试图合并；
- 如果合并有冲突，则解决冲突，并在本地提交；
- 没有冲突或者解决掉冲突后，再用
```
git push origin branch-name
```
推送就能成功！

> 如果git pull提示“no tracking information”，则说明本地分支和远程分支的链接关系没有创建，用命令`git branch --set-upstream branch-name origin/branch-name`

### 标签
- 新建一个标签
```
git tag <name>
```
默认为HEAD，也可以指定一个commit id；
- 指定标签信息
```
git tag -a <tagname> -m "blablabla..."
```
- PGP签名标签
```
git tag -s <tagname> -m "blablabla..."
```
- 查看所有标签
```
git tag
```
- 推送一个本地标签
```
git push origin <tagname>
```
- 推送全部未推送过的本地标签
```
git push origin --tags
```
- 删除一个本地标签
```
git tag -d <tagname>
```
- 删除一个远程标签
```
git push origin :refs/tags/<tagname>
```

### 配置命令的别名

```
git config --global alias.别名 '命令'
```
示例：
```
git config --global alias.unstage 'reset HEAD'
```

### 忽略或者只提交某些文件
- 在工作目录下新建.gitignore文件，
文件里#开头的是注释
```
#忽略所有.txt文件：
*.txt
#以此类推
```
- 若要只提交某些文件，比如只提交.py文件，可以先忽略所有文件，再取消忽略.py文件
```
*
!*.py
```

