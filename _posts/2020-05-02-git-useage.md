---
layout: post
title: Servlet配置和尝试使用
date: 2020-05-02
categories: tools
tags: git github
---


# Git 分布式版本控制

### 安装

git官网

---



### 配置

配置文件为 `~/.gitconfig` ，执行任何Git配置命令后文件将自动创建。

第一个要配置的是你个人的用户名称和电子邮件地址。这两条配置很重要，每次 Git 提交时都会引用这两条信息，说明是谁提交了更新，所以会随更新内容一起被永久纳入历史记录：

**全局**

```text
git config --global user.email "2300071698@qq.com"
git config --global user.name "hundunren"
```

**当前项目**

```git
git config user.email "3124123112418@qq.com"
git config user.name "other"
```



---



### 常用命令

1. 初始化新仓库 `git init`
2. 克隆旧仓库 `git clone https://github.com/tututu-Panda/QACommunity.git`
3. 查看状态 `git status`
4. 提交单个文件 `git add index.php`
5. 提交所有文件 `git add -A`
6. 使用通配符提交 `git add *.js`
7. 提交到仓库中 `git commit -m '提示信息'`
8. 提交已经跟踪过的文件，不需要执行add `git commit -a -m '提交信息'`
9. 删除版本库与项目目录中的文件 `git rm index.php`
10. 只删除版本库中文件但保存项目目录中文件 `git rm --cached index.php`
11. 修改最后一次提交 `git commit --amend`



---



### 自定义提交规则

仓库名下新建文件`.gitignore`

```git
f.txt
!d.txt
/vendor
/vendor/*.php
/vendor/**/*.php
```

含义分别是

不提交`f.txt`文件

不提交除`d.txt`文件以外的文件

不提交`vendor/`文件夹

不提交`vendor/`文件夹下的所有php文件

不提交`vendor/`文件夹下的所有文件夹以及所有php文件



---



### 删除文件

`git rm a.txt` 

从版本库删除,本地也被移除了

`git rm --cached a.txt`

从版本库中移除文件,而本地不删除



---



### 修改文件名

`git mv readme.txt readme.md`

修改版本库中的文件名称



---



### log日志查看历史操作步骤

1. 查看日志 `git log`
2. 查看最近2次提交日志并显示文件差异 `git log -p -2`
3. 显示已修改的文件清单 `git log --name-only`
4. 显示新增、修改、删除的文件清单 `git log --name-status`
5. 一行显示并只显示SHA-1的前几个字符 `git log --oneline`



---



### amend修改最近一次提交

`git commit --amend`

修改最新一次提交



---



### 撤销在缓存区中的文件

`git rm --cached a.txt`

没有提交过,第一次放入暂存区中撤销

`git reset HEAD a.txt`

非第一次提交,从暂存区撤销

`git checkout -- a.txt`

恢复到上次提交的内容



---



### 修改别名

家目录下,隐藏文件`.gitconfig`

```git
[filter "lfs"]
	smudge = git-lfs smudge -- %f
	process = git-lfs filter-process
	required = true
	clean = git-lfs clean -- %f
[user]
	name = lfqtm
	email = 740942486@qq.com
[difftool "sourcetree"]
	cmd = '' \"$LOCAL\" \"$REMOTE\"
[mergetool "sourcetree"]
	cmd = "'' "
	trustExitCode = true
[alias]
	a = add .
	s = status
	l = log
	c = commit
```



---



### Git分支Branch

`git branch`

分支列表, HEAD所在分支会显示一个`*`号

`git branch -a`

显示远程分支

`git branch ask`

创建一个分支

`git checkout ask`

切换HEAD到改分支,简单来说,使用`git branch`该分支下会出现 * 号

` git checkout -b bbs`

*组合命令*, 创建分支并切换到该分支下



---



### 合并和删除分支

`git merge ask`

需要将HEAD移入到master分支下执行合并

`git branch  -d ask`

还是master下,删除ask分支



---



### 冲突(重点)

两个分支对master主分支的文件都进行了更改,在都需要合并的时候会出现文件冲突问题

```bat
D:\Documents\gitclone\test\hdd (master)
λ git merge ask
Auto-merging xj.txt
CONFLICT (content): Merge conflict in xj.txt
Automatic merge failed; fix conflicts and then commit the result.

D:\Documents\gitclone\test\hdd (master)
λ vim xj.txt # 编辑改文件,会打印出多分支的操作记录,并进行修改
```



### 查看分支是否以及强制删除

`git branch --merged`

查看哪些分支是合并了的

`git branch --no-merged`

查看哪些分支未合并

`git branch -D qa`

强制删除



---



### Git工作流

![image-20200501203440447](/images/posts/git/image-20200501203440447.png)

---



### stash临时缓存区

场景描述

存在两个分支 bbs 和 ask (不包括 master)

现在在ask中新建一个文件并提交了一次,然后有需要修改,但并未进行第二次提交

这时候,急需要切换HEAD到bbs下进行代码修复操作,这时候会出现

文件未提交问题

*解决方案*

`git stash`

建立当前分支的临时存储区

`git stash list`

临时存储区列表

切换bbs,并完成代码修复工作后

`git stash apply`

恢复缓存区, ask分支下

`git stash drop stash@{0}`

删除不必要的临时缓存区

`git stash pop`

*组合功能* : 恢复缓存区并删除



---



### tag标签

`git tag`

查看已添加的标签

`git tag v1.0.1`

添加版本号



---



### 生成master分支的压缩包

`git archive master --prefix='hdcms/' -format=zip > hdcms.zip`



---



### 系统别名

修改`~/.base_profile`



---



### rebase合理的优化分支合并

*场景*

从主分支的某个提交点(a提交点)分出一个分支以后

如果主分支没有产生任何其他的提交,分支合并不存在问题

**如果**主分支产生了其他的提交记录(最新的提交点为b),分支合并会产生问题

问题的解决方法需要分支作者将分支的提交点由之前的a提交点更改为b提交点

`git rebase master`

HEAD在分支下执行



---



### SSH秘钥

**生成秘钥**

使用ssh连接Github发送指令更加安全可靠，也可以免掉每次输入密码的困扰。

在命令行中输入以下代码（windows用户使用 Git Bash）

```text
ssh-keygen -t rsa
```

一直按回车键直到结束。系统会在`~/.ssh` 目录中生成 `id_rsa`和`id_rsa.pub`，即密钥`id_rsa`和公钥`id_rsa.pub`。

**向GitHub添加秘钥**

![1526219105062](http://houdunren.gitee.io/note/assets/img/1526219105062-4856466.7379665a.png)

点击 `New SSH key` 按钮，添加上面生成的 `id_rsa.pub` 公钥内容。

再使用git clone ssh 克隆远程项目, git push 提交



---



### 远程关联

1. 创建本地库并完成初始提交

   ```text
   echo "# hd-xj" >> README.md
   git init
   git add README.md
   git commit -m "first commit"
   ```

2. 添加远程仓库

   ```text
   git remote add origin git@github.com:houdunwang/xj.git
   ```

3. 查看远程库

   ```text
    git remote -v
   ```

4. 推送数据到远程仓库

   ```text
   git push -u origin master
   ```

5. 删除远程仓库关联

   ```text
   git remote rm origin
   ```

> 通过 clone 克隆的仓库，本地与远程已经自动关联，上面几步都可以省略。



---



### 本地分支推送到远程仓库

*本地*

`git checkout -b ask`

ask分支下完成一次本地仓库的提交

`git push`会报错

需要执行**提示**的远程仓库ask分支的创建



---



### 新入职员工参与项目开发时的分支使用

本地检出远程创建的正在开发的项目分支ask

`git branck -a`

先查看本地和远程的分支状况

`git pull origin ask:ask`

检出远程仓库的分支ask



---



### 远程分支的合并

本地已经完成ask分支的功能提交

`git checkout master`

切换到本地的master分支

`git pull`

更新本地仓库的master分支与远程一致

`git branch ask`

再切换到本地的ask分支下

`git rebase master`

将ask分支的起始点后移到master分支的最后提交位置

`git checkout master`

接着切换到master下,准备合并ask

`git merge ask`

合并ask

`git push`

完成推送

`git branck --merged`

查看已经完成合并的分支



---



### 删除远程分支

`git push origin --delete ask`

删除远程仓库的ask分支

`git branch -d ask`

删除本地ask分支



---



### Git自动部署

...暂时没服务器
