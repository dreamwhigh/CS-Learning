#### Git 学习笔记

-------

本人所用学习教程见：[廖雪峰的官方网站—Git 教程](<https://www.liaoxuefeng.com/wiki/896043488029600>)

##### git 安装教程

[git 国内镜像下载地址](<https://github.com/waylau/git-for-win/>)

##### git 初始配置

选择一个空目录 E:\GitHub\test ，在该目录下右击选择 Git Bash Here

1. 配置**用户名和邮箱**，使用`--global`表示所有的项目都会默认使用这里配置的用户信息

```
$ git config --global user.name "dreamwhigh"
$ git config --global user.email huiting.w@foxmail.com
```

2.  配置**默认文本编辑器**

```
$ git config --global core.editor="D:/software2/Notepad++/Notepad++.exe"
```

##### 创建版本库

1. `mkdir ` 创建空目录 learngit 

   `pwd` 用于显示当前目录

```c
$ mkdir learngit
$ cd learngit
$ pwd
/e/GitHub/test/learngit
```

2. `git init`把这个目录变成Git可以管理的仓库，当前目录下多了一个 .git 的目录

```
$ git init
Initialized empty Git repository in E:/GitHub/test/learngit/.git/
```

3.  ` git add ` 添加文件 readme.txt 进仓库

```
$ git add readme
```

4.  `git commit -m "notes"` ，把文件提交到仓库，notes 处填写对本次提交的说明

```
$ git commit -m "notes for this submission"
[master (root-commit) b4f1449] notes for this submission
 1 file changed, 1 insertion(+)
 create mode 100644 readme
```

#### 时光穿梭机

##### 版本回退

`git status` 显示仓库当前的状态，下面的命令输出告诉我们，`readme.txt`被修改过了，但还没有准备提交的修改。

```
$ git status
On branch master
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme

no changes added to commit (use "git add" and/or "git commit -a")
```

`git diff` 查看difference，显示的格式正是 Unix 通用的 diff 格式。

```
$ git diff readme
diff --git a/readme b/readme
index 95acc50..013b5bc 100644
--- a/readme
+++ b/readme
@@ -1 +1,2 @@
-test wht
\ No newline at end of file
+Git is a distributed version control system.
+Git is free software.
\ No newline at end of file
```

`git log`命令显示从最近到最远的提交日志

```
$ git log
commit 4d8facceb54a484cc312830ceca5081988d82098 (HEAD -> master)
Author: dreamwhigh <huiting.w@foxmail.com>
Date:   Tue Jul 9 20:48:26 2019 +0800

    append GPL

commit cfc540afd0dd566eed0dff04fdfdf74b15ce7b26
Author: dreamwhigh <huiting.w@foxmail.com>
Date:   Tue Jul 9 20:45:53 2019 +0800

    add something

commit b4f14491db9cdc359547c42f30904646bd64cf43
Author: dreamwhigh <huiting.w@foxmail.com>
Date:   Tue Jul 9 20:20:32 2019 +0800

    notes for this submissionn
```

`git log --pretty=oneline` 显示从最近到最远提交的 commit id（版本号）

```
$ git log --pretty=oneline
4d8facceb54a484cc312830ceca5081988d82098 (HEAD -> master) append GPL
cfc540afd0dd566eed0dff04fdfdf74b15ce7b26 add something
b4f14491db9cdc359547c42f30904646bd64cf43 notes for this submissionn
```

在Git中，用`HEAD`表示当前版本，也就是最新的提交`4d8facc...`，上一个版本就是`HEAD^`，上上一个版本就是`HEAD^^`，当然往上100个版本写100个`^`比较容易数不过来，所以写成`HEAD~100`

###### 小结：

- 要随时掌握工作区的状态，使用`git status`命令。
- 如果`git status`告诉你有文件被修改过，用`git diff`可以查看修改内容。

- `HEAD`指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令`git reset --hard commit_id`。
- 穿梭前，用`git log`可以查看提交历史，以便确定要回退到哪个版本。
- 要重返未来，用`git reflog`查看命令历史，以便确定要回到未来的哪个版本。
- ` cat readme`  显示 readme 中的内容

##### 工作区和暂存区

###### 工作区（Working Directory）

就是你在电脑里能看到的目录，比如我的`learngit`文件夹就是一个工作区：

###### 版本库（Repository）

工作区有一个隐藏目录`.git`，这个不算工作区，而是Git的版本库。

Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支`master`，以及指向`master`的一个指针叫`HEAD`。

前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：

第一步是用`git add`把文件添加进去，实际上就是把文件修改添加到**暂存区**；

第二步是用`git commit`提交更改，实际上就是把暂存区的所有内容提交到**当前分支**。

##### 撤销修改

场景1：当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令`git checkout -- file`。

场景2：当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令`git reset HEAD <file>`，就回到了场景1，第二步按场景1操作。

场景3：已经提交了不合适的修改到版本库时，想要撤销本次提交，利用`git reset --hard commit_id` 进行版本回退，不过前提是没有推送到远程库。

##### 删除

创建一个操作文件,提交到版本库

```
$ git add test
$ git commit -m "add test"
```

删除文件

```
$ rm test.txt
$ git status
On branch master
Changes not staged for commit:
  (use "git add/rm <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        deleted:    test

no changes added to commit (use "git add" and/or "git commit -a")
```

提示说明有两个选择：

一是确实要从版本库中删除该文件，那就用命令`git rm`删掉，并且`git commit`：

```
$ git rm test
rm 'test'
$ git commit -m "delete"
```

二是删错了，运行 git checkout`用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

```
$ git checkout -- test
```

#### 远程仓库

##### SSH

创建SSH，输入 `ssh-keygen -t rsa -C "huiting.w@foxmail.com"` 命令，一路默认按回车键即可。

```
$ ssh-keygen -t rsa -C "huiting.w@foxmail.com"
Generating public/private rsa key pair.

Enter file in which to save the key (/c/Users/dreamwhigh/.ssh/id_rsa): Enter passphrase (empty for no passphrase):
Enter same passphrase again:
Your identification has been saved in /c/Users/dreamwhigh/.ssh/id_rsa.
Your public key has been saved in /c/Users/dreamwhigh/.ssh/id_rsa.pub.
```

登陆GitHub，打开“Account settings”，“SSH Keys”页面：

然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴`id_rsa.pub`文件的内容：

##### 添加到远程仓库

在 GitHub 上新建一个仓库，根据提示将本地仓库推送到 GitHub 上

第一步：关联

- 通过 HTTPS 关联，每次推送都必须输入口令，速度慢

  ```
  $ git remote add origin https://github.com/dreamwhigh/learngit.git
  ```

- 通过 SSH 关联，通过`ssh`支持的原生`git`协议速度最快

  ```
  $ git remote add origin git@github.com:dreamwhigh/test.git
  ```

第二步 推送到远程仓库

- 由于远程库是空的，第一次推送`master`分支时，加上了`-u`参数，Git 不但会把本地的`master`分支内容推送的远程新的`master`分支，还会把本地的`master`分支和远程的`master`分支关联起来，在以后的推送或者拉取时就可以简化命令

  ```
  $ git push -u origin master
  Enumerating objects: 19, done.
  Counting objects: 100% (19/19), done.
  Delta compression using up to 4 threads
  Compressing objects: 100% (13/13), done.
  Writing objects: 100% (19/19), 1.52 KiB | 86.00 KiB/s, done.
  Total 19 (delta 4), reused 0 (delta 0)
  remote: Resolving deltas: 100% (4/4), done.
  To https://github.com/dreamwhigh/learngit.git
   * [new branch]      master -> master
  Branch 'master' set up to track remote branch 'master' from 'origin'.
  
  ```

- 再次修改，并在本地提交后，可以直接通过下面的命令把本地`master`分支的最新修改推送至GitHub

  ```
  $ git push origin master
  ```

##### 从远程仓库克隆

`git clone` 第一次通过 SSH 连接，会出现 SSH 警告，输入 yes 后继续。

```
$  git clone git@github.com:dreamwhigh/gitskills.git
Cloning into 'gitskills'...
The authenticity of host 'github.com (13.250.177.223)' can't be established.
RSA key fingerprint is ...
Are you sure you want to continue connecting (yes/no/[fingerprint])? yes
Warning: Permanently added 'github.com,13.250.177.223' (RSA) to the list of known hosts.
remote: Enumerating objects: 3, done.
remote: Counting objects: 100% (3/3), done.
remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 0
Receiving objects: 100% (3/3), done.

```

#### 分支管理

##### 创建与合并分支

`git checkout -b `创建 dev 分支，并切换到 dev 分支

```
$ git checkout -b dev
Switched to a new branch 'dev'
```

`git checkout -b`相当于下面两条命令

```
$ git branch dev
$ git checkout dev
Switched to branch 'dev'
```

` git branch `查看当前分支，`git branch` 命令会列出所有分支，当前分支前面会标一个`*`号。

```
$ git branch
* dev
  master
```

修改 readme 文件后保存，添加到暂存区并提交

```
$ git add readme
$ git commit -m "branch test"
[dev 3cd7f88] branch test
 1 file changed, 2 insertions(+), 1 deletion(-)
 
 $ cat readme
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
branch test
```

dev 分支的工作完成，切换回 master 分支，此时的 readme 并没有发生改变

```
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

$ cat readme
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
```

把`dev`分支的工作成果合并到`master`分支上，现在和`dev`分支的最新提交是完全一样的

这里的`Fast-forward`信息，Git告诉我们，这次合并是“快进模式”，也就是直接把`master`指向`dev`的当前提交，所以合并速度非常快。

```
$ git merge dev
Updating 27f0555..3cd7f88
Fast-forward
 readme | 3 ++-
 1 file changed, 2 insertions(+), 1 deletion(-)

$ cat readme
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
branch test
```

合并完成后，可以` git branch -d` 命令删除 dev 分支，此后只有 master 一个分支

```
$ git branch -d dev
Deleted branch dev (was 3cd7f88).

$ git branch
* master
```

###### 小结

查看分支：`git branch`

创建分支：`git branch <name>`

切换分支：`git checkout <name>`

创建+切换分支：`git checkout -b <name>`

合并某分支到当前分支：`git merge <name>`

删除分支：`git branch -d <name>`

##### 解决冲突

建立新的分支 feature1 ，修改 readme 文件后保存，添加并提交。

```
$ git checkout -b featurel
Switched to a new branch 'featurel'

$ git add readme
$ git commit -m "AND simple"
[featurel e0f5724] AND simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

切换回 master 分支，提示我们当前`master`分支比远程的`master`分支要超前1个提交

```
$ git checkout master
Switched to branch 'master'
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)

```

在 master 分支上修改 readme 文件，添加并提交

```
$ git add readme
$ git commit -m "& simple"
[master 425fdf7] & simple
 1 file changed, 1 insertion(+), 1 deletion(-)
```

这时无法快速合并，存在冲突

```
$ git merge feature1
Auto-merging readme
CONFLICT (content): Merge conflict in readme
Automatic merge failed; fix conflicts and then commit the result.

$ git status
On branch master
Your branch is ahead of 'origin/master' by 3 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)

        both modified:   readme

no changes added to commit (use "git add" and/or "git commit -a")

```

直接查看 readme 内容

```
Git is a distributed version control system.
Git is free software distributed under the GPL.
Git has a mutable index called stage.
Git tracks changes.
<<<<<<< HEAD
Creating a new branch is quick and simple.
=======
Creating a new branch is quick AND simple.
>>>>>>> feature1
```

在其中修改冲突的部分后，重新提交，即可解决冲突

```
$ git add readme
$ git commit -m "conflict fixed"
[master cd4af03] conflict fixed
```

最后删除分支

```
$ git branch -d feature1
Deleted branch feature1 (was 64c1488).
```

###### 小结

当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。

解决冲突就是把Git合并失败的文件手动编辑为我们希望的内容，再提交。

用`git log --graph`命令可以看到分支合并图。

##### 分支管理策略

将新的分支dev合并到master ，`--on-ff` 参数表示禁用 Fast forward

```
$ git merge --no-ff -m "merge with no-ff" dev
Merge made by the 'recursive' strategy.
 readme | 7 ++-----
 1 file changed, 2 insertions(+), 5 deletions(-)
```

合并后，用` git log` 查看分支历史

```
$ git log --graph --pretty=oneline --abbrev-commit
*   849c5f7 (HEAD -> master) merge with no-ff
|\
| * 1a7753a (dev) add merge
|/
* 6539249 conflict fixed
```

###### 小结

Git分支十分强大，在团队开发中应该充分应用。

合并分支时，加上`--no-ff`参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而`fast forward`合并就看不出来曾经做过合并。

##### Bug 分支

当手头工作没有完成时，先把工作现场`git stash`一下，此时用 `git status` 会显示工作区是干净的，用 `git stash list` 可以查看被保存的工作现场。

然后去修复bug，修复后，再`git stash pop`，回到工作现场，恢复的同时把stash内容也删了。

或者用`git stash apply`恢复，但是恢复后，stash内容并不删除，你需要用`git stash drop`来删除

```
$ git status
On branch dev
Changes to be committed:
  (use "git reset HEAD <file>..." to unstage)

        modified:   readme

$ git stash
Saved working directory and index state WIP on dev: 1a7753a add merge

$ git status
On branch dev
nothing to commit, working tree clean

$ git stash list
stash@{0}: WIP on dev: 1a7753a add merge

$ git stash pop
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (453d1a853f3adb43903df113acdf946d61c8f766)
```

用`git stash list`命令可以查看保存的工作现场列表

```
$ git stash list
stash@{0}: WIP on dev: 1a7753a add merge
stash@{1}: WIP on dev: 1a7753a add merge
stash@{2}: WIP on dev: 1a7753a add merge
```

`  git stash apply` 可以指定恢复某个工作现场

```
$ git stash apply stash@{2}
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git checkout -- <file>..." to discard changes in working directory)

        modified:   readme

no changes added to commit (use "git add" and/or "git commit -a")
```

##### Feature 分支

开发一个新feature，最好新建一个分支；

如果要丢弃一个没有被合并过的分支，可以通过`git branch -D <name>`强行删除。

##### 多人协作

查看远程库信息：

- `git remote ` 可查看简略信息
- `git remote -v` 可查看详细信息，包括可以抓取和推送的 origin 的地址

```
$ git remote
origin

$ git remote -v
origin  https://github.com/dreamwhigh/learngit.git (fetch)
origin  https://github.com/dreamwhigh/learngit.git (push)
```

###### 推送分支

推送分支，就是把该分支上的所有本地提交推送到远程库。推送时，要指定本地分支，这样，Git就会把该分支推送到远程库对应的远程分支上。

```
$ git push origin master

$ git push origin dev
```

- `master`分支是主分支，因此要时刻与远程同步；
- `dev`分支是开发分支，团队所有成员都需要在上面工作，所以也需要与远程同步；
- bug分支只用于在本地修复bug，就没必要推到远程了，除非老板要看看你每周到底修复了几个bug；
- feature分支是否推到远程，取决于你是否和你的小伙伴合作在上面开发。

###### 抓取分支

当其他人的最新提交和你试图推送的提交有冲突时，Git已经提示我们，先用`git pull`把最新的提交从`origin/dev`抓下来，然后，在本地合并，解决冲突，再推送：

```
$ git push origin dev
To github.com:michaelliao/learngit.git
 ! [rejected]        dev -> dev (non-fast-forward)
error: failed to push some refs to 'git@github.com:michaelliao/learngit.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```

抓取最新的提交

```
$ git pull
There is no tracking information for the current branch.
Please specify which branch you want to merge with.
See git-pull(1) for details.

    git pull <remote> <branch>

If you wish to set tracking information for this branch you can do so with:

    git branch --set-upstream-to=origin/<branch> dev
```

出现错误，原因是没有指定本地`dev`分支与远程`origin/dev`分支的链接，根据提示，设置`dev`和`origin/dev`的链接

```
$ git branch --set-upstream-to=origin/dev dev
Branch 'dev' set up to track remote branch 'dev' from 'origin'.
```

成功抓取

```
$ git pull
Auto-merging env.txt
CONFLICT (add/add): Merge conflict in env.txt
Automatic merge failed; fix conflicts and then commit the result.
```

解决冲突

```
$ git commit -m "fix env conflict"
[dev 57c53ab] fix env conflict

$ git push origin dev
Counting objects: 6, done.
Delta compression using up to 4 threads.
Compressing objects: 100% (4/4), done.
Writing objects: 100% (6/6), 621 bytes | 621.00 KiB/s, done.
Total 6 (delta 0), reused 0 (delta 0)
To github.com:michaelliao/learngit.git
   7a5e5dd..57c53ab  dev -> dev
```

###### 小结

- 查看远程库信息，使用`git remote -v`；
- 本地新建的分支如果不推送到远程，对其他人就是不可见的；
- 从本地推送分支，使用`git push origin branch-name`，如果推送失败，先用`git pull`抓取远程的新提交；
- 在本地创建和远程分支对应的分支，使用`git checkout -b branch-name origin/branch-name`，本地和远程分支的名称最好一致；
- 建立本地分支和远程分支的关联，使用`git branch --set-upstream branch-name origin/branch-name`；
- 从远程抓取分支，使用`git pull`，如果有冲突，要先处理冲突。

##### Rebase

- rebase操作可以把本地未push的分叉提交历史整理成直线；
- rebase的目的是使得我们在查看历史提交的变化时更容易，因为分叉的提交需要三方对比。

#### 标签管理

##### 创建标签

发布一个版本时，我们通常先在版本库中打一个标签（tag），这样，就唯一确定了打标签时刻的版本。将来无论什么时候，取某个标签的版本，就是把那个打标签的时刻的历史版本取出来。所以，标签也是版本库的一个快照。

Git的标签虽然是版本库的快照，但其实它就是指向某个commit的指针。

- 命令`git tag <tagname>`用于新建一个标签，默认为`HEAD`，也可以指定一个commit id；
- 命令`git tag -a <tagname> -m "blablabla..."`可以指定标签信息；
- 命令`git tag`可以查看所有标签。

##### 操作标签

- 命令`git push origin <tagname>`可以推送一个本地标签；
- 命令`git push origin --tags`可以推送全部未推送过的本地标签；
- 命令`git tag -d <tagname>`可以删除一个本地标签；
- 命令`git push origin :refs/tags/<tagname>`可以删除一个远程标签。

#### 使用 GitHub

- 在GitHub上，可以任意Fork开源仓库；
- 自己拥有Fork后的仓库的读写权限；
- 可以推送pull request给官方仓库来贡献代码。
