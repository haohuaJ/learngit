# 创建版本库
## 基本操作命令
git init 初始化仓库

git add  readme.txt 提交文件，该文件必须在仓库目录下  把文件修改添加到暂存区(stage)

git commit -m "wrote a readme file" 添加提交说明   把暂存区的所有内容提交到当前分支,且只提交暂存区的内容，若修改后的内容未添加到暂存区，则不会被提交

git commit -a -m  省一步 git add ，但也只是对修改和删除文件有效， 新文件还是要 git add，不然就是 untracked 状态

git status 查看工作区的状态

git diff readme.txt 比较文件不同
如果git status告诉你有文件被修改过，用git diff可以查看修改内容

git log --pretty=oneline 显示版本记录 （只显示一行）

git reset --hard HEAD^ 回退上一个版本 

git reset --hard commit_id前几位即可 回退到对应版本

git reflog 用来记录你的每一次命令

git restore file 回退工作区的修改(删除)

git restore --staged <file> 回退已添加到暂存区的修改

rm file 删除文件

git rm <file> 删除git 之后提交

git checkout -- <file> 作用等同 git restore file

# 远程仓库
## 添加远程仓库
第1步：创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，如果已经有了，可直接跳到下一步。
$ ssh-keygen -t rsa -C "haohuajing0723@163.com"
```
公钥目录 /c/Users/Administrator/.ssh/id_rsa.pub
```
你需要把邮件地址换成你自己的邮件地址，然后一路回车，使用默认值即可。
如果一切顺利的话，可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以放心地告诉任何人。

第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：
然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
点“Add Key”。
## 添加远程库 
登陆GitHub，然后，在右上角找到“Create a new repo”按钮，创建一个新的仓库：
在Repository name填入名称，其他保持默认设置，点击“Create repository”按钮，就成功地创建了一个新的Git仓库：
GitHub告诉我们，可以从这个仓库克隆出新的仓库，也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。
现在，我们根据GitHub的提示，在本地的仓库下运行命令：
```
$ git remote add origin git@github.com:haohuaJ/learngit.git
```
```
origin为远程仓库名称
```
下一步，就可以把本地库的所有内容推送到远程库上：
```
$ git push -u origin master
```
把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。
由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，
还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
推送成功后，可以立刻在GitHub页面中看到远程库的内容已经和本地一模一样：
从现在起，只要本地作了提交，就可以通过命令：
```
$ git push origin master
```
把本地master分支的最新修改推送至GitHub。

SSH警告
当你第一次使用Git的clone或者push命令连接GitHub时，会得到一个警告：
The authenticity of host 'github.com (xx.xx.xx.xx)' can't be established.
RSA key fingerprint is xx.xx.xx.xx.xx.
Are you sure you want to continue connecting (yes/no)?
这是因为Git使用SSH连接，而SSH连接在第一次验证GitHub服务器的Key时，需要你确认GitHub的Key的指纹信息是否真的来自GitHub的服务器，输入yes回车即可。
Git会输出一个警告，告诉你已经把GitHub的Key添加到本机的一个信任列表里了：
Warning: Permanently added 'github.com' (RSA) to the list of known hosts.
这个警告只会出现一次，后面的操作就不会有任何警告了。
如果你实在担心有人冒充GitHub服务器，输入yes前可以对照GitHub的RSA Key的指纹信息是否与SSH连接给出的一致。

## 删除远程库
如果添加的时候地址写错了，或者就是想删除远程库，可以用git remote rm <name>命令。使用前，建议先用git remote -v查看远程库信息：
```
$ git remote -v

origin  git@github.com:haohuaJ/learngit.git (fetch)
origin  git@github.com:haohuaJ/learngit.git (push)
```
然后，根据名字删除，比如删除origin：
```
$ git remote rm origin
```
此处的“删除”其实是解除了本地和远程的绑定关系，并不是物理上删除了远程库。远程库本身并没有任何改动。要真正删除远程库，需要登录到GitHub，
在后台页面找到删除按钮再删除。

## 从远程库克隆
```
git clone git@github.com:git地址 
```
例如git clone git@github.com:haohuaJ/config-repo

# 分支管理
## 创建与合并分支
查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>或者git switch <name> 
前面带有*号的是当前分支

创建+切换分支：git checkout -b <name>或者git switch -c <name>

先切换回master分支再进行合并
git switch master
git merge dev
当前分支有未提交的内容，则不允许切换分支。会报错
合并某分支到当前分支：git merge <name>
删除本地仓库分支：git branch -d <name>
删除远程仓库分支：git push origin --delete [branchname]
根据指定版本号创建分支: git checkout -b branchName commitId
清理本地无效分支(远程已删除本地没删除的分支): git fetch -p
如果分支太多，还可以用此命令进行分支模糊查找: git branch | grep 'branchName'
## 解决冲突
git switch -c feature1
git commit -a -m 添加并修改新分支内容
git switch master 
```
Your branch is ahead of 'origin/master' by 1 commit.
  (use "git push" to publish your local commits)
```
git commit -a -m 修改master分支内容
此时 master分支和feature1分支各自都分别有新的提交
这种情况下，Git无法执行“快速合并”，只能试图把各自的修改合并起来，但这种合并就可能会有冲突
```
$ git merge feature1
Auto-merging git操作.md
CONFLICT (content): Merge conflict in git操作.md
Automatic merge failed; fix conflicts and then commit the result.
```
git status也可以告诉我们冲突的文件
```
$ git status
On branch master
Your branch is ahead of 'origin/master' by 2 commits.
  (use "git push" to publish your local commits)

You have unmerged paths.
  (fix conflicts and run "git commit")
  (use "git merge --abort" to abort the merge)

Unmerged paths:
  (use "git add <file>..." to mark resolution)
        both modified:   "git\346\223\215\344\275\234.md"

no changes added to commit (use "git add" and/or "git commit -a")

```
直接查看冲突文件内容
Git用<<<<<<<，=======，>>>>>>>标记出不同分支的内容，我们修改如下后保存：

```
$ git commit -a -m 修改冲突后提交
[master 6ffe415] 修改冲突后提交
```
使用 git log --graph命令可以看到分支合并图
```
$ git log --graph --pretty=oneline --abbrev-commit
*   6ffe415 (HEAD -> master) 修改冲突后提交
|\
| * b448b39 (feature1) 添加并修改新分支内容
* | e0ea07b 修改master分支内容
|/
* 0a40e05 添加
* 96c4027 (origin/master) x
* 87087c6 增加分支dev
* a76a6dc 将txt换位md文件
* a848a78 修改内容14:05
* af81fd3 修改内容13:58
* 923ecce 删除文件
* e75b0c8 删除文件
* eb3ec1d 更新内容
* 2ec5fde 测试删除文件
* cb2a912 添加暂存区概念
* 435dac1 修改命令操作文档
* 1b4f069 修改命令操作文档
* 0de48db 添加git命令操作
* 0c66f18 add distributed
* 816ea99 wrote a readme file
```
删除分支
git branch -d feature1
## 分支管理策略
在实际开发中，我们应该按照几个基本原则进行分支管理：

首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；

那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；

每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
```
 git merge --no-ff -m "merge with no-ff" dev
```
git merge 模式不同，效果不同
fast-forward -ff 默认

Git 合并两个分支时，如果顺着一个分支走下去可以到达另一个分支的话，那么 Git 在合并两者时，只会简单地把指针右移，叫做“快进”（fast-forward）不过这种情况如果删除分支，则会丢失merge分支信息。

–squash

把一些不必要commit进行压缩，比如说，你的feature在开发的时候写的commit很乱，那么我们合并的时候不希望把这些历史commit带过来，于是使用–squash进行合并，此时文件已经同合并后一样了，但不移动HEAD，不提交。需要进行一次额外的commit来“总结”一下，然后完成最终的合并。

–no-ff

关闭fast-forward模式，在提交的时候，会创建一个merge的commit信息，然后合并的和master分支
merge的不同行为，向后看，其实最终都会将代码合并到master分支，而区别仅仅只是分支上的简洁清晰的问题，然后，向前看，也就是我们使用reset 的时候，就会发现，不同的行为就带来了不同的影响

--ff(默认不需要带此参数)master进行reset操作时，如果合并前dev分支进行了多次提交操作，会回滚到分支提交的最后一个版本，其内容已经融入到要合并的分支（master）中
--no-ff  master进行reset操作时，无论合并前dev分支进行了多少次提交操作，会回滚到master合并前的版本

## bug分支
<<<<<<< HEAD
当前工作分支dev尚无法提交，有需要重新开启一个bug分支时，可使用
=======
当前工作分支dev尚无法提交，有需要重新开启一个bug分支时，可使用、
>>>>>>> dev
git stash 可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
```
$ git stash
Saved working directory and index state WIP on dev: 75132a1 分支策略完成
```
现在，用git status查看工作区，就是干净的（除非有没有被Git管理的文件），因此可以放心地创建分支来修复bug。

首先确定要在哪个分支上修复bug，假定需要在master分支上修复，就从master创建临时分支：
```
Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (dev)
$ git checkout master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (master)
$ git checkout -b issue-101
Switched to a new branch 'issue-101'

Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (issue-101)
$ git commit -a -m 测试bug分支
[issue-101 7ce0159] 测试bug分支
 1 file changed, 22 insertions(+), 1 deletion(-)

Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (issue-101)
$ git switch master
Switched to branch 'master'
Your branch is up to date with 'origin/master'.

Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (master)
$ git merge --no-ff -m "merged bug fix 101" issue-101
Merge made by the 'recursive' strategy.
 "git\346\223\215\344\275\234.md" | 23 ++++++++++++++++++++++-
 1 file changed, 22 insertions(+), 1 deletion(-)

```
修复bug后，删除bug分支，切换回dev继续工作
```
Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (dev)
$ git status
On branch dev
nothing to commit, working tree clean

Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (dev)
$ git stash list
stash@{0}: WIP on dev: 75132a1 分支策略完成
```
工作现场还在，Git把stash内容存在某个地方了，但是需要恢复一下，有两个办法：

一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
另一种方式是用git stash pop，恢复的同时把stash内容也删了：

```
Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (dev)
$ git stash pop  
On branch dev
Changes not staged for commit:
  (use "git add <file>..." to update what will be committed)
  (use "git restore <file>..." to discard changes in working directory)
        modified:   "git\346\223\215\344\275\234.md"

no changes added to commit (use "git add" and/or "git commit -a")
Dropped refs/stash@{0} (d627e66cbff06f7d565d3005d7a07971b87fd9d0)

Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (dev)
$ git stash list
此时看不到stash内容了
```
在master分支上修复了bug后，怎么在dev分支上修复同样的bug
同样的bug，要在dev上修复，我们只需要把ca42a72 修复bug，删除bug分支这个提交所做的修改“复制”到dev分支。注意：我们只想复制ca42a72 修复bug，删除bug分支这个提交所做的修改，并不是把整个master分支merge过来。

为了方便操作，Git专门提供了一个cherry-pick命令，让我们能复制一个特定的提交到当前分支：
```
$ git cherry-pick ca42a72
error: Your local changes to the following files would be overwritten by merge:
        git操作.md
Please commit your changes or stash them before you merge.
Aborting
fatal: cherry-pick failed

Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (dev)
$ git commit -a -m x
[dev c28908d] x
 1 file changed, 4 insertions(+), 1 deletion(-)

Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (dev)
$ git cherry-pick ca42a72
Auto-merging git操作.md
CONFLICT (content): Merge conflict in git操作.md
error: could not apply ca42a72... 修复bug，删除bug分支
hint: after resolving the conflicts, mark the corrected paths
hint: with 'git add <paths>' or 'git rm <paths>'
hint: and commit the result with 'git commit'

Administrator@DESKTOP-U24771V MINGW64 /e/gitRepository (dev|CHERRY-PICKING)
$ git commit -a -m 将bug修复的内容同步到dev上
[dev eae664c] 将bug修复的内容同步到dev上
 Date: Tue Feb 8 17:37:55 2022 +0800
 1 file changed, 37 insertions(+), 1 deletion(-)
```

