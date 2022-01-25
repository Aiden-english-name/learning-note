1.创建本地目录，用于存储要上传的文本文件。可以手动创建也可以用带命令行

mkdir <文件名>



2.进入文件夹cd <文件名> 



3第一次创建时需要初始化仓库git init

mac显示隐藏文件SHIFT+COMMAND+.

mac路径和主目录可以通过设置和调出来。



4.设置身份，身份可以在主目录里.gitconfig文件夹里看（mac）

设置项目级别的身份

git config user.name 英文名

git config user.email 邮箱

信息保存位置 ./.git/config文件



设置系统用户级别的身份

git config --global  user.name 英文名

git config --global  user.email 邮箱



5.如果有文件可以提交

加入暂存区git add <文件>

如果不需要git rm --cached <文件>将文件从暂存区中撤销

加入本地库git commit <文件>,打开vim编辑器输入commit 信息,（mac）按Esc,输入:wq退出。

或者git commit -m "输入commit" <文件>信息



6 实现版本的穿梭（新手可略）

基于索引值的操作

（1）git reset --hard 索引值

（2）使用^符号：只能往后

git reset --hard HEAD^   一个^表示后退一个版本，两个表示后退两个版本

（3）使用～符合：只能后退

git reset --hard HEAD~3 表示后退三步







补充代码:

ls -la 查看当前目录

cat <文件名> 查看文件内容

pwd 查看当前路径

cd ~返回主目录

git status查看当前状态 ，查看工作区，暂存区状态

vim <文本文件> 创建文本文件，进入vim编辑模式,（mac）按Esc,输入:wq退出。

:set nu在vim编辑器中显示行号

git log 打印历史记录

git log --pretty=oneline打印历史记录只显示一行

git log --oneline打印历史消息，哈希值只显示一部分

git reflog查看历史记录，并且看到回到之前版本的步数

tail -n 3 <文件名> 显示文件的末三行

