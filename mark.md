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





使用一个新的仓库的时候，需要先对本地的文件夹进行仓库的初始化，之后创建云上的仓库，之后再在本地的仓库上 git remote add 仓库名 git@github.com:Heisenberg1997/xx.git进行本地和远程仓库的关联，之后再在本地仓库进行git pull 一下 在上传即可

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

ik的结算只有四个 其他的都是对解算结果进行优化或者说递增.

***

常见命令:

grep 查找命令,

grep ** folder -r -n 进行递归查找

Unicode把65536（2^16）作为一个范围的大小，将111 4112分成了17份，这每一份又有一个概念名词，叫平面。

Unicode 是一种将全部的文字格式进行编码的格式   0xFFFF 一共 17x2^16 种  这中编码一般是对字符集的 就是将字符集对应到某个16进制的数字 而对码位的为 对应到某个二进制串

一个文本文件，里面全部都是用英语来表达书写的（写代码的文件不一般都这样嘛），那么这个文件里所有字符的码位都是在0-127之间，Unicode兼容一般的Ascii编码，也就是第一个字节。大写字母A的码位是U+0061。十进制表示的码位为127的那个字符的十六进制码位为U+00FF。

**Unicode是将字符编码到码位，而UTF-8、UTF-16或UTF-32等是将码位编码到二进制串。**

汉字字符“字”，码位是U+5B57。如果采用UTF-32编码方式，那么二进制串是“00000000 00000000 01011011 01010111”，采用了4个字节，32位二进制来表示了，所以是UTF-32。

如果采用现在最通用的编码方式UTF-8，那么“字”的二进制串是“11100101 10101101 10010111”。只用了三个字节，24位二进制就表示了。

UT-8 的编码规则:

1）对于单字节的码位，字节的第一位设为0，后面7位为这个符号的Unicode码位。因此对于英语字母，UTF-8编码和ASCII码是相同的。

 

2）对于n字节的符号（n>1），第一个字节的前n位都设为1，第n+1位设为0，后面字节的前两位一律设为10。剩下的没有提及的二进制位，全部为这个字符的Unicode码位的二进制。

也就是将一个字符编码成码位的, 可以进行分页 就是每一页一部分的



HTTP协议无状态的 而且是不安全的 因此可以在cookie中设置其安全属性 

更好的方法是对cookie进行数据加密





session 是一种记录客户状态的机制,cookie保存在客户端处,一般需要提交,而session保存在服务器上,是对客户提交的信息进行筛选

 哈希计算是在进行数据传输的时候能够对数据进行检验和校正 对任意一段字符串,可以进行哈希计算,得到一个固定长度的字符, 无法通过函数返回过去 也就是单向的计算,

比如一段数据 计算hash值之后一起发送出去,接受到之后再进行hash比对,判断数据是否经过了修改 如果对hash一起进行攻击,则可以对其签名(内容+hash)进行

数字证书包括了多种算法的集合  其中的指纹是hash算法 保证内容不被修改 而hash又被私钥进行签名算法进行加密,只能用公钥解开 得到hash值

***

巴赫抗躁动、海顿抗抑郁、莫扎特抗失眠、贝多芬抗萎靡、柴科夫斯基抗饥饿、马勒抗瞌睡、拉赫玛尼诺夫抗寂寞...最后还必须得指出一个最管用的:布鲁克纳，抗吃醋后的不良情绪反应。