# Git使用

---

### 新建项目

1. 浏览器进入gitlab

		http://gitlab.yintech.net/
		
2. 登录或注册

3. 点击New project

	![usage_1](images/usage_1.jpg)
	
4. 填写项目名称，项目描述和选择可视等级,然后点击Create Project

	![usage_1](images/usage_2.jpg)
	
5. 复制项目地址，在git bash上执行
	
		git clone 项目地址

	![usage_1](images/usage_3.png)
	
	\*注：在新建项目下方有一些常用的命令，可供参考
	
6. 至此，在执行命令的目录下会产生一个项目目录

### 基本操作

#### git init——初始化仓库

	要使用Git进行版本管理，必须先出实话仓库。Git是使用git init命名进行初始化的。如果出实话成功，执行了git init的命令目录下就会生成.git目录。这个目录里存储着管理当前内容所需的仓库数据。
		
#### git status——查看仓库的状态

	工作树和仓库在被操作的过程中，状态会不断发生变化。git status命令用于显示Git仓库的状态。
	
#### git add——向缓冲区中添加文件

	要想让文件成为Git仓库的管理对象，就需要用git add命令将其加入暂存区（Stage或者Index）中。暂存区市提交之前的一个临时区域。
	
#### git commit——保存仓库的历史记录

	git commit命令可以将当前暂存区中的文件保存到仓库的历史记录中。通过这些记录，我们就可以在工作树中复原文件。
	
#### git log——查看提交日志

	git log命令可以查看以往仓库中提交的日志。如果想查看提交所带来的改动，可以加上-p参数，文件的前后差别就会显示在提交信息之后。
	
#### git diff——查看更改前后的差别

	git diff命令可以查看工作树、暂存区、最新提交之间的差别。
	
#### git branch——显示分支一览表

	git branch命令可以将分支名列表显示，同时可以确认当前所在分支。
	
#### git checkout -b——创建、切换分支

	如果想以当前的master分支为基础创建新的分支，我们需要用到git checkout -b命令。
	git checkout master命令可以切换到主分支，git checkout -可以切换回上一分支。
	
#### git merge——合并分支

	首先切换到master分支，为了在历史记录中明确记录下本次分支合并，我们需要创建合并提交。因此，在合并时加上--no-ff参数。
	git merge --no-ff 需合并的分支名
	
#### git reset——回溯历史版本

	要让仓库的HEAD、暂存区、当前工作树回溯到指定状态，需要用到git reset --hard命令。
	
#### git push——推送至远程仓库
	
	如果想将当前分支下本地仓库中的内容推送至远程仓库，需要用到git push命令。
	
#### git clone——获取远程仓库

	执行git clone命令后我们会默认处于master分支下，同时系统会自动将origin设置成该远程仓库的标识符。
	
#### git pull——获取最新的远程仓库分支

	使用git pull命令，将本地的分支更新到最新状态。
	如果两人同时修改同一部分的源代码，push时就很容易发生冲突。所以多名开发者在同一分支进行作业时，为减少冲突情况的发生，建议更频繁地进行push和pull操作。

	