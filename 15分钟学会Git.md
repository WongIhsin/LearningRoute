# 15分钟学会Git

2019年5月2日 星期四 晴

###### Learn by reading

1. Git Handbook手册

   VCS Version Control System
   Git----DVCS Distributed version control system

2. Cheat Sheets备忘单

   https://github.github.com/training-kit/downloads/zh_CN/github-git-cheat-sheet/

##### 一、Usage:

Learn by reading
	Git Handbook手册
		VCS Version Control System
		Git----DVCS Distributed version control system
	Cheat Sheets备忘单
		https://github.github.com/training-kit/downloads/zh_CN/github-git-cheat-sheet/

一、Usage:
	1、下载git
		yum -y install git
		sudo apt-get install git
	2、配置全局变量
		git config --global user.name "Ihsin"
		git config --global user.email "tjuwangyixin@163.com"
		git config --global color.ui true
	3、创建仓库
		mkdir learngit
	4、初始化一个仓库
		cd learngit
		git init
	5、新写一个文件
		touch readme.txt
		vim readme.txt
	6、添加到仓库，可以多次添加
		git add readme.txt readme2.txt...
	7、将多次添加的文件一起提交
		git commit -m "说明内容"
	8、修改readme.txt之后，使用git status查看当前仓库状况
		vim readme.txt
		git status
	9、查看文件修改地方
		git diff readme.txt
	10、查看提交历史记录
		git log
		git log --pretty=oneline
		git log --graph --pretty=oneline --abbrev-commit
	11、HEAD表示当前版本
		HEAD^表示上一个版本
		HEAD^^表示上上一个版本
		HEAD~100表示前100个版本
	12、退回上一个版本
		git reset --hard HEAD^
		git reset --hard (commit id: 6ce977c)
	13、重返未来，查看命令历史
		git reflog
		可以看到commit id
	14、在工作区的修改全部撤销
		git checkout -- readme.txt
		让这个文件回到最近一次 git commit 或 git add 时候的状态
	15、删除
		git rm filename
		git commit -m "说明内容"
		删除一个文件，如果一个文件已经被提交到版本库，可以恢复
		git checkout -- test.txt
			实际上是用版本库中的版本替代工作区的版本
			修改和删除都可以一键还原
	16、分支操作
		git checkout -b dev
			-b参数表示创建并切换
			相当于
				git branch dev 创建分支
				git checkout dev 切换分支
			git branch 查看当前分支
		git merge dev 合并指定分支到当前分支
		git merge --no-ff -m "描述内容" dev 普通模式合并
		git branch -d dev 删除分支
		git branch -D dev 强行删除分支
		····························································
		·git checkout -b dev origin/dev 创建远程origin的dev分支到本地·
		·git branch --set-upstream-to=origin/dev dev 指定本地 dev 分支与远程 origin/dev 分支的链接
		····························································
	17、BUG分支
		git stash 把工作现场“储藏”起来，等以后恢复现场之后继续工作
		修复完成之后
		git stash list 列出stash内容
		1、git stash apply 恢复现场但需要使用 git stash drop 来删除stash
		2、git stash pop 恢复的同时把stash也删除
		3、多个stash时，使用 git stash apply stash@{0}
			删除也是使用 git stash drop stash@{0}
			删除第0个，第一个的序号会变为0
	18、撤销操作
		git reset HEAD file.name
		把暂存区的修改撤销掉
	19、配置别名
		git config --global alias.st status
		git config --global alias.unstage 'reset HEAD'
	20、配置文件位置
		每个仓库的 Git 配置文件在.git/config
			可以修改别名
		.gitignore 是忽略文件
		当前用户的 Git 配置文件放在用户主目录下的一个隐藏文件 .gitconfig 中
			可以修改用户名和email和修改别名

二、专业术语
	Git和其他版本控制系统如SVN的一个不同之处就是有暂存区的概念
	工作区 Working Directory
		/learngit
	版本库 Repository
		.git是Git的版本库
		1、称为stage(或者index)的暂存区
		2、Git自动创建的第一个分支master
		3、指向master的一个指针HEAD
	commit相当于一个快照、存档

	Gitosis管理公钥工具
	Gitolite严格控制权限工具


三、忽略特殊文件
	.git是Git仓库、版本库信息
	.gitignore是要忽略的文件放入这个文件夹，会自动忽略
	忽略文件的原则是：
		1、忽略操作系统自动产生的文件，如缩略图等
		2、忽略编译生成的中间文件、可执行文件等，也就是如果一个文件是通过另一个文件自动生成的，那自动生成的文件就没有必要放进版本库，如Java的.class文件
		3、忽略自己的带有敏感信息的配置文件，如存放口令的配置文件
	强制添加某个被忽略文件
		git add -f xx.class
	git check-ignore 来检查.gitignore 规则是否正确
		git check-ignore -v xx.class

四、GitHub
	注册一个GitHub既可以拥有一个免费的Git远程仓库
	关联一个远程库：
		git remote add origin https://github.com/WongIhsin/learngit.git
	删除一个远程库：
		git remote rm origin
	关联后使用：
		git push -u origin master第一次推送master分支的所有内容
		此后，每次本地提交后，只要有必要，就可以使用
		git push origin master 推送最新修改
		git push origin dev 选择适当的分支推送
		git push origin <tag-name>/--tags
	在GitHub上，可以任意Fork开源仓库
	自己拥有Fork后的仓库的读写权限
	可以推送 pull request 给官方仓库来贡献代码

····························································		
五、克隆仓库
	git clone git@github.com:WongIhsin/gitskills.git
	https和git都可以，git受ssh支持，速度快，不用输密码
····························································

六、分支策略
	1、Master分支应该是非常稳定的，也就是仅用来发布新版本，平时不能再上面干活
	2、平时干活应该在dev上面，dev分支是不稳定的，到某个时候，比如1.0版本发布时，再把dev分支合并到master上面，在master分支发布1.0版本
	3、dev只是一个名字，可以是michael, bob
	4、git merge 默认使用Fast Forward 合并并不会看出曾经合并过
		加上--no-ff -m "描述内容" 用普通模式合并，合并后的历史有分支，能看出曾经合并过
	5、修复bug时，会通过创建新的bug分支进行修复，然后合并，最后删除
		当手头没有工作完成时，可是先把现场 git stash
		然后修复bug后，git stash pop/apply 回到工作现场
	·?·
	·6、合并后需要手动删除分支·
	7、多人协作时工作模式
		1、首先可以试图用 git push origin <branch-name> 推送自己的修改
		2、推送失败，因为远程分支比本地的更新，需要先用 git pull 试图合并
		3、合并有冲突，解决冲突，本地提交
		4、没有冲突或解决冲突之后，git push origin <branch-name> 推送即可成功
		如果 git pull 提示 no tracking information，则说明本地分支和远程分支的链接关系没有创建，用命令 git branch --set-upstream-to <branch-name> origin/<branch-name>
	8、git rebase 让本地未push的分叉提交历史整理成直线
		使得我们在查看历史提交的变化时更容易
		缺点是本地的分叉提交已经被修改过了
		远程分支的提交历史也是一条直线

七、标签管理
	1、发布一个版本时，通常会先在版本库中打一个标签tag, 这样就唯一确定了打标签时刻的版本，将来无论什么时候，取得某个标签的版本，就是把那个打标签的时刻的历史版本取出来。标签是版本库的一个快照。
	2、标签是指向某个commit的指针，不能移动
	3、标签是容易记忆的一个有意义的名字，和commit绑在一起
	4、Usage
		git tag <tag-name>
			默认标签是打在最新提交的commit上的
		git tag -a v0.1 -m "标签说明内容" 1094abd
			-a 指定标签名 -m 指定说明内容
		git tag 显示所有标签
		git tag <tag-name> <commit-id>
			给指定commit-id打标签
		git show <tag-name>
		标签总是和某个commit挂钩，如果这个commit既在master分支出现，又在dev分支出现则在这两个分支上都可以看到这个标签
		git tag -d v0.1 删除某个标签
		git push origin <tag-name> 推送一个本地标签到远程
		git push origin --tags 推送所有尚未推送的本地标签
		git push origin :/refs/tags/v0.9 删除一个远程标签