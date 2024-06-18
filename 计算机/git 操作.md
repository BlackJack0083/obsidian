> 此处仅列举了常用的一些命令，详细请自行STFW和使用`man`命令

>[!note] 惨痛教训
>切记：下班前push代码，上班后第一件事要pull代码

相关网址：[【GeekHour】一小时Git教程](https://www.bilibili.com/video/BV1HM411377j/?p=5&share_source=copy_web&vd_source=7e5be2911b148305be8878070abd5138)
[十分钟学会正确的github工作流，和开源作者们使用同一套流程](https://www.bilibili.com/video/BV19e4y1q7JJ/?share_source=copy_web&vd_source=7e5be2911b148305be8878070abd5138)
- HEAD:  指向分支的最新提交节点
- HEAD~ 上一个版本

### 整套流程
```shell
git clone [域名] #复制整个仓库
git checkout -b my-feature
#非常重要！！！ 每次都要记得，不然直接在主分支上面改容易暴雷
#创建my-feature分支并跳转至该分支
git diff #比较分支与硬盘上的有什么区别，最好先看一下
git add [文件] #上传文件至暂存区
git commit # 提交commit
git push origin my-feature # 代码提交到远程仓库

# 如果主分支又有分支，需要进行同步
git checkout main # 切换到main
git pull origin master # 将主分支pull下来同步至local 
git switch my-feature
git rebase main # 变基，查看会不会有问题

git push -f origin my-feature # 由于有更改，要强制更新

# pull request
# squash and merge 多条commit合并
# delete branch
git branch -D my-feature # 删除my-feature分支
git pull origin master # 重新同步
```
##### 记录下失败的一次操作。。。
每次开别的仓库同步时总会出一些问题。。。这次记录一下：
```bash
 git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'github.com:BlackJack0083/obsidian.git'
```
这里的原因是当时创建仓库时候往里面丢了个文件。。。导致两边的文件不一致，一直没办法push上去，解决的办法是先`pull`下来进行同步
```bash
git push -u origin master
error: src refspec master does not match any
error: failed to push some refs to 'github.com:BlackJack0083/obsidian.git'
```
这里的大坑是`master`。。。一定要记得新开的仓库用的是`main`。。
解决：
```shell
 git pull --rebase origin main
#remote: Enumerating objects: 11, done.
#remote: Counting objects: 100% (11/11), done.
#remote: Compressing objects: 100% (9/9), done.
#remote: Total 11 (delta 0), reused 0 (delta 0), pack-reused 0
#Unpacking objects: 100% (11/11), 294.81 KiB | 384.00 KiB/s, done.
#From github.com:BlackJack0083/obsidian
# * branch            main       -> FETCH_HEAD
# * [new branch]      main       -> origin/main
Successfully rebased and updated refs/heads/main.
hejiakai@LAPTOP-EOPIIVUT:/mnt/c/Users/何嘉凯/Documents/obsidian Vault$ git push origin main
#Enumerating objects: 1401, done.
#Counting objects: 100% (1401/1401), done.
#Delta compression using up to 16 threads
#Compressing objects: 100% (1381/1381), done.
#Writing objects: 100% (1400/1400), 139.49 MiB | 1.03 MiB/s, done.
#Total 1400 (delta 38), reused 0 (delta 0), pack-reused 0
#remote: Resolving deltas: 100% (38/38), done.
To github.com:BlackJack0083/obsidian.git
   eefee17..e1ffafe  main -> main
```

### 创建仓库
`git init` 对仓库进行初始化
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git init
#hint: Using 'master' as the name for the initial branch. This default branch name
#hint: is subject to change. To configure the initial branch name to use in all
#hint: of your new repositories, which will suppress this warning, call:
#hint:
#hint:   git config --global init.defaultBranch <name>
#hint:
#hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
#hint: 'development'. The just-created branch can be renamed via this command:
#hint:
#hint:   git branch -m <name>
Initialized empty Git repository in /home/hejiakai/git_test/.git/
```
### 查看仓库状态
`git-status`
`-s` 简略查看状态 第一列表示暂存区状态，第二列表示工作区状态
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ ls
# 现在是空的
# 注意 .git是隐藏文件，需要在后面加上-a来查看
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ ls -a
.  ..  .git

hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git status
On branch master

No commits yet

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .vscode/
        helloworld
        helloworld.cpp

nothing added to commit but untracked files present (use "git add" to track)
```
### 添加到暂存区
`git add`
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add helloworld
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add ./*
```
### 取消暂存
`git rm --cached <文件名>`
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git rm --cached helloworld.cpp
rm 'helloworld.cpp'
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git status
On branch master

No commits yet

Changes to be committed:
  (use "git rm --cached <file>..." to unstage)
        new file:   helloworld

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        .vscode/
        helloworld.cpp
```
### 直接删除文件
`git rm <文件名>`将文件从工作区和暂存区删除
### 提交到本地仓库
`git commit -m "备注"`: 提交暂存区的文件，注意要有`-m "备注"`对提交信息进行说明(当然也可以用vim编辑信息)
`-am `参数 同时完成添加至暂存区和提交至仓库的两个操作
```bash
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add ./*
# 这里的 ./* 表示将所有文件均添加到暂存区，
# ./*.txt 意为将所有txt文件添加到暂存区
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git commit -m "初次提交，创建新文件，并创建基础网页模板"
[master (root-commit) 0bd5b45] 初次提交，创建新文件，并创建基础网页模板
 3 files changed, 21 insertions(+)
 create mode 100755 helloworld
 create mode 100644 helloworld.cpp
 create mode 100644 helloworld.html
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git status
On branch master
Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        .vscode/

nothing added to commit but untracked files present (use "git add" to track)
```
### 查看提交记录
`git log`
会给出commit 的id, Author和Date
`git log --oneline` 只显示每次提交的id和提交信息
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git log
commit 0bd5b45adbcfbbd8f6d9723f9ca0e4c8af3eedfd (HEAD -> master)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:17:07 2024 +0800

    初次提交，创建新文件，并创建基础网页模板
```
### 分支
`git branch` 既可以查看当前分支，也可以创建新的分支
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add ./*
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git commit -m "更换背景和字体"
[master e515ad9] 更换背景和字体
 2 files changed, 7 insertions(+)
 create mode 100644 style.css
 # 查看分支
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git branch
* master
*
# 创建新的分支
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git branch test_new_branch
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git branch
* master
  test_new_branch
  ```
### 切换分支
`git checkout`  可用于切换分支和恢复文件
`git switch` 专门用于切换分支
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git checkout -b test2_new_branch
Switched to a new branch 'test2_new_branch'
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git switch test_new_branch
Switched to branch 'test_new_branch'
# 切换到test分支
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git log
commit dd68df6e8b1bb1d3203b16fb43883275d191e441 (HEAD -> test_new_branch)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:37:00 2024 +0800

    新增按钮并设计样式

commit e515ad928bd53b7f5610de7c7505b6717bea71b0 (test2_new_branch, master)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:23:37 2024 +0800

    更换背景和字体

commit 0bd5b45adbcfbbd8f6d9723f9ca0e4c8af3eedfd
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
# 切回master分支
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git switch master
Switched to branch 'master'
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git log
commit e515ad928bd53b7f5610de7c7505b6717bea71b0 (HEAD -> master, test2_new_branch)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:23:37 2024 +0800

    更换背景和字体

commit 0bd5b45adbcfbbd8f6d9723f9ca0e4c8af3eedfd
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:17:07 2024 +0800

    初次提交，创建新文件，并创建基础网页模板
```
发现test分支上的新建按钮没有同步到master分支
### 分支合并
`git merge` 在主分支上合并其他分支
```shell
# 在 main 分支上合并 test 分支
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git merge test2_new_branch
Already up to date.
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git merge test_new_branch
Updating e515ad9..9bf4df8
Fast-forward
 helloworld.html |  1 +
 style.css       | 14 ++++++++++++++
 2 files changed, 15 insertions(+)
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git log
commit 9bf4df8d0ec1a681d35da749d43d18d60c95c613 (HEAD -> master, test_new_branch)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:40:10 2024 +0800

    新增h1样式

commit dd68df6e8b1bb1d3203b16fb43883275d191e441
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:37:00 2024 +0800

    新增按钮并设计样式

commit e515ad928bd53b7f5610de7c7505b6717bea71b0 (test2_new_branch)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>

hejiakai@LAPTOP-EOPIIVUT:~$ cd git_test
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git log
commit 9bf4df8d0ec1a681d35da749d43d18d60c95c613 (HEAD -> master, test_new_branch)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:40:10 2024 +0800

    新增h1样式

commit dd68df6e8b1bb1d3203b16fb43883275d191e441
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:37:00 2024 +0800

    新增按钮并设计样式

commit e515ad928bd53b7f5610de7c7505b6717bea71b0 (test2_new_branch)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:23:37 2024 +0800
```
但有的时候会发生**合并冲突**情况：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git commit -m "first feat commit"
[feat 26b70d9] first feat commit
 6 files changed, 6 insertions(+), 2 deletions(-)
 create mode 160000 my-repo
 create mode 160000 my-repo2
 create mode 160000 remote-repo
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git switch master
warning: unable to rmdir 'my-repo': Directory not empty
warning: unable to rmdir 'my-repo2': Directory not empty
warning: unable to rmdir 'remote-repo': Directory not empty
Switched to branch 'master'
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ cat helloworld.html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <link rel="stylesheet" href="./style.css">
</head>
<body>
    <h1>Hello World</h1>
    <p>
        My name is chenluoxuan
        nihao
    </p>
    <button>HELLOWORLD</button>
</body>
</html>hejiakai@LAPTOP-EOPIIVUT:~/git_test$ vim helloworld.html
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git commit -am
error: switch `m` requires a value
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git commit -am "main commit first"
[master 1a83d9e] main commit first
 1 file changed, 2 insertions(+), 2 deletions(-)
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git merge feat
Auto-merging helloworld.html
CONFLICT (content): Merge conflict in helloworld.html
Automatic merge failed; fix conflicts and then commit the result.
```
这时候可以比较两个文件差异：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git diff
diff --cc helloworld.html
index 448f62a,f6c530a..0000000
--- a/helloworld.html
+++ b/helloworld.html
@@@ -9,7 -9,7 +9,11 @@@
  <body>
      <h1>Hello World</h1>
      <p>
++<<<<<<< HEAD
 +        My name is hejiakai.
++=======
+         
++>>>>>>> feat
          nihao
      </p>
      <button>HELLOWORLD</button>
```
使用vim进行手动合并，然后再来看看发生了什么
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ vim helloworld.html
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add .
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git commit -m "merge conflict"
[master 40f0cad] merge conflict
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git status
On branch master
nothing to commit, working tree clean
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git log
commit 40f0cad7addd553771d75517b534d83610fde70f (HEAD -> master)
Merge: 1a83d9e 26b70d9
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Sat Mar 9 11:09:50 2024 +0800

    merge conflict

commit 1a83d9e0216705b5c3dadd8b34b75e4cdda4fc9d
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Sat Mar 9 11:02:51 2024 +0800

    main commit first

commit 26b70d932a2dea53edc3419d3a8a8c1619cdc6fb (feat)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Sat Mar 9 10:51:53 2024 +0800

    first feat commit
```
### 变基
![[微信图片_20240309111414.png|500]]
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git log --oneline --graph --decorate --all
*   40f0cad (HEAD -> master) merge conflict
|\  
| * 26b70d9 first feat commit
* | 1a83d9e main commit first
|/  
* 6c725f0 ignore file sample
* 9bf4df8 新增h1样式
* dd68df6 新增按钮并设计样式
* e515ad9 更换背景和字体
* 0bd5b45 初次提交，创建新文件，并创建基础网页模板
```
回退到之前没有merge的时候：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git reset 26b70d9
Unstaged changes after reset:
M       helloworld.html
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git rebase master
error: cannot rebase: You have unstaged changes.
error: Please commit or stash them.
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git commit -m "commit"
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   helloworld.html

no changes added to commit (use "git add" and/or "git commit -a")
# 需要先add 才能commit
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add .
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git commit -m "feat commit"
[master 570b446] feat commit
 1 file changed, 6 insertions(+), 2 deletions(-)
 # 使用rebase 命令
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git rebase master
Current branch master is up to date.
# 现在被合并成一条线了
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git log --oneline --graph --decorate --all
* 570b446 (HEAD -> master) feat commit
* 26b70d9 first feat commit
* 6c725f0 ignore file sample
* 9bf4df8 新增h1样式
* dd68df6 新增按钮并设计样式
* e515ad9 更换背景和字体
* 0bd5b45 初次提交，创建新文件，并创建基础网页模板
```
优缺点评价：
![[微信截图_20240309112159.png|500]]
![[微信截图_20240309112146.png|500]]
一般多人协作用`merge`比较好
### 回退命令2
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git revert dd68df6e8b1bb1d3203b16fb43883275d191e441
Auto-merging style.css
CONFLICT (content): Merge conflict in style.css
error: could not revert dd68df6... 新增按钮并设计样式
hint: After resolving the conflicts, mark them with
hint: "git add/rm <pathspec>", then run
hint: "git revert --continue".
hint: You can instead skip this commit with "git revert --skip".
hint: To abort and get back to the state before "git revert",
hint: run "git revert --abort".
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add./*
git: 'add./*' is not a git command. See 'git --help'.
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add ./*
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git revert --continue
[master a550b99] Revert "新增按钮并设计样式"
 2 files changed, 15 deletions(-)
 ```
### 分支删除
`git branch -d <分支名>` 如果一个分支不被需要了，就将其删除，前提是已经被合并
如果未被合并，需要改为`-D`进行强制删除
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ ls
access.log  helloworld  helloworld.cpp  helloworld.html  my-repo  my-repo2  other.log  remote-repo  style.css
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git branch
* master
  test2_new_branch
  test_new_branch
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git branch -d test2_new_branch
Deleted branch test2_new_branch (was e515ad9).
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git branch
* master
  test_new_branch
# 发现test2分支已经被删除
```
### 版本回退
 `git reset`
 默认mixed
 `git reflog`可以查看操作记录，防止误操作
 ![[微信截图_20240308144414.png|500]]
```bash
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git reset 9bf4df8d0ec1a681d35da749d43d18d60c95c613
Unstaged changes after reset:
M       helloworld.html
M       style.css
# 回到了上一版本，显示reset后后两个改变没有提交
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   helloworld.html
        modified:   style.css

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        .vscode/

no changes added to commit (use "git add" and/or "git commit -a")

hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git reset dd68df6e8b1bb1d3203b16fb43883275d191e441
Unstaged changes after reset:
M       helloworld.html
M       style.css
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git reset 9bf4df8d0ec1a681d35da749d43d18d60c95c613 --hard
HEAD is now at 9bf4df8 新增h1样式
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git reset dd68df6e8b1bb1d3203b16fb43883275d191e441 --hard
HEAD is now at dd68df6 新增按钮并设计样式
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git reset 9bf4df8d0ec1a681d35da749d43d18d60c95c613
Unstaged changes after reset:
M       style.css
```

```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git checkout 9bf4df8d0ec1a681d35da749d43d18d60c95c613
M       style.css
Note: switching to '9bf4df8d0ec1a681d35da749d43d18d60c95c613'.

You are in 'detached HEAD' state. You can look around, make experimental
changes and commit them, and you can discard any commits you make in this
state without impacting any branches by switching back to a branch.

If you want to create a new branch to retain commits you create, you may
do so (now or later) by using -c with the switch command. Example:

  git switch -c <new-branch-name>

Or undo this operation with:

  git switch -

Turn off this advice by setting config variable advice.detachedHead to false

HEAD is now at 9bf4df8 新增h1样式
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git log
commit 9bf4df8d0ec1a681d35da749d43d18d60c95c613 (HEAD, test_new_branch, master)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:40:10 2024 +0800

    新增h1样式

commit dd68df6e8b1bb1d3203b16fb43883275d191e441
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:37:00 2024 +0800

    新增按钮并设计样式

commit e515ad928bd53b7f5610de7c7505b6717bea71b0 (test2_new_branch)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:23:37 2024 +0800

    更换背景和字体

commit 0bd5b45adbcfbbd8f6d9723f9ca0e4c8af3eedfd
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:17:07 2024 +0800
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git all ./*
git: 'all' is not a git command. See 'git --help'.

The most similar command is
        pull
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add ./*
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git commit -m "更换按钮颜色"
[detached HEAD d2540b0] 更换按钮颜色
 1 file changed, 1 insertion(+), 8 deletions(-)
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git log
commit d2540b0b366b7a3c74dc144c5bac74f133afecce (HEAD)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 11:57:21 2024 +0800

    更换按钮颜色

commit 9bf4df8d0ec1a681d35da749d43d18d60c95c613 (test_new_branch, master)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:40:10 2024 +0800

    新增h1样式

commit dd68df6e8b1bb1d3203b16fb43883275d191e441
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:37:00 2024 +0800

    新增按钮并设计样式

commit e515ad928bd53b7f5610de7c7505b6717bea71b0 (test2_new_branch)
Author: 202211079261-He Jiakai <202211079261@mail.bnu.edu.cn>
Date:   Fri Mar 8 10:23:37 2024 +0800
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git switch master
Warning: you are leaving 1 commit behind, not connected to
any of your branches:

  d2540b0 更换按钮颜色

If you want to keep it by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> d2540b0

Switched to branch 'master'
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git branch test d2540b0
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git branch
* master
  test
  test2_new_branch
  test_new_branch
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git switch test
Switched to branch 'test'
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git switch master
Switched to branch 'master'
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git branch --delete test
error: The branch 'test' is not fully merged.
If you are sure you want to delete it, run 'git branch -D test'.
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git branch -D test
Deleted branch test (was d2540b0).
```
### 查看版本差异
`git diff`
虽然一般都采用图形化界面(比如vscode的git graph之类的)来查看差异，但是diff格式还是有必要了解一下
- 什么都不加：默认比较**工作区和暂存区**的内容，显示发生更改的文件与更改的详细信息
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add helloworld.cpp
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git diff
diff --git a/helloworld.cpp b/helloworld.cpp
index aa227c8..3290792 100644
--- a/helloworld.cpp
+++ b/helloworld.cpp
@@ -3,4 +3,5 @@ using namespace std;
 
 int main(void){
     cout <<"hello world" << endl;
+    cout << "一键三连了吗" << endl;
 }
\* No newline at end of file
```
```shell
# 添加到暂存区后比较，发现没有内容输出，说明内容是相同的
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add ./*
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git diff
```
- `HEAD`比较和版本库的差异
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git diff HEAD
diff --git a/helloworld.cpp b/helloworld.cpp
index aa227c8..3290792 100644
--- a/helloworld.cpp
+++ b/helloworld.cpp
@@ -3,4 +3,5 @@ using namespace std;
 
 int main(void){
     cout <<"hello world" << endl;
+    cout << "一键三连了吗" << endl;
 }
\ No newline at end of file
```
- `--cached` 比较暂存区和版本库
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git diff --cached
diff --git a/helloworld.cpp b/helloworld.cpp
index aa227c8..3290792 100644
--- a/helloworld.cpp
+++ b/helloworld.cpp
@@ -3,4 +3,5 @@ using namespace std;
 
 int main(void){
     cout <<"hello world" << endl;
+    cout << "一键三连了吗" << endl;
 }
```
- `<提交id 提交id>` 可以查看两次提交的区别
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git diff 0bd5b45adbcfbbd8f6d9723f9ca0e4c8af3eedfd HEAD
diff --git a/helloworld.html b/helloworld.html
index 0552622..c60d2d8 100644
--- a/helloworld.html
+++ b/helloworld.html
@@ -4,6 +4,7 @@
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Document</title>
+    <link rel="stylesheet" href="./style.css">
 </head>
 <body>
     <h1>Hello World</h1>
@@ -11,5 +12,6 @@
         My name is chenluoxuan
         nihao
     </p>
+    <button>HELLOWORLD</button>
 </body>
 </html>
\ No newline at end of file
diff --git a/style.css b/style.css

hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git diff 0bd5b45adbcfbbd8f6d9723f9ca0e4c8af3eedfd HEAD
diff --git a/helloworld.html b/helloworld.html
index 0552622..c60d2d8 100644
--- a/helloworld.html
+++ b/helloworld.html
@@ -4,6 +4,7 @@
     <meta charset="UTF-8">
     <meta name="viewport" content="width=device-width, initial-scale=1.0">
     <title>Document</title>
+    <link rel="stylesheet" href="./style.css">
 </head>
 <body>
     <h1>Hello World</h1>
@@ -11,5 +12,6 @@
         My name is chenluoxuan
         nihao
     </p>
+    <button>HELLOWORLD</button>
 </body>
 </html>
\ No newline at end of file
diff --git a/style.css b/style.css

# 查看前两个版本
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git diff HEAD~2 HEAD
diff --git a/helloworld.html b/helloworld.html
index 852af89..c60d2d8 100644
--- a/helloworld.html
+++ b/helloworld.html
@@ -12,5 +12,6 @@
         My name is chenluoxuan
         nihao
     </p>
+    <button>HELLOWORLD</button>
 </body>
 </html>
\ No newline at end of file
diff --git a/style.css b/style.css
index b6ccc16..2d6573f 100644
--- a/style.css
+++ b/style.css
@@ -3,4 +3,18 @@ body{
     font-family: Arial, Helvetica, sans-serif;
     margin: 0;
     padding: 0%;
+}
```

### .gitignore文件
用于添加一些不希望被加入到暂存区的文件
![[微信截图_20240308231336.png|500]]
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ echo "other log" > other.log
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   helloworld.cpp

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        .vscode/
        other.log

hejiakai@LAPTOP-EOPIIVUT:~/git_test$ echo "access log" > access.log
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ echo access.log > .gitignore
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ cat .gitignore
access.log
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git status
On branch master
Changes to be committed:
  (use "git restore --staged <file>..." to unstage)
        modified:   helloworld.cpp

Untracked files:
  (use "git add <file>..." to include in what will be committed)
        .gitignore
        .vscode/
        other.log
# 发现此时access.log文件消失，只有other.log文件
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git add .
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git commit -m "ignore file sample"
[master 6c725f0] ignore file sample
 4 files changed, 31 insertions(+)
 create mode 100644 .gitignore
 create mode 100644 .vscode/tasks.json
 create mode 100644 other.log
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git status
On branch master
nothing to commit, working tree clean
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git ls-files
.gitignore
.vscode/tasks.json
helloworld
helloworld.cpp
helloworld.html
other.log
style.css
```
一般会使用通配符来修改gitignore
gitignore只能对未被添加到版本库的文件生效，如果文件已经被添加到版本库中，那么对其进行修改，git仍能进行追踪

忽略文件夹：
```vim
temp\
```

### github远程仓库
这里我之前创建过ssh密钥了...由于有点怕出问题，所以只是记录了下代码
```shell
cd 
cd .ssh
ssh-keygen -t rsa -b 4096
# 生成密钥
# 连接
 git clone git@github.com:BlackJack0083/remote-repo.git
Cloning into 'remote-repo'...
warning: You appear to have cloned an empty repository.
# 这里显示已clone
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git clone git@github.com:BlackJack0083/remote-repo.git
fatal: destination path 'remote-repo' already exists and is not an empty directory.
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ ls
access.log  helloworld  helloworld.cpp  helloworld.html  other.log  remote-repo  style.css
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ cd remote-repo
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ ls
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git ls-files
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ echo hello > hello.txt
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git add .
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git commit -m "first commit"
[main (root-commit) ef11931] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git ls-files
hello.txt
# 此时并没有同步到远程仓库，只是在本地仓库中
```
### push/pull命令
![[微信截图_20240308235716.png|475]]
- `push` 将本地仓库修改推送到远程仓库
- `pull` 将远程仓库修改拉取到本地仓库

注意这里有个坑，每次一定要先pull再push，否则会像这样：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git push
To github.com:BlackJack0083/remote-repo.git
 ! [rejected]        main -> main (fetch first)
error: failed to push some refs to 'github.com:BlackJack0083/remote-repo.git'
hint: Updates were rejected because the remote contains work that you do
hint: not have locally. This is usually caused by another repository pushing
hint: to the same ref. You may want to first integrate the remote changes
hint: (e.g., 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
这是因为我之前丢了个txt文件到远程仓库中，但是本地没有pull。。。。造成了提交冲突
这时候进行push操作也会报错：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git push
To github.com:BlackJack0083/remote-repo.git
 ! [rejected]        main -> main (non-fast-forward)
error: failed to push some refs to 'github.com:BlackJack0083/remote-repo.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

STFW解决办法：
1. 将自己新写的代码备份到其他地方。
2. 删除本地项目里自己新写的代码。 
3. git pull 下拉代码，使本地代码与远端代码一致。
4. 重新上传代码

BTW, 实际使用的时候...
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git rm hello.txt
rm 'hello.txt'
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git ls-files
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git pull
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Unpacking objects: 100% (3/3), 2.06 KiB | 2.06 MiB/s, done.
From github.com:BlackJack0083/remote-repo
 * [new branch]      main       -> origin/main
hint: You have divergent branches and need to specify how to reconcile them.
hint: You can do so by running one of the following commands sometime before
hint: your next pull:
hint:
hint:   git config pull.rebase false  # merge (the default strategy)
hint:   git config pull.rebase true   # rebase
hint:   git config pull.ff only       # fast-forward only
hint:
hint: You can replace "git config" with "git config --global" to set a default
hint: preference for all repositories. You can also pass --rebase, --no-rebase,
hint: or --ff-only on the command line to override the configured default per
hint: invocation.
fatal: Need to specify how to reconcile divergent branches.
```
又报错了....

再次STFW，意思是：
```shell
➜git:(test) git pull origin test
提示:您有不同的分支，需要指定如何协调它们。
提示:您可以通过在之前某个时间运行以下命令之一来做到这一点
提示:你的下一招:
提示:
提示:git config pull.rebase false 	# 合并(默认策略)
提示:git config pull.rebase true  	# Rebase
提示:git config pull.ff only	 	# 仅快进
提示:
提示:可以将“git config”替换为“git config——global”来设置默认值
提示:首选所有存储库。你也可以传递——rebase，——no-rebase，
提示:或命令行上的——ff-only，以覆盖配置的默认per
提示:调用。
fatal:需要指定如何协调不同的分支。
```
这是由于拉取pull分支前，进行过merge合并更新分支操作，而其他人在你之前已经push过一个版本，导致版本不一致

STFW找到了一些解决办法：
第一种解决方法：比较简单
- 执行`git config pull.rebase false`
- 默认将pull下来的代码与现有改动的代码进行合并
- 但是可能会造成代码冲突，需要处理下这个问题，代码冲突如果2个人都改了同一个文件，需要联系之前push的同学，看看这块代码怎么保存(啊这里因为只有我一个人写了，so直接跳过)

BTW，这么做之后
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git config pull.rebase false
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git ls-files
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git pull
fatal: refusing to merge unrelated histories
（拒绝合并不相关的历史）
```
呃还是有fatal

第二种解决方法(stackoverflow)：
>I think you have committed in the remote repository, and when you pull, this error happens.
>在pull命令后紧接着使用--allow-unrelated-history选项来解决问题（该选项可以合并两个独立启动仓库的历史）
>Use this command:
```shell
git pull origin master --allow-unrelated-histories
git merge origin origin/master
```

BTW
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git pull origin master --allow-unrelated-histories
fatal: couldn\'t find remote ref master
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git merge origin origin/master
merge: origin - not something we can merge
```

有点头大，开始想把这个文件夹删除或者断开连接...
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ ls
access.log  helloworld  helloworld.cpp  helloworld.html  other.log  remote-repo  style.css
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ rm remote-repo
rm: cannot remove 'remote-repo': Is a directory
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ rmdir remote-repo
rmdir: failed to remove 'remote-repo': Directory not empty
```
删除貌似不太行...尝试断开连接
查找到如下命令：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git remote -v
# 只有当断开或者不在git仓库时显示为空
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ cd remote-repo
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git remote -v
origin  git@github.com:BlackJack0083/remote-repo.git (fetch)
origin  git@github.com:BlackJack0083/remote-repo.git (push)
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git remote rm origin
# 断开连接
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git remote -v
# 没有内容了，说明断开连接了
```
...感觉好像成功了？然后我又犯病了...
```shell
# 再次clone
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git clone git@github.com:BlackJack0083/remote-repo.git
Cloning into 'remote-repo'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
# 尝试pull，这次成功了
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=<remote>/<branch> main

```

然鹅...当我尝试add的时候，发现非常致命的问题：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ echo hello > hello.txt
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git add .
warning: adding embedded git repository: remote-repo
hint: You've added another git repository inside your current repository.
hint: Clones of the outer repository will not contain the contents of
hint: the embedded repository and will not know how to obtain it.
hint: If you meant to add a submodule, use:
hint:
hint:   git submodule add <url> remote-repo
hint:
hint: If you added this path by mistake, you can remove it from the
hint: index with:
hint:
hint:   git rm --cached remote-repo
hint:
hint: See "git help submodule" for more information.
```
问题是....我把clone放在了remote-repo里面....这样里面就有了两个git仓库了....犯病了
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ l
hello.txt  remote-repo/
# 这个remote-repo/ 是remote-repo里面的文件夹啊....
```
尝试一系列操作...均以失败告终：
```shell
# 一开始想把里面这个文件夹给删除，但是失败了...
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ rm hello.txt
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ cd remote-repo
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo/remote-repo$ git remote -v
origin  git@github.com:BlackJack0083/remote-repo.git (fetch)
origin  git@github.com:BlackJack0083/remote-repo.git (push)
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo/remote-repo$ git remote rm origin
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo/remote-repo$ cd ..
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ ls
remote-repo
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ rmdir remote-repo
rmdir: failed to remove 'remote-repo': Directory not empty
# 删不掉？进去看看
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ cd remote-repo
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo/remote-repo$ ls
三创赛第一次会议复盘.txt
# ！这里竟然有之前Pull下来的txt文件，删掉再试试
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo/remote-repo$ rm 三创赛第一次会议复盘.txt
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo/remote-repo$ cd ..
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ rmdir remote-repo
rmdir: failed to remove 'remote-repo': Directory not empty
# 还是不行，有隐藏文件？
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ cd remote-repo
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo/remote-repo$ ls -a
.  ..  .git
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo/remote-repo$ sudo rm .git
[sudo] password for hejiakai:
rm: 无法删除 '.git': 是一个目录
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo/remote-repo$ rmdir .git
rmdir: failed to remove '.git': Directory not empty
# 闭环了。。。.git也是个文件夹，开始想要暴力删除
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo/remote-repo$ cd ..
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ cd ..
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ rm -f remote-repo
rm: cannot remove 'remote-repo': Is a directory
# remote-repo是文件夹，不能用rm命令啊
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ rmdir -f remote-repo
rmdir: invalid option -- 'f'
Try 'rmdir --help' for more information.
# ？ rmdir 不能强制删除吗？ 看看help 
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ rmdir --force remote-repo
rmdir: unrecognized option '--force'
Try 'rmdir --help' for more information.
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ rmdir --help
Usage: rmdir [OPTION]... DIRECTORY...
Remove the DIRECTORY(ies), if they are empty.

      --ignore-fail-on-non-empty
                  ignore each failure that is solely because a directory
                    is non-empty
  -p, --parents   remove DIRECTORY and its ancestors; e.g., 'rmdir -p a/b/c' is
                    similar to 'rmdir a/b/c a/b a'
  -v, --verbose   output a diagnostic for every directory processed
      --help     display this help and exit
      --version  output version information and exit

GNU coreutils online help: <https://www.gnu.org/software/coreutils/>
Report any translation bugs to <https://translationproject.org/team/>
Full documentation <https://www.gnu.org/software/coreutils/rmdir>
or available locally via: info '(coreutils) rmdir invocation'
```
最后发现rmdir好像没有强制删除.....玉玉了

再次STFW，发现rm好像有不同的用法
> rm （remove）删除文件或目录:
```shell
rm  -d      （-directory）         #直接把需删除的目录的硬连接数据删成0，删除该目录 
rm  -f      （--force）            #强制删除文件或目录：忽略不存在的文件，不提示确认
rm  -i      （interactive）        #删除既有文件或目录之前先询问用户 
rm  -r或-R  （--recursive）        #递归删除，防止目录里面有文件不能删除
rm  -rf                           #递归强制删除非空文件夹 
```

那就再来试试：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ rm -rf remote-repo
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ ls
access.log  helloworld  helloworld.cpp  helloworld.html  other.log  style.css
```
好像有戏？
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git clone git@github.com:BlackJack0083/remote-repo.git
Cloning into 'remote-repo'...
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Compressing objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ ls
access.log  helloworld  helloworld.cpp  helloworld.html  other.log  remote-repo  style.css
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ git remote-repo
git: 'remote-repo' is not a git command. See 'git --help'.
hejiakai@LAPTOP-EOPIIVUT:~/git_test$ cd remote-repo
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git pull
Already up to date.
```
能够成功clone，看看里面的信息：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git ls-files
"\344\270\211\345\210\233\350\265\233\347\254\254\344\270\200\346\254\241\344\274\232\350\256\256\345\244\215\347\233\230.txt"
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ ls
三创赛第一次会议复盘.txt
```
？这个 ls-files得到的这个是什么？
`ls-files` **功能：输出暂存区的文件信息，或者输出暂存区与工作区之间的差异化信息**

？一开始在暂存区的是什么呢，我还没有add内容呢？
> 这个地方还米有找到好的解释，可能是编码的问题？

不过现在可以正常进行add,commit了：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ echo hello > hello.txt
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git add .
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git commit -m "first commit"
[main 919d51e] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 hello.txt
```
再看看能不能push:
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo$ git push
Enumerating objects: 4, done.
Counting objects: 100% (4/4), done.
Delta compression using up to 16 threads
Compressing objects: 100% (2/2), done.
Writing objects: 100% (3/3), 314 bytes | 314.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:BlackJack0083/remote-repo.git
   13c096a..919d51e  main -> main
```
成功噜

> [!info] 
> 虽然成功了，但是emm感觉这种方法估计很暴力，建议再找找有没有别的好办法
> 啊最大的教训是: 一定要记得pull !!!

### 关联本地仓库和远程仓库
啊这里有详细说到之前的remote指令是干啥的
`git remote add <shortname><url>` 添加一个远程仓库
`git remote -v` 查看当前仓库对应的远程仓库的别名和地址
```shell
# 截一下之前的例子
hejiakai@LAPTOP-EOPIIVUT:~/git_test/remote-repo/remote-repo$ git remote -v
origin  git@github.com:BlackJack0083/remote-repo.git (fetch)
origin  git@github.com:BlackJack0083/remote-repo.git (push)
```

`git branch -M main` 指定分支为main
`git push -u origin main:main`: 将本地仓库与远程仓库关联起来

```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git init
hint: Using 'master' as the name for the initial branch. This default branch name
hint: is subject to change. To configure the initial branch name to use in all
hint: of your new repositories, which will suppress this warning, call:
hint:
hint:   git config --global init.defaultBranch <name>
hint:
hint: Names commonly chosen instead of 'master' are 'main', 'trunk' and
hint: 'development'. The just-created branch can be renamed via this command:
hint:
hint:   git branch -m <name>
Initialized empty Git repository in /home/hejiakai/git_test/my-repo/.git/
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git remote add origin git@github.com:BlackJack0083/first-repo.git
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git remote -v
origin  git@github.com:BlackJack0083/first-repo.git (fetch)
origin  git@github.com:BlackJack0083/first-repo.git (push)
# 糟糕的部分来了
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'github.com:BlackJack0083/first-repo.git'
```
emmm明明按照教程做，但是为啥还是报错？
STFW，原因是我们处在老版本，里面并没有main分支，只有master分支
> GitHub is working on replacing the term “master” on its service with a neutral term like “main” to avoid any unnecessary references to slavery

执行`git branch -m master main`，尝试将**master**改为**main**，继续push看看
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git branch -m master main
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'github.com:BlackJack0083/first-repo.git'
```
呃，还是报错呢
再来STFW，发现好像又回到了一开始找的答案：
> 试着合并初始提交与你的提交,这也是我**最推荐的方法**：
```shell
git fetch example
git merge --allow-unrelated-histories example/main
```

那试一试：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git fetch first-repo
fatal: 'first-repo' does not appear to be a git repository
fatal: Could not read from remote repository.

Please make sure you have the correct access rights
and the repository exists.
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git merge --allow-unrelated-histories example/main
merge: example/main - not something we can merge
```
呃，有点破防了

还给了一种方法哎，试一试：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git push --force
fatal: The current branch main has no upstream branch.
To push the current branch and set the remote as upstream, use

    git push --set-upstream origin main
```
呃还是不行呢，那查查报错原因吧
>[!note] 小心
>不建议用csdn呢...或者用的时候最好还是看看评论...有时候给的指令可能会让一天的活白干
>还是看看google和stackoverflow叭

好家伙...stackoverflow高赞也说是main和master的问题，但是呃两个都试过还是不行哎：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'github.com:BlackJack0083/first-repo.git'
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git branch -m main
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'github.com:BlackJack0083/first-repo.git'
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git push -u origin master
error: src refspec master does not match any
error: failed to push some refs to 'github.com:BlackJack0083/first-repo.git'
```
还有人说是没有初始化add 和 commit导致的...
BTW：
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git add .
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git commit -m "Second commit"
On branch main

Initial commit

nothing to commit (create/copy files and use "git add" to track)
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo$ git push -u origin main
error: src refspec main does not match any
error: failed to push some refs to 'github.com:BlackJack0083/first-repo.git'
```
喔，奇妙的是，添加了一个**readme文档**后就可以正常连接了
```shell
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo2$ echo "# first-repo" >> README.md
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo2$ git add README.md
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo2$ git commit -m "first commit"
[master (root-commit) eada23a] first commit
 1 file changed, 1 insertion(+)
 create mode 100644 README.md
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo2$ git branch -M main
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo2$ git remote add origin git@github.com:BlackJack0083/first-repo.git
error: remote origin already exists.
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo2$ git push -u origin main
Enumerating objects: 3, done.
Counting objects: 100% (3/3), done.
Writing objects: 100% (3/3), 232 bytes | 232.00 KiB/s, done.
Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
To github.com:BlackJack0083/first-repo.git
 * [new branch]      main -> main
Branch 'main' set up to track remote branch 'main' from 'origin'.
hejiakai@LAPTOP-EOPIIVUT:~/git_test/my-repo2$ git remote -v
origin  git@github.com:BlackJack0083/first-repo.git (fetch)
origin  git@github.com:BlackJack0083/first-repo.git (push)
```
### git 版本管理规范
![[微信截图_20240309112713.png|500]]

### github 图床管理
图床一般是指储存图片的服务器，有国内和国外之分。国外的图床由于有空间距离等因素决定访问速度很慢影响图片的显示速度。国内也分为单线空间、多线空间和cdn加速三种

CDN是构建在数据网络上的一种分布式的内容分发网。CDN的作用是采用流媒体服务器集群技术，克服单机系统输出带宽以及并发能力不足的缺点，可极大提升系统支持的并发流数目，减少或避免单点失效带来的不良影响

- 首先创建仓库
- 下载picgo，进入图床设置
![image.png](https://cdn.jsdelivr.net/gh/BlackJack0083/image@main/img/20240414113947.png)
- 输入相关内容，在github上获得token
`setting` -> `Developer Setting` -> `Personal access token` -> `tokens(classic)` 即可选择生成


