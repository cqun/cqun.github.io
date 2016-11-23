---
layout: post
title: git fetch 与 git pull 的区别
author: HelloDBA
tags: [git]
category: Git
---

在弄清 git fetch 与 git pull 区别之前，我们先从远端库git clone两个本地库（A库、B库）进行实际操作。

## A库进行一次git commit 并 push 到远程库

```
$ cat refs/heads/master
db39bda75ddb5e8c8b594e4a170167d95d6d3e41

$ cat refs/remotes/origin/master
db39bda75ddb5e8c8b594e4a170167d95d6d3e41

```

## B库执行 git fetch 观察库最新版本变化

```
$ cat refs/heads/master
01ed31d989693b92664161f16944b4ac2f5fbb12

$ cat refs/remotes/origin/master
db39bda75ddb5e8c8b594e4a170167d95d6d3e41

$ cat refs/FETCH_HEAD
db39bda75ddb5e8c8b594e4a170167d95d6d3e41		branch 'master' of github.com:HelloDBA/gitlab

```
**git fetch 只是更新了本地库关联的远程库的最新版本指向，本地库文件版本并没有变化**

*   git fetch：这将更新git remote中所有的远程repo所包含分支的最新commit-id, 将其记录到.git/FETCH_HEAD文件中
*   git fetch remote_repo：这将更新名称为remote_repo 的远程repo上的所有branch的最新commit-id，将其记录 
*   git fetch remote_repo remote_branch_name：这将这将更新名称为remote_repo 的远程repo上的分支： remote_branch_name
*   git fetch remote_repo remote_branch_name:local_branch_name：这将这将更新名称为remote_repo 的远程repo上的分支： remote_branch_name ，并在本地创建local_branch_name 本地分支保存远端分支的所有数据。

## B库执行 git commit 观察最新版本变化并git fetch

```
$ cat refs/heads/master
3b565936eb8f41ceca5991e9c096a403fbdad0a8

$ cat refs/remotes/origin/master
db39bda75ddb5e8c8b594e4a170167d95d6d3e41

$ cat FETCH_HEAD
db39bda75ddb5e8c8b594e4a170167d95d6d3e41                branch 'master' of github.com:HelloDBA/gitlab

```
本地库并没有变化，也就是说，git fetch只会将本地库所关联的远程库的commit id更新至最新

这时我们对B库进行git push的话肯定会失败，因为和远端库存在冲突，冲突的原因是两个本地库进行了不同的向前版本推进，并且其中一个库已经push到远程，而另外一个库在对分支操作前没有进行从远程获新数据（这也不可避免，毕竟要多人协作）

## B库执行 git pull 合并冲突并git commit，在A库执行git pull 观察最新版本变化

```
$ cat refs/heads/master
2e1005440605f4189fc6be947b71349b6edec713

$ cat refs/remotes/origin/master
2e1005440605f4189fc6be947b71349b6edec713

$ cat FETCH_HEAD
2e1005440605f4189fc6be947b71349b6edec713                branch 'master' of github.com:HelloDBA/gitlab

```
**执行git pull 后本地库更新至最新，git pull会将本地库更新至远程库的最新状态，由于本地库进行了更新，HEAD也会相应的指向最新的commit id。**

所以虽然从结果上来看，git pull = git fetch origin master:tmp + git merge tmp（git fetch远程分支到本地的一个临时分支，然后再将其合并到本地分支）。

为了更好理解，可以参考一下图：

![git fetch git pull](/images/git-fetch-git-pull.png)
