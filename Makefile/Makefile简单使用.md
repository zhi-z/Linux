# Makefile简单使用

## 1 Makefile的引入及规则

使用keil, mdk, avr等工具开发程序时点点鼠标就可以编译了，
它的内部机制是什么？它怎么组织管理程序？怎么决定编译哪一个文件？

## 2 对文件进行编译

### 2.1 直接使用gcc进行编译

它们要经过预处理、编译、汇编和连接过程，最终的到可执行的文件。

简单, 但是会对所有文件都处理一次, 文件多时如果只修改其中一个文件会导致效率低

```
gcc -o test a.c b.c  
```

其实上面主要经过两个步骤：第一个是把a.c和b.c编译分别得到一个xxx.s文件，然后再进行汇编得到两个xxx.o文件，最后再把这两个文件进行连接得到可执行的文件。

### 2.2 使用Makefile方式

- Makefile的核心---规则 

```
目标 : 依赖1 依赖2 ...
	命令
```

> 执行命令的规则：当"目标文件"不存在, 或某个依赖文件比目标文件"新",则: 执行"命令"

例如：

```
// 当执行make命令的时候，他会对a.o和b.o进行编译，然后执行gcc命令进行链接
test:a.o b.o
	gcc -o test a.o b.o	
a.o : a.c
	gcc -c -o a.o a.c

b.o : b.c
	gcc -c -o b.o b.c
```

当只修改a.c文件时候，就会只执行a.c一下的命令。

- 如果编译生成的需要在arm中运行，则可以使用如下：

```
all:
	arm-linux-gcc -c -o led_on.o led_on.S     // 编译 
	arm-linux-ld -Ttext 0 led_on.o -o led_on.elf  // 链接
	arm-linux-objcopy -O binary -S led_on.elf led_on.bin  // 生成bin文件
	arm-linux-objdump -D led_on.elf > led_on.dis  // 把 led_on.elf进行反汇编，这样可以查看汇编指令了
clean:
	rm *.bin *.o *.elf
	
```



### 2.3 Makefile的语法

#### 2.3.1 常用符号

- 通配符: %.o
-  $@ 表示目标
- $< 表示第1个依赖文件
- $^ 表示所有依赖文件

例如：

```
test: a.o b.o c.o
	gcc -o test $^
	
%.o : %.c
	gcc -c -o $@ $<  

```

#### 2.3.2 清除文件

```
test: a.o b.o c.o
	gcc -o test $^
	
%.o : %.c
	gcc -c -o $@ $<

clean:
	rm *.o test

```

#### 2.3.3 假想目标: .PHONY

如上进行清除文件操作，执行make clean 命令的时候，如果当前文件夹下有clean文件，那么执行make clean 命令就没有效果，所示引入假象目标，这样执行的时候他就不会判断该文件是否存在。例如：

```
test: a.o b.o c.o
	gcc -o test $^
	
%.o : %.c
	gcc -c -o $@ $<

clean:
	rm *.o test

.PHONY: clean	
```

#### 2.3.4 变量

变量有即时变量、延时变量

:=   # 即时变量
=    # 延时变量
?=   # 延时变量, 如果是第1次定义才起效, 如果在前面该变量已定义则忽略这句
+=   # 附加, 它是即时变量还是延时变量取决于前面的定义

例如：

```
A := xxx   # A的值即刻确定，在定义时即确定
B = xxx    # B的值使用到时才确定 
```

#### 2.3.5 Makefile函数

- $(foreach var,list,text)
- $(filter pattern...,text)      # 在text中取出符合patten格式的值
- $(filter-out pattern...,text)  # 在text中取出不符合patten格式的值
- $(wildcard pattern)            # pattern定义了文件名的格式,

​                                                     #wildcard取出其中存在的文件

- $(patsubst pattern,replacement,$(var))  # 从列表中取出每一个值

​                                                                              #如果符合pattern
								              #则替换为replacement

例如：

```

A = a b c 
B = $(foreach f, $(A), $(f).o)
# 结果：a.o  b.o  c.o

C = a b c d/
# 取出%/的格式
D = $(filter %/, $(C))
# out:d/
# 这个与上面的相反
E = $(filter-out %/, $(C))

# 查找当前目录下符合这些格式的文件有哪些
files = $(wildcard *.c)

# 判断哪些文件真实存在
files2 = a.c b.c c.c d.c e.c  abc
files3 = $(wildcard $(files2))

# 从列表中取出每一个值，如果符合pattern，则替换为replacement
dep_files = $(patsubst %.c,%.d,$(files2))

all:
	@echo B = $(B)
	@echo D = $(D)
	@echo E = $(E)
	@echo files = $(files)
	@echo files3 = $(files3)
	@echo dep_files = $(dep_files)
```

## 参考文档:

- 官方文档: http://www.gnu.org/software/make/manual/