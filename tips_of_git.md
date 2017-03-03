# Leaning Git

[TOC]

## 配置

1. `git config --list` 检查配置信息。
2. `git config --global user.name/user.email "XXX"`设定用户名，email
3. `$ ssh -T git@github.com` 如果不行`Permission denied (publickey`，可能是publickey 失效，用`$ssh-keygen`创建新ssh key， 默认会存储在`～/.ssh/id_rsa.pub`,用记事本打开，复制所有内容，在github里的setting-personal setting-ssh key，add ssh key，完毕。

## 使用

#### 克隆远端仓库

标准流程。先用的克隆远端仓库。在github上先创建仓库，然后克隆到本地。`git clone git@github.com:lzbupt2008/python_study.git`

#### 本地仓库上传

本地仓库上传。先创建一个远端仓库，与本地仓库同名，创建本地仓库，初始化，add,commit。关键点：用ssh地址，`git remote add origin git@github.com:lzbupt2008/NewRepository.git`,（中间发现ssh key问题）然后就可以`git push -u origin master`

#### 版本回退

`git log`看commit序号，用ctrl+c跳出，`git log --pretty=oneline`

`git reset --hard HEAD^`回退一个版本，`HEAD^^`两个版本，

或用`git reset --hard XXXX`xxxx是commit序号

若已经关闭了bash，用`git reflog`显示之前的序号，并用`git reset --hard XXXX`必须用全部序号，不能简略。

#### 工作区，暂存区，分支，远程仓库

工作区（working directory）:arrow_forward:通过`git add *`推送至暂存区(stage):arrow_forward:通过`git commit -m 'message'`推送至分支:arrow_forward:通过`git push origin master`推送至远端仓库

`git diff`用于看工作区和暂存区的区别

 `git diff --cached`用于看暂存区和分支的区别



## 错误

1. > `fatal: remote origin already exists.` 

   `$ git remote rm origin`，之后 `$ git remote add git@github.com:lzbupt2008/NewRepository.git`

2. > 'If no other git process is currently running, this probably means a git process crashed in this repository earlier. Make sure no other git process is running and remove the file manually to continue.'

   将`.git`里的`index.lock`文件删除

   ## 语句

   ### 创建

   克隆 `git clone ssh key`

   创建`git init`

   ### 查看，修改与提交

   查看状态`git status`

   查看工作区与暂存区区别`git diff`

   查看暂存区与分支区别`git diff --cached`

   查看HEAD与文件的区别`git diff HEAD -- <filename>`，

   将改动从工作区全部提交给暂存区`git add .`

   将改动从暂存区全部提交给分支`git commit -m 'message'`

   将改动从分支全部提交给远程仓库`git push origin master`

   ### 撤销

   1. 在add之前，撤销工作区的修改`git checkout -- <filename>`，`git status`会告诉你可以add，或`git checkout -- <filename>`
   2. **head 是指自己工作流程的头端，可以指向工作区，暂存区，分支**，所以有查看HEAD与文件的区别`git diff HEAD -- <filename>`
   3. 在add之后，commit之前，即在暂存区时，`git status`会告诉你可以commit，或`git reset HEAD '<filename>' `回到工作区（HEAD），\<filename\>可以没有，直接写`git reset HEAD`。此时暂存区清除，`no changes added to commit`,回到1。
   4. commit之后，`git reset HEAD^`回退版本到上次commit的版本。`git log`看commit序号，`git log --pretty=oneline`，用ctrl+c跳出。`git reset --hard HEAD^`回退一个版本，`HEAD^^`两个版本。但此时只是回退到分支上，如果进行了push，那么远程仓库并没有回退！此时

   > Updates were rejected because the tip of your current branch is behind its remote counterpart.

   此时要先把远端仓库pull下来才能继续push或用`git reset --hard XXXX`xxxx是commit序号？

   5. 如果在工作区误删除了文件，可以用`git checkout -- <filename>`恢复文件，其实上述语句就是用版本库/分支（commit）上的文件代替工作区的文件，也可以起到撤销改动的作用。

   ### 分支/版本库

   1. 创建新分支`git branch <branch_name>`

   2. 切换分支`git checkout <branch_name>`

   3. 合二为一，创建+切换`git checkout -b <branch_name>`

   4. 在不同分支上做的不同修改，如果已经commit到了分支/版本库，那么切换分支（第2行）会恢复在该分支上的内容，比如在other分支上有test.txt，在another上没有，如果`git checkout another`,test.txt就会消失。

   5. 合并某分支到当前分支：`git merge -m 'message' <name>`

   6. 删除分支：`git branch -d <name>`

   7. 观看示意图`git log --graph --pretty=oneline --abbrev-commit`

   8. 禁用'fast-forward'

      fast-forward：新建了一个other的分支，并在其上进行一系列提交，完成时，回到 master分支，此时，master分支在创建other分支之后并未产生任何新的commit。此时将other合并到master上，这种合并就叫fast forward。合并完之后的视图为扁平状，看不出develop分支开发的任何信息

      在合并的时候使用--no-ff(no fast forward)禁止fast forward 模式

       `git merge  --no-ff -m 'message' branch_name`使用no-ff后，会多生成一个commit 记录，并强制保留other分支的开发记录（而fast-forward的话则是直接合并，看不出之前other的任何记录）。这对于以后代码进行分析特别有用。

   9. 当手头工作没有完成时并不想推送到版本库时，即在暂存区时，先`add`，然后把`git stash`一下，就会保存在stash中，然后去创建分支修复bug，修复后，再`git stash pop`，回到工作现场。

   10. 如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除

   11. ​

       ​