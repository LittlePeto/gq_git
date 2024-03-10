# 一、GCC工具

1、GCC：交叉平台编译器，能够在当前CPU平台上为多种不同体系结构的硬件平台开发软件。
编译命令：gcc  [ 选项]  源文件  [选项]  [目标文件]
运行命令：./目标文件
如果有多文件交叉使用，需要将多个源文件一起编译；例如
gcc  1.c  2.c  3.c  -o test

2、C程序编译阶段：
预处理：处理程序的头文件展开、宏定义、注释等内容，生成.i文件；
gcc  -E  hello.c -o  hello.i
编译：检查代码的规范性，是否有语法错误，将代码翻译成汇编语言；生成 .s文件
gcc  -S  hello.i -o  hello.s
汇编：将.s文件转换成二进制目标文件，生成.o文件
gcc  -c  hello.s  -o  hello.o
链接：将程序中所用到的库加载到代码中，一般路径是/usr/lib；生成最终的可执行文件
gcc  hello.o  -o  hello
如果不选择选项和目标文件，则会默认生成一个a.out的可执行文件

# 二、GDB调试工具

1、gdb可以让用户查看程序的内部结构、打印变量值、设置断点、以及单步调试源码。
	
   加上 -g 选项编译才能使用gdb工具：gcc -g hello.c -o test
   然后使用命令进入调试界面： gdb  目标可执行文件test

2、GDB命令：
查看所载入文件的源代码，每次默认显示10行：list  
设置显示的行数：set  listsize  行数
在指定行设置断点，程序运行到此行之前：break  行号
恢复程序运行：continue
查看断点信息：info break
查看当前的文件/共享库：info files/share
运行代码，从第一行/指定行开始运行到断点处：run   [行号]
查看变量值：p  变量 ；一般是程序运行到某个断点停止后，即可使用该命令查看变量值。
单步/多步运行：step/next  [count多少步]
step 会进入函数内部；next 不会进入函数内部
退出gdb : q

# 三、Makefile工具

1、Makefile 文件可有效地控制多模块文件的项目编译。
2、使用 make 命令启动应用了 Makefile 文件的项目编译过程。
3、若非使用-f 选项指定 make 所用的 makefile，make 将在当前目录下以 GNUmakefile、makefile、Makefile 的顺序寻找。//文件名只能三选一。

4、Makefile 的编写规则：
	自顶向下分步求精地结构化表述，
   句法是： 
	目标：与此目标直接相关的依赖文件列表   
	(TAB)如何利用这些依赖文件生成目标     //方法

   例子：双文件联合编译。c - o - 目标文件。
	准备好2个c文件；准备好编译文件Makefile

	(如果使用了用户头文件，应排在与目标有关的文件列表行中。如：helloa.o: helloa.c helloa.h） 
	说明部分：//只涉及 c文件  o文件和 可执行文件。

5、Makefile 文件的简化： 
 Makefile 有几个很有用的变量，其中常用的三个意义分别是: 
$@ --目标文件, 
$^ -- 所有的依赖文件, 
$< -- 第一个依赖文件. 
利用这三个变量可以简化我们的 Makefile 文件为: 
 hello: helloa.o hellob.o 
     gcc -o $@ $^ 
 helloa.o: helloa.c 
     gcc –c $< 
 hellob.o: hellob.c 
     gcc –c $< 


6、利用 Makefile 的缺省规则 .c.o: gcc -c $< (这个规则表示所有的 .o 文件都是依赖与相应的.c 文件) 还可以进一步简化： 
   hello: helloa.o hellob.o 
     gcc -o $@ $^ 
 .c.o: gcc -c $< 
(只要修改文件中目标名和目标文件名即可应用于其它项目)。

7、在 Makefile 文件中还可以增加其它目标，比如追加两行： 
 clean: 
     rm –f hello *.o      //移除所有目标文件。
 对编译无影响，但执行 make clean 命令可删除所有目标模块。 


8、makefile的使用
   make ：自动编译管理器。文件名字必须为makefile，或者Makefile。

   makefile基本结构：
    目标 ： 依赖
    tab    命令
    hello.o : hello.c 
        gcc -c hello.c -o hello.o

   运行  make 

9、意义：make：解决多文件联合编译的问题。  
    1）gcc也可以少数多文件联合编译，如果有10个，100个c文件，就用make工具。
    2）自己编写一遍，makefile 编译文件，别人就不用再写一遍了。节省时间的工程管理工具。
	安装： sudo apt-get install make
	使用：make 没有参数，当执行make命令时，会自动寻找默认的配置文件，makefile。
    终极目标（要生成的目标程序文件） ：    依赖型列表（生成目标所需要哪些文件，哪些材料）
    （这里必须是一个TAB键） 指令 利用依赖型列表来生成目标指令。

    例如： power ： power.c              make 很简单 就是 命令的排列 写在文件中就行。
        gcc power.c -o power 
    
    练习一：一条语句的 
    练习二：多条语句的，两个文件的。
    练习三： 包含头文件的怎么写。
    只要把头文件加在 第一步即可  
    
        main.o : main.c max.h     //头文件加在后面。
        gcc -c main.c main.h
    
    max.o : max.c max.h
        gcc -c max.c max.h

如果用不到的话，就加上 rm *.o -rf  把后面用不到的中间过程文件删掉。但要执行 make clean；