---
title: Git命令总结
date: 2016-12-31
categories: fullstack
tags:
  - cheatsheet
---

在 rails101 中用到的 Git 命令总结如下：

## 基本命令
```ruby
git init
# git 初始化
git add .
# 将所有变动的文件放入备选，如果不想全部变动都纪录，把 . 符号改成相应的文件名；
git commit -m "description"
# 保存一次进度，description是用户对这次变动的描述，以利于将来做 review
```

## 分支命令
```ruby
git checkout -b name
# 新建一个分支，name 是分支的名称，可以任意，但是最好有逻辑顺序
git checkout name
# 切换分支到 name
git branch -d name
# 删除某一分支
```

## 上传命令
```ruby
git remote add origin https://github.com/xxx/xxx.git
# 与 Github 账户远程连结起来
git push -u origin master
# 将主代码上传
git push --all orgin
# 将所有分支上传
```
