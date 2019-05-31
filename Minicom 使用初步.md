# [Minicom 使用初步](https://www.cnblogs.com/pied/archive/2012/04/10/2441021.html)

因为现在电脑基本不配备串行接口，所以，usb转串口成为硬件调试时的必然选择。目前知道的，PL2303的驱动是有的，在dev下的名称是ttyUSB#。
minicom，tkterm都是linux下应用比较广泛的串口软件，这里简单介绍minicom使用。

**一，安装**
debian 系，比如 ubuntu、mint等：

sudo apt-get install minicom

**二，配置**

首先，查看串口设备是否可用。linux下的所有操作面向用户的都是文件操作，在对串口操作之前，我们应该先确认自己对该文件有没有读写权限。
ls -l /dev/ttyUSB*
linux下的usb串口命名为ttyUSB，运行上面命令，可以看到有几个设备挂载。
我们这里是：
crw-rw---- 1 root dialout 188, 0 Apr 10 17:10 /dev/ttyUSB0
只有ttuUSB0.
但是正如上面显示，ttyUSB0这个设备是root所有的，所以，我们以普通用户身份打开minicom是没法访问该文件的。

然后，运行$ sudo minicom -s ， 设置串口。
进入minicom的配置界面，使用上下键选择Serial port setup，回车。此时光标在“change which setting”后面停留，它的上面有如下菜单：

```
Serial port setup [Enter]
+-------------------------------------------------------------+
| A - Serial Device : /dev/ttyUSB0 |
| B - Lockfile Location : /var/lock |
| C - Callin Program : |
| D - Callout Program -: |
| E - Bps/Par/Bits : 115200 8N1 |
| F - Hardware Flow Control : No |
| G - Software Flow Control : No |
| |
| Change which setting? |
+-------------------------------------------------------------+
```

我们只需输入上面对应的字母，就可以进如相应的菜单进行设置。设置完成，回车，光标会回到“change which setting”后面，如此重复。完成按回车返回主菜单即可。
注意：如果不是 USB 转串口，而是直接使用串口，那么 Serial Device 要配置为 /dev/ttyS0（具体哪个设备，系统不同名字不同，如果你不知道，那就看你运气了）。

返回主菜单后，选择“Save setup as dfl”，将其保存为默认设置，然后选择 Exit 退出，注意，不是退出 minicom。

**三，使用**

如果上面设置顺利，重新给开发板上电后，此时，窗口里就有信息打印出来了。minicom基本操作如下：
1）组合键 Ctrl+a 进入设置状态；
2）按 z 打开帮助菜单，或者你用的熟练的话，直接输入菜单对应的字母即可；
（1）S键：发送文件到目标系统中；
（2）W键：自动卷屏。当显示的内容超过一行之後，自动将後面的内容换行。这个功能在查看内核的啓动信息时很有用。
（3）C键：清除屏幕的显示内容；
（4）B键：浏览minicom的历史显示；
（5）X键：退出mInicom，会提示确认退出。

3) 比如，使用 Ctrl-A-->O 进入 minicom 配置菜单：
+-----[configuration]------+
| Filenames and paths |
| File transfer protocols -|
| Serial port setup |
| Modem and dialing |
| Screen and keyboard |
| Save setup as dfl |
| Save setup as.. |
| Exit |
+--------------------------+
我们可以配置使用 Ctrl-A-S 发送文件时的默认路径，可选择"Filenames and paths"进行配置：
+-----------------------------------------------------------------------+
| A - Download directory : /home/crliu |
| B - Upload directory : /tmp |
| C - Script directory : |
| D - Script program : runscript |
| E - Kermit program : |
| F - Logging options |
| |
| Change which setting? |
+-----------------------------------------------------------------------+
如上，A - download 下载文件的存放位置（开发板 ---> PC, rz）开发板上的文件将被传输到PC机上的/home/crliu目录下。
B - upload 从此处读取上传的文件（PC ---> 开发板, sz）PC机向开发板发送文件，需要发送的文件在/tmp目录下（PC机上的目录）。
 
**四，其他技巧**
启动 minicom 的时候，会检测 MINICOM 环境变量，将它们作为命令参数，如果命令中指定了某个参数，那么这个 MINICOM 中的相应的某个参数会被替换。

我们在 ~/.bashrc 文件中添加这一行，使得 minicom 默认每次都进入彩色模式，并自动进入串口配置： 
export MINICOM='-s -c on'

The configuration 参数更有趣。通常，minicom从文件“minirc.dfl”中获取其缺省值。不过，若你给minicom一个参数，它将尝试从文件“minirc.configuration”中获取缺省值。因此，为不同端口、不同用户等创建多个配置文件是可能的。最好使用设备名，如：tty1， tty64，sio2等。如果用户创建了自己的配置文件，那么该文件将以“.minirc.dfl”为名出现在他的home目录中。 

minicom的全局配置文件通常在/etc/minirc.dfl. minicom将其配置文件保存在一个目录中，通常是：/var/lib/minicom, /usr/local/etc或者/etc。要想知道minicom编译时内定的缺省目录，可用命令“minicom -h”，在那儿你可能还会找到runscript(1) 的demo文件。 

配置文件的文件名格式：minirc.user。minicom默认加载配置文件minirc.df1，但你也可以仿照minirc.df1编辑自己的配置文件minirc.user，输入minicom user启动时则加载minirc.user。如在本机上，我做了一个minirc.root，输入minicom root 启动minicom，则系统自动加载/etc/minirc.root中的配置。 

$HOME/.minirc.* 

$HOME/.dialdir 

$HOME/minicom.log 

/usr/share/locale/*/LC_MESSAGES/minicom.mo 


***命令行启动minicom的参数** 

更多的参数，参见"man minicom"的输出。 
-s 
设置。 root 使用此选项在/etc/minirc.dfl中编辑系统范围的缺省值。使用此参数后，minicom 将 不 进行初始化， 而是直接进入配置菜单。如果因为你的系统被改变，或者第一次运行minicom时，minicom不能启动，这个参数就会很有用。对于多数系统，已经内定了比较合适的缺省值。 
-o 
不进行初始化。minicom 将跳过初始化代码。如果你未复位(reset)就退出了minicom，又想重启一次会话(session)，那么用这个选项就比较爽(不会再有错误提示：modem is locked ---- 注)。但是也有潜在的危险：由于未对lock文件等进行检查，因此一般用户可能会与uucp之类的东东发生冲突......也许以后这个参数会被去掉。现在姑且假定，使用modem的用户对自己的行为足够负责。 
-m 
用Meta或Alt键重载命令键。在1.80版中这是缺省值，也可以在minicom 菜单中配置这个选项。不过若你一直使用不同的终端，其中有些没有Meta或Alt键，那么方便的做法还是把缺省的命令键设置为Ctrl-A，当你有了支持 Meta或Alt键的键盘时再使用此选项。Minicom假定你的Meta键发送ESC前缀，而不是设置字符最高位的那一种(见下)。 
-M 
跟“-m”一样，但是假定你的Meta键设置字符高端的第八位(发送128+字符代码)。 
-z 
使用终端状态行。仅当终端支持，并且在其termcap或terminfo数据库入口中有相关信息时才可用。 
-l 
逐字翻译 高位被置位的字符。使用此标志， minicom将不再尝试将IBM行字符翻译为ASCII码，而是将其直接传送。许多PC-Unix克隆不经翻译也能正确显示它们(Linux使用专门的模式：Coherent和Sco)。 
-w 
在启动的时候就自动换行，这样启动之后不用运行<C-a> w进行设置了，详细参考后面。 
-a 
特性使用。 有些终端，特别是televideo终端， 有个很讨厌的特性处理(串行而非并行)。minicom缺省使用‘-a on’，但若你在用这样的终端，你就可以(必须!)加上选项‘-a off’。尾字‘on’或‘off’需要加上。 
-t 
终端类型。使用此标志， 你可以重载环境变量TERM， 这在环境变量MINICOM 中使用很方便； 你可以创建一个专门的 termcap 入口以备minicom 在控制台上使用，它将屏幕初始化为raw模式， 这样，连同‘-l’标志一起，就可以不经翻译而显示IBM行字符。 
-c 
颜色使用。些终端(如Linux控制台)支持标准ANSI转义序列色彩。由于termcap显然没有对于色彩的支持，因而minicom 硬性内置了这些转义序列的代码。所以此选项缺省为off。使用‘-c on’可以打开此项。把这个标志，还有‘-m’放入MINICOM环境变量中是个不错的选择。 
-S 
脚本。 启动时执行给定名字的脚本。到目前为止，还不支持将用户名和口令传送给启动脚本。如果你还使用了‘-d’选项，以在启动时开始拨号，此脚本将在拨号之前运行，拨号项目入口由‘-d’指明。 
-p 
要使用的伪终端。 它超载配置文件中定义的终端端口，但仅当其为伪tty设备。提供的文件名必须采用这样的形式：(/dev/)tty[p-z][0-f] 
-C 
文件名。 启动时打开捕获文件。 
-8 
不经修改地传送8位字符。“连续”意指未对地点/特性进行真正改变，就不插入地点/特性控制序列。此模式用于显示8位多字节字符，比如日本字(TMD！应该是中国字!!)。不是8位字符的语言都需要(例如显示芬兰文字就不需要这个)。