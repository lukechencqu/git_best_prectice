# git_best_prectice
git最佳经验汇总




============================================
经过验证OK的最常用操作
git add .
git commit -m "xxx"
git push -u origin dev
如果提示远程仓库有新的更新，那么就要进行下面操作
git log
git pull origin dev
git push -u origin dev
============================================

==== 下行：更新并merge远程git仓库到本地仓库
https://blog.csdn.net/qq_38350702/article/details/106773076
git fetch origin master:tmp
git diff tmp
git merge tmp

方式2：
在本地已经存在foxy-lifecycle分支的情况下（但比较老了），将远程仓库foxy-lifecycle分支源码更新到本地分支
git pull origin foxy-lifecycle

==== 上行：上传本地仓库到远程仓库
新建一个远程仓库并上传本地源码到此新建远程仓库
git checkout -b new
git add .
git commit -m "xx"
git push -u origin xx

==== 删除远程或本地分支仓库
git branch #查看当前所在分支
git branch -d dev_lizhen #删除本地名为dev_lizhen的分支
git push origin --delete dev_lizhen #删除远程仓库中名为dev_lizhen的分支

==== 切换本地分支
git checkout 分支名

==== 切换到远程某个分支，并下载该分支源码
https://blog.csdn.net/zhoumoon/article/details/111995282
步骤1：查看远程所有分支
git branch -a
步骤2：本地新建一个与准备切换到的远程分支同名的本地分支，并与远程分支关联
git checkout -b foxy-lifecycle origin/foxy-lifecycle
步骤3：将远程分支源码拉取到本地分支
git pull origin foxy-lifecycle

==== 查看所有远程分支
git branch -a

==== 当地新建分支，并推到远程仓库（远程仓库尚未有此新分支）
https://www.cnblogs.com/twoheads/p/9836716.html
git push --set-upstream origin wangxiao

## github
### 新建仓库
```
Quick setup — if you’ve done this kind of thing before
or	
https://github.com/lukechencqu/11.git
Get started by creating a new file or uploading an existing file. We recommend every repository include a README, LICENSE, and .gitignore.

…or create a new repository on the command line
echo "# 11" >> README.md
git init
git add README.md
git commit -m "first commit"
git branch -M main
git remote add origin https://github.com/lukechencqu/11.git
git push -u origin main
…or push an existing repository from the command line
git remote add origin https://github.com/lukechencqu/11.git
git branch -M main
git push -u origin main
…or import code from another repository
You can initialize this repository with code from a Subversion, Mercurial, or TFS project.

```

## gitlab
### 新建仓库
```
Command line instructions
You can also upload existing files from your computer using the instructions below.


Git global setup
git config --global user.name "chenlu"
git config --global user.email "chenlu@dreame.tech"

Create a new repository
git clone git@git.dreame.tech:MJR/SLAM/12.git
cd 12
git switch -c master
touch README.md
git add README.md
git commit -m "add README"
git push -u origin master

Push an existing folder
cd existing_folder
git init --initial-branch=master
git remote add origin git@git.dreame.tech:MJR/SLAM/12.git
git add .
git commit -m "Initial commit"
git push -u origin master

Push an existing Git repository
cd existing_repo
git remote rename origin old-origin
git remote add origin git@git.dreame.tech:MJR/SLAM/12.git
git push -u origin --all
git push -u origin --tags
```
