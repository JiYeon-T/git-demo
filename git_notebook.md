#### Git学习笔记

###### 1. windows 版本的 git 无法提交代码的问题还没有解决:

https://www.jianshu.com/p/5a7028b515ad

可以在 windows 提交代码的Git 配置，需要翻墙，否则"run time error"。配置如图:

![image-20211122224205739](C:\Users\qz\AppData\Roaming\Typora\typora-user-images\image-20211122224205739.png)

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
git add 文件名	# 从工作区添加到暂存区, eg: git add .
git rm --caches 文件名	# 从暂存区删除某一个文件，只是从暂存区删除文件
git commit	#提交到本地库, git commit -m "日志信息" 文件名
git reflog		# 查看历史版本粗略信息
git log		# 查看版本详细信息
git reset --hard 版本号	# 版本穿梭
git reset --hard 8a5fc91	# 通过git reflog即可查看精简的版本号， Git切换版本，底层就是移动指针
git reset --soft HEAD^	# 本地代码回退

git checkout -f # 删除本地所有为暂存的修改
git reset --hard	# 将修改加入暂存区
git clean -xdf
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
?
?
?
?
?
?
?
?
?
?
？
？
？
？

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






















