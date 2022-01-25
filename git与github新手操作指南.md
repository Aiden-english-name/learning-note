一、本地库操作

1.创建本地目录，用于存储要上传的文本文件。可以手动创建也可以用带命令行

mkdir <文件名>



2.进入文件夹cd <文件名> 



3第一次创建时需要初始化仓库git init

mac显示隐藏文件SHIFT+COMMAND+.

mac路径和主目录可以通过设置和调出来。



4.设置身份

设置项目级别的身份

git config user.name 英文名

git config user.email 邮箱

信息保存位置 ./.git/config文件



设置系统用户级别的身份

git config --global  user.name 英文名

git config --global  user.email 邮箱

身份可以在主目录里.gitconfig文件夹里看（mac）



5.如果有文件可以提交

加入暂存区git add <文件>

如果不需要git rm --cached <文件>将文件从暂存区中撤销

加入本地库git commit <文件>,打开vim编辑器输入commit 信息（第一行）,（mac）按Esc,输入:wq退出。

或者git commit -m "输入commit" <文件>信息



6 实现版本的穿梭

基于索引值的操作

（1）git reset --hard 索引值

（2）使用^符号：只能往后

git reset --hard HEAD^   一个^表示后退一个版本，两个表示后退两个版本

（3）使用～符合：只能后退

git reset --hard HEAD~3 表示后退三步



补充代码：

git log 打印历史记录

git log --pretty=oneline打印历史记录只显示一行

git log --oneline打印历史消息，哈希值只显示一部分

git reflog查看历史记录，并且看到回到之前版本的步数



reset命令的三个参数对比(了解)

​	soft//作用用本地库移动指针，突出暂存区

​	mixed//在本地库移动指针，重置暂存区，突出工作区

​	hard//在本地库移动指针，重置暂存区，移动工作区

​	git reset --hard HEAD对暂存区工作区都进行重置



7.删除本地库文件

rm <文件名>删除文件

删除也是一种状态可以保存。

删除状态保存到了暂存区git reset --hard HEAD取回

删除状态保存到了本地库git reset --hard <哈希值>回退



8比较文件

git diff <文件名>将工作区的文件和暂存区进行比较

git diff <本地库中某一个历史版本> <文件名>将工作区的文件和本地库的历史记录进行比较

git diff <本地库中某一个历史版本>不带文件名进行比较，和当前工作区中的所有记录比较



9git分支操作

git status//查看当前所在分支

git branch -v//查看所有分支

git branch 分支名//创建新的分支

git checkout 分支名//进行分支的切换



10合并分支

切换到接受修改的分支上（被合并）

git merge <被合并的分支名>



11解决冲突

当不同分支修改同一行然后合并到一起就会产生冲突，冲突后文件里面就会有变化如：

分支冲突的表现：

<<<<<<<<<< HEAD

当前分支内容

==========

另一分支内容

.>>>>>>>>> master

人为决定手动修改

git add <文件名>

git commit -m " 日志" 

注：此时commit不能带文件名

#### 

二、远程库操作

(1)推送

1.注册github账号



2.new repositoriy创建仓库



3.在本地库创建远程库别名

git remote -v查看本地库创建远程库地址的别名

git remote add 别名 远程库地址 //本地库创建远程库别名



4推送到远程库git push 别名 分支



5如果第一次操作需要输入账号和密钥

Settings->developer settings->personal access token->generate new token->输入令牌名字，保质期，授予权限（从命令行访问存储库，请选择repo）->generate token(密钥只存在一次)



(2)克隆
1
2
3
4
5



11

补充代码:

ls -la 查看当前目录

cat <文件名> 查看文件内容

pwd 查看当前路径

cd ~返回主目录

git status查看当前状态 ，查看工作区，暂存区状态

vim <文本文件> 创建文本文件，进入vim编辑模式,（mac）按Esc,输入:wq退出。

:set nu在vim编辑器中显示行号

tail -n 3 <文件名> 显示文件的末三行



git diff  <文件名>将工作区的文件和暂存区进行比较

git diff <本地库中某一个历史版本><文件名>
