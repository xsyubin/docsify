GIT去中心化，但是一般还是会用到远程仓库。
Git跟踪并管理的是修改，而非文件。修改1--add--修改2--commit：只提交修改1
Git在1秒钟之内就能完成分支！无论你的版本库是1个文件还是1万个文件


## git配置

ssh-keygen -t rsa -C "542676819@qq.com"

git config --global  --list
git config --global user.email "542676819@qq.com"
git config --global user.name "xsyubin"
 


## git查看版本

git version					:查看git版本

## git从远程仓克隆

git clone [-b branch url 	:从远程仓库克隆，默认远程库名：origin
git clone --branch v5.1.4.RELEASE git@github.com:spring-projects/spring-framework.git  
git clone --branch HikariCP-3.2.0 git@github.com:brettwooldridge/HikariCP.git
git clone --branch v1.0.0 git@github.com:seata/seata.git
git clone ssh://git@gitlab.uf-tobacco.com:8902/tc/springboot_hsf_test.git
git clone git@github.com:xsyubin/shiro-example.git

## git远程仓操作：添加、删除、重定向、详情
git remote
git remote show 			:查看所有远程库，等同于git remote -v
git remote add refs url		:添加新的远程库，refs为仓库名
git remote add origin git@github.com:xsyubin/webserver.git 	:首次需要关联到远程仓库
git remote rm refs          :删除远程库
git remote rename refs dota  :重命名远程库
git remote set-url refs url :更改远程库URL。git remote set-url --add dota url（对dota增加另一个远程地址，实现1对多）
git remote show dota		:查看远程库的详情


## git本地仓操作：初始化，add、commit

git init testgit			:初始化创建本地版本库，testgit可以为空，代表当前目录
git status					:查看当前仓库的文件状态，删除啦、添加啦、变更啦
git diff readme.txt 		:查看发生什么变更,输入字母q，直接退出（可能需要回车）
git add b.png a.txt 		:添加文件到暂存区,类似SVN的add（纳入版本控制，暂不提交）
git rm a.txt 				:暂存区的文件打删除标记，提交后可删除
git commit -m "提交说明"		:提交到本地仓库
git log						:查看提交记录。git log --pretty=oneline
git log --graph --pretty=oneline --abbrev-commit
git reset --hard HEAD^		:回退版本，HEAD：当前版本。HEAD^：上一个版本。HEAD^^：上上版本。HEAD~100：上100个版本
git reset --hard 1094a		:回退到指定的版本ID号

git reflog					:最全的log,命令记录

特殊：

1、checkout --组合：代表撤销工作区
git checkout -- readme.txt 	:撤销工作区的修改。--很重要，没有--，就变成了“切换到另一个分支”的命令
2、reset HEAD：代表撤销暂存区
git reset HEAD readme.txt 	:撤销暂存区的修改,add的反操作.
							reset命令既可以回退版本，也可以把暂存区的修改回退到工作区。当我们用HEAD时，表示最新的版本。

## git提交到远程

git push -u [refs][branch]	:第一次推送XX分支的所有内容到远程仓库。refs为远程仓库，默认origin；branch为分支，默认当前分支
git pull [refs][branch]		:从远程拉取代码,refs为远程仓库，默认origin；branch为分支，默认当前分支
例如：git pull origin/2-transaction-message
git pull = git fetch + git merge FETCH_HEAD


## git分支操作

git branch	[-r -a]			:查看分支概要信息，默认查看本地分支，-r：远程；-a：查看所有
git branch -vv              :查看分支详情
git branch test				:创建分支test
git checkout test 			:切换到分支test
git checkout -b test 		:创建分支test，并切换到test分支，等于上面2个的合并
git checkout -b dev origin/dev :创建分支,并切换到分支，同时和远程分支关联
git branch -d <分支名> 		:删除分支,-D 则为强制删除分支

git branch --set-upstream-to=origin/dev dev ：本地分支和远程分支关联

git switch -c dev			:创建并切换分支
git switch master			:切回主分支


## git分支合并

git merge dev 				:默认模式：Fast forward
git merge test，test2 		:把test、test2分支合并到master分支上（前提：切回master）
git merge --no-ff -m "merge with no-ff" dev 	:带一次commit的分支合并，会在log留痕



## 工作区暂存

git stash 					:把当前工作区暂存起来，这样可以切其他分支或者本分支的代码
git stash list 				:查看暂存列表
git stash apply stash@{0} 	:恢复,后面的stash@{0}可选，多个时指定
git stash drop 				:删除暂存的数据
git stash pop 				:恢复并删除，以上2指令的合并


## git特殊：
git cherry-pick 4c805e2     :在当前分支重做一次其他分支的动作，最后是动作id

git rebase 					:本地提交历史就成了一条直线


## git标签使用

git tag v0.9 f52c633		:标签名+提交id，提交id为空，则最后一次
git show <tagname>			:查看标签信息
git tag -a <tagname> -m "blablabla..."可以指定标签信息
git tag -d v0.1 				:删除标签
git push origin v1.0 		:推送XX标签到远程
git push origin --tags 		:推送未提交的标签到远程
git push origin :refs/tags/v0.9 :删除远程标签



docker login http://f1361db2.m.daocloud.io


curl -X GET http://hub.c.163.com/v2/dfc-nacos/tags/list

curl -X GET https://registry-1.docker.io/v2/dfc-nacos/tags/list

curl -X GET http://1.1.1.100:5000/v2/_catalog


docker pull hub.c.163.com/library/dfc-nacos:1.0



docker pull yflyers/dfc-nacos:1.0
