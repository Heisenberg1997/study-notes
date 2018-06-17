#### git 的使用步骤以及对其的理解

##### 基本命令以及解释：

使用git 先gitadd 在进行提交 git commit -m“” 标注设定
git add 是先将文件添加到暂存区 之后使用commit 将文件从暂存区添加到当前分支
git status 可以显示当前仓库里面的文件的修改的状态

当在使进行一个未完成的新的程序的时候，可以使用分支进行管理

查看分支 git branch

创建分支 git branch <name>
切换分支 git checkout <name>
创建+切换分支 git checkout —b<name>
合并分支 用于写完程序 git merge<name>
删除分支 git branch -d <name>

##### HTTPS的方式需要输入一些账号和密码，但是明文的输入并不ok 因此采用ssh进行数据的传输







****



可以使用此来进行例行性工作排程

crontable是可以用来周期性执行命令的 其运行一般在后台运行	

时间格式**f1 f2 f3 f4 f5 program** 

分钟、小时、一个月中的第几日、月份、一个星期中的第几天、需要执行的程序

如果是星号* 表示没分钟或者小时都要执行 而如果是一个数字 则表示执行的而时间 */2表示没两分钟执行一次

位置在**/etc/crontable**

**如果这个能够显示表示程序已经能够自动备份代码到github**

sudo /etc/init.d/cron restart 重启cron

stop start等多个

如果这能够显示成果则表示我的脚本已经能够在每天晚上19点的时候的时候自动备份自己的代码

asdfsdfdsffffff

****

DNS服务器用于将域名与对应ip进行匹配的服务









****

#### 机械臂的规划器

普通的是KDL 进行规划 在进行setup的时候可以使用进行，但是有奇点和无法求解的位置，可更换成TRAC-IK的规划 修改yaml文件进行配置