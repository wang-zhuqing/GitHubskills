# GitHubskills

﻿学习  不忘初心  虽然现在很水


之前上传Git,使用的idea,这次使用的Git仓库，把D：gitLearn设置为仓库了

这个文件用于测试，是否上传到。。。到哪里了，这个还真不知道，没有要我输入我GitHub的地址啊？


初始化一个Git仓库，使用git init命令。

1 添加文件到Git仓库，分两步：

	使用命令git add <file>，注意，可反复多次使用，添加多个文件；
	使用命令git commit -m <message>，完成。


2 修改后提交步骤

	运行git status命令看看结果：

	$ git status <这行是注释：这行是命令行，下面是提交命令后的展示内容>
			On branch master
			Changes not staged for commit:
			  (use "git add <file>..." to update what will be committed)
			  (use "git checkout -- <file>..." to discard changes in working directory)

				modified:   readme.txt

			no changes added to commit (use "git add" and/or "git commit -a")

	git status命令可以让我们时刻掌握仓库当前的状态，上面的命令输出告诉我们，readme.txt被修改过了，但还没有准备提交的修改。

	虽然Git告诉我们readme.txt被修改了，但如果能看看具体修改了什么内容，自然是很好的。比如你休假两周从国外回来，
	第一天上班时，已经记不清上次怎么修改的readme.txt，所以，需要用git diff这个命令看看：

	下一步，就可以放心地提交了：

	$ git commit -m "add distributed"

3 版本回退

	像这样，你不断对文件进行修改，然后不断提交修改到版本库里，就好比玩RPG游戏时，每通过一关就会自动把游戏状态存盘，
	如果某一关没过去，你还可以选择读取前   一关的状态。
	有些时候，在打Boss之前，你会手动存盘，以便万一打Boss失败了，可以从最近的地方重新开始。
	Git也是一样，每当你觉得文件修改到一定程度的时候，就可以“保存一个快照”，这个快照在Git中被称为commit。
	一旦你把文件改乱了，或者误删了文件，还可以从最近的一个commit恢复，然后继续工作，而不是把几个月的工作成果全部丢失。

	当然了，在实际工作中，我们脑子里怎么可能记得一个几千行的文件每次都改了什么内容，不然要版本控制系统干什么。
	版本控制系统肯定有某个命令可以告诉我们历史记录，
	在Git中，我们用git log命令查看：

	如果嫌输出信息太多，看得眼花缭乱的，可以试试加上--pretty=oneline参数：

	$ git log --pretty=oneline

	首先，Git必须知道当前版本是哪个版本，在Git中，用HEAD表示当前版本，也就是最新的提交1094adb...（注意我的提交ID和你的肯定不一样），
	上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100。
	现在，我们要把当前版本回退到上一个版本，就可以使用git reset命令：
	$ git reset --hard HEAD^

	好比你从21世纪坐时光穿梭机来到了19世纪，想再回去已经回不去了，肿么办？
	办法其实还是有的，只要上面的命令行窗口还没有被关掉，你就可以顺着往上找啊找啊，
	找到那个append GPL的commit id是1094adb...，于是就可以指定回到未来的某个版本：
	$ git reset --hard 1094a

	现在，你回退到了某个版本，关掉了电脑，第二天早上就后悔了，想恢复到新版本怎么办？找不到新版本的commit id怎么办？
	在Git中，总是有后悔药可以吃的。当你用$ git reset --hard HEAD^回退到add distributed版本时，再想恢复到append GPL，
	就必须找到append GPL的commit id。Git提供了一个命令git reflog用来记录你的每一次命令：
	$ git reflog
			e475afc HEAD@{1}: reset: moving to HEAD^
			1094adb (HEAD -> master) HEAD@{2}: commit: append GPL
			e475afc HEAD@{3}: commit: add distributed
			eaadf4e HEAD@{4}: commit (initial): wrote a readme file
	终于舒了口气，从输出可知，append GPL的commit id是1094adb，现在，你又可以乘坐时光机回到未来了。

	现在总结一下：

		HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。

		穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。

		要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。

4 工作区和暂存区

	工作区（Working Directory）
	就是你在电脑里能看到的目录，比如我的learngit文件夹就是一个工作区：

	工作区有一个隐藏目录.git，这个不算工作区，而是Git的版本库。
	Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，
	还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。

	分支和HEAD的概念我们以后再讲。
	前面讲了我们把文件往Git版本库里添加的时候，是分两步执行的：
	第一步是用git add把文件添加进去，实际上就是把文件修改添加到暂存区；
	第二步是用git commit提交更改，实际上就是把暂存区的所有内容提交到当前分支。
	因为我们创建Git版本库时，Git自动为我们创建了唯一一个master分支，所以，现在，git commit就是往master分支上提交更改。
	你可以简单理解为，需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。

	所以，git add命令实际上就是把要提交的所有修改放到暂存区（Stage），然后，执行git commit就可以一次性把暂存区的所有修改提交到分支。
	一旦提交后，如果你又没有对工作区做任何修改，那么工作区就是“干净”的：

	提交后，用git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别：


5 撤销修改

	自然，你是不会犯错的。不过现在是凌晨两点，你正在赶一份工作报告，你在readme.txt中添加了一行：

		$ cat readme.txt
		Git is a distributed version control system.
		Git is free software distributed under the GPL.
		Git has a mutable index called stage.
		Git tracks changes of files.
		My stupid boss still prefers SVN.
	在你准备提交前，一杯咖啡起了作用，你猛然发现了stupid boss可能会让你丢掉这个月的奖金！

	既然错误发现得很及时，就可以很容易地纠正它。你可以删掉最后一行，手动把文件恢复到上一个版本的状态。如果用git status查看一下：

			$ git status
			On branch master
			Changes not staged for commit:
			  (use "git add <file>..." to update what will be committed)
			  (use "git checkout -- <file>..." to discard changes in working directory)

				modified:   readme.txt

			no changes added to commit (use "git add" and/or "git commit -a")
	你可以发现，Git会告诉你，git checkout -- file可以丢弃工作区的修改：
	命令git checkout -- readme.txt意思就是，把readme.txt文件在工作区的修改全部撤销，这里有两种情况：

	一种是readme.txt自修改后还没有被放到暂存区，现在，撤销修改就回到和版本库一模一样的状态；

	一种是readme.txt已经添加到暂存区后，又作了修改，现在，撤销修改就回到添加到暂存区后的状态。

	总之，就是让这个文件回到最近一次git commit或git add时的状态。


6  删除文件

	一般情况下，你通常直接在文件管理器中把没用的文件删了.
	这个时候，Git知道你删除了文件，因此，工作区和版本库就不一致了，git status命令会立刻告诉你哪些文件被删除了：

			$ git status
			On branch master
			Changes not staged for commit:
			  (use "git add/rm <file>..." to update what will be committed)
			  (use "git checkout -- <file>..." to discard changes in working directory)

				deleted:    test.txt

			no changes added to commit (use "git add" and/or "git commit -a")

	现在你有两个选择，一是确实要从版本库中删除该文件，那就用命令git rm删掉，并且git commit：

			$ git rm test.txt
			rm 'test.txt'

			$ git commit -m "remove test.txt"
			[master d46f35e] remove test.txt
			 1 file changed, 1 deletion(-)
			 delete mode 100644 test.txt
	现在，文件就从版本库中被删除了。

	另一种情况是删错了，因为版本库里还有呢，所以可以很轻松地把误删的文件恢复到最新版本：

	$ git checkout -- test.txt
		git checkout其实是用版本库里的版本替换工作区的版本，无论工作区是修改还是删除，都可以“一键还原”。

7 提交到Github

	目前，在GitHub上的这个learngit仓库还是空的，GitHub告诉我们，可以从这个仓库克隆出新的仓库，
	也可以把一个已有的本地仓库与之关联，然后，把本地仓库的内容推送到GitHub仓库。

	现在，我们根据GitHub的提示，在本地的learngit仓库下运行命令：
	$ git remote add origin git@github.com:xxxxxxxxxx
	注意：git@github地址是你自己的GitHub的ssh地址

	添加后，远程库的名字就是origin，这是Git默认的叫法，也可以改成别的，但是origin这个名字一看就知道是远程库。

	下一步，就可以把本地库的所有内容推送到远程库上：

			$ git push -u origin master
			Counting objects: 20, done.
			Delta compression using up to 4 threads.
			Compressing objects: 100% (15/15), done.
			Writing objects: 100% (20/20), 1.64 KiB | 560.00 KiB/s, done.
			Total 20 (delta 5), reused 0 (delta 0)
			remote: Resolving deltas: 100% (5/5), done.
			To github.com:michaelliao/learngit.git
			 * [new branch]      master -> master
			Branch 'master' set up to track remote branch 'master' from 'origin'.
			
	把本地库的内容推送到远程，用git push命令，实际上是把当前分支master推送到远程。

	由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，
	还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。

	每次本地提交后，只要有必要，就可以使用命令git push origin master推送最新修改；

	从现在起，只要本地作了提交，就可以通过命令：
		$ git push origin master
	把本地master分支的最新修改推送至GitHub，现在，你就拥有了真正的分布式版本库！

8 从远程库克隆

	上次我们讲了先有本地库，后有远程库的时候，如何关联远程库。

	现在，假设我们从零开发，那么最好的方式是先创建远程库，然后，从远程库克隆。

	首先，登陆GitHub，创建一个新的仓库，名字叫gitskills

	现在，远程库已经准备好了，下一步是用命令git clone克隆一个本地库：

		$ git clone git@github.com:michaelliao/gitskills.git
		Cloning into 'gitskills'...
		remote: Counting objects: 3, done.
		remote: Total 3 (delta 0), reused 0 (delta 0), pack-reused 3
		Receiving objects: 100% (3/3), done.
	注意把Git库的地址换成你自己的，然后进入gitskills目录看看，已经有README.md文件了：

		$ cd gitskills
		$ ls
		README.md

	如果有多个人协作开发，那么每个人各自从远程克隆一份就可以了。

	你也许还注意到，GitHub给出的地址不止一个，还可以用https://github.com/michaelliao/gitskills.git这样的地址。实际上，Git支持多种协议，默认的git://使用ssh，但也可以使用https等其他协议。

	使用https除了速度慢以外，还有个最大的麻烦是每次推送都必须输入口令，但是在某些只开放http端口的公司内部就无法使用ssh协议而只能用https。

	小结
	要克隆一个仓库，首先必须知道仓库的地址，然后使用git clone命令克隆。

	Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。














