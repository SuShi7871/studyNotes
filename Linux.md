# 一.Linux

## 1.Linux介绍

## 2.vm安装和CentOs

  安装地址：官网下载或者 去www.nocmd.com下载
  Linux分区：/boot分区，引导分区 ；

​						/swap 分区 ；

​						/root  根分区；s用

## 3.网络连接的三种方式

  桥接模式，虚拟系统可以和s外部系统通讯，但是容易导致ip冲突。
  NAT模式，网络地址转换模式，虚拟系统可以和外部系统通讯，但是不会导致ip冲突。
  主机模式，独立的系统。

## 4.虚拟机克隆

​    方式1：直接拷贝一份安装好的虚拟系统；
​    方式2：使用VMware的克隆操作，注意克隆时要先关闭安装好的Linux系统；

## 5.虚拟机快照

​    使用系统的时候想回到原先的状态的操作。

## 6.虚拟机的迁移和删除

​    本质是文件的迁移和删除

## 7.安装vmtools

   安装vmtools后可以在Windows下更好的管理Linux虚拟机,可以设置Windows和Linux虚拟机共享文件夹

## 8.Linux的目录结构

在Linux里一切皆文件,具体的目录结构：

```cmd
/bin 			# 存放着常用的指令
/sbin 			# 存放系统管理员使用的系统管理程序
/home 			# 存放普通用户的主目录
/root 			# 系统管理员
/lib 			# 系统开机所需要的最基本的动态连接共享库
/lost+found 	# 这个目录一般情况下是空的，当系统遇到非法关机后存放一些文件；
/etc 			# 系统管理所需要的配置文件和子目录，比如mysql数据库my.conf
/usr			# 用户的很多应用程序和文件都在这个目录下，类似于windows的program files目录；
/boot 			# 存放启动Linux时的一些核心文件
/proc			# 【不能动】	是一个虚拟目录，是系统内存的映射，访问这个目录来获取系统的信息
/srv			# 【不能动】
/sys 			# 【不能动】
/temp 			# 用来存放一些临时文件
/dev 			# 类似于windows的设备管理器，把所有的硬件以文件的形式存储
/media 			# Linux会自动识别一些设备，比如U盘，光驱等，当识别后Linux会把识别的设备挂载到这个目录下
/mnt 			# 系统提供该目录是为了让用户临时挂载别的文件系统的
/opt 			# 主机额外安装软件所使用的目录
/usr/local 		# 主机额外安装软件的目录
/var 			# 存放着不断扩充的东西，包括各种日志文件
/selinux 		# 是一个安全子系统，他的控制程序只能访问特定文件；
```



## vi和vim基本介绍

### 三种模式：

a.正常模式：

```cmd
	# 拷贝当前行 	yy+p
	# 拷贝多行	n+yy+p
	# 删除当前行   dd
	# 删除多行   	n+dd
    # 跳转到文件的最后一行（在一般模式下） 输入	G
    # 跳转到文件的第一行（在一般模式下） 输入  gg
    # 撤销动作 esc+u
    # 编辑文件，在一般模式下将光标移动到第20行，可以输入20+shift+g
```

b.插入模式：

按下`i,I,a,A,r,R`可以进入插入模式

c.命令行模式：

   按下Esc再按 ：或者 / 可以进入命令行模式
   查找关键词  输入 /+关键字，再输入n就是查找下一个

​    设置文件的行号 

```cmd
 :set nu
 # 取消 设置文件的行号 
 :set nonu
```

### 关机重启指令

```cmd
shutdown -h now  		# 立刻进行关机
shutdown -h 1 			# 1分钟后关机
shutdown -r now			# 现在重新启动计算机
halt 					# 关机
rebot 					# 重启
sync 					# 把内存数据同步到磁盘
```

### 用户登录和注销

```cmd
su - 用户名    # 切换用户账户
logout或者exit # 退出当前用户 
```

## 用户管理   

### 常用指令

```cmd
usreadd 用户名		该用户的默认目录在/home/		# 添加用户         
useradd -d  用户名					# 添加用户并指定用户目录的位置
passwd 用户名						# 设置密码                 
pwd								# 显示当前用户所在的目录         
userdel  用户名					# 删除用户（保留家目录） 	   
userdel -r 用户名					# 删除用户（不保留家目录） 	  
id 用户名							# 查询用户信息               
who am i	 # 切换到指定用户下，然后输入,查询何时登录到系统 
# 查看服务器信息
uname -a 
lsb_release -a
```

### 用户组

```cmd
groupadd 组名					 # 新增组		        
groupdel 组名                   # 删除组     			
useradd -g 组名 用户名     		# 增加用户时直接加上组
usermod	 -g  用户组  用户名      # 修改用户组
```

用户和组相关的文件

```cmd
/etc/passwd文件 # 用户的配置文件
/etc/shadow文件 # 口令的配置文件
/etc/group文件  # 组的配置文件
```

## 基本指令

### 指定运行级别基本介绍

0：关机

1：单用户【找回丢失密码】

2：多用户状态无网络

3：多用户状态有网络 （常用）

4：系统未使用保留给用户

5：图形界面 （常用）

6：系统重启

systemctl get-default 显示当前运行级别

### 找回登录密码

### 帮助指令

man 获取帮助信息

```
基本语法	man ls
```

​	在Linux下，隐藏文件以  .开头

​	help指令	获得shell内置命令的帮助信息

### 文件目录类指令

```
cd ~						回到自己的家目录
mkidr   -p /路径/路径	 创建多级目录
rmdir    					 # 删除目录
rm   -rf  /路径           	#不管目录是否为空都会删除
touch   文件名称       		  #创建空文件
cp  [选项]  source dest 		#拷贝文件
cp -r    [选项]  source dest  #递归复制文件到另一个文件夹下
rm -r    [选项]	#递归删除整个文件夹
rm -f    [选项]	#强制删除不提示
mv   oldName File    newNameFile (如果两个文件在一个目录下，这是重命名操作)
mv   /temp/movefile    /targetFolder   (如果两个文件不在一个目录下，这是移动文件操作)
cat	#查看文件内容
cat	【选项】要查看的文件
cat -n  【选项】要查看的文件
cat 只能查看文件。不能修改
cat 	-n  /etc/profile | more
more 要查看的文件
```

less 分屏查看文件内容，less指令在显示文件内容时，并不是一次将整个文件加载后才显示，而是根据需要加载的内容 显示，对于显示大型文件具有很高的效率。

```
less	#要查看的文件
echo 	#输出内容到控制台
echo	[选项]	[输出内容]  
head 		#显示文件的开头部分
head  文件	#输出文件前10行
head 	 -n 	文件	#输出文件前n行
tail	文件	#输出文件后10行
tail	 -n 	文件	#输出文件后n行
tail -f  文件 	#实时追踪该文档的所有更新
```

```
>  重定向
>> 追加
基本语法
ls -l >文件 #将列表的内容写入a.txt中
ls -al >> 文件  #将列表的内容追加到aa。txt的末尾
cat 文件1> 文件2 #将文件1的内容覆盖到文件2中
echo "内容" >> 文件 #追加
```

```
rm 	/home/myroot 	#删除home下的软连接名myroot
ln #软连接也称符号连接
ln -s  [源文件或目录]  [软连接名]	#给原文件创建一个软链接
```

### 日期指令

```cmd
history  #查看使用过的历史命令
history  10	#查看使用过的10条历史命令
!5 	#执行历史编号为5的命令
date		#显示当前时间
date  +%Y	#显示当前年份，加号不能少
date  +%m	#显示当前月份，加号不能少
date  +%d	 #显示当前日份，加号不能少
date   ”+%Y-%m-%d  %H:%M:%S    #显示年月时分秒，加号不能少
date -s 字符串时间	#设置时期
cal 	[选项]      #显示当年日历
cal    年份         #显示指定年份日历
```

### 查找指令

**find指令**

find   [搜索范围]   [选项]

选项说明：

```cmd
name    #按名查找
find   /home  -name  hello.txt
-user     #查找属于指定用户名的文件
find  / -user root
-size      #按照指定的文件大小查找(+n  代表大于，-n    代表小于 ，n代表等于)
find / -size +200M
```

locate指令，可以快速定位文件路径

```cmd
updatedb
locate [路径]   #使用之前必须先使用 updatedb指令
```

grep指令，过滤查找，管道符，“|”，表示将前一个命令的处理结果输出传递给后面的命令处理。

```cmd
grep  [选项]   查找内容  源文件
```

选项说明：

-n  显示匹配行及行号

-i   忽略字母大小写

```cmd
cat   /home/hello.txt |grep "yes"    #在hello.txt文件中查找yes所在的行，并显示行号
grep  - n  "yes"  /home/hello.txt	#写法2
```

### 压缩和解压类

gzip 压缩文件

```cmd
gzip 文件  #压缩文件，只能将文件压缩为.gz文件
gzip  /home/hello.txt
```

gunzip  解压文件

```cmd
gunzip  文件.gz   #解压缩文件命令
gunzip /hello/hello.txt.gz 
```

常用选项

```cmd
-r  # 递归压缩，即压缩目录
zip -r myhome.zip /home/     #将home下面的文件夹压缩成myhome.zip
```

-d<目录> #指定解压后的文件存放目录

```cmd
mkdir  /opt/tmp						#先创建一个目录
unzip -d /opt/tmp /home/myhome.zip   #将myhome.zip解压到opt/tmp目录下
```

tar指令，是打包指令，最后的打包后的文件是.tar.gz

基本语法

tar [选项] xxxx.tar.gz  打包的内容		#打包目录，压缩后的文件格式是.tat.gz

选项说明：

-c		产生.tar打包文件

-v		显示详细信息

-f		指定压缩后的文件名

-z		打包同时压缩

-x		解包.tar文件

具体演示：

```cmd
tar -zcvf   pc.tar.gz   /home/pig.txt   /home/dog.txt  

# 压缩多个文件，将home下面的pig.txt和dog.txt压缩成pc.tar.gz

tar -zcvf myhome.tar.gz   /home/      #将home的文件夹压缩成myhome.tar.gz

tar -zxvf   pc.tar.gz    # 将pc.tar.gz解压到当前目录

mkdir /opt/tmp2

tar -zxvf /home/myhome.tar.gz  -C /opt/tmp2	#将myhome.tar.gz 解压到/opt/tmp2目录下
```

## 网络配置

### NAT网络配置

service network restart  	#重启网络服务

### host配置

```cmd
# 设置主机名
hostname		#查看主机名
vim    /etc/hostname    #在这里面修改主机名
# 设置hosts映射
windows 
在 C:.Windows.System32.drivers.etc.hosts 里面设置
# linux
在 /etc/hosts  文件指定
```

### 主机名解析过程

浏览器缓存 -> DNS缓存 -> hosts文件  -> 域名服务DNS解析器 查找过程

host是用来记录IP和主机名Hostname的映射关系

DNS  就是域名系统，是互联网上作为域名和IP地址相互映射的一个分布式数据库

## 组和权限管理

### 组

1）所有者

2）所在组

3）其他组

4）改变用户所在的组

```
ls -ahl	#查看文件或者目录的所有者
chown   用户名  文件名		#修改文件的所有者
```

实例

```
groupadd  monster 		#创建一个组
useradd -g   moster   fox  #创建一个用户fox，并将它放到monster组中
```

所在组

```
chgrp 组名 文件名   #修改文件所在的组
```

改变用户所在的组

```
usermod -g  新组名   用户名
usermod -d 目录名   用户名 	#改变用户登陆的初始目录，注意：用户需要有进入到新目录的权限
```

### 权限

-rw-r--r--.  1 root  root          0 9月   2 11:23 1.txt

0-9位说明：

1.第0位确定文件类型（d - l c b）

​	l 是链接，类似于windows的快捷方式

​    d 是目录

​	c是字符设备，如鼠标，键盘

​	b是块设备，比如硬盘

2.1~3位确定文件所有者拥有的权限，r w x

​		作用到目录和文件是不同的

3.4~6位确定所属组拥有改文件的权限

4.7~9位确定其他用户拥有该文件的权限

修改权限-chown

第一种方式：+，-，=更改权限

u：所有者	g：所有组	o：其他人 	a:所有人

```cmd
chown newowner 文件/目录  					#改变所有者
chown  newowner : newgroup   文件/目录   	#改变所有者和组
chown kevin /home/abc.txt				   #将/home/abc.txt下面的文件所有者修改成Kevin
```

-R 	#如果是目录，则使其下所有的子文件或目录递归生效  

```cmd
chown   -R tom   /home/test		#将/home/test目录下所有的文件和目录所有者都修改成tom
chgrp newgroup  文件/目录		#修改所在组
chgrp shaolin /home/pig.txt    #将 /home/pig.txt的所在组修改成shaolin
```

## 定时任务调度

crond 进行定时任务调度

```cmd
crond [选项]
-e 	编辑定时任务
-l 	查询定时任务
-r	删除定时任务
```

## rpm包的管理

用于互联网下载包的打包工具

```cmd
rpm -qa|grep xx			   		# 查询已安装的rpm列表， 
rpm -q 软件包名					# 查询软件包是否安装
rpm -qi  软件报名      		 	# 查询软件包信息
rpm -ql  软件报名     			# 查询软件包中的文件
rpm -qf   文件全路径名  	   		# 查询文件所属的软件包
rpm -e 软件包的名称  		   		# 删除软件包
rpm -ivh rpm包的全路径名称    		# 安装rpm包
# yum是一个shell前端软件包管理器。
yum list | grep xx软件列表 	  	# 查询某个软件是否已安装
yun install	xxx  			 	# 安装某个软件包
```

## Linux磁盘分区挂载

1. Linux硬盘分IDE硬盘和SCSI硬盘，目前基本上是SCSI硬盘

2. IDE硬盘，驱动器标识符为hdx~，其中hd表面分区所在的设备类型，x为盘号（a为基本盘，b为 基本从属盘，c为辅助主盘，d为辅助从属盘） ~代表分区，前四个分区用数字1-4表示，它们是主分区或扩展分区，从5开始就是逻辑分区。
3. SCSI硬盘则标识为sdx~，sd来表示分区所在的设备类型，则其余的和IDE硬盘一样
4. `lsblk` 或者   `lsblk -f`		#查看所有设备的挂载情况

![image-20221028112750793](C:.Users.Administrator01.AppData.Roaming.Typora.typora-user-images.image-20221028112750793.png)

## 进程管理

### 进程操作

ps用来查看系统中哪些进行正在执行

```cmd
ps -aux 				# 查看所有进程
ps -aux | grep sshd		# 查看系统有没有ssh进程
ps -ef					# 以全格式显示所有的进程
UID		用户id
PID		进程id
PPID  	父进程id
C:			cpu用于执行优先级的因子，数值越大，执行优先级越低；
STIME:	进程启动时间
TTY:		完整的终端名称
TIME		cpu时间
CMD:		启动进程所用的命令和参数
```

```cmd
kill 进程名	#杀掉进程
kill all 进程名		#杀死进程
kill all gedit		#杀死多个进程
-9	 # 表示强迫进程立即停止
/bin/systemctl start sshd.service	#重启sshd服务
```

### 服务管理

service管理指令

```cmd
service	服务名（start|stop|reload|status）
setup	#查看所有进程
```

运行级别

开机流程

开机		BIOS 	/boot 		systemd 进程1 		运行级别 		运行级别对应的服务

```cmd
systemctl get-default		#查看系统当前的运行级别
chkconfig 给各个服务设置各自的运行级别
chkconfig --list 	#查看服务
chkconfig --level 3 network off  	#把network在3运行级别关闭
chkconfig --level 3 network on  	#把network在3运行级别打开
chkconfig # 设置完之后需要重新启动
systemctl # 管理指令
systemctl	[start|stop|restart|status]	服务名 	#基本语法
systemctl # 可以设置服务的自启动状态
systemctl  enable 	服务名	#设置服务开机自启动
systemctl  disable	服务名	#关闭服务开机自启动
systemctl is-enabled	服务名	#查询某个服务是否自启动
```

注意

1.要让服务自启动永久生效要使用disabled 和enabled命令，start和stop只能让服务临时生效

firewall指令

真正的生产环境防火墙是需要打开的

```cmd
firewall-cmd --permanent --add-port=端口号/协议		#打开端口
firewall-cmd --permanent --add-port=111/tcp
firewall-cmd --permanent --remove-port=端口号/协议	 #关闭端口
firewall-cmd --reload								 #重新载入，才能生效
firewall-cmd --query-port=端口/协议 				  #查询端口是否开放
```

top动态监控进程

```cmd
top [选项]
-d 秒数		#指定top命令每隔几秒跟新，默认是3秒
-i			#使top不显示任何闲置或者僵死进程
-p			#通过指定监控进程ID来仅仅监控某个进程的状态
p				#以cpu使用率排序
m				#以内存排序
n				#以pid排序
q				#退出top
输入k				#输入进程号杀死进程
输入u				#输入用户名查看哪个用户在使用
```

netstat	查看网络情况

```cmd
netstat	[选项]
-an		#按一定的排列顺序输出
-p		#显示哪个进程在调用
netstat -anp | grep sshd	#查看ssh服务的网络情况
```

使用vim修改文件报错，系统提示如下：

E37: No write since last change (add ! to override)

**故障原因：**

文件为只读文件，无法修改。

**解决办法：**

使用命令:w!强制存盘即可，在vim模式下，键入以下命令：

```cmd
:w！
```

存盘后在使用vim命令检查是否保存

## wsl使用

### WSL的安装

1. 控制面板->程序->启用或关闭 windows 功能，开启 Windows [虚拟化](https://www.zhihu.com/search?q=虚拟化&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A3217764764})和 Linux [子系统](https://www.zhihu.com/search?q=子系统&search_source=Entity&hybrid_search_source=Entity&hybrid_search_extra={"sourceType"%3A"answer"%2C"sourceId"%3A3217764764})(WSL2)以及Hyper-V

2. 就是在控制台安装wsl

   ```cmd
   wsl --install
   ```

3. WSL 安装Ubuntu,也可以安装其他的操作系统（通过Microsoft Store）

4. 在cmd控制窗口点击+打开已经安装好的系统

![1715932354854](C:\Users\104\AppData\Roaming\Typora\typora-user-images\1715932354854.png)

然后可以在系统里面进行一系列操作

### wsl的迁移

wsl默认安装在c盘，特别浪费空间，可以通过一下方式进行迁移到其他盘

- 查看虚拟机的状态，需要将虚拟机下停下来

```cmd
wsl -l -v  # 查看wsl虚拟机的名称与状态。
wsl --shutdown 虚拟机名称 # 使虚拟机停止运行
```

- 导出备份

```cmd
wsl --export Ubuntu D:\Ubuntu_WSL\Ubuntu.tar # 将其导出备份到D盘
wsl --unregister Ubuntu # 注销原有的虚拟机
wsl --import Ubuntu D:\Ubuntu_WSL D:\Ubuntu_WSL\Ubuntu.tar # 恢复备份
```

然后启动WSL，恢复正常

- 恢复默认用户

在CMD中，输入 虚拟机名称.exe config --default-user 原本用户名`

```cmd
Ubuntu config --default-user kevin
```

## shell编程

### shell简介

​	shell是一个命令行解释器，用户可以使用shell来启动，挂起甚至是编写一些程序。

​	脚本以#!/bin/bash开头

​	创建一个脚本输出hello world

```cmd
vim hello.sh
#!/bin/hello.sh
echo "hello world"
```

​	shell脚本执行方式一：

​		先给脚本赋权限，然后使用./xxx.sh执行

```cmd
chmod u+x hello.sh
./hello.sh					#相对路径
/root/shcodr/hello.sh 		#使用相对路径执行脚本
```

​	shell脚本执行方式二：

​	直接使用sh xxx.sh执行脚本

```cmd
sh hello.sh
```

### shell变量

​	**系统变量:**

​	HOME	​PWD	​SHELL ​USER

​	set	#查看所有的系统变量

​	**2.2.自定义变量**

​	定义变量 	变量名=值

​	撤销变量	unset 变量

​	声明静态变量：readonly 变量 #不能撤销

​	1.shell编程等号两侧不能有空格

​	2.将命令的返回值赋给变量

​		a.A=`date`    反引号   #运行里面的命令，并将结果返回给A

​		b.A=$(date)   		#运行里面的命令，并将结果返回给A

​	**环境变量**

 	export 变量名 =变量值

​	source 配置文件	

​	echo $(变量名)		#查询环境变量的值

```cmd
	export JAVA_HOME=/usr/local/java/jdk1.8.0_261
	export PATH=$JAVA_HOME/bin:$PATH
	source  /etc/profile
	echo #JAVA_HOME				#输出环境变量的值
```

shell脚本多行注释

```cmd
：<<!内容!
```

2.4.位置参数变量

