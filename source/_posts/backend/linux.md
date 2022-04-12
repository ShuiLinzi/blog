---
title: linux学习笔记
date: 2022-02-09
tags: 
- 操作系统
- linux
categories:
- "后端"
---
## 创建一个虚拟机
- 最多分配主机cpu*内核的总个数，不能超过，cpu和内核可以在任务管理器里面的资源监视器查看
  
- 软件选择里面选择gnome桌面，勾选开发工具，兼容性程序库，传统X windows系统的兼容性 
- 进行分区，包含/boot，/swap，/ 根分区，选择标准文件，ext4
- 网络和主机名，以太网打开，主机名可以选择修改，也可以不进行修改
- 视频里面的把安全政策(security poliy)关闭了
- 设置密码，也可以设置一个权限比较低的用户
- 网络连接的三种模式
   - 桥接模式：虚拟系统可以和外部系统通讯，但是容易造成IP冲突
  
   - NAT模式：网络地址转换模式，虚拟系统可以和外部系统通讯，而且不会造成IP冲突，nat模式，可以从虚拟机访问外部网络，但是外部网络无法访问到虚拟机
   - 主机模式：不和外部发生联系，是独立的系统
- 安装vmtools工具，可以使Linux虚拟机访问Windows系统里面的文件，在虚拟机窗口安装工具
## Linux目类详解
- /bin 是Binary的缩写，这个目录存放着最经常使用的命令
- /sbin s是super的缩写，这里存放是系统管理员使用的系统管理程序
- /home 存放普通用户的主目录，在Linux中每个用户都有一个自己的目录，一般该目录名是以用户的账号命名
- /root 该目录为系统管理员，也称作超级权限者的用户的主目录
- /lib 系统开机所需要的最基本的动态连接共享库，其作用类似于Windows里的DLL文件，几乎所有的应用程序都需要用到这些共享库
- /etc 所有系统管理所需要配置文件和子目录 my.conf
- /usr 这是一个非常重要的目类，用户的很多应用程序和文件都放在这个目录下，类似于Windows下的program files目录
- /boot 存放的是启动Linuxs时使用的一些核心文件，包括一些连接文件和镜像文件
- /dev 类似于Windows的设备管理器，把所有的硬件用文件的形式存储
- /media Linux系统会自动识别一些设备，例如u盘，光驱等等，当识别后，Linux会把内容挂载到这个目录下
- /mnt 系统提供该目录是为了让用户临时挂载别的文件系统，我们可以将外部存储挂载到/mnt/上，然后进入该目录就可以查看里面的内容了
- /opt 这是给主机额外安装软件所存放的目录
  
- /usr/local 这是另一个给主机安装软件所在的目录，一般是通过编译源码方式安装的程序
- /var 这个目录存放着不断扩充着的东西，习惯将经常被修改的目录放到这个目录下，包括各种日志文件
## vim的三种模式
- 正常模式：以vim打开一个档案就直接进入正常模式(这是默认的模式)，在这个模式中可以删除，复制粘贴等等
- 插入模式：按下i,I,o,O,a,A,r,R等任何一个字母才会进入编辑模式，一般来说按i即可

- 命令行模式：在这个模式中，可以提供你相关指令，完成读取，存盘，替换，离开vim等等
## 文件目录指令
- pwd 查看当前所在目录
-  ls -a 列出所有文件，包含隐藏文件(以点符号开头的文件是隐藏文件)
- ls -l 是列出文件的详细信息，以一行一行的列出来
- cd ~ 回到自己的家目录，比如你是root， 就会直接到/root目录
- cd ..返回到上级目录
- /是根目录，下面分为root和home两个分支
- mkdir [选项] 要创建的目录，常用选项 -p 创建多级目录
- rmdir 删除空目录，使用细节：rmdir删除的是空目录，如有目录下有东西是无法删除的，需要使用rm -rf进行删除要删除的目录
- touch指令创建空文件
- cp 文件 指定目录 ，如果是拷贝整个文件夹，用cp -r 文件 指定目录
- rm指令删除文件或者目录，基本语法 rm [选项] 要删除的文件或者目录，常用选项：-f递归删除整个文件夹，-f 强制删除且不提示
- mv指令 mv移动文件与目录或重命名，位于同一个目录就是重命名，不同目录就是移动
- cat 查看文件内容，-n会显示行号
- more指令是一个基于vi编辑器的文本过滤器，它以全屏幕的方式按页显示文本文件的内容，more指令常见快捷键如下
![image](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/image.fqdpymz3ib4.jpg)
- less 用于大文件，分页查看
- echo 输入内容到控制台

- />指令 和 >>指令 >输出重定向 >> 追加
- ln指令，软链接也称符号链接，类似于Windows里面的快捷方式，主要存放链接其他文件的路径
- history，查看已经执行过的历史命令，也可以执行历史命令
- date显示当前时间 date "日期相应的格式" date -s 设置时间
- find 查找文件，可以按用户名，按文件格式，按大小
- grep指令和管道符号 |,grep过滤查找，管道符"|",表示将前一个命令的处理结果输出传递给后面的命令处理
- gzip用来压缩文件，gunzip用于解压文件
- zip/unzip 压缩解压文件夹 zip -r 是压缩文件夹
- tar指令 是打包指令，最后打包的文件格式为.tar.gz的文件
  - tar [选项] XXX.tar.gz 打包的内容
  - 选项说明： -c 产生.tar打包文件
  - -v 显示详细信息
  - -f 指定压缩后的文件名
  
  - -z 打包同时压缩
  - -x 解压.tar文件
  - tar -zcvf xx.tar.gz /home/.. /..压缩文件的指令
  - tar -zxvf xx.tar.gz 解压文件到当前目录
  - tar -zxvf xx.tar.gz -C /.... 解压文件到指定目录

## Linux组基本介绍
在Linux中的每个用户必须属于一个组，不能独立于组外，在Linux中每个文件都有所有者，所在组和其他组的概念
- ls -ahl 查看文件的所有者

- 修改文件所有者 chown 用户名 文件名
- 组的创建 groupadd 组名
- 创建一个用户fox，并放入到monster组中 useradd -g monster fox
- 修改文件所在组 chgroup 组名 文件名
- 改变用户所在组，再添加用户时，可以指定将该用户添加到哪个组，也可以用root管理权限可以改变某个用户所在的组 usermod -g 新组名 用户名
## 权限的基本介绍
ls -l中显示的内容如下
-rwxrw-r-- 1 root root 1213 Feb 2 09:39 abc
0-9位说明
1. 第0位(-)确定文件类型(d,-,l,c,b)
  - -代表是普通的文件
  
  - l是链接，相当于Windows的快捷方式
  - d是目录，相当于Windows的文件夹
  - c是字符设备文件，例如鼠标，键盘
  - b是块设备，比如硬盘
2. 第1-3位(rwx)确定所有者(该文件所有者)拥有该文件的权限 ---User

3. 第4-6位(rw-)确定所属组(同用户组)拥有该文件的权限 ---Group
4. 第7-9(r--)位确定其他用户拥有该文件的权限 ---Other
5. rwx作用到文件
   - 其中[r]代表可读(read)，可以读取，查看
   - [w]代表可写，可以修改，但是不代表可以删除该文件，删除一个文件的前提条件是对该文件所在的目录有写权限，才能删除该文件
   - [x]代表可执行(execute)，可以被执行
6. rwx作用到目录
   - [r]代表可读(read)，可以读取，ls查看目录内容
   - [w]代表可写(write)，可以修改，对目录内创建+删除+重命名目录
   - [x]代表可执行(execute)，可以进入该目录，比如cd
7. 修改权限 chmod
   - 第一种方式：+，-，=变更权限 u所拥有者，g所拥有组 o其他人 a所有人
      - chmod u=rwx，g=rx，o=x 文件/目录
## crond任务调度
crond 进行定时任务的设置，任务调度是指系统在某个时间执行的特定的命令或者程序

任务调度分类：系统工作或者个别用户工作

基本语法：crontab [选项]

常用选项：-e 编辑crontab定时任务 -l 查询crontab定时任务 -r删除当前用户所有的crontab定时任务
具体应用
定时任务的时间表达式，五个占位符
![占位符](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/占位符.jpg)
## at定时任务
- 基本介绍
1. at命令是一次性定时计划任务，at的守护进程atd会以后台模式运行，检查作业队列来运行。
2. 默认情况下，atd守护进程每60s检查作业队列，有作业时，会检查作业运行时间，如果时间与当前时间匹配，则运行此作业
3. at命令是一次性定时计划任务，执行完一个任务后不再执行此任务了
4. 在使用at命令的时候，一定要保证atd进程的启动，可以使用相关指令来查看，ps -ef | grep atd //可以检测atd是否在后台运行
- at命令格式
at [选项] [时间] Ctrl + D 结束at命令的输入
![at命令选项](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/at命令选项.jpg)
atq命令来查看系统中有没有执行的工作任务
atrm删除已经设置好的任务 atrm [编号]
## 磁盘的添加挂载
### 如何增加一块磁盘
- 虚拟机添加硬盘
  - lsblk查看硬盘情况
  - lsblk -f 查看挂载点，分区id
![添加硬盘](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/添加硬盘.webp)
- 分区
  - 分区命令 fdisk /dev/sdb，开始对/sdb分区 m 显示命令列表，p 显示磁盘分区 同fdisk -l，n是新增分区，d是删除分区，w写入并且退出
  - 说明：开始分区后输入n，新增分区，然后选择p，分区类型为主分区。两次回车默认剩余全部空间，最后输入w写入分区并退出，若不保存退出输入q
- 格式化磁盘
  - 命令：mkfs -t ext4 /dev/sdb1，其中ext4是分区类型
- 挂载
   - 创建要挂载的目录
   - mount [分区目录] [挂载目录]
   - 卸载挂载 umount [分区目录]/[挂载目录](二选一)
   - *注意：用命令行挂载，重启后会失效*
- 设置可以自动挂载
  - 永久挂载：通过修改/etc/fstab实现挂载
  - 添加完成后，执行mount -a即可生效
## 磁盘情况查询
- df -h 查看磁盘分区使用情况
- 查询指定目录磁盘占用情况
   - du -h 查询指定目录磁盘占用情况，默认为当前目录
   - -s 指定目录占用大小汇总
   - -h 带计量单位
   - -a 含文件
   - --max-depth=1 子目录深度
   - -c 列出明细的同时，增加汇总值

## Linux网络配置原理
### NAT网络配置
![添加硬盘](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/添加硬盘.webp)
### 查看虚拟网络编辑器和修改IP地址
- 查看Windows环境中的VMnet8网络配置:ipconfig
- 查看Linux网络配置:ifconfig
### linux网络环境配置
- 第一种方式(自动获取)：登陆后，通过节目来设置自动获取ip，特点：Linux每次启动后会自动获取IP，缺点：每次自动获取的ip地址可能不一样
- 第二种方式(指定ip)：直接修改配置文件来指定IP，并且可以连接到外网
  - 编辑 vim /etc/sysconfig/network-scripts/ifcfg-ens33，要求：将ip地址配置为静态的
![默认网络设置](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/默认网络设置.webp)
![修改之后网络设置](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/修改之后网络设置.webp)

**注意：图片里面的应为GATEWAY，不是GATEWAT坑了自己一把(lll￢ω￢)**

![修改虚拟网卡](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/修改虚拟网卡.webp)

## 设置主机名和hosts映射
### 设置hosts映射
- Windows: 在C盘下的一个目录文件里面指定即可，案例： 192.168.200.130 linuxLearn
- Linux: 在/etc/hosts 文件指定，案例： 192.168.200.1 xxx
### 主机名解析过程分析(Hosts、DNS)
1. Hosts是什么
一个文本文件，用来记录IP和Hostname(主机名)的映射关系
2. DNS是什么
   - DNS，是Domain Name System的缩写，翻译过来就是域名系统
   - 是互联网上作为域名和IP地址相互映射的一个分布式数据库
## 显示系统执行的进程
### 基本介绍
ps命令是用来查看系统中，有哪些正在执行，以及他们的执行状况，可以不加任何参数
- ps -a显示当前终端的所有进程信息
- ps -u以用户的格式显示进程信息
- ps -x显示后台进程运行的参数
![进程详解](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/进程详解.webp)
- ps -ef是以全格式显示当前所有的进程
- -e 显示所有进程 -f 全格式
![参数解释](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/参数解释.webp)
### 终止进程kill和killall
kill [选项] 进程号 ：通过进程号杀死进程
kill all 进程名称 ： 通过进程名称杀死进程，支持通配符，这在系统因负载过大而变得很慢时很有用
kill -9 进程号 ：表示强迫进程立即停止
pstree 以树状的形式显示进程号
## 服务(service)管理
### 介绍
服务本质就是进程，但是是运行在后台的，通常都会监听某个端口，等待其他程序的请求，比如(mysqld,sshd,防火墙等)，因此我们也称为守护进程
### service管理指令
1. service服务名 [start|stop|restart|reload|status]
2. 在CentOS7.0以后，很多服务不再使用service，而是systemctl
3. service指令管理的服务在/etc/init.d 查看
### 服务的运行级别(runlevel):
Linux系统有七种运行级别：<font color=red >常用的级别3和5</font>
- 运行级別0:系统停机状态。系统默认运行级别不能设为0,否则不能正常启动

- 运行级别1:单用户工作状态，root权限，用于系统维护，禁止远程登陆
- 运行级别2:多用户状态(没有NFS),不支持网络
- 运行级别3:完全的多用户状态(有NFS),无界面，登陆后进入控制台命令行模式
- 运行级别4:系统未使用，保留
- 运行级别5:X11控制台，登陆后进入图形GUI模式
- 运行级別6:系统正常关闭并重启，默认运行级別不能设为6,否则不能正常启动

systemctl get-default 获取当前运行级别
systemctl set-default xxx 设置运行级别
### 开机流程          
![开机流程](https://cdn.jsdelivr.net/gh/ShuiLinzi/blog-image@master/后端/开机流程.webp)
### systemctl管理指令
- 基本语法：systemctl [start|stop|restart|status]服务名

- systemctl指令管理的服务在/usr/lib/systemd/ststem 查看
- systemctl设置服务的自启动状态
  - systemctl list-unit-files (查看服务开机启动状态，可以使用grep进行过滤)
  - systemctl enable 服务名(设置服务开机启动)
  - systemctl disable 服务名 (关闭服务开机启动)
  
  - systemctl is-enabled 服务名 (查询某个服务是否是自启动的)
- <font color =#8600FF>使用案例：查看当前防火墙的状况，关闭防火墙和重启防火墙</font>
  - 查看当前防火墙状况：systemctl status firewalled
  
  - 关闭/打开防火墙：systemctl stop/start firewalld
### firewall相关指令
在真正的生产环境，往往需要将防火墙打开，但是如果我们把防火墙打开，那么外部请求数据包就不能跟服务器监听端口通讯，此时需要打开指定端口
- firewall指令
  - netstat -anp 可以查看相关端口号和协议
  - 打开端口：firewall-cmd --permanent --add-port=端口号/协议
  - 关闭端口：firewall-cmd --premanent --remove-port=端口号/协议
  - 重新载入：firewall-cmd --reload
  - 查询端口是否开放：firewall-cmd --query-port=端口/协议
### 监控网络状态
- 查看系统网络情况netstat
## rpm包的管理
- rpm -qa 查询所安装的所有rpm软件包
- rpm -q 软件包名，查询软件包是否安装
- rpm -qi 软件包名 查询软件包信息
- rpm -ql 软件包名 查询软件包的文件
- rpm -qf 文件全路径名 查询文件所属的软件包
- rpm -e 软件名，删除软件
- rpm -ivh(i=install 安装;v=verbose 提示;h=hash 进度条) RPM包全路径名称

### yum
- yum list 列出所有能安装的软件目录
- yum install xxx 下载安装

## Shell
Shell是一个命令行解释器，他为用户提供了一个向Linux内核发送请求以便运行程序的界面系统程序，用户可以用Shell来启动，挂起，停止甚至是编写一些程序
### Shell脚本的执行方式
- 脚本格式要求
  - 脚本以#/bin/bash开头
  - 脚本需要有可执行权限
- 脚本常用的执行方式
  - 方式一：输入脚本的相对路径或者绝对路径，说明：首先要赋予helloworld.sh脚本 +x 权限，再执行脚本
  - 方式二：sh+脚本，不用赋予脚本 +x权限，直接执行即可
### Shell变量
- Shell变量介绍
  - Linux Shell中的变量分为：系统变量和用户自定义变量
  - 系统变量：$HOME,$PWD,$SHELL等等
  - 显示当前shell中所有变量:set
## webmin可视化工具
### 基本介绍
Webmin是功能强大的基于Web的Unix/Linux系统管理工具。管理员通过浏览器访问Webmin的各种管理功能并完成相应的管理操作。
