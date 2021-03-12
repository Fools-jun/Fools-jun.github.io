---
title: Git使用
copyright: true
mathjax: true
categories:
  - 使用说明
tags:
  - Git
abbrlink: 54372
date: 2021-03-12 14:47:27
---

# 1.将本地项目初始化到远程仓库中

## 方法1: 无敌暴力法

clone项目到本地，然后复制文件上传



## 方法2: 强行合并法

1. 将本地项目初始化为git仓库，并将项目中的文件上传到新建的本地git仓库中，如果本地项目已经是一个git仓库了，请跳过这一步。

    ```
    git init
    git add .
    git commit -m "描述"
    ```

2. 将本地仓库和远程仓库进行关联。

    ```
    git remote add origin 远程仓库地址
    ```

3. 将远程仓库中的数据拉取到本地仓库中

    ```
    git pull origin master --allow-unrelated-histories
    ```

    如果含有共同的文件时需要：

    ```
    git merge origin/master --allow-unrelated-histories
    ```

4. 将本地仓库的数据推送到远程仓库中

    ```
    git push -u origin master
    ```

    用 `git push` 命令，实际上是把当前分支 `master` 推送到远程。

    > 注：由于远程库是空的，我们第一次推送 `master` 分支时，加上了 `-u` 参数，Git不但会把本地的 `master` 分支内容推送的远程新的 `master` 分支，还会把本地的 `master` 分支和远程的 `master` 分支关联起来，在以后的推送或者拉取时就可以简化命令。