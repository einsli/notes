# Git 版本控制学习笔记

# 一、git 常用命令

## 1.1 查看帮助

git 或者git help 即可查看帮助信息

```bash
# git 

# git help
```

加上某一参数即可查看某一参数详细信息

如查看add 信息

```bash
# git help add
```

文档内容显示过长，按f键可以向下翻页，b想上翻页，按q退出

## 1.2 git配置

git 配置有三个范围

- 系统范围
- 全局范围 global
- 项目范围

一般用全局范围配置

1.2.1 配置全局用户信息

```bash
# git config --global user.name "einsli"
# git config --global user.email "einsli@123.com"
```

1.2.2 查看配置信息

```bash
# git config --list
```

output 如下

```
color.ui=true
user.name=einsli
user.email=einsli@123.com
credential.helper=cache  --timeout=200
filter.media.clean=git-media-clean %f
filter.media.smudge-git-media-smudge %f
alias.co=checkout
core.excludesfile=/Users/einsli/.gitignore_global
```

1.2.3 重新配置用户名，可使用 unset命令

```bash
# git config --unset --global user.name

# git config --list
```

output 如下

```
color.ui=true
user.email=einsli@123.com
credential.helper=cache  --timeout=200
filter.media.clean=git-media-clean %f
filter.media.smudge-git-media-smudge %f
alias.co=checkout
core.excludesfile=/Users/einsli/.gitignore_global
```

用户名选项已经移除

1.2.4 git 输出主题颜色配置

```bash
# git config --global color.ui true
```

1.2.5 产看git配置

git 配置会保存在当前用户主目录下面

```bash
# cat ~/.gitconfig
```

output 如下

```
[color]
				ui = true
[user]
				email = einsli@123.com
				name = einsli
[core]
[web]
[credential]
				helper = cache --timeout=200
[filter "media"]
				clean = git-media-clean %f
				smudge = git-media-smudge %f
[alias]
				co = check out
[core]
				excludesfile = /Users/einsli/.gitignore_global
```

1.2.6 git 别名设置

设置别名用alias，比如给check out 设置别名

```bash
# git config ==global alias.co check out
```

可以在系统级别设置别名, 当前平台MacOS平台，用户配置文件名为.bash_profile

```bash
# vim ~/.bash_profile

// 将 alias goc="git check out" 添加到第一行即可

// 退出 并更新
# souroce ~/.bash_profile
```

1.2.7 忽略跟踪文件 全局

设置全局列表中忽略的文件

```bash
# git config --global core.excludesfile=~/.gitignore_global
```

1.2.8 忽略跟踪文件 项目级别

在项目目录下，创建.gitignore

输入需要忽略的内容即可

比如忽略所有后缀名为.log的文件

.gitngnore

```
*.log
```

可参考如下地址给出的gitignore的模板

https://github.com/github/gitignore

## 1.3 项目操作

1.3.1 git init

例如当前目录有个test-server，需要进行git追踪

```
# cd test-server
# git init
```

output 如下

```
Initialized empty Git repository in /Users/einsli/Desktop/server-test/.git
```

生成之后，git会跟踪server-test目录下所有的目录变化，这样就可以使用git提供的版本控制

查看生成的.git文件

```bash
# ls .git
```

output如下

```
HEAD        config        hooks    objects
branches    description   info      refs
```

如果不想用git追踪此项目，删除.git文件即可

1.3.2 git commit 

项目提交

新创建一个index.html文件

提交前可查看当前状态信息

```bash
# git status
```

output 如下

```
On branch master

Initial commit

Untracked files:
  (use "git add <file>..." to include in what will be committed)
				index.html

nothing added to commit but untracked files present(use "git add" to track)
```

commit 之前先执行git add

```bash
# git add index.html
```

然后查看下状态

```bash
# git status
```

output如下

```
On branch master

Initial commit

Changes to be committed:
  (use "git tm --cached <file>..." to unstage)
		  new files: index.html
```

确认提交

```bash
# git commit -m '条件index.html'
```

output如下

```
1 file changed, 10 insertions(+)
create mode 100644 index.html
```

然后查看下状态

```bash
# git status
```

output如下

```
On branch master
nothing to commit, working directory clean1.
```

1.3.3 git log 

查看用户操作记录

```bash
# git log
# git log --oneline //每行显示一次提交
```

output 如下

```
commit dagdgakhregjakgjkf7423gdgdjk4334gue61
Author: einsli <einsli@123.com>
Date: Sun Nov 11:07:00 2020 +0800
		添加index.html文件
```

1.3.4 git diff

查看文件修改前和修改后的区别

比较的文件为工作区暂与存区(git add后里面的内容)里面的内容

```bash
# git diff index.html
```

如果想比较respository与暂存区里面的内容，则需要输入下面命令

```bash
# git diff --staged
```

1.3.5 git mv 

重命名git 已经追踪的文件

方法一

例如:

当前目录下新建style.css，添加内容后并提交

```bash
# git add style.css
# git commit -m "添加style.css"
```

在可视化界面将style.css重命名为them.css

查看当前状态

```bash
# git status
```

output 如下

```
On branch master
Changes not stages for commit:
		(use "git add/rm <file>..." to update what will be committed)
		(use "git checkout -- <file>..." to discard changes in working directory)
				deleted; sytle.css
Untracked files:
		(use "git add <file>..." to include in what will will be committed)
				theme.css
no changes added to commit (use "git add" and/or "git commit -a")
```

由此看来，git不知道通过可视化界面重命名了此文件

可以这样执行

```
# git rm style.css
// output
// rm 'style.css'
# git add theme.css
```

查看状态

```bash
# git status
```

```
Changes to be committed:
		(use "git reset HEAD <file>..." to unstage)
					renamed: style.css ->them.css
```

然后提交即可

```bash
# git commit -m "重命名style.css为them.css"
```

方法二 

git mv 

```bash
# git mv theme.css einsli-theme.css
```

然后产看状态

```bash
# git status
```

output 如下

```bash
Changes to be committed:
		(use "git reset HEAD <file>..." to unstage)
					renamed: theme.css ->einsli-them.css
```

然后提交即可

**git不会跟踪空白的目录**

在当前目录下创建css文件

```bash
# mkdir css
```

查看状态

```bash
# git status
```

output如下

```bash
On branch master
Nothing to commit, working directory clean
```

1.3.6 git rm

删除文件有两种方法

方法一

将文件通过可视化命令直接删除

方法二

git rm 文件名

删除单个文件

```bash
# git rm einsli-theme.css
```

1.3.7 恢复放放或修改的文件

比如删除index.html

```bash
# git rm index.html
```

删除后我们想恢复，可使用git checkout命令

```bash
# git checkout HEAD -- index.html
```

**注**: HEAD 表示当前提交，— 表示当前分支， index.html表示要恢复的文件

删除之后进行了提交怎么恢复文件？

比如删除了index.html，并进行了提交，要恢复index.html改怎么做?

需要把index.html恢复到上一次提交的状态

```bash
# git checkout HEAD^ -- index.html
```

**注**: HEAD^ 表示最近一次提交的上一次提交，HEAD^^表示最近一次提交的上两次提交

1.3.8 git revert 

恢复文件的历史版本

比如新添加了bootstrap框架，并进行了提交，后面不想用，想恢复之前的状态

```bash
# git log --oneline
```

output如下

```
b6b86f 添加了JQuery 1.2.1
bb95e87 添加了Bootstrap框架
aa5673a 恢复了index.html
```

不想用boorstrap框架，可以执行以下操作

```bash
# git revert bb95e87 
```

会弹出编辑器，添加描述内容或者直接保存后即可

然后在查看下提交日志

```bash
# git log --oneline
```

output如下

```
aaab1c0 Revert "添加了Bootstrap框架"
b6b86f 添加了JQuery 1.2.1
bb95e87 添加了Bootstrap框架
aa5673a 恢复了index.html
```

1.3.9 git reset

提交方式

- —soft 软重置：不会影响工作区与暂存区
- —hard ：他会把工作区与暂存区恢复到某个工作状态
- mixed：会把暂存区里面的恢复到某个状态

展示:

查看提交记录

```bash
# git log --oneline
```

output如下

```
aaab1c0 Revert "添加了Bootstrap框架"
b6b86f 添加了JQuery 1.2.1
bb95e87 添加了Bootstrap框架
aa5673a 恢复了index.html
```

使用git reset

— soft

```bash
#  git reset --soft b6b86
```

提交后会覆盖掉aaab1c0这次提交

— mixed

执行后暂存区里面的内容不存在了

**注** 与—soft项目，少了类似 git add的操作

—hard

相当于直接重置到b6b86提交后的状态

1.3.10 git stash

可以把修改暂时存到一个地方，保存工作进度

例如当前目录创建了一个新的txt文档，但是提交的时候，不想将当前状态提交

```bash
# git stash save "修改了txt文档"
```

查看保存的工作进度

```bash
# git stash list
```

output如下

```
stash{0}: On text-server: 修改了txt文档
```

查看保存工作进度与当前工作进度有什么区别

```bash
# git stash show -p stash{0}
```

**注** -p 以补丁的方式查看 stash{0}为stash工作代号

恢复工作进度

```
# git stash apply stash{0}
```

**注**：stash{0}为stash代号

删除工作进度

```
# git stash drop stash{0}
```

**注** stash{0}为工作代号

1.3.11 git log

查看提交的日志

查看详细信息

```bash
# git long
```

显示简单的日志列表

```bash
# git log --oneline
```

控制输出的行数，如显示最近的5条

```bash
# git log --oneline -5
```

可以查看指定作者的提交

```bash
# git log --online --author="einsli"
```

git log 也支持grep语法

比如显示关于index.html的提交

```bash
# git log --oneline --grep="index.html"
```

也可以指定日志

比如指定在2020年11月1号之前的提交

```bash
# git log --oneline --before="2020-11-01"
```

找出一个礼拜之前的提交

```bash
# git log --oneline --before="1 week"
```

找出三天前的提交

```bash
# git log --oneline --before="3 days"
```