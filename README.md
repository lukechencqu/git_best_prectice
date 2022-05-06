# git_best_prectice
git最佳经验汇总

# token令牌
解决问题：  
```
remote: Support for password authentication was removed on August 13, 2021. Please use a personal access token instead.
remote: Please see https://github.blog/2020-12-15-token-authentication-requirements-for-git-operations/ for more information.
```
```
git config --local credential.helper ""
vim .git/config
修改为
[remote "origin"]
        url = https://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@github.com/lukechencqu/git_best_prectice.git

备注：也可以用命令行修改
git remote set-url origin https://<your_token>@github.com/<USERNAME>/<REPO>.git
<your_token>：换成你自己得到的token
<USERNAME>：是你自己github的用户名
<REPO>：是你的仓库名称
```

## 注意事项

- 只有github生成令牌时才会显示令牌内容，此时一定要把令牌复制保存到本地文件，否则一旦忘记令牌的话，github不能够显示出来之前生成的令牌内容
- 如果实在忘记了令牌内容，那么需要更新（重新生成）令牌，并将新令牌更新到git仓库config文件中，同时，之前依赖此令牌的仓库也必须要更新为此新令牌（实在太麻烦了，因此，一定要记住令牌内容，不要搞忘记了！）   

参考：  
https://zhuanlan.zhihu.com/p/414028184  
https://baijiahao.baidu.com/s?id=1714142496447512417&wfr=spider&for=pc   


# commit规范
Commit message（提交说明）   
Angular规范（貌似是针对js开发的？）是目前使用最广的写法，比较合理和系统化  
```
feat：新功能（feature）
fix：修补bug
docs：文档（documentation）
style： 格式（不影响代码运行的变动）
refactor：重构（即不是新增功能，也不是修改bug的代码变动）
test：增加测试
chore：构建过程或辅助工具的变动
``` 

### 生成日志：从规范生成代码变动日志changle log
git命令行指令：   
```
将所有原始log保存：
git log > CHANGLELOG.txt
使用正则表达式将所有type包含关键词feat和docs的保存：
git log -E --grep="^(feat|docs)"> CHANGLELOG.txt
将所有包含docs关键词的style日志保存：
git log -E --grep="^(feat|docs)*docs"> CHANGLELOG.txt

更复杂一些的保存，保存指定tags之间的日志：
git log $(git describe --tags --abbrev=0)..HEAD --pretty=format:"%s" -i -E --grep="^(\[INTERNAL\]|\[FEATURE\]|\[FIX\]|\[DOC\])*\[FEATURE\]" > myfile.txt
上述指令的注释：
$(git describe --tags --abbrev=0)…HEAD
$(git describe --tags --abbrev=0)：提取最近的一次tag
HEAD：则是最新的Commit
此段含义：最近的tag到最新的Commit的期间
```
参考：  
https://zhuanlan.zhihu.com/p/438070919
https://blogs.sap.com/2018/06/22/generating-release-notes-from-git-commit-messages-using-basic-shell-commands-gitgrep/  


### 查看日志： git log
https://blogs.sap.com/2018/06/22/generating-release-notes-from-git-commit-messages-using-basic-shell-commands-gitgrep/
```
指定打印排版格式：
git log --pretty=format:"%h - %an, %ar : %s"
指定起始日期：
git log --pretty=format:"%h - %an, %ar : %s" --since=2.weeks
打印提交总次数：
git log | wc -l
打印自指定日期开始至今的提交总次数：
git log --since=2.weeks | wc -l
打印自指定tag至今的提交总次数：
git log --pretty=oneline 1.6.1..HEAD | wc -l
查看指定分支的总提交次数：
git rev-list master --count
```


以下为js开发用的：   
conventional-changelog    https://github.com/conventional-changelog/conventional-changelog   
参考：   
https://docs.google.com/document/d/1QrDFcIiPjSLDn3EL15IJygNPiHORgU1_OOAqWjiDU5Y/edit#
https://www.jianshu.com/p/201bd81e7dc9  

### 规范模板工具(目前只发现js版本的) //TODO
git命令行：   //TODO  
```
git config --global commit.template /home/yu/git-commit-template.txt 
```
参考：  
https://zhuanlan.zhihu.com/p/438070919   
https://www.cnblogs.com/guyuyun/p/15028409.html  

js开发用的：  
Commitizen  

# 最常用的操作
### 合并




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


### 回滚到之前的某个commit
```
2种方式：
在工作区保留代码的修改及commit信息
git reset --soft 0acc888dbb0d1a7bc299e9d0692ef56fc61d7f3e
完全删除代码修改与commit信息
git reset --hard 0acc888dbb0d1a7bc299e9d0692ef56fc61d7f3e
```

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
