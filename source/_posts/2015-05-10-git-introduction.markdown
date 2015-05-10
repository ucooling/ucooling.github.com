---
layout: post
title: "Git introduction"
date: 2015-05-10 11:51:44 +0800
comments: true
categories: 
---

##背景介绍
参加工作的两年时间里，刚开始工作的时候是一家日企，使用的版本管理工具是SVN，对于一个初涉职场的新手来说学习这个不是很困难，并且有很多的工具可以使用，但是这种工具有很多的局限性，而且很不方便，对于公司层面来讲确实是一个很好的选择，但是对于个人来讲使用SVN来管理自己的代码确实不是一个很好的选择，而且很不方面。从我准备开始换工作的时候我开始注意到了Git，开始的时候对它有一种敬畏的心态，总觉的这个东西很难学会，事实上确实很难学会，他的学习曲线是比较长，但是当你真正的掌握他的精髓的时候你就会开始觉得这会是一个很好的工具，下面的内容我会介绍一些我自己对于Git的一些使用的理解。

##什么是版本控制
下面这段是从Git的官网教程中摘抄来的，“版本控制是一种记录一个或者若干文件内容变化，以便将来查阅特定版本修改情况的系统”。采用版本控制是一个明智的选择，有了它你可以将某个文件回溯到之前的状态，甚至将整个项目回退到之前某个时间点的状态。你可以比较文件的细节变化，查出最后是谁修改了那个地方，从而找出问题的原因，有是谁在什么时间提交修改了什么文件。使用版本控制系统还意味着，就算你把整个项目中的文件搞的乱七八糟，你也照样可以轻松恢复到原来的样子，但是额外增加的工作量却微乎其微。

##集中式和分布式的比较
先说集中式版本控制系统，版本库是集中存放在中央服务器的，而干活的时候用的都是自己的电脑，所以每次干活之前都需要从中央服务器取得最新的版本，然后开始自己的工作，干完活之后，再把自己的东西提交到中央服务器。
集中式最大的痛点就是必须联网才能工作，如果中央服务器在局域网还好，但是如果在互联网，如果网速慢那就悲剧了，如果你是一个急性子的人，你会被气的得精神病的。
和集中式版本控制系统相比，分布式版本控制系统的安全性要高很多，因为每个人的电脑里都有一个完整的版本库，某一个人得电脑坏了不要紧，随时从其他人呢那里复制一个就可以了，而集中式版本控制系统的中央服务器出了问题，所有的人就都没有办法干活了。

##Git基础
* 直接记录快照，而非进行差异比较

![Git版本控制](images/git.png)

* 几乎所有的操作都在本地进行

Git不用跑到外边的服务器上拿数据，而直接是从本地的数据库中获取数据，所以在任何的时候你都可以马上的翻阅自己的历史记录，无需等待，如果想要看当前版本的文件和一个月前的版本的差异，Git会取出当前一个月的快照，不用到远程的服务器来查看快照。

* 时刻保持数据的完整性
在保存Git之前，所有的数据都要进行内容的检验和计算，并将此结果作为数据标识和缩引，Git使用SHA-1算法计算数据的校验和，通过对文件的内容和目录的结构做出一个SHA-1哈希值，作为指纹字符串，Git的工作完全依赖于这类指纹字符串。


* 文件的三种状态

对于Git管理的任何一个文件，在Git的内部都只有三种状态：已经提交（committed）,已经修改（modified）和已经暂存（staged），已经提交表示已经提交到本地的数据库中，已经修改表示修改了某个文件但是还没有提交保存，已经暂存表示已经被被修改的文件保存到了下次提交的保存清单中git
![Git文件状态](images/git_status.png)

Git的基本工作流程是，
1 从master分支上新建一个分支，记住永远不要再master分支上直接工作，这样做有很多的弊端
2 切换到新建的分支，在该分支上对某些文件进行修改
3 对修改后的文件进行快照，然后保存到暂存区域
4 提交更新，将保存在暂存区域的文件快照永久存储到Git目录中。

##Mac上安装Git

```
brew install git
```

##第一次使用Git时的简单配置
Git提供了一个叫做git config的工具，改工具专门用来配置或者读取响应的工作环境变量，这些变量会被存储在三个地方
* /etc/gitconfig 对系统中的所有用户都普遍适用的配置
* ~/.gitconfig 用户目录下的配置文件只适用于改用户
* 当前项目的Git配置文件（.git/config）

常用命令

```
git config --global user.name "Username"
git config --global user.email Username@example.com
git config --global merge.tool vimdiff
git config --list
```
##Git项目初始化

* 在本地初始化一个Git项目


```
git init .
```
* 从远程克隆一个现有项目

```
git clone git://github.com/username/project.git
git clone git://github.com/username/project.git
```
##Git常使用命令解析
* 查看文件状态

```
git status
```
* 查看尚未暂存的文件更新了那部分

此命令查看的是工作目录中当前文件和暂存区域快照之间的差异，也就是修改之后还没有暂存起来的变化内容


```
git diff
```
* 查看已经暂存起来的文件和上次提交时的快照之间的差异

```
git diff --cached
#more than 1.6.1 version
git diff --staged
```
* 提交更新

```
git commit -m "comments"
```
* 跳过使用暂存区域

```
git commit -a -m "comments"
```
* 移动文件

```
git mv file_from file_to
```
运行上边的命令相当于执行了下边的三条命令

```
mv test test.rb
git rm test
gir add test.rb
```
* 查看提交历史

```
git log
#查看最近两次提交的内容差异
git log -p -2
#每次提交增改行数的统计
git log --stat

```
* 修改最后一次提交

有时候我们提交完了之后发现漏掉了一些文件没有提交，或者是提交信息写错了，想要撤销刚才的提交操作，可以使用 --amend选项重新提交
```
git commit --amend
```

* 取消已经暂存的文件

```
git reset .
git reset HEAD file_name
```
* 取消对文件的修改

```
git checkout file_name
```
* 查看当前远程库信息

```
git remote -v
```
* 添加远程仓库

```
git remote add [shortname] [url]
git remote add upstream git://github.com/paulboone/ticgit.git
```
* 从远程仓库抓取数据

要抓取本次仓库没有的信息可以运行下面的命令
```
git fetch [remote-name]
```
* 推送数据到远程仓库

```
git push origin master
```
* 修改、删除远程分支

```
git remote rename upstream origin
git remote rm upstream
```
* 标签相关

```
git tag  //显示所有的标签
git tag -l 'v1.4.2.*'  //只显示1.4.2系列的标签
git tag -a v1.1 -m 'my first version 1.1'  //添加标签
```
* 添加并切换分支

```
git branch -b [branch-name]
```
* 切换分支

```
git checkout [branch-name]
```
* 合并分支

```
git checkout -b test-branch //新建了一个分支
git merge master //将master分支合并到该分支
```
* 删除分支

```
git branch -d [branch-name]
```
* 遇到冲突是的分支合并

可以看到 ======= 隔开的上半部分，是 HEAD中的内容，下半部分是分支中的内容，二者自选其一手动整合

```
git mergetool //使用图形界面来解决冲突
```
* 查看个分支最后一次提交的信息

```
git branch -v
```
* 查看已经合并/没有合并的分支

```
git branch --merged
git branch --no-merged
```
* 分支的衍合

把一个分支中的修改整合到另一个分支的办法有两种：merge和rebase
对于merge理解非常的简单，就是合并两个分支，合并两个分支的另一种方法就是衍合，简单地来讲，就是把另一个分支产生的变化补丁在需要衍合的分支中重新打一遍，这种操作就叫做衍合
```
git checkout [branch-name]
git rebase master  //衍合master分支的内容到现在的分支
```
* 生成SSH公钥

```
ssh-keygen
```
* 储藏你的工作

```
git stash
git stash pop
git stash list
git stash apply stash@{2}
```








































