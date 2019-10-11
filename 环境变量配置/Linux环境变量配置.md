# Linux环境变量配置

查看PATH：echo $PATH
以添加mongodb server为列
修改方法一：
export PATH=/usr/local/mongodb/bin:$PATH
//配置完后可以通过echo $PATH查看配置结果。
生效方法：立即生效
有效期限：临时改变，只能在当前的终端窗口中有效，当前窗口关闭后就会恢复原有的path配置
用户局限：仅对当前用户

 

修改方法二：
通过修改.bashrc文件:
vim ~/.bashrc 
//在最后一行添上：
export PATH=/usr/local/mongodb/bin:$PATH
生效方法：（有以下两种）
1、关闭当前终端窗口，重新打开一个新终端窗口就能生效
2、输入“source ~/.bashrc”命令，立即生效
有效期限：永久有效
用户局限：仅对当前用户

 

修改方法三:
通过修改profile文件:
vim /etc/profile
/export PATH //找到设置PATH的行，添加
export PATH=/usr/local/mongodb/bin:$PATH
生效方法：系统重启
有效期限：永久有效
用户局限：对所有用户

 

修改方法四:
通过修改environment文件:
vim /etc/environment
在PATH="/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/usr/games:/usr/local/games"中加入“:/usr/local/mongodb/bin”
生效方法：系统重启
有效期限：永久有效
用户局限：对所有用户