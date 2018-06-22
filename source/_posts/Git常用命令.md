---
title: Git常用命令
date: 2018-03-08 15:11:48
tags:
---

![git图片](https://timgsa.baidu.com/timg?image&quality=80&size=b9999_10000&sec=1493224022490&di=4b9c36844b17e0c6116d1fd5aa883baf&imgtype=0&src=http%3A%2F%2Fimages.cnblogs.com%2Fcnblogs_com%2Fqyf404%2F612381%2Fo_git-logo.png)

<!--more-->

### Git 中 SSH key 生成步骤
```
$ ssh-keygen -t rsa -C "youremail@example.com"
```

# Git常用命令

```
初始化新版本库：git init
全局设置：git config --global user.name "xiao"  git config --global user.email "xiao.qq.com"
克隆版本库：git clone "url"
查看分支：git branch
创建分支：git branch branch_nema
切换分支：git checkout branch_name
创建+切换分支：git checkout -b branch_name
合并某分支到当前分支：git merge branch_name
重命名分支：git branch -m branch_name branch_new_name //不会覆盖已经存在的分支
重命名分支：git branch -M branch_name branch_new_name //会覆盖已经存在的分支
删除分支：git branch -d branch_name 
强制删除分支： git branch -D branch_name
删除远程分支： git push origin : branch_name //可以使用这种语法，推送一个空分支到远程分支，其实就相当于删除远程分支
拉取代码：git pull orgin branch_name
查看更改：git status
查看更改细节：git diff file_name
查看谁修改过代码：git blame filename
回到上次修改：git reset --hard
添加单个文件：git add filename.js
添加所有js文件：git add *.js
添加所有文件：git add .
提交添加的文件：git commit -m "your description about this branch"
提交单个文件：git commit -m "your description about this branch" filename.js
push分支：git push orgin your_branch_name
暂存当前分支内容：git stash
查看历史记录：git log
创建标签：git tag 1.0.0  //标签无法重命名
显示标签列表：git tag
切出标签：git checkout 1.0.0
删除标签：git tag -d 1.0.0
查看git远程地址：git remote -v
添加一个新源： git remote add temp [GIT URL]
更改远程git地址：git remote set-url origin [GIT URL]
删除源：git remote rm origin
提交内容修改：git commit --amend

```

### 提交到了错误的分支上的处理
```
第一种.
# 取消最新的提交，然后保留现场原状
git reset HEAD~
git stash

# 切换到正确的分支
git checkout name-of-the-correct-branch
git stash pop
git add .    # 或添加特定文件
git commit -m "你的提交说明"
# 现在你已经提交到正确的分支上了

第二种.
git co master
git co -b feature-xb-26.2
git cherry-pick 7b92f99473776d973c585b37175652659c2ea4b7
git ci -m '首页Enter事件添加'
git push temp feature-xb-26.2

```

### 命令行简写

```
$ git config --global alias.co checkout
$ git config --global alias.ci commit
$ git config --global alias.br branch
$ git config --global alias.st status
```

### 将远程的dev最新内容拉取下来

```
git fetch origin dev:issues/#663  拉取最新dev，并创建分支

git checkout issues/#663
```

### 切换之前的内容

```
git reset --hard 21ae772cb7690c876c8ce8bba16519dd674755b2
```

### 提交流程

```
git status

git add .

git commit -m "前端修改(refs #203)";

git pull --rebase origin dev

git push
```

### 解决冲突

```
git pull --rebase origin dev   与远程dev分支同步

both_modified   为冲突文件

git add .

git rebase --contine

----------
git add .

git rebase --continue
----------

git push -f
```

### 解决冲突2

```
1.切换到dev分支, 将远程的dev最新内容拉取下来

git checkout dev

git pull origin dev

2. 将dev最新代码合并到特性分支

git checkout issues/#ID

git merge dev (这里合并时,就会终端,需要手动合并)

both_modified就是冲突的文件

3. 手动解决冲突,  解决一个文件,就添加一个(git add /path/to/file)

所有冲突文件解决完后, 提交(可以用图形 或者命令行 git commit)

4. 推送到远程
git push origin issues/#ID
```

### 使用tig，查看git提交分支内容

```
brew install tig

j 向下 k 向上
git remote -v 查看Git仓库
```