1.创建仓库目录  1.mkdir learngit 2.cd learngit 3.pwd（显示当前目录）
2.目录变成Git可以管理的仓库 1.git init  2.ls -ah（查看当前目录下文件包括隐藏文件）
3.把文件放到git 仓库 1.git add readme.txt （文件添加到仓库）2.git commit -m "wrote a readme file" （用命令git commit告诉Git，把文件提交到仓库 -m后面输入的是本次提交的说明）
4.git status命令可以让我们时刻掌握仓库当前的状态
5.查看文件上次修改的内容 1.git diff readme.txt
6.修改提交与新增提交相同 1.git add readme.txt （文件添加到仓库）2.git commit -m "wrote a readme file" 
7.显示从最近到最远的提交日志 1.git log (最早的一次是wrote a readme file) 2.git log --pretty=oneline (显示简单信息) 3.git reflog (用来记录你的每一次命令)
8.回退版本 1.git reset --hard HEAD^（回退到上一个版本） 2.git reset --hard 3628164（指定回到未来的某个版本，版本号没必要写全，前几位就可以了，Git会自动去找。当然也不能只写前一两位，因为Git可能会找到多个版本号，就无法确定是哪一个了。）
9.撤销修改 1.git checkout -- file可以丢弃工作区的修改 （也可以还原工作区删除的文件）,
两种情况一种是file自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；一种是file已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。总之，就是让这个文件回到最近一次git commit或git add时的状态。(此时需要先 git reset HEAD file 再git checkout -- file)
10.git reset HEAD file 也可以把暂存区的修改撤销掉（unstage），重新放回工作区
11.删除版本库中的文件 1.git rm LICENSE.txt 删除文件 2. git commit -m "remove LICENSE.txt" 提交



----与远程版本库关联
必须先在github创建对应库
1.git remote add origin git@github.com/mark201166/learngit.git --关联到远程版本库
2.git push -u origin master  --把本地库的所有内容推送到远程库上 由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。 git push origin master

注：Git 提示fatal: remote origin already exists 错误解决办法
1.先删除远程 Git 仓库 git remote rm origin
2.再添加远程 Git 仓库 git remote add origin git@github.com:mark201166/maven.git
3.如果执行 git remote rm origin 报错的话，我们可以手动修改gitconfig文件的内容 1. vi .git/config 2.把 [remote “origin”] 那一行删掉就好了。3.:wq 保存


 

---远程版本库关联到本地
1.git clone git@github.com:michaelliao/gitskills.git


--- 分支管理
1.创建分支 1.git checkout -b dev 创建分支并切换到dev分支 （git checkout命令加上-b参数表示创建并切换 相当于1.git branch dev 2.git checkout dev） 切换到的 dev 分支上后可以的内容进行修改提交
2.git branch命令查看当前分支 (git branch命令会列出所有分支，当前分支前面会标一个*号)
3.git checkout master 切换到主分支
4.git merge dev 合并指定分支（dev）到当前分支(master)
5.git branch -d dev 删除指定分支（dev）
6.当Git无法自动合并分支时，就必须首先解决冲突。解决冲突后，再提交，合并完成。用git log --graph命令可以看到分支合并图。


---- 合并分支禁用 Fast forward 模式保留分支信息
1.git merge --no-ff -m "merge with no-ff" dev  请注意--no-ff参数，表示禁用Fast forward 因为本次合并要创建一个新的commit，所以加上-m参数，把commit描述写进去。

分支策略

在实际开发中，我们应该按照几个基本原则进行分支管理：
首先，master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能在上面干活；
那在哪干活呢？干活都在dev分支上，也就是说，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上，在master分支发布1.0版本；
你和你的小伙伴们每个人都在dev分支上干活，每个人都有自己的分支，时不时地往dev分支上合并就可以了。
合并分支时，加上--no-ff参数就可以用普通模式合并，合并后的历史有分支，能看出来曾经做过合并，而fast forward合并就看不出来曾经做过合并。

----bug 分支（在工作修复其他分支bug）
1.git stash 可以把当前工作现场“储藏”起来，等以后恢复现场后继续工作
2.git checkout master 切换到需要修改的分支
3.git checkout -b issue-101  在修改分支上创建一个bug 修改分支
4.修改bug 分支（issue-101）并提交
5.切换到修改前分支(master) ,合并修改bug分支（issue-101） git merge --no-ff -m "merged bug fix 101" issue-101 删除修改bug 分支 
6.切换到工作分支 1.git checkout dev
7.查看工作现场 1.git stash list
8.恢复工作现场 两种方法一是用git stash apply恢复，但是恢复后，stash内容并不删除，你需要用git stash drop来删除；
另一种方式是用git stash pop，恢复的同时把stash内容也删了
注：你可以多次stash，恢复的时候，先用git stash list查看，然后恢复指定的stash，用命令：git stash apply stash@{0}

--强行删除未合并的分支  git branch -D <name>


-- 标签管理
  1.切换到打标签的分支
  2.git tag v1.0 (对当前分支最新版本打标签)
  3.根据commitID打标签，1. git log --pretty=oneline --abbrev-commit 查找历史版本，2. git tag v0.9 6224937 对应commitd d=打标签
  4.查看标签 git tag
  5.查看标签明细信息 git show <tagname>
  6.创建带有说明的标签，用-a指定标签名，-m指定说明文字： git tag -a v0.1 -m "version 0.1 released" 3628164
  7.-s用私钥签名一个标签 git tag -s v0.2 -m "signed version 0.2 released" fec145a (签名采用PGP签名，因此，必须首先安装gpg（GnuPG），如果没有找到gpg，或者没有gpg密钥对，就会报错)
  8.删除标签 1.git tag -d v0.1
  9.推送某个标签到远程，使用命令 git push origin <tagname>
  10.一次性推送全部尚未推送到远程的本地标签 git push origin --tags
  11.远程删除标签 git push origin :refs/tags/v0.9




