# 创建版本库
## 基本操作命令
git init 初始化仓库
git add  readme.txt 提交文件，该文件必须在仓库目录下  把文件修改添加到暂存区(stage)
git commit -m "wrote a readme file" 添加提交说明   把暂存区的所有内容提交到当前分支,且只提交暂存区的内容，若修改后的内容未添加到暂存区，则不会被提交
git status 查看工作区的状态，
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

查看分支：git branch

创建分支：git branch <name>

切换分支：git checkout <name>或者git switch <name> 切换后可以看到切换到的分支颜色变了，也就是指向了该分支

创建+切换分支：git checkout -b <name>或者git switch -c <name>

先切换回master分支再进行合并
git switch master
git merge dev
当前分支有未提交的内容，则不允许切换分支。会报错
合并某分支到当前分支：git merge <name>

删除本地仓库分支：git branch -d <name>
删除远程仓库分支：
