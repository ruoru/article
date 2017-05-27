# Git简介
## Git历史
由[Linus Torvalds](https://github.com/torvalds)开发，用于托管Linux内核代码。于2005年4月3日开发，4月18日完成分支合并。

## Git与SVN
1. Git是分布式SCM，SVN是集中式。
2. Git每个历史版本存储完整的文件，SVN存储文件的差异。
3. Git可离线完成大部分操作，SVN则相反。
4. Git有着更优雅的分支和合并实现。
5. Git有更强的撤销修改和修改版本历史的能力。
6. Git速度更快，效率更高。

## 为什么学会使用Git
1. 越来越多的开源项目迁移到Git。
2. 代码版本控制。

# Git安装与简单配置
## 安装
[Git官网](https://git-scm.com/)
>➜  ~ which -a git  
/usr/bin/git  
➜  ~ git --version  
git version 2.11.0 (Apple Git-81)  

## 配置
### Git config
Git 提供了一个叫做 git config 的工具（译注：实际是 git-config 命令，只不过可以通过 git 加一个名字来呼叫此命令。），专门用来配置或读取相应的工作环境变量。而正是由这些环境变量，决定了 Git 在各个环节的具体工作方式和行为。这些变量可以存放在以下三个不同的地方：

- /etc/gitconfig 文件
系统中对所有用户都普遍适用的配置。若使用 git config 时用 --system 选项，读写的就是这个文件。
- ~/.gitconfig 文件
用户目录下的配置文件只适用于该用户。若使用 git config 时用 --global 选项，读写的就是这个文件。
- 当前项目的 Git 目录中的配置文件（也就是工作目录中的 .git/config 文件）
这里的配置仅仅针对当前项目有效。每一个级别的配置都会覆盖上层的相同配置，所以 .git/config 里的配置会覆盖 /etc/gitconfig 中的同名变量。

### Git 最基本配置
祖传代码查看对齐
打开 git config -e --global  
加入：  
>[core]  
	editor = code --wait  
[alias]  
  lg = log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit

### Git配置三个级别
1. git config --system
2. git config --global
3. git config --local

### Git配置的增删改查
>git config --global --add user.name xxxxx  
git config user.name  
=>xxxxx  
git config --get user.name  
=>xxxxx  
git config --list --global  
=>user.name = xxxxx  
git config --global --unset user.name  
git config --global --unset user.name xxxxx  
git config --global user.name yyyyy

### 为Git常用子和参数命令配置别名
>git config --global alias.co checkout  
git config --global alias.br branch  
git config --global alias.st status  
git config --global alias.ci commit  
git config --global alias.lol 'log --oneline'  

# Git基本的工作流程
git使用40个16进制字符的SHA-1 Hash来标识唯一对象。
1. blob 二进制或文本文件
2. tree 目录
3. commit 历史提交
4. tag 
tag -> commit -> tree（包含其他tree） -> 多个blob

获取Git仓库 
1. git init
2. git clone

Git工作区
1. 工作区 -add-> 暂存区
2. 暂存区 -commit-> 历史区
3. 历史区 -checkout-> 工作区

常用命令
1. git add 从工作区添加到暂存区
2. git commit 从暂存区添加到历史区
3. git status 查看暂存区和工作区别
4. git rm 从工作区和暂存区删除不需要的 只删除暂存区不删除工作目录上的 git rm --cached
5. git mv 在工作去重命名或移动文件，添加到暂存区。
6. git ignore 确保工作区不需要添加到暂存区和历史区

# Git暂存区
>git add  
git rm  
git mv  

# Git本地分支与合并
git branch 创建分支  
git tag 查看tag  
git tag '' commit(7位SHA-1 hex) 添加一个tag  
git checkout 切换分支  
git checkout -b 创建分支并切换到  
git checkout tagName 切换到tag  
git stash  
git stash save -a 'xxxxxx' 保存当前工作进度  
git stash list 查看当前工作进度记录  
git stash pop --index stash@{0} 取出工作记录并还原到工作区和暂存区，并清除工作区  
git stash save -a 'xxxxx' 保存当前工作记录  
git stash apply --index stash@{0} 还原到暂存区  
git stash drop stash@{0} 删除  
git stash clear 清除工作记录  
git merge 合并分支  
non-fast-farword merge | fast-farword merge  
git merge --abort 取消这次合并  
git

# 查看与对比历史记录
>git show  
git log  
git diff  

# 撤销修改
git checkout  
git rest  
git clean  
git revert  

# 重写历史记录
git commit --amend  
git rebase  
git reset  
git reflog  

## 编辑commit
git rebase -i origin/master

## 合并分支
|  
|  
master2    
|  
|  
master1 -- branchA  

将branchA合并到master分支上：
1. 切换到本地对应远程的master分支；
2. 拉取远程的更新并合并进本地master分支；
3. 切换到branchA分支，git rebase master；

## 删除文件夹以及该文件夹下所有文件
git rm 文件 -r -f

## 删除远程分支
```
git push [远程名]: [分支名]
eg: git push dev :add/class-startsAt-endsAt
```

## .gitignore
.gitignore是一个文件，.gitignore位置在项目根目录上，通常我们是不希望git将本地的编辑器缓存的内容，包括.git文件，以及庞大的node-modules安装包推送到远程，如果仓库里含有.gitignore这个文件，git push会识别哪些是你不想推送到远端的文件，并选择性不推送到远端
来看一个.gitignore文件
```
#忽略.DS_Store目录下的所有文件
.DS_Store
#忽略node_modules目录下的所有文件
node_modules
npm-debug.log
#仅仅忽略 demo/.svn文件
demo/.svn/
.sass-cache
#忽略所有以.swp结尾的文件
*.swp
.sw*
.idea/*
```

## git 修改远程仓库名称
```
git remote add =name= root@=ip=:=repositorieName=
```