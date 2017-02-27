# Leaning Git

[TOC]

## 配置

1. `git config --list` 检查配置信息。
2. `git config --global user.name/user.email "XXX"`设定用户名，email
3. `$ ssh -T git@github.com` 如果不行`Permission denied (publickey`，可能是publickey 失效，用`$ssh-keygen`创建新ssh key， 默认会存储在`～/.ssh/id_rsa.pub`,用记事本打开，复制所有内容，在github里的setting-personal setting-ssh key，add ssh key，完毕。

## 使用

先用的克隆远端仓库。在github上先创建仓库，然后克隆到本地。

本地仓库上传。先创建一个远端仓库，与本地仓库同名，创建本地仓库，初始化，add,commit。关键点：用ssh地址，`git remote add origin git@github.com:lzbupt2008/NewRepository.git`,（中间发现ssh key问题）然后就可以`git push -u origin master`

## 错误

1. `fatal: remote origin already exists.` 方法：`$ git remote rm origin`之后 `$ git remote add git@github.com:lzbupt2008/NewRepository.git`

2. ​

   ​