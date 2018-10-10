---
title: Git 常用命令及使用场景
date: 2017-08-04 09:52:18
categories:
tags:
---

<!--more-->


本文只写git命令相关内容，git安装及配置https://git-scm.com/、github如何创建repository、添加ssh等，请到https://help.github.com/

以github作为远程仓库托管平台为例：


如果本地项目已经创建好，在github上创建好仓库（最好和项目同名）

本地切换到项目主目录 cd <path>
git init 	//创建初始化仓库
git add .		// 把工作区的修改的内容存入暂存区
git commit -m "提交信息"	// 把暂存区的文件保存为一个commit
git remote add origin <path>	// 添加远程仓库地址
git push -f origin master		// 把本地仓库修改push到远程仓库

如果已经有远程仓库：
git clone <repo_address>	// 把远程仓库克隆到本地

这里解释两个名词：
工作区：刚刚修改的文件，是被存储在工作区的。可以使用git status查看修改的文件，或者使用git diff查看修改的文件的详细内容
暂存区：一个缓存区，临时保存你的改动，git add 之后会把工作区的文件存入暂存区。可以执行git status查看分别存在工作区和暂存区修改的文件。存入暂存区后git diff就不能查看了，这时你可以执行git reset <filename>把暂存区的内容保存后工作区


基础使用：
下面逐个命令分析：

首先执行
git init
创建并初始化git仓库

创建仓库后，你可以执行：
git status
查看改动了那些文件

如果想看每个文件的具体改动，可以
git diff <filename>

全部改动
git diff
但是查看改动的文件，只能在工作区，如果被添加到暂存区，就看不到了
我们可以把暂存区的改动撤销到工作区，请看下面的git reset

修改完成后，执行
git add <filename>
把工作区中你修改的要提交的文件添加到暂存区

或者使用如下命令：
git add .
把工作区的全部修改添加到暂存区

更高级一点，你可以交互式的添加
git add -i

使用如下命令提交你的改动：
git commit -m "提交信息"

如果你想添加和提交一条命令：
git commit -am "提交信息"

假如你的"提交信息"输入错误，你可以执行：
git commit --amend
会进入你配置的编辑器，然后你可以修改你的"提交信息"。比如设置vim为编辑器
git config --global core.editor vim
git config --global core.editor /usr/bin/vim

或者直接
git commit --amend -m "新的提交信息"

提交完修改后，发现工作区有漏掉的文件没有提交，或者想把新修改的文件添加到最新的一次本地commit，你需要：
git add .
git commit --amend
这样，你的漏掉或者新的改动就会提交到最新的一次本地commit

你的改动现在只保存在本地仓库，执行如下命令可以推送你的改动到远端仓库
git push origin master
有时可能会push失败，需要（谨慎使用）
git push -f origin master
强制推送

如果你没有添加远端仓库地址，你可以执行
git remote add origin <git@github.com:you_github_name/you_repository_name.git>
添加远程仓库地址

如果远程仓库别人有新的提交，你可以执行：
git pull
同步到你的本地仓库。第一次pull时，这时候可能会错误，按照错误提示修改一些就好了


分支（branch）：
多人协作工作时，一般会基于主分支创建自己的分支，执行自己的任务。任务完成后再把自己的分支合并到主分支。

你可以执行如下命令：
git branch
查看本地分支，或者
git branch -a
查看本地及远程分支

你可以基于当前分支创建新的分支，如新分支命名为new_branch
git branch <new_branch>
或者基于指定的分支branch，创建新的分支new_branch
git branch <new_branch> <branch>

创建好分支后，切换到新分支new_branch
git checkout <new_branch>

以上两步可以用一条命令代替，创建新分支，并切换到新分支
git checkout -b <new_branch>
或者
git checkout -b <new_branch> <branch>

分支创建好后，只存在本地，只能自己看到，如果想同步到远程
git push origin <new_branch> <new_branch>

git branch -v
可以查看每个分支最后的提交信息

如果你想删除某个分支，需要切换到其他分支，比如master:
git checkout master
再把新建的分支删掉：
git checkout -d <new_branch>
当要删除的分支有为推送的commit时，删除失败，可以使用-D强制删除，如下
git checkout -D <new_branch>

现在你知识删除了本地分支，远程的分支怎么删除呢，你可以执行：
git push origin :<new_branch>
这行命令表示，你把一个空的分支同步到了new_branch，这样就删掉了

log:
如果你想查看本地仓库的提交记录，你可以执行
git log

你也可以添加一些参数，更精确的得到自己想要的结果，比如每条提交记录压缩为一行输出：
git log --pretty=oneline

只想查看某一个人的提交记录
git log --author=name

或者通过 ASCII 艺术的树形结构来展示所有的分支, 每个分支都标示了他的名字和标签
git log --graph --oneline --decorate --all

查看每次提交记录，改动了哪些文件
git log --name-status
想要查看更多参数，请参考：
git log --help

如果想要查看某个提交的改动的具体内容，可以执行
git show <commit_id>

合并：
在你的分支完成任务，提交改动后，你需要把你的分支合并到主分支，如master：
git checkout master
合并前需要同步远程仓库到本地：
git pull
然后执行合并操作：
git merge <new_branch>

合并前你可以查看改动前后的差异：
git diff master <new_branch>

合并时冲突不是能完成避免的，产生冲突时如何你想撤销合并，执行，
git merge --abort
如果不想撤销，你可以解决冲突后，把工作区的文件暂存到暂存区：
git add .
然后继续完成合并：
git commit
然后把本地的更新推送的远程仓库：
git push

a -> b -> c -> f -- g -> h (master)
          	 \          /
	          d -> e  (new_branch)

如上图，合并后生成新的commit_id: f

如果此时发现有bug，你想撤销合并，可以执行：
git reset --hard f

或者，使用revert撤销合并的内容:
git revert -m 1 g
（m指定主线，1代表parent）会生成一个新的提交G，如下图：
a -> b -> c -> f -- g -> h -> G (master)
        		  \          /
	            d -> e  (new_branch)

如果new_branch的bug修改后，
a -> b -> c -> f -- g -> h -> G -> i (master)
  	         \    	     /
	           d -> e -> j -> k (new_branch)
如果你直接合并，会丢失d和e的提交信息，所以你需要
git checkout master
git revert G
git merge new_branch
完成后，如下图：
a -> b -> c -> f -- g -> h -> G -> i -> G' -- m (master)
                 \            /                          		     /
	           d -> e -> j -> k ---------------    (dev)


你可以用rebase进行合并，也就是变基操作，而且会减少不必要的合并信息，产生一个更简洁的提交历史：
git rebase master new_branch
git checkout master
git merge new_branch

有时候，我们需要把自己分支的某一个commit，合并到主分支，
git checkout master
git cherry-pick commit_id

合并某个分支上一系列相连的commits：
在一些特定情况下，合并单个commit并不够，你需要合并一系列相连的commits。这种情况下就不要选择cherry-pick了，rebase更适合。假如你需要合并feature分支的commit 23kj23j4~ak1kj23到master分支，首先你需要基于feature创建一个新的分支，并指明新的分支的最后一个commit：
git checkout -b new_branch ak1kj23

然后，rebase这个新分支的commit到master。23kj23j4^指明你想从哪个特定的commit开始。
git rebase --onto=master 23kj23j4^
得到的结果就是feature分支的commit 23kj23j4~ak1kj23都被合并到了master分支

修改改动：
有时我们会把暂存区的内容，想要查看改动的内容，或者不想本次提交，想要分两次提交，可以执行：
git checkout <filename>
把暂存区的文件撤销到工作区

假如你想丢弃到本次的所有改动（工作区和暂存区），可以执行：
git reset --hard
撤销所有未commit的改动

如果你想撤销最后一次提交（未推送到远程），可以执行：
git reset HEAD^
撤销最后一次提交（未push的远程）到工作区，如果已push到远程，会创建一个新的commit，同revert

你也撤销指定的commit_id之后的所有提交，并把改动内容撤销到工作区
git reset commit_id

也可以直接抛弃掉：
git reset --hard commit_id

如果要丢弃掉所有本地改动与提交，可以到服务器上获取最新的版本历史，并将你本地主分支指向它
git reset --hard origin/master


如果想回退版本后，再修改回来，你需要使用
git revert
进行版本回退，会产生一个新的提交内容G
如果想修改回来，再执行：
git revert G
就可以了

你可以撤销前一次的commit：
git revert HEAD

或者撤销前前一次的commit：
git revert HEAD^

甚至撤销指定的commit
git revert commit-id


储藏（stash）：
有时我们改动的文件后，想要更新远程的改动、或者不想提交需要先保存起来、或者想要应用到不同的分支，这是我们可以使用：
git stash

我们可知储藏的改动添加说明信息：
git stash save "说明嘻嘻"

也可以查看储藏几条：
git stash list

可以查看每一条改动的文件：
git stash show <stash_id>

或者把某一个储藏内容，应用到当前分支，并不从储藏室删除：
git stash apply <stash_id>

或者直接应用到分支，并且删除：
git stash pop
默认是最新的一条stash

也可指定stash：
git stash pop <stash_id>

apply或者pop是可能会产生冲突，如果pop时产生冲突，就不会立即从储藏室删除了

如果想删除掉储藏室的某一条：
git stash drop <stash_id>

或者直接清除储藏室：
git stash clear



查找：

如果想要查看文件的每个部分是谁修改的，那么git blame就是不二选择，可以看到每一行的详细修改信息：包括SHA串，日期和作者
git blame <filename>

如果想查找某一段文字，比如 Macro_iPad
git grep Macro_iPad

还可以显示行号
git grep -n Macro_iPad

或者只显示文件名
git grep --name-only Macro_iPad

每个文件里有多少行匹配内容
git grep -c Macro_iPad

或者某个tag里的内容
git grep Macro_iPad v1.1.0


git bisect
有时你可能会遇到这种情况：
如果一个代码仓库中发现了一个bug，想要找到出现bug的commit，再修复bug时，可以使用git checkout到很久的一个commit，然后查看是否存在bug，如果没有再git checkout到这个commit和最新的commit中间的一个commit，查看是否存在bug，以这种二分查找法查找。是的，我最初是这么做的，现在我告诉你一个更方便的方法，git disect，git 自带的二分查找。
git bisect start
git bisect good <good_commit>
git bisect bad <bad_commit>
git bisect good
git bisect good
git bisect bad
git bisect good
......
git bisect reset 	// 退出

git bisect最初是用于寻找bug的引入的坏点而不是fix bug的好点，如果某个bug被不知不觉的修复了，故想要查找修复的bug的commit时，只要把good和bad的概念颠倒一些就可以了
git bisect start
git bisect good <bad_commit>
git bisect bad <good_commit>
......
git bisect reset


git fsck -lost-found
commit删除后，变成dangling commit(悬空对象)，并不是真正的删除，只要把dangling commit(悬空对象)找出来，git rebase或git merge把他们恢复

其他：

查看远程仓库地址：
git remote -v

删除远程仓库：
git remote rm origin

添加多个远程仓库
git remote set-url --add origin <repo_address>
可以在配置中查看
.git/config

有时远程别人的分支已经删除了，本地却还能看到，可以执行
git fetch -p
同步一下，这样本地也就没有了


软件发布时，推荐为每个版本创建一个标签，方便以后的管理
git tag 1.0 <commit_id>

推荐的开发流程：
基于主分支创建自己的分支->完成任务后，提交commit->把自己的分支推送到github->在github上创建一个pull request->其他开发人员review->没有问题后合并到主分支
