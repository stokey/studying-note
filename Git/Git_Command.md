##Git 命令远程操作
---
+ `git remote`:远程主机
+ `git remote -v`:查看远程主机网址
+ `git remote show`:查看远程主机的详细信息
+ `git clone -o testBranch xx-url`:修改远程主机名称（默认origin）

+ `git remote add <主机名，网址>`:手动添加远程主机
+ `git remote rm <主机名>`:删除远程主机
+ `git remote rename`:重命名远程主机
+ `git fetch <远程主机名>`:取回所有分支的更新
+ `git fetch <远程主机名> <分支名>`:取回特定主机的特定分支的更新
+ `git branch -r`:查看远程分支
+ `git branch -a`:查看所有分支
+ `git checkout -b <newBranchName> <主机名／远程分支>`:在远程分支基础上创建一个新的本地分支

###合并分支：
+ `git merge <远程分支>`
+ `git rebase <远程分支>`

	`一般采用先同步远程分支无冲突后再向远程分支发起合并`：

+ `git pull --rebase <远程主机名><远程分支>` 

	[ git pull/git fetch: *pull会主动合并，fetch不会* ]
+ `git pull <远程主机名><远程分支名>:<本地分支名>［如果远程分支是与当前分支合并，则冒号后面的部分可以省略］`


###Git建立追踪关系：
+ `git branch --set-upstream-to <本地分支> <远程主机名/远程分支名>`
+ `git pull -p / git fetch -p`
	
	*远程分支本删除后要求本地分支与远程分支保持一致（即同时删除被删除的分支）*

+ `git push <远程主机名><远程分支名>:<本地分支名>`:将本地分支的更新推送到远程主机，如果远程分支不存在则会新建同名远程分支

+ *如果当前分支与多个主机存在追踪关系，则可以使用 -u 选项指定一个默认主机，这样后面的就可以不加任何参数使用`git push:git push -u origin master`[该命令将本地的master分支推送到origin主机，同时制定origin为默认主机，后面的git push命令可以不加任何参数使用]*
+ `git push --all <远程主机名>`:将本地的所有分支都推送到远程分支
+ `git push --force <远程主机名>`:强制推送，可能会造成某些内容被覆盖。谨慎使用
+ `git push <远程主机名> --tags:`向远程主机推送更新，并打上标签

###删除分支
+ 删除远程分支：`git push origin :branch-name`//冒号前空格不能省略，原理是把一个空分支push到server上
+ 删除本地分支：`git branch -D branch-name`

##Git 暂存区
---
+ `git add file`：添加文件到暂存区
+ `git reset HEAD file`：取消文件暂存
+ `git checkout -- file`：撤消文件修改（*操作的文件修改小时并且不能恢复*）
+ `HEAD`文件指向master，master执行当前分支

