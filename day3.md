# 四、系统管理命令（sudo 管理员权限）

26、	adduser	添加用户
		adduser  用户名
		-home 路径   ：指定用户主目录
		-uid  编号   ：指定用户编号
		-gid  编号   ：指定用户组编号
	当添加一个用户时会自动将用户信息添加到/etc/passwd, /etc/group, /etc/shadow
	并且会在/home下创建用户主目录

27、	finger  查询用户的详细信息
		finger  目标用户名
	

28、	passwd  设置密码
		passwd  目标用户
		-l锁定用户；	-u解锁用户；	-S查询用户状态：sudo passwd -S edu 		

29、	userdel	删除用户
		userdel 目标用户
	-r  删除用户时删除用户主目录：sudo userdel -r edu
		
30、	su 切换用户
		su  目标用户
		-c  执行指令
		

# 五、磁盘管理命令

​	
31、	df	查询磁盘设备名
​		让ubantu识别U盘，插入U盘，选择连接到虚拟机---虚拟机选项---可移动设备---U盘名(disk2)---连接---桌面左侧可看到U盘
​		虚拟机如果识别到U盘，在/dev目录下多了一个设备/dev/sdb1
​	
32、	fdisk	-l  设备名（查看设备的文件系统类型）
​		sudo fdisk -l 	或者  sudo fdisk -l /dev/sdb1   ----->可知道sdb1文件类型是FAT32，属于vfat类型

33、	mount	挂载U盘、移动硬盘、光盘、网络文件系统（NFS）、windows的C、D分区
		mount -t 目标文件系统类型   源设备名（df）  目标子目录(mkdir)
		
	步骤：
			1、使用df命令查询源设备名称，（查看设备文件系统类型sudo fdisk -l）
			2、使用mkdir创建目标子目录/mnt/usb
			3、使用mount命令挂载 mount -t vfat /dev/sdb1  /mnt/usb
			4、使用文件或目录操作，cp\mv
			5、使用umount命令取消挂载/卸载设备
	
	实例：挂载光盘镜像文件mydisk.iso：
			1、创建光盘映像文件：mkisofs -r -J -V mybin -o mybin.iso  /bin
				       含义：创建iso文件系统   -V指定光盘名字   -o指定光盘映像文件名   /bin源文件路径，把谁创建成iso文件
			   之后，就可以在当前目录下ls看到iso文件，cat mybin.iso  看到乱码，是因为文件系统不一致
			2、创建挂载点目录：mkdir /mnt/vcdrom
			3、挂载：sudo mount -t iso9660 -o loop  mybin.iso  /mnt/vcdrom
				就可以在/mnt/vcdrom下看到一个bin的链接文件


34、	umount	卸载文件系统
		umount  目标设备名/目标子目录

35、	fdisk   查询磁盘分区和对磁盘分区
		1、fdisk  -l  磁盘设备名   ：查询
		2、fdisk  磁盘设备名（例如/dev/sdb）  :对/dev/sdb磁盘进行分区，可以根据提示输入m获取命令表、n创建分区、w保存退出

# 六、其他命令

36、	kill	向进程发送指定信号
		kill  信号值   进程号
		kill -l  ：查看64个信号值：1是挂起，2是中断，3是退出，9是结束进程，
		ps -aux、 ps -ef   ：查看全部进程

37、	apt-get	在线安装/本地安装
		安装程序：sudo  apt-get install  程序名
		更新：sudo apt-get update
		卸载：sudo apt-get autoremove/remove  程序名

	（redhat）rpm安装
		安装：rpm -ivh  安装包文件（*.rpm）(jdk...rpm)
		卸载：rpm -e  应用程序名（gcj）
		查询：rpm -qa | grep 应用程序名

# 七、网络相关命令

38、	ifconfig  查询，临时设置网卡，启用和关闭网卡
		1、查询所有网卡设备：ifconfig
		2、查询指定网卡设备：ifconfig  网卡设备名
	   	3、设置网卡信息：
			网卡IPv4：ifconfig  设备名  IP  
			网卡子网掩码：ifconfig  设备名  netmask  子网掩码
			网卡广播：ifconfig  设备名  broadcast  广播地址
			注意：只是临时修改，要永久修改，需要使用图形界面或修改配置文件/etc/network/interfaces文件
			

			网卡IPv6地址：ifconfig  设备名  add  IPv6地址
		4、启用、关闭网卡
			启用：ifconfig  设备名 up
			禁用：ifconfig  设备名 down
	
	windows下查看IP（win+R打开,命令输入cmd进入终端）：ipconfig

39、ping	测试网络连通性
		ping  目标IP/目标域名
	

linux与windows网络互通
	关闭防火墙：service  ufw stop
	windows物理网卡和虚拟网卡都处于工作状态（控制面板----网络适配器---查看网卡是否正常；vmnet1和vmnet8是虚拟网卡）
	虚拟机的net服务和DHCP服务要启用：
			步骤：我的电脑-->管理-->服务与应用程序-->服务-->vmware的5个服务都要开启
	ubantu联网
	把windows对应虚拟网卡的IP和ubantu下IP设置为相同网段

telnet  远程登陆（tcp协议编程）
	telnet 目标IP/目标域名（pop.163.com，实际上就是一个IP，DNS服务）   端口号（1-65535）（110）  ：能够识别主机的作用；端口号：用来区分主机中的应用程序身份。
	163邮箱---账户---设置---smtp/pop3/icmp选中（允许可以telnet方式访问）----保存设置

ssh	安全远程登陆（对用户和密码信息进行加密）
	ssh  目标IP/目标域名   端口号

# 八、vim编辑器

1、vim 是 Unix 类操作系统中最为流行的文本编辑器。尽管目前已有 gedit 等一些工作在图形界面下使用起来也更为方便
的文本编辑器，但在很多情况下，vim 这种专为字符界面操作而设计的编辑器恐怕还是要充当首选，比如通过 telnet
网络登录来使用系统时。 

2、要使用 vim 编辑一个文本文件，终端命令：
         （1）vim   文件名   ：  打开一个文件，光标在首行，如果没有该文件则创建再打开。
   	 （2）vim   文件名 +行号：打开文件直接进入指定行。

   在无法使用光标键的情况下，可用 k、j、h、l 上下左右移动光标到指定位置，

3、vim的模式  
	命令模式、编辑（插入）模式、底行模式
   命令模式：控制编辑器的操作，例如复制粘贴剪切、保存退出等。
   编辑模式：专门编辑代码的状态。
   底行模式：以冒号开头输入命令的状态。

   命令模式--->编辑模式，可按这些按键：i（在当前光标之前），a（在当前光标之后），o（在当前光标的下一行新建一行进行插入） ，A（在当前光标所在行的末尾）进入编辑模式；   
   编辑模式--->命令模式：按下Esc键
		
4、命令/底行模式下，常用的命令：
（1）文件保存修改
	“:w”：保存文件 ； 如果不小心按了ctrl s保存，此时编辑器会进入假卡死状态，按下ctrl q恢复
	“:q”：退出文件，回到终端命令行
	“:wq”：保存并退回到 shell； 
  	“:q!”：放弃对文件的修改，强制退回到终端。
	“:set nu”：显示行号
	“:set nonumber” ：不显示行号
	“:w 文件名”：保存该文件，并另存为某一个文件；
	“:%s/aaa/bbb/g”：将全文的aaa都替换成bbb
	“:m,ns/aaa/bbb/g”：将m行到n行的aaa都替换成bbb；m、n是数字

（2）文件内容修改
	“x” ：删除当前字符
	“nx”：删除光标所在位置往后n个字符
	“dd”：删除光标所在行
	“ndd”：删除光标起始的n行
	D : 删除光标后面的内容；

	u：撤销 ； 
	ctrl + r ：撤销的恢复	
	yy：复制 ； 
	nyy：复制光标起始的n行
	p :paste粘贴

（3）移动
	/字符串: 在当前文件查找该字符串  
	% ：在命令模式下，把光标定位到某个括号（）/{}处，输入%，会自动跳转到匹配的另一边括号处，这样会方便查看匹配的括号。
	
	“:gg”：光标移动到文件开头
	“:G” ：光标移动到文件结尾
	“:行号”：移动到指定行
	ctrl+f  ：向前翻动一页
	ctrl+b  ：向后翻动一页

（4）查找
	/\<关键字\> ：全字匹配
	\>关键字\>    ：只匹配关键字末尾
	\<关键字    ：只匹配关键字开头
	n   ：查找下一个
	?   ：向上查找    ；  也可以？关键字  ，按下n键，会向上查找

（5）快速查找
	*
一行的开头  home键，或者 ^  符号 ；  一行的结尾 ： end或者  $  符号。



5、vim其他命令

给光标所在行设置下划线，但不固定：set cursorline   set nocursorline   
语法高亮 ： syntax on ； syntax off    //关键字变颜色
自动缩进： set autoindent
    set noautoindent
永久改变，编写配置文件：  ~/.vimrc    

6、分屏操作：
    在已经打开的文件中对文件进行操作。
    1）上下分屏底行 ：sp 增加屏幕上下分屏可以增加很多。  split。
    2）ctrl+ 2 下w 在两个屏幕之间切换光标   ;     ctrl + w + hjkl : 移动光标到不同界面。
    3）左右分屏：垂直分屏
    ：vsp      ; vertical
    4）q:关闭窗口
    5）ctrl + w +o  关闭光标不在的所有屏幕。打开多个。打开多个，一次关闭。
vim 分屏  一个在编辑器里面  一个在编辑器外面。

    1）水平分屏：new + filename   创建分屏 并打开文件filename
    2）垂直分屏：vnew + filename   创建垂直7分屏  并且打开另一个文件。
    3）ctrl + w+o  取消其他分屏
    
        4）  ctrl + w hjkl  上左下右  切换可以用。
        5） vim filename1  filename2   同时打开多个文件。
    ：bn   或者  ：bp  来切换。