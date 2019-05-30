+++
title = "Git"
date = 2019-05-30T13:54:25+08:00
tags = [""]
categories = [""]
draft = false
+++
## git log
+ git reset -- hard version
+ git relog
+ git log --graph

## git checkout
+ 未 git add： git checkout -- file
+ git add 后： git reset HEAD <file>, git checkout -- file
+ 创建新分支并且切换： git checkout -b 分支名
+ 切换分支 git checkout 分支
+ 拉取新的远程分支到本地：git checkout -b dev origin/dev

## git remote
+ git remote add 分支名（origin）远程地址
+ git remote -v
+ 

## git clone

## git merge
+ dev -> master
+ git merge dev

## git branch
+ git branch -d 分支
+ git branch -D 分支（强行删除没有被合并过的分支）
+ 远程分支跟本地分支关联：git branch --set-upstream-to=origin/dev dev 

## git stash
+ git stash pop(git stash apply(version), git stash drop)
+ git stash 

## git rebase
本地分支未提交，git commit --amend 可以修改提交信息。111
+ rebase操作可以把本地未push的分叉提交历史整理成直线 2.0 test
