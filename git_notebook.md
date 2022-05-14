#### Git学习笔记

###### 1. windows 版本的 git 无法提交代码的问题还没有解决:

https://www.jianshu.com/p/5a7028b515ad

可以在 windows 提交代码的Git 配置，需要翻墙，否则"run time error"。配置如图:

![image-202111	22224205739](C:\Users\qz\AppData\Roaming\Typora\typora-user-images\image-20211122224205739.png)

###### 1.Git 简介

一个分布式的代码版本控制工具
版本控制：个人开发  ->过渡->  团队协作
集中式：服务器，缺点：宕机风险
分布式：除了本地版本控制之外，还有一个远程库

Git工作机制：
工作区(写代码)：项目存储的磁盘的本地目录；
暂存区（临时存储）：将代码从工作区添加到暂存区；
本地库：暂存区的代码提交到本地库，会生成历史版本（历史版本无法删除）

代码托管中心：
局域网：GitLab
互联网：Github, Gitee

##### 2.Git 基础操作

```shell
git bash here
git --version
git config --global user.name 用户名	# 设置用户签名,记录提交者，而不是用来登录的用户名
git config --global user.email 邮箱	# 设置用户签名
在根目录下可以查看，
cat ~/.gitconfig
git init	# 初始化本地库，作用：让git获取这个目录的管理权限
git status 	# 查看本地库的状态
git status -s # 精简输出
git add 文件名	# 从工作区添加到暂存区, eg: git add .
git add -i ./	# --interactive 如果创建了新文件, -i 会将新文件也添加到修改
git add -u ./	# --update 仅添加在原来的基础上修改的文件, 新增加的文件不添加
git add -A ./	# --all 所有文件都添加
git add -p ./ 	# 添加一个文件中的部分内容，会使用 diff 打印要提交内容的修改
git rm--chched <file> # 将暂存区的文件删除, 相当于 git add 的逆过程
git rm --caches 文件名	# 从暂存区删除某一个文件，只是从暂存区删除文件
git rm <file> # 不仅删除暂存区的文件，工作区的文件也被删除了
git commit	#提交到本地库, git commit -m "日志信息" 文件名
git commit --amend --reset-author --allow-empty # 允许空提交
git commit --amend # 补充到上一笔的提交中, 而不产生额外的提交
git reflog		# 查看历史版本粗略信息
git log		# 查看版本详细信息
git log --prety=fuller # 输出格式更方便观察
git log --pretty=oneline # 仅输出 commit log
git log --stat	# 可以在日志中看到每一笔提交的文件变更情况
git reset --hard 版本号	# 版本穿梭
git reset --hard 8a5fc91	# 通过git reflog即可查看精简的版本号， Git切换版本，底层就是移动指针
git reset --soft HEAD^	# 本地代码回退
git reset  # 将所有暂存区的文件退回到工作区
git clean -d # 清空还没有 add 和 commit 的文件

git checkout -f # 删除本地所有为暂存的修改
git reset --hard	# 将修改加入暂存区
git clean -xdf

# 回退
git checkout . #本地所有修改的。没有的提交的，都返回到原来的状态
git stash #把所有没有提交的修改暂存到stash里面。可用git stash pop回复。
git reset --hard HASH #返回到某个节点，不保留修改。
git reset --soft HASH #返回到某个节点。保留修改

# 变基操作
git rebase -i 123456

# diff
git diff # 工作区与暂存区的区别(只能看到工作区的修改内容)
git diff HEAD # 工作区与本地库的区别
git diff --cached # 暂存区与本地库的区别
git diff --staged # == --cached

# pull & fetch
git fetch # 会将当前项目的所有分支的修改全部获取, 但不直接合并, 需要 git merge 进行合并
git pull # git pull = git fetch + git merget FETCH_HEAD
# git pull使用给定的参数运行git fetch，并调用git merge将检索到的分支头合并到当前分支中。
```

**其它辅助命令**

```shell
grep -i "a" ./test/a.txt # 默认是在当前目录下搜索的
git grep # 不会搜索 .git 文件
git grep "hello" # 默认在当前目录的文件中进行搜索
```



**Linux命令**

```shell
diff a.txt b.txt > diff.txt # 但是 GNU 的 diff 不会比较二进制文件
patch a.txt < diff.txt	# 打包
# git diff 可以比较二进制文件
```



**分支操作:**

```shell
git branch 分支名		# 创建分支
git branch -v		  # 查看分支
git branch -a 	# 查看所有分支
git checkout 分支名		# 切换分支
git branch -d main	# 删除本地 main 分支
git branch -D main	# 强制删除
git merge 分支名		# 将“指定的分支 ”合并到 “当前分支”, eg: git merge hot-fix, 合并 hot-fix 分支到当前分支
git merge --abort		//取消合并
```

合并冲突：两个人在同一个文件的同一个位置，都做了不同的修改，就会出现解决合并冲突:

需要手动决定哪个保留哪个部分，删除不要的部分即可

再进行合并

![服务器运行](D:\install_location\Github\服务器运行.png)

![分支](D:\install_location\Github\分支.png)

![分支2](D:\install_location\Github\分支2.png)

```shell
#eg:提交代码
git add hello.txt
git commit
git push origin HEAD:~/refs/for/Pre_DEV
------------------团队协作---------------
团队协作：通过代码托管中心
团队内协作/跨团队协作
```

---------------GitHub-------------
远程库操作:

**本地库的名字最好和远程库的名字一致，远程库的默认名字"origin", git remote -v 查看**

```shell
git remote -v 	# 查看当前所有远程地址别名
git remote add 别名 远程地址	# 添加远程仓库
git push 别名 分支		# 推送本地分支上的内容到远程仓库
git clone 远程地址		# 将远程地址上的内容克隆到本地
git pull 远程库地址别名 远程分支名	# 将远程仓库对于分支最新内容拉下来后与当前本地分支直接合并, eg: git pull
git remote -v 
git remote add git_project1 https://github.com/NumberNineT/git_project1.git	# 创建别名，最好和库的名字一致
git push git_project1 master		# 推送本地库到远程库，推送时的最小单位是分支，将master分支推送到远程库
git pull git_project1 master	# git pull
```

**团队内协作，其他人修改代码，首先需要项目管理员为这个人赋予权限才可以**

```shell
git clone https://github.com/NumberNineT/git_project1.git	//1)拉取代码; 2)初始化本地仓库；  3)创建别名
vim hello.txt		# 修改代码
git diff # 查看所有修改的代码
git diff hello.txt  # 查看指定文件的修改情况
git add hello.txt
git commit -m "qi ta ren." hello.txt	# 通常用git commit, commit 的内容需要详细填写
git push https://github.com/NumberNineT/git_project1.git master	//将master分支提交, git push origin master
```

![团队内协作](D:\install_location\Github\团队内协作.png)

![团队协作](D:\install_location\Github\团队协作.png)

**跨团队协作**

fork代码到自己的代码托管中心
clone到本地库
修改后，再commit到自己的本地库
pull requests 请求管理员拉取这个团队修改的代码

```shell
# fork
git clone
git commit
# pull requests
```

![跨团队协作](D:\install_location\Github\跨团队协作.png)

###### 2.1 SSH

-------------------------------SSH-------------------------------------
使用SSH登录的配置方法

2.基于windows进行讲解

3. 记住git的常用命令

4. 分支的创建， 特性， 转换， 合并， 代码冲突等

5. IDEA 继承 Git.

-----------------------------Github------------------------------
1.代码推送 push
2.代码拉取 pull
3.代码克隆 clone
4.ssh免密登录
5.IDEA继承github

----------------------------Gitee码云(国内服务器)----------------------------
码云创建远程库
Idea继承Gitee
码云连接Github进行代码复制和迁移

---------------------------GitLab(局域网代码托管服务器)--------------------------------
GitLab服务器搭建和部署
Idea集成GitLab

-----------------------------------------------------------------------------
vim中拷贝以及粘贴内容，
查看模式下：
复制：yy
粘贴:p

Linux:查看文件末尾内容：
tail -n 1 hello.txt

----------------------------------------IDEA使用Git----------------------------------------





**eg1**:Git多分支平行发展（一个仓库包含多个不同的项目）

```shell
git init	# 初始化本地仓库
git remote add origin git@github.com:2846256621/React_study.git # 关联到已经有的分支
git remote -v 	# 查看是否关联成功, 远程参数仓库是否有
step3: git branch STM_Pre_Dev	# 创建分支
git branch -a	# 查看分支是否创建成功
step4:git checkout STM_Pre_Dev	# 切换分支
git checkout -b STM_Pre_Dev 	# 创建并切换分支 = step3 + step4
git add .
git commit 
git push origin STM_PreDev		# 提交到远程库的 STM_Pre_Dev 分支
git pull -b STM_PreDev git@github.com:2846256621/React_study.git	# git pull 默认拉取所有分支，-b 拉取指定的分支
```

##### git cherry-pick 以及 git patch 具体怎么使用? 以及使用场景
```
git cherry-pick # 1.可以用于同步一个同步一个分支的修改到另一个分支
git patch	# 打 patch, 有可能是是这么用的，兼容 Linux
git diff > diff.txt 
git patch < diff.txt
```



##### 2. Git 的使用场景，讲当前修改保存起来，拉取最新的代码，进行操作后，再拉出之前保存的代码

```shell
git stash # git stash save "save some code change through stash"
git stash list # 查看堆栈中保存的信息
git stash show # 查看堆栈中最新保存的stash和当前目录的差异
git stash show stash@{0} # 查看指定堆栈的内容与当前工作目录的差异
git stash show -p # 查看详细的不同
git stash apply # git stash apply stash@{0}, 将栈顶的内容应用到当前工作目录，但是不出栈
git stash pop # 出栈
git stash drop	# 移除指定的 stash
git stash clear # 清空 stash
```

##### git checkout

```shell
# （1）切换分支
git checkout branch_xxx 
# (2)回退版本, 将暂存区的文件恢复到当前工作目录
git checkout -- filename 
git checkout commit_id
```



###### 2.撤销操作

- (1)提交完代码发现有些东西没有提交，或者需要修改，可以修改完之后使用

```shell
git commit -m "first commit"
git add forget_commit_file1.c forget_commit_file2.c # 添加忘记提交的文件
git commit --amend
```

这个命令会将暂存区（git add）的文件提交到本地库，如果没有修改，则快照保持不变。最终本地库记录只有一个提交，第二次提交回代替第一次的提交。

- 取消暂存区文件

```shell
git reset HEAD error_commit.c # 撤销暂存区的提交记录
```

- 撤销对文件的修改

```shell
git checkout -- contributing.c # 从暂存区撤销对文件的修改
git status # 会提示很多你可能进行的操作, 可以仔细看看
```



##### 详细讲解

- git 配置变量, 保存在 ~/.gitconfig 文件中 或者系统文件 /etc/gitconfig 中

```shell
git --version
git config --global user.name "qz"
git config --global user.email "897707210@qq.com"
git config --system alias.st status # 设置 git 命令的别名 git st
git config --system alias.ci commit # git ci
git config --system alias.lg "log --pretty=fuller" # 设置长的别名
git config --unset --system alias.lg  # unalias ???
# 如果拥有管理员权限则可以使用 sudo 让所有用户都使用这个别名
sudo git config --system alias.st status
sudo git config --system alias.ci commit
# 设置颜色
git config --global color.ui true
vi ~/.gitconfig
# 查看配置文件所有位置 ，INI 文件格式
git config -e # 查看版本库级别的配置文件, 当前库目录下 .git/config 文件，使用的时候优先级最高
git config -e --global # 全局的配置文件, 在用户家目录下的 ~/.gitconfig 文件， windows:C:/user/qz/.gitconfig
git config -e --system # 系统级的配置, /etc/gitconfig 文件, windows:D:/install_location/GitInstallFile/Git/etc/gitconfig
# 删除设置
git config --unset --global color.ui
# git config 命令, 传入key-get, 传入key-val:set，
git confg user.name # 读取设置的参数
git commit --allow-empty -m "firt commit" # 可以没有作者提交
# git config 可以打开任何 INI 格式的文件并设置参数
GIT_CONFIG=test.ini git config a.b.c.d "hello,world" # 在当前目录下生成一个 test.ini(INI格式)的文件，可以用于其它软件的配置使用
GIT_CONFIG=test.ini git config a.b.c.d
git config --unset a.b.c.d # 删除 当前版本库目录下 .git/config 文件中的 [a "b.c"]
```

![img](C:\Users\qz\AppData\Local\Temp\企业微信截图_16524906117055.png)

- 版本库初始化

```shell
git init # git init demo, 会创建一个 demo 目录, 并初始化仓库
echo "hello" > hello.txt
git add hello.txt
git commit -m "first commit" # 如果不想使用 vim 则可以使用 -m 参数编辑提交说明
git rev-parse --git-dir # 获取版本库所在目录（.git目录位置）, 这些命令写 shell 脚本的时候有可能会用到
git rev-parse --show-toplevel # 显示版本库的位置
git rev-parse --show-prefix # 项目版本库的相对路径
git rev-parse --show-cdup # 显示工作区的深度
git rev-parse --help # file:///D:/install_location/GitInstallFile/Git/mingw64/share/doc/git-doc/git-rev-parse.html
```

- strace 命令查看命令执行过程

```shell
strace ls # 会打印这个命令执行的详细过程, 比如：从创建线程创建，加载动态库，创建进程一直到进程结束
strace -e 'trace=file' git status # 跟踪 git status 向上递归查找 .git 文件的过程
```

**可以学习的地方有很多比如 log保存格式，创建进程，进程中创建线程等等**

- ReaMine bug 管理系统

**Android 项目为了更好的使用 Git 实现对代码的集中管理，开发了一套叫做 Gerrit 的审核服务器来管理 Git 提交，对提交者的邮件地址进行审核**

- 暂存区

```shell
ls --full-time a.txt # 会显示时间戳的
```

- HEAD

HEAD 是指向 master 分支的 cursor, 出现 HEAD 的地方就可以用分支名替换, 

```shell
git diff master
```





















