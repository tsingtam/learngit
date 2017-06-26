msysgit是Windows版的Git，从https://git-for-windows.github.io下载，然后按默认选项安装即可。
	git config --global user.name "Your Name"
	git config --global user.email "email@example.com"
	git config命令的--global参数，用了这个参数，表示你这台机器上所有的Git仓库都会使用这个配置，当然也可以对某个仓库指定不同的用户名和Email地址。
	
初始化一个Git仓库，使用git init命令。
	首先创建一个目录： 	mkdir learnGit 
						cd learnGit 
						pwd 
	其次使用git init初始化learGit
						git init 
	
添加文件到Git仓库，分两步：
	第一步，使用命令git add <file>，注意，可反复多次使用，添加多个文件；
	第二步，使用命令git commit，完成。
						git add -fileName // 添加文件；
						git commit -m "注释说明"   // 提交添加的文件
						
要随时掌握工作区的状态，使用git status命令。
						git status //查看文件
如果git status告诉你有文件被修改过，用git diff可以查看修改内容。
						git diff fileName   // 查看文件修改的内容
											
穿梭前，用git log可以查看提交历史，以便确定要回退到哪个版本。
						git log  //查看git提交过的日志
				commit 4c663e1bcc00012d4332a6018e37cbda3cb70697（commit_id） (HEAD -> master, origin/master)		
HEAD指向的版本就是当前版本，因此，Git允许我们在版本的历史之间穿梭，使用命令git reset --hard commit_id。
		在Git中，用HEAD表示当前版本，也就是最新的提交ID为4c663e1bcc00012d4332a6018e37cbda3cb70697的版本（注意我的提交ID和你的肯定不一样），
		上一个版本就是HEAD^，上上一个版本就是HEAD^^，当然往上100个版本写100个^比较容易数不过来，所以写成HEAD~100
						git reset --hard HEAD^
						cat fileName // 查看回退版本的文件内容
要重返未来，用git reflog查看命令历史，以便确定要回到未来的哪个版本。
						git reflog // 查看git提交过的日志的ID的前几位 方便使用 git reset --hard commit_id。
					ea34578 HEAD@{0}: reset: moving to HEAD^ 
					3628164 HEAD@{1}: commit: append GPL
					ea34578 HEAD@{2}: commit: add distributed
					cb926e7 HEAD@{3}: commit (initial): wrote a readme file
					
工作区和暂存区
	工作区就是自己创建的文件目录（learnGit）
	工作区中的隐藏的文件目录.git 为版本库 
	Git的版本库里存了很多东西，其中最重要的就是称为stage（或者叫index）的暂存区，还有Git为我们自动创建的第一个分支master，以及指向master的一个指针叫HEAD。
	
	简单的可以理解为需要提交的文件修改通通放到暂存区，然后，一次性提交暂存区的所有修改。
		比如 git add A.js 
			 git add B.js 
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	