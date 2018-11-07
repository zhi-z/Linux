## Linux小抄

Linux中普通用户用sudo执行命令时报”xxx is not in the sudoers file.This incident will be reported”错误，解决方法就是在/etc/sudoers文件里给该用户添加权限。如下：

1.切换到root用户下 
　　方法为直接在命令行输入：su，然后输入密码（即你的登录密码，且密码默认不可见）。

2./etc/sudoers文件默认是只读的，对root来说也是，因此需先添加sudoers文件的写权限,命令是: 
即执行操作：chmod u+w /etc/sudoers

3.编辑sudoers文件 
即执行：vi /etc/sudoers 
找到这行 root ALL=(ALL) ALL,在他下面添加xxx ALL=(ALL) ALL (这里的xxx是你的用户名)

ps:这里说下你可以sudoers添加下面四行中任意一条 
youuser ALL=(ALL) ALL 
%youuser ALL=(ALL) ALL 
youuser ALL=(ALL) NOPASSWD: ALL 
%youuser ALL=(ALL) NOPASSWD: ALL

第一行:允许用户youuser执行sudo命令(需要输入密码). 
第二行:允许用户组youuser里面的用户执行sudo命令(需要输入密码). 
第三行:允许用户youuser执行sudo命令,并且在执行的时候不输入密码. 
第四行:允许用户组youuser里面的用户执行sudo命令,并且在执行的时候不输入密码.

4.撤销sudoers文件写权限,命令: 
chmod u-w /etc/sudoers



Ubuntu ssh安装

1.准备工作： 
确认Ubuntu中安装并启动了ssh，方法如下：

sudo apt-get install openssh-server
1
如果没安装过ssh，那么就会安装，如果提示已经安装过了，那就接着执行：

ps -e |grep ssh
1
如果只有ssh-agent那ssh-server还没有启动，需要/etc/init.d/ssh start，如果看到sshd那说明ssh-server已经启动了。 
2.连接 
vitualbox中设置->网络->网络，设置连接方式为桥接 
ifconfig获得inet地址 
用putty连接该地址的22端口即可
--------------------- 
作者：somehow1002 
来源：CSDN 
原文：https://blog.csdn.net/somehow1002/article/details/78511783 
版权声明：本文为博主原创文章，转载请附上博文链接！