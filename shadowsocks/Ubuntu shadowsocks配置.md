#  Ubuntu shadowsocks配置

## 1 启动SS客户端

### 1.1 方式1

前两步很简单，可是有人就纳闷了安装好了不知道怎么用，其实可以用sslocal -help来查看帮助就知道了。

```
sslocal -s server_ip -p server_port  -l 1080 -k password -t 600 -m aes-256-cfb
例如：
sslocal -s jpcn2.thatseed.org -p 43000  -l 1080 -k password -t 600 -m aes-128-ctr
```

-s表示服务IP, -p指的是服务端的端口，-l是本地端口默认是1080, -k 是密码（要加""）, -t超时默认300,-m是加密方法默认aes-256-cfb，

可以简单的写为：sslocal -s ip  -p  port -k "password"    #用-s -p -k这三个参数就好，其他的默认将服务端的加密方法设为aes-256-cfb。然后就可以启动代理。

现在试试打开你的终端，输入sslocal -s 服务端IP  -p  1080 -k "密码" 回车

参考链接：<https://www.zybuluo.com/Frankchen/note/307683>

### 1.2 方式2

vim /etc/ssclient.json

```
{
    "server":"your_server_ip",      #ss服务器IP
    "server_port":your_server_port, #端口
    "local_address": "127.0.0.1",   #本地ip
    "local_port":1080,              #本地端口
    "password":"your_server_passwd",#连接ss密码
    "timeout":300,                  #等待超时
    "method":"rc4-md5",             #加密方式
    "fast_open": false,             # true 或 false。如果你的服务器 Linux 内核在3.7+，可以开启 fast_open 以降低延迟。开启方法： echo 3 > /proc/sys/net/ipv4/tcp_fastopen 开启之后，将 fast_open 的配置设置为 true 即可
    "workers": 1                    # 工作线程数
}

例如：
{
    "server":"jpcn.baidu.com",
    "server_port":43000,
    "local_address": "127.0.0.1",
    "local_port":1080,
    "password":"123456789",
    "timeout":300,
    "method":"aes-128-ctr",
    "fast_open": false,
    "workers": 1
}
```

启动：

```
nohup sslocal -c /etc/ssclient.json /dev/null 2>&1 &
```

命令解释：

nohup命令：如果你正在运行一个进程，而且你觉得在退出帐户时该进程还不会结束，那么可以使用nohup命令。该命令可以在你退出帐户/关闭终端之后继续运行相应的进程。nohup就是不挂起的意思( n ohang up)

在Linux中：

0:表示键盘输入(stdin)

1:表示标准输出(stdout),系统默认是1

2:表示错误输出(stderr)

shell命令：command >/dev/null  2>&1  &  等同于   command 1>/dev/null 2>&1  &

1)command:表示shell命令或一个可执行的程序

2)>:表示重定向到

3)/dev/null:表示Linux的空设备文件

4)2:表示标准错误输出

5)&1:&表示等同于的意思,2>&1,表示2的输出重定向等同于1的重定向

6)&:表示后台执行这条指令

1>/dev/null:表示标准输出重定向到空设备文件,即不输出任何信息到终端。

2>&1:表示错误输出重定向等同于标准输出,因为之前标准输出已经重定向到了空设备文件,所以错误输出也重定向到空设备文件。

上述例子中的shell命令的意思就是在后台执行这个程序,并将错误输出2重定向到标准输出1,然后将标准输出1全部放到/dev/null文件,也就是清空.（shell命令：command >/dev/null  2>&1  &  等同于   command 1>/dev/null 2>&1  &）

" >/dev/null 2>&1 "常用来避免shell命令或者程序等运行中有内容输出。

### 1.3 增加开机启动选项

```
echo " nohup sslocal -c /etc/shadowsocks.json /dev/null 2>&1 &" /etc/rc.local
```

执行"ps aux |grep sslocal |grep -v "grep""查看后台sslocal是否运行。