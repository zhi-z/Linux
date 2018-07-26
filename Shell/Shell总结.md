# shell总结

## 1 简介

Shell 是一个用 C 语言编写的程序，它是用户使用 Linux 的桥梁。Shell 既是一种命令语言，又是一种程序设计语言。

Shell 是指一种应用程序，这个应用程序提供了一个界面，用户通过这个界面访问操作系统内核的服务。

Ken Thompson 的 sh 是第一种 Unix Shell，Windows Explorer 是一个典型的图形界面 Shell。

## 2 第一个shell脚本

### 2.1 编写脚本

打开文本编辑器(可以使用 vi/vim 命令来创建文件)，新建一个文件 test.sh，扩展名为 sh（sh代表shell），扩展名并不影响脚本执行，见名知意就好，如果你用 php 写 shell 脚本，扩展名就用 php 好了。 首先，来一个hello world:

```
#!/bin/bash
echo "Hello World"
```

\#! 是一个约定的标记，它告诉系统这个脚本需要什么解释器来执行，即使用哪一种 Shell。

echo 命令用于向窗口输出文本。 

### 2.2 运行脚本

#### 2.2.1 可执行程序运行

到所在文件目录下，输入如下命令：

```
chmod +x ./test.sh  #使脚本具有执行权限
./test.sh  #执行脚本
```

一定要写成 ./test.sh，而不是 test.sh，运行其它二进制的程序也一样，直接写 test.sh，linux 系统会去 PATH 里寻找有没有叫 test.sh 的，而只有 /bin, /sbin, /usr/bin，/usr/sbin 等在 PATH 里，你的当前目录通常不在 PATH 里，所以写成 test.sh 是会找不到命令的，要用 ./test.sh 告诉系统说，就在当前目录找。 

#### 2.2.2 解释器参数

这种运行方式是，直接运行解释器，其参数就是 shell 脚本的文件名，如： 

```
/bin/sh test.sh
```

完成第一个脚本的实现。

## 3 Shell 变量

### 3.1 变量赋值

定义一个字符串变量，如：

```
myShell="hello world"
```

注意，变量名和等号之间不能有空格.

### 3.2 变量使用

#### 3.2.1 变量使用

使用一个定义过的变量，只要在变量名前面加美元符号即可，如：

```
myShell="qinjx"
echo $myShell
echo ${myShell}
```

变量名外面的花括号是可选的，加不加都行，加花括号是为了帮助解释器识别变量的边界，比如下面这种情况： 

```
for skill in Ada Coffe Action Java; do
    echo "I am good at ${skill}Script"
done
```

运行运行结果：

```
I am good at AdaScript
I am good at CoffeScript
I am good at ActionScript
I am good at JavaScript
```

#### 3.2.1 变量删除

使用 unset 命令可以删除变量。语法：

```
unset variable_name
```

变量被删除后不能再次使用。unset 命令不能删除只读变量。

## 4 Shell注释

常用#开头作行注释，程序运行的时候就会忽略该行。

sh中没有多行注释，只能美一行加一个#号，例如：

```
#--------------------------------------------
# 这是一个注释
# author：DataH
# site：https://github.com/zhi-z
#--------------------------------------------
##### 用户配置区 开始 #####
#
#
# 这里可以添加脚本描述信息
# 
#
##### 用户配置区 结束  #####
```

## 5 Shell传递参数 

我们可以在执行 Shell 脚本时，向脚本传递参数，脚本内获取参数的格式为：**$n**。**n** 代表一个数字，1 为执行脚本的第一个参数，2 为执行脚本的第二个参数，以此类推……

例如：将如下命令保存到tesh.sh文件中，然后运行。

```
#!/bin/bash
# author:DataH
# url:github.com/zhi-z

echo "Shell 传递参数实例！";
echo "执行的文件名：$0";
echo "第一个参数为：$1";
echo "第二个参数为：$2";
echo "第三个参数为：$3";
```

运行：

```
$ chmod +x test.sh 
$ ./test.sh 1 2 3
```

结果：

![](/home/datah/Desktop/GitHub/Linux/Shell/image/shell_1.png)

另外常用参数传递例子：

| 参数处理 | 说明                                                         |
| -------- | ------------------------------------------------------------ |
| $#       | 传递到脚本的参数个数                                         |
| $*       | 以一个单字符串显示所有向脚本传递的参数。  如"$*"用「"」括起来的情况、以"$1 $2 … $n"的形式输出所有参数。 |
| $$       | 脚本运行的当前进程ID号                                       |
| $!       | 后台运行的最后一个进程的ID号                                 |
| $@       | 与$*相同，但是使用时加引号，并在引号中返回每个参数。   如"$@"用「"」括起来的情况、以"$1" "$2" … "$n" 的形式输出所有参数。 |
| $-       | 显示Shell使用的当前选项，与set命令功能相同。                 |
| $?       | 显示最后命令的退出状态。0表示没有错误，其他任何值表明有错误。 |

例如：

```
echo "Shell 传递参数实例！";
echo "第一个参数为：$1";

echo "参数个数为：$#";
echo "传递的参数作为一个字符串显示：$*";
```

运行：

```
$ chmod +x test.sh 
$ ./test.sh 1 2 3
```

结果：

```
Shell 传递参数实例！
第一个参数为：1
参数个数为：3
传递的参数作为一个字符串显示：1 2 3
```

## 6 运算符

Shell的基本运算符和其他语言基本一样，常用的有：

- 算术运算符
- 关系运算符
- 布尔运算符
- 字符串运算符

 原生bash不支持简单的数学运算，但是可以通过其他命令来实现，例如 awk 和 expr，expr 最常用。 expr 是一款表达式计算工具，使用它能完成表达式的求值操作。 

例如，两个数相加(**注意使用的是反引号 ` 而不是单引号 '**)：

```
#!/bin/bash

val=`expr 2 + 2`
echo "两数之和为 : $val"
```

运行结果：

```
两数之和为 : 4
```

### 6.1 算术运算符

下表列出了常用的算术运算符，假定变量 a 为 10，变量 b 为 20：

| 运算符 | 说明                                          | 举例                          |
| ------ | --------------------------------------------- | ----------------------------- |
| +      | 加法                                          | `expr $a + $b` 结果为 30。    |
| -      | 减法                                          | `expr $a - $b` 结果为 -10。   |
| *      | 乘法                                          | `expr $a \* $b` 结果为  200。 |
| /      | 除法                                          | `expr $b / $a` 结果为 2。     |
| %      | 取余                                          | `expr $b % $a` 结果为 0。     |
| =      | 赋值                                          | a=$b 将把变量 b 的值赋给 a。  |
| ==     | 相等。用于比较两个数字，相同则返回 true。     | [ $a == $b ] 返回 false。     |
| !=     | 不相等。用于比较两个数字，不相同则返回 true。 | [ $a != $b ] 返回 true。      |

 **注意：**条件表达式要放在方括号之间，并且要有空格，例如: **[$a==$b]** 是错误的，必须写成 **[ $a == $b ]**。

实例：

```
a=10
b=20

val=`expr $a + $b`
echo "a + b : $val"

val=`expr $a - $b`
echo "a - b : $val"

val=`expr $a \* $b`
echo "a * b : $val"

val=`expr $b / $a`
echo "b / a : $val"

val=`expr $b % $a`
echo "b % a : $val"

if [ $a == $b ]
then
   echo "a 等于 b"
fi
if [ $a != $b ]
then
   echo "a 不等于 b"
fi
```

输出结果

```
a + b : 30
a - b : -10
a * b : 200
b / a : 2
b % a : 0
a 不等于 b
```

**注意：**

- 乘号(*)前边必须加反斜杠(/\)才能实现乘法运算；
- if...then...fi 是条件语句，后续将会讲解。

### 6.2 字符串运算符



| =    | 检测两个字符串是否相等，相等返回 true。   | [ $a = $b ] 返回 false。 |
| ---- | ----------------------------------------- | ------------------------ |
| !=   | 检测两个字符串是否相等，不相等返回 true。 | [ $a != $b ] 返回 true。 |
| -z   | 检测字符串长度是否为0，为0返回 true。     | [ -z $a ] 返回 false。   |
| -n   | 检测字符串长度是否为0，不为0返回 true。   | [ -n "$a" ] 返回 true。  |
| str  | 检测字符串是否为空，不为空返回 true。     | [ $a ] 返回 true。       |

实例：编写如如下脚本文件，并保存到shell_test.sh

```
#!/bin/bash

a="abc"
b="efg"

if [ $a = $b ]
then
   echo "$a = $b : a 等于 b"
else
   echo "$a = $b: a 不等于 b"
fi
if [ $a != $b ]
then
   echo "$a != $b : a 不等于 b"
else
   echo "$a != $b: a 等于 b"
fi
if [ -z $a ]
then
   echo "-z $a : 字符串长度为 0"
else
   echo "-z $a : 字符串长度不为 0"
fi
if [ -n "$a" ]
then
   echo "-n $a : 字符串长度不为 0"
else
   echo "-n $a : 字符串长度为 0"
fi
if [ $a ]
then
   echo "$a : 字符串不为空"
else
   echo "$a : 字符串为空"
fi
```

运行结果：

![](/home/datah/Desktop/GitHub/Linux/Shell/image/shell_2.png)

### 6.3 其他运算符

在Shell中还有其他运算符，在这里不过多介绍。

## 7 echo命令

命令 格式：

```
echo string
```

### 7.1 显示普通字符串

```
echo "my name is shell"
```

结果：my name is shell

### 7.2 转移字符

```
echo "\"my name is shell\""
```

结果：“my name is shell”

### 7.3 显示结果定向至文件

```
echo "It is a test" > test.txt
```

### 7.4 其他显示

其他显示还有换行等等。

## 8 控制流程

### 8.1 if esle语句

#### 8.1.1 if

if 语句语法格式：

```
if condition
then
    command1 
    command2
    ...
    commandN 
fi            # 注意这李是与if是倒过来的
```

#### 8.1.2 if else

if else 语法格式：

```
if condition
then
    command1 
    command2
    ...
    commandN
else
    command
fi
```

#### 8.1.3 if else-if else

if else-if else 语法格式：

```
if condition1
then
    command1
elif condition2 
then 
    command2
else
    commandN
fi
```

条件语句应用：

```
a=10
b=20
if [ $a == $b ]
then
   echo "a 等于 b"
elif [ $a -gt $b ]
then
   echo "a 大于 b"
elif [ $a -lt $b ]
then
   echo "a 小于 b"
else
   echo "没有符合的条件"
fi
```

输出：a 小于 b

### 8.2 for语句

for循环一般格式为：

```
for var in item1 item2 ... itemN
do
    command1
    command2
    ...
    commandN
done
```

实例：编写如下脚本，并运行。

```
#!/bin/sh

for i in {1..5}
do
    echo $i
done


for i in 5 6 7 8 9
do
    echo $i
done
```

输出结果：

![](/home/datah/Desktop/GitHub/Linux/Shell/image/shell_3.png)

### 8.3 while 语句

while循环用于不断执行一系列命令，也用于从输入文件中读取数据；命令通常为测试条件。其格式为：

```
while condition
do
    command
done
```

以下是一个基本的while循环，测试条件是：如果int小于等于5，那么条件返回真。int从0开始，每次循环处理时，int加1。运行上述脚本，返回数字1到5，然后终止。

```
#!/bin/bash
int=1
while(( $int<=5 ))
do
    echo $int
    let "int++"
done
```

运行脚本，输出：

```
1
2
3
4
5
```

## 9 函数

### 9.1 一般函数

shell中函数的定义格式如下：

```
[ function ] funname [()]

{

    action;

    [return int;]

}
```

- 可以带function fun() 定义，也可以直接fun() 定义,不带任何参数。
- 参数返回，可以显示加：return 返回，如果不加，将以最后一条命令运行结果，作为返回值。 return后跟数值n(0-255

实例：

```
demoFun(){
    echo "这是我的第一个 shell 函数!"
}
echo "-----函数开始执行-----"
demoFun
echo "-----函数执行完毕-----"
```

运行结果：

```
-----函数开始执行-----
这是我的第一个 shell 函数!
-----函数执行完毕-----
```

### 9.2 传参函数

例如：

```
funWithParam(){
    echo "第一个参数为 $1 !"
    echo "第二个参数为 $2 !"
    echo "第十个参数为 $10 !"
    echo "第十个参数为 ${10} !"
    echo "第十一个参数为 ${11} !"
    echo "参数总数有 $# 个!"
    echo "作为一个字符串输出所有参数 $* !"
}
funWithParam 1 2 3 4 5 6 7 8 9 34 73
```

运行结果：

```
第一个参数为 1 !
第二个参数为 2 !
第十个参数为 10 !
第十个参数为 34 !
第十一个参数为 73 !
参数总数有 11 个!
作为一个字符串输出所有参数 1 2 3 4 5 6 7 8 9 34 73 !
```

## 10 test 命令

Shell中的 test 命令用于检查某个条件是否成立，它可以进行数值、字符和文件三个方面的测试。

例如：

- 数值测试：代码中的 [] 执行基本的算数运算

  ```
  num1=100
  num2=100
  if test $[num1] -eq $[num2]
  then
      echo '两个数相等！'
  else
      echo '两个数不相等！'
  fi
  
  ```

  输出结果：

  ```
  两个数相等！
  ```

- 字符串测试

  ```
  num1="ru1noob"
  num2="runoob"
  if test $num1 = $num2
  then
      echo '两个字符串相等!'
  else
      echo '两个字符串不相等!'
  fi
  ```

  输出结果：

  ```
  两个字符串不相等!
  ```

- 文件测试：

  ```
  cd /bin
  if test -e ./bash
  then
      echo '文件已存在!'
  else
      echo '文件不存在!'
  fi
  ```

  输出结果：

  ```
  文件已存在!
  ```

## 12 重定向

```
$echo result > file  #将结果写入文件，结果不会在控制台展示，而是在文件中，覆盖写
$echo result >> file  #将结果写入文件，结果不会在控制台展示，而是在文件中，追加写
echo input < file  #获取输入流
```

## 13 自动提交GitHub脚本

```
#!/bin/bash
echo "-------Begin-------"
git add .
git commit -m $1
echo $1
git push origin master
echo "--------End--------"
```

## 参考文献：

1[ 菜鸟Shell教程](http://www.runoob.com/linux/linux-shell-basic-operators.html)

2 [一篇文章学懂Shell脚本](https://www.jianshu.com/p/71cb62f08768)

