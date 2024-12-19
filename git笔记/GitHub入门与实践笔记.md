```
-----------------------------------------------------------------
# 基础设置
# "[]"表示可选、"<>"表示必选
1、设置user.name和user.email
	1)git config --global user.name <"your name">
		gitconfig --global user.emai <"your email">
	2)进入~/.gitconfig直接修改
		[user]
            name = lqh
            email = 2931451523@qq.com
2、设置github的ssh key
	将本地git用户关联到github上用户
	1)生成ssh key的公钥和密钥
		 ssh-keygen -t rsa -C "2931451523@qq.com"
	2)将公钥放入github账户上，进行绑定
	3)验证是否绑定成功
		ssh -T git@github.com
------------------------------------------------------------------
# github上创建仓库,默认仓库地址：https://github.com/用户名/仓库名
# git clone [-b/--branch] [branch_name] git@github.com:用户名/该用户下的仓库名.git
	-b:可以指定克隆远程某一分支，若无默认只会克隆main/master分支
	也可本地文件运行git init初始化仓库
# git add
# git commit -m ["说明"]
	无说明打开默认编辑器
# git status 
	查看哪些没有加入暂存区，那些没有commit
# git log [-p] [filename] [--graphy/--oneline]
	查看什么时候git clone\什么时候git commit
	-p:会显示差异: 是每次commit提交的文件，与上一次的差异
	filename:查看指定文件的log
	只会看当前分支或已经被合并分支的信息
	一般用git log查看分支在哪个提交上，指针在哪个提交或者分支上
# git push [-u/--set-upstream][remote] [branch]
	-u:在推送的同时，将远程仓库origin设置为本地仓库的上游
		--set-upstream效果类似
	默认remote为origin，branch为当前分支
-------------------------------------------------------------------
# git diff的使用
1、git diff [HEAD]
	HEAD:查看工作树与本地仓库的差异，若为空表示工作树与暂存区的差异
-------------------------------------------------------------------
# 分支操作
# git checkout [-b] <分支/commit_hash/HEAD^> <origin/feature-D>
	-b：创建并切换到分支
	分支：当-b为空时，分支可用-表示切换到上一次分支
	commit_hash:-b为空，commit_hash不为空表示移动HEAD指针与git reset --hard
	origin/feature-D: 在本地创建分支，以远程仓库的feature-D分支为源
# git branch [分支] [-a]
	分支：创建分支，若为空表示显示当前分支情况
	-a:若是查看分支情况，表示查看远程和本地仓库的所有分支
1、创建分支、合并分支、删除分支
	创建分支：
		git checkout -b feature-A或者
		git branch feature-A && git checkout feature-A
	push新分支：
		第一次使用需用git push --set-upstream origin feature-A命令
	合并分支：
		若当前分支(main)的HEAD指针指向要合并分支(feature-A)的祖先，git会默认执行快合并(fast-forward merge)，分支会简单地移动到要合并的分支的最新提交，而不会创建新的合并提交，使用--no-ff参数表示创建新的合并提交
		git merge --no-ff feature-A	
	删除分支：
		git branch -d <branch_name>
2、更改提交操作
	1)回溯之前版本并新建分支：修改README.md并提交
		git checkout HEAD^或者git reset --hard HEAD^
		git checkout -b Feature-B
		git add .&& git commit .
	2)回到主分支的最新提交节点
		git checkout main或者git checkout main && git reset --hard <最新一次的提交>
	3)合并当前分支与之前新建的分支，解决冲突,commit提交
		git merge --no-ff Feature-B
		vi README.md解决冲突
		git add . && git commit .
	4)修改之前提交信息
		git commit --amend
			会修改当前HEAD指向的提交信息
			会导致远程仓库和本地仓库不一致，需要强制推送
3、rebase的使用将最新提交说明压缩到历史记录说明中
	1)回到最新节点，新建分支feature-C，修改README.md提交
		略
	2)继续修改README.md，提交
		使用 git commit -am "fix feature-c"
	3)第二次提交是独立的，需要将第二次提交压缩到第一次中
		git rebase -i HEAD~2
			-i:默认打开编辑器操作
	4)合并main分支
-------------------------------------------------------------------
# 多人协同远程仓库基础
# 新建目录1将本地仓库推送到新的远程仓库
# 新建目录2，从远程仓库克隆，修改信息，push提交
# 回到原目录1，进行pull操作更新信息

1、git remote [add] [set-url] <origin> <url> [-v]
	add:本地仓库与远程仓库绑定
	set-url:本地仓库和远程仓库换绑
	-v:查看当前绑定的远程仓库
2、git checkout [-b] [branch_name] [origin/branchh_name]
	从远程仓库获得分支
3、git pull origin branch_name
	更新本地分支branch_name，远程仓库的信息会直接覆盖当前分支上的信息
```





# 1.git checkout 与git reset的区别

```
# HEAD和分支的区别
	HEAD指向分支，分支指向提交

1、git checkout只能移动HEAD指针，不能移动分支
	分离的HEAD，HEAD指针不依赖于分支，在HEAD节点修改代码不会影响分支,一般为新建分支
2、git reset --hard会移动HEAD指针该指针指向的分支
	将HEAD和分支移动到某一提交，原分支的代码会被清除，替换为移动位置的分支代码，相当于为移动位置分支又起了个别名，由于原分支会被清除，所以该操作一般会在分支内操作，如果需要回到最新分支，使用git reflog查看最新分支的hash值，通过git reset --hard <hash>回到最新分支
```

![9aafe0c178d626ccd467b9524540ca98](C:\Users\13665\Desktop\黄金屋和颜如玉\imgs\9aafe0c178d626ccd467b9524540ca98.png)

```
3、git reset的用途
	1)需要在本分支之前的某个提交上新建一个分支
		用git checkout HEAD^再新建分支一样的
	2)同步本地仓库和远程仓库,如下图
		git reset --hard origin/master
```

![e743ae67c4e1a2f54c62ad6f4bb50aec](C:\Users\13665\Desktop\黄金屋和颜如玉\imgs\e743ae67c4e1a2f54c62ad6f4bb50aec.png)

![44848c87f415cf47e274bfa7dd9b4342](C:\Users\13665\Desktop\黄金屋和颜如玉\imgs\44848c87f415cf47e274bfa7dd9b4342.png)





