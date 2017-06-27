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
		比如 首先在工作区目录新建两个文件 执行下面两个命令
			git add A.js 
			git add B.js 
		执行命令后 暂存区会增加 A B两个文件  使用git commit -m “add A B”  命令后 暂存区会清空 一次性提交暂存区的文件到分支master

第一次修改 -> git add -> 第二次修改 -> git commit
Git管理的是修改，当你用git add命令后，在工作区的第一次修改被放入暂存区，
	准备提交，但是，在工作区的第二次修改并没有放入暂存区，所以，
	git commit只负责把暂存区的修改提交了，也就是第一次的修改被提交了，第二次的修改不会被提交。
提交后，用git diff HEAD -- readme.txt命令可以查看工作区和版本库里面最新版本的区别		
							git diff HEAD -- fileName 

当你改乱了工作区某个文件的内容，想直接丢弃工作区的修改时，用命令git checkout -- fileName。
											git checkout -- fileName // 把文件在工作区的修改全部撤销
当你不但改乱了工作区某个文件的内容，还添加到了暂存区时，想丢弃修改，分两步，第一步用命令git reset HEAD fileName，第二步用命令 git checkout -- fileName.
											git reset HEAD fileName
											git checkout -- fileName
已经提交了不合适的修改到版本库时，想要撤销本次提交，可以使用 git reset --hard HEAD^ 或者 git reset --hard commit_id .不过前提是没有推送到远程库。
											git reset --hard commit_id

远程仓库
	只要注册一个GitHub账号，就可以免费获得Git远程仓库。
		第1步：	创建SSH Key。在用户主目录下，看看有没有.ssh目录，如果有，再看看这个目录下有没有id_rsa和id_rsa.pub这两个文件，
				如果已经有了，可直接跳到下一步。如果没有，打开Shell（Windows下打开Git Bash），创建SSH Key：ssh-keygen -t rsa -C "youremail@example.com"
			可以在用户主目录里找到.ssh目录，里面有id_rsa和id_rsa.pub两个文件，这两个就是SSH Key的秘钥对，id_rsa是私钥，不能泄露出去，id_rsa.pub是公钥，可以告诉别人
		第2步：登陆GitHub，打开“Account settings”，“SSH Keys”页面：然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容：
		第3步：创建一个新的仓库（learnGit），使用 git remote add origin git@github.com:xxxx/learnGit.git 命令把本地和远程仓库管理在一起 
											git remote add origin git@github.com:tanxin/learnGit.git
		第4步： 本地和远程仓库关联后，使用 git push -u origin master 命令把本地内容推送到远程仓库中
											git push -u origin master //第一次推送
											git push origin master // 第二次以后的推送
			由于远程库是空的，我们第一次推送master分支时，加上了-u参数，Git不但会把本地的master分支内容推送的远程新的master分支，
				还会把本地的master分支和远程的master分支关联起来，在以后的推送或者拉取时就可以简化命令。
		第5步： 在github上新创建一个项目，创建项目时记住勾选Initialize this repository with a README，这样GitHub会自动为我们创建一个README.md文件。
			远程库准备好后，使用命令git clone克隆一个本地库
											git clone git@github.com:tanxin/others.git // 把新建的仓库克隆到本地
			Git支持多种协议，包括https，但通过ssh支持的原生git协议速度最快。
			
											
		
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	
	