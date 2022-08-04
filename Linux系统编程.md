# Linux系统编程

**简介：** 这篇笔记是跟着邢文鹏老师的学习笔记，主要分为Linux的基础使用和系统编程两部分

***笔记目录***

1. Linux基本使用
   1. Linux基本命令
   2. Linux娱乐
   3. 编辑器 vim
   4. 编译器 Gnu工具链-gcc
   5. 调试器 GDB
   6. Makefile
2. 系统编程
   1. 文件I/O
   2. 文件系统
   3. 进程
      1. 进程控制原语
      2. 进程间通信
      3. 进程间关系
   4. 信号
   5. 线程
      1. 线程控制原语
      2. 线程同步机制
   6. 网络编程
      1. socket套接字
      2. TCP/IP/UDP
      3. 并发服务器开发
         1. 多进程并发
         2. 多线程并发
         3. 异步I/O
            1. epoll
            2. select
            3. poll
3. shell编程
   1. 正则表达式
4. 数据库

## 1. Linux基本使用

|Linux系统代表  |  |
|--|--|
| ubuntu | 人性，专注桌面应用的系统半年一个版本 |
| Red hat | 服务收费 RHEL |
| CentOS | 社区-开源免费，去掉商标和不开源的代码再编译 |
| kali | 前身是BT5，黑客和安全专家专用系统，内置很多安防工具 |
| Android | 操作系统核心基于Linux |

|开发环境|  |
|--|--|
| 编辑器 | vi，vim，emacs，gedit |
| 编译器 | gcc |
| 调试器 | gdb |
| 项目管理工具 | Makefile/make |

***关于终端的小操作***

- ctrl + shift + t：创建终端标签
- alt + 标签号：切换标签
- ctrl + shift + n：创建新终端

### 1.1. Linux基本命令

|命令| 功能 |
|--|--|
| tar | 归档多个文件到一个压缩包 |
| jobs | 显示所有当前作业及其状态 |
| kill | 终止程序 |
| ping | 检查与服务器的连接状态 |
| wget | 从Internet上下载文件 |
| uname | 打印计算机的信息 |
| top | 显示正在运行的进程以及每个进程使用多少cpu列表 |
| echo | 将一些数据移到文件中 |
| zip，unzip | 压缩到zip，从zip中提取 |
| hostname | 主机/网络的名称 |
| data | 返回当前时间 |
| umask [-p] -S [mode] | 指定用户创建文件时的掩码，mode和chmod的命令中的格式一样 |

|有关容量的||
|--|--|
| du catalog | 检查文件或目录占用多少空间 |
| du -hm | 以M为单位 |
| du -hb | 以字节为单位 |
| du -hk | 以K为单位 |
| df --block-size | 获取有关磁盘空间使用情况的报告，并以size为单位 |

| wc | 计算文件的byte数、字数、列数 |
|--|--|
| wc -c(或-bytes、-chars)| 只显示byte数 |
| wc -l(或-lines) | 只显示行数 |
| wc -w(或-words) | 只显示字数 |

| 创建删除 |  |
|--|--|
| cp file catalog| 将文件从当前目录复制到另一个目录 |
| mv file catalog | 移动文件（应用于重命名） |
| mkdir catalog | 创建一个新目录 |
| rmdir catalog | 删除目录（仅能删除空目录） |
| rm catalog | 删除目录及其中的内容 |
| touch file | 创建一个空白文件 |
| useradd，userdel | 创建删除用户 |

| 有关查找的 |  |
|--|--|
| pwd | 找到您当前所在工作目录的路径。（返回绝对路径） |
| cd catalog | 浏览Linux文件或目录 |
| cat catalog | 在标准输出上列出文件内容 |
| locate file | 定位文件 |
| find xx | 查找文件或目录，可以再指定目录中查找 |
| grep word text | 搜索指定文件中的所有文本 |
| head -n x file | 显示任何文本的第一行或自定义的前x行 |
| tail | 显示最后十行 |
| diff | 逐行比较两个文件的内容输出不匹配的行 |
| which | 查看指定命令所在路径 |
| history | 查看使用了的命令 |
| man | 查看某命令详细 |

|find| 根据文件名查找 |
|--|--|
| find ./*-name "*.avi" | 从当前目录下查找以.avi结尾的文件 |
| find /*-name "file*" | 从根目录下查找以file开始的文件 |

| grep | 根据文件内容查找 |
|--|--|
| grep "hello" ./* -R | 递归查找当前目录下文件中含有hello字符串的文件 |

| ls | 查看当前目录的内容 |
|--|--|
| ls -a | 列出隐藏文件（文件以·开头的文件） |
| ls -l | 列出文件的详细信息 |
| ls -R | 连同子目录中的内容一起列出 |

|od -t file | 指定数据的显示格式 |
|--|--|
| c | ASCII字符或反斜杠序列 |
| d | 有符号十进制数 |
| f | 浮点数 |
| o | 八进制 |
| u | 无符号十进制 |
| x | 十六进制 |

#### 1.1.1. 主键盘快捷键

| 功能 | 快捷键 | 助记 |
|--|--|--|
| 上 | Ctrl-p | previous |
| 下 |Ctrl-n  | next |
| 左 | Ctrl-b | backword |
| 右 | Ctrl-f | forward |
| Del | Ctrl-d | delete光标后面的 |
| Home | Ctrl-a | the first letter |
| End | Ctrl-e | end |
| Backspace | Backspace  | delete光标前面的 |

#### 1.1.2. Linux的目录结构

***它类似于一棵树，所有的目录全在根目录 “/” 下***

| 目录 | 功能 |
|--|--|
| bin | 系统可执行程序，如命令 |
| boot | 内核和启动程序，所有和启动相关的文件都保存在这里 |
| boot/grub | 引导器相关文件 |
| dev | 设备文件 |
| etc | 系统软件的启动和配置文件，系统在启动过程中需要读取的文件都在此目录下（LILO参数，用户账号密码） |
| home | 用户的主目录 |
| lib | 系统的程序库文件，最基本的动态链接共享库 |
| media | 挂载媒体设备，光驱 |
| mnt | 用户临时挂载别的文件系统 |
| opt | 可选的应用软件包 |
| proc | 系统内存的映射，可以通过它来获取系统信息 |
| sbin | 管理员系统程序 |
| selinux |  |
| srv |  |
| sys | udev用到的设备目录树，/sys反映你机器当前所接的设备 |
| tmp | 临时文件夹 |
| usr | 最大的目录，大多应用程序的文件都保存在此 |
| usr-bin | 应用程序 |
| usr-game | 游戏程序 |
| usr-include |  |
| usr-lib | 应用程序的库文件 |
| usr-lib64 |  |
| usr-local | 包含用户程序等 |
| usr-sbin | 管理员应用程序 |

***cd ·· ：表示返回上一级目录***

#### 1.1.3. 文件属性

| 文件属性 |  |
|--|--|
| whoami | 查看当前登录用户 |
| chomd | 更改文件和目录的读取、写入和执行权限 |
| chown | 更改文件或目录的所有权给指定用户和指定组 |
| chgrp | 更改文件所属的用户组 |
| sudo | 使用超级管理员权限 |

| chmod | 更改文件权限 |
|--|--|
|chmod u/g/o/a +/-/= rwx 文件|  |
|  | u：文件所有者 |
|  | g：所有者相同组成员 |
|  | o：其他用户 |
|  | a：所有用户 |
|  | +：增加权限 |
|  | -：撤销权限 |
|  | =：设定权限 |
| 数字法设定权限： |  |
|  |r权限数为4，w权限数为2，x权限数为1 。若是没有权限则为0 ，要是三个权限都有就是7（4+2+1）|
| 如：chmod u=rwx g=rx o=r filename | 就等同于：chmod u=7 g=5 o=4 filename |
|  | 简化一下就是：chmod 754 filename |
| 目录权限：| 递归所有目录加上相同权限，需要加上参数 -R 。如：chmod 777 d |

| chown | 更改文件或目录的所有权给指定用户和指定组 |
|--|--|
| -R | 递归的改变指定目录及其下的所有子目录和文件的拥有者 |
| -v | 显示chown命令所作的工作 |
| 注意： | chown必须特权用户才可以执行（sudo chown root：root file -R） |
|  | 一个文件的owner和ownering group是没有关联的 |

***使用 “ls -l” 命令显示的信息中，开头由十个字符构成的字符串，第一个字符表示文件类型***

| 文件类型 |  |
|--|--|
| - | 普通文件 |
| d | 目录 |
| l | 符号链接 |
| b | 块设备文件 |
| c | 字符设备文件 |
| s | socket文件，网络套接字 |
| p | 管道 |

***紧跟着的九个字符表示文件的访问权限，分三组，每组三类，第一组表示文件属主的权限，第二组表同组用户的权限，第三组表其他用户的权限。每一组的三个字符分别表示：对文件的读、写和执行的权限。***

| 权限 | 含义 |
|--|--|
| r | 读 |
| w | 写 |
| x | 可执行 |
| s | 当文件被执行时，把文件的UID或GID赋予执行进程的UID（用户ID）或GID（组ID） |
| t | 设置标志位 |
| - | 没有相应位置的权限 |

| 链接技术 | 当一个文件的硬链接数为零的时候，这个文件被删除 |
|--|--|
| 硬链接 | ln file1 file2 |
| 软链接 | ln -s file1 file2 |

#### 1.1.4. 安装和卸载软件

***apt-get（必须连网）***

|更新源服务器列表|
|--|
| sudo vim /etc/apt/sources.list |

| 命令 | 功能 |
|--|--|
| sudo apt-get update | 更新源（更新完服务器列表后需要更新下源） |
| sudo apt-get install package | 安装包 |
| sudo apt-get remove package | 删除包 |
| sudo apt-cache search package | 搜索软件包 |
| sudo apt-cache show package | 获取包的相关信息，如大小、版本、说明等 |
| sudo apt-get install package --reinstall | 重新安装包  |
| sudo apt-get -f install | 修复安装 |
| sudo apt-get remove package --purge | 删除包，包括配置文件等 |
| sudo apt-get build-dep package | 安装相关的编译环境 |
| sudo apt-get upgrade | 更新已安装的包 |
| sudo apt-get dist-upgrade | 升级系统 |
| sudo apt-cache depends package | 了解使用该包依赖哪些包 |
| sudo apt-cache rdepends package | 查看该包被哪些包依赖 |
| sudo apt-get source package | 下载该包的源代码 |
| sudo apt-get clean && sudo apt-get autoclean | 清理无用的包 |
| sudo apt-get check | 检查是否有损坏的依赖 |

***deb包安装***

| 命令 | 功能 |
|--|--|
| sudo dpkg -i xxx.deb | 安装deb软件包 |
| sudo dpkg -r xxx.deb | 删除软件包 |
| sudo dpkg -r --purge xxx.deb | 连同配置文件一起删除 |
| sudo dpkg -info xxx.deb | 查看软件包信息 |
| sudo dpkg -L xxx.deb | 查看文件拷贝详情 |
| sudo dpkg -l | 查看系统中已安装软件包信息 |
| sudo dpkg-reconfigure xxx | 重新配置软件 |

***原码安装***

1. 解压缩源代码包
2. cd dir
3. ./configure 检测文件是否缺失，创建Makefile，检测编译环境
4. make  编译源码，生成库和可执行程序
5. sudo make install 把库和可执行程序安装到系统路径下

#### 1.1.5. 磁盘管理

**挂载**
linux在没有图形化界面是不能直接操作U盘的
需要进行：

1. 检测存储设备名称：sudo fdisk -l
2. 挂载存储设备sdb1到挂载点/mnt目录：sudo mount /dev/sdb1 /mnt
3. 访问/mnt
4. 卸载/mnt ：sudo umount  /mnt

**拷贝**
拷贝光盘和文件（光盘的格式需要是标准的iso9660格式）

1. 拷贝光盘：dd if=/dev/cdrom of=cdrom.iso
2. 拷贝文件：dd if=sfile of=dfile
3. 创建一个100M的空文件：dd if=/dev/zero of=hello.txt bs=100M count=1

#### 1.1.6. 压缩包管理

**tar**
为文件和目录创建档案。利用tar命令用户可以为某一特定文件创建档案，使用tar命令时有必须的主选项：
| 选项 | 作用 |
|--|--|
| c | 创建新的档案文件（用在备份目录或文件） |
| r | 把要存档的文件追加到档案文件的末尾 |
| t | 列出档案文件的内容，查看已备份了哪些文件 |
| u | 更新文件，用新增的文件取代原备份文件，找不到就追加到备份文件的最后 |
| x | 从档案文件中释放文件（解包） |

也有非必须的辅选项：

| 选项 | 作用 |
|--|--|
| f | 使用档案文件或设备，通常必选 |
| k | 保存已经存在的文件 |
| m | 在还原文件时，把所有文件的修改时间设置为现在 |
| M | 创建多卷的档案文件，以便在几个磁盘中存放 |
| v | 详细报告tar处理的文件信息 |
| w | 每一步都要求确认 |
| z | 用gzip来压缩/解压缩文件（用它压缩也要用它解压缩） |
| j | 用bzip2来压缩/解压缩文件（同上） |

打包命令：

1. tar cvf dir.tar dir
2. tar xvf dir.tar dir
3. 打gz压缩包：tar zcvf dir.tar.gz dir
4. 解gz压缩包：tar zxvf dir.tar.gz
5. 打bz2压缩包：tar jcvf dir.tar.bz2 dir
6. 解bz2压缩包：tar jxvf dir.tar.bz2
7. 指定目录解压缩：tar zxvf dir.tar.gz -C ~/test

***rar***

1. 打包：rar a -r newdir dir
2. 解包：unrar x newdir.rar

***zip***

1. 打包：zip -r dir.zip dir
2. 解包：unzip dir.zip

#### 1.1.7. 进程管理

***who***

查看当前在线上的用户情况

tty1到tty6给字符窗口(黑窗口)预留，tty7给图形界面预留的
图形下的终端是pts/n命名

***ps***

监控后台进程的工作情况，部分选项如下：
| 选项 | 功能 |
|--|--|
| -e | 显示所有进程 |
| -f | 全格式 |
| -h | 不显示标题 |
| -l | 长格式 |
| -w | 宽输出 |
| a | 显示终端上的所有进程包括其他用户的进程 |
| r | 只显示正在运行的进程 |
| x | 显示没有控制终端的进程 |
|  |  |
| u、a、x | 最常用的三个参数 |
| aux、ajx、-Lf | 常用的三种组合 |

列出进程信息含义：
| 名称 | 含义 |
|--|--|
| USER | 用户名 |
| UID | 用户ID |
| PID | 进程ID |
| PPID | 父进程的进程ID |
| SID | 会话ID |
| %CPU | 进程的CPU占用率 |
| %MEM | 进程的内存占用率 |
| VSZ | 进程使用的虚存的大小（Virtual Size） |
| RSS | 进程使用的驻留集大小或者是实际内存大小 |
| TTY | 与进程关联的终端 |
| STAT | 进程的状态：使用字符表示 |
| START | 进程启动时间和日期 |
| TIME | 进程使用的总CPU时间 |
| COMMAND | 正在执行的命令行命令 |
| NI | 优先级（Nice） |
| PRI | 进程优先级编号（Priority） |
| WCHAN | 进程正在休眠的内核名称；该函数的名称是从/rot/system.map文件中获得的 |
| FLAGS | 与进程相关的数字标识 |

| STAT的状态码 |  |
|--|--|
| R | Runnable。运行，正在运行或在运行队列中等待 |
| S | Sleeping。睡眠，休眠中，受阻，等待某个条件的形成或接受到信号 |
| I | Idle。空闲 |
| Z | Zombie。僵死，进程已终止，但进程描述符存在，直到父进程调用wait4()系统调用后释放 |
| D | Uninterruptible sleep。不可中断，收到信号不唤醒和不可运行，进程必须等待直到有终端发生 |
| T | Terminate。停止，进程收到SIGSTOP、SIGSTP、SIGTIN、SIGTOU信号后停止运行 |
| P | 等待交换页 |
| W | has no resident pages。无驻留页，没有足够的记忆体分页可分配 |
| X | 死掉的进程 |
| < | 高优先级的进程 |
| N | 低优先级的进程 |
| L | Lock。内存锁页，有记忆体分页分配并缩在记忆体内 |
| s | 进程的领导者（在它之下有子进程） |
| l | 多进程的（使用CLONE_THREAD，类似NPTL pthreads） |
| + | 位于后台的进程组 |

***jobs、fg、bg***

用来显示当前shell下正在运行哪些作业

- cat运行
- 使用Ctrl + z挂起当前进程（Ctrl + c终止进程）
- 使用jobs显示所有的进程
- 使用fg把指定的挂起的进程运行起来
- 使用bg把指定的挂起的进程切换到后台运行

***kill***

向指定的进程发送信号

- 查看信号：kill -l
![前32个信号才是常用信号](MD/assert/Linux系统编程/1-1-7-kill.png)
- kill 编号 进程编号（大多数都是使进程停止的信号）

***env***

查看当前进程的环境变量

配置当前用户环境变量

- vim ~/.bashrc

配置系统环境变量：

- vim /etc/profile
- 配置系统环境变量，配置时需要有root权限
- 在文件末尾加上：export PATH=$PATH:新路径

***source***

打开一个脚本，并执行脚本的内容

```txt
source test.sh
等同于：
. test.sh
```

#### 1.1.8. 用户管理

##### 创建用户

- sudo useradd -s /bin/bash -g fittiger -d /home/fittiger -m fittiger
- -s：指定新用户登录时shell类型
- -g：组名
- -d：home目录
- -m：用户名
- sudo useradd -s /bin/bash -g group -G adm,root xwp
- -G：指定附属组，该组必须已经存在

##### 设置用户组

- sudo groupadd fittiger

##### 设置密码

- sudo passwd fittiger

##### 切换用户

- su 用户
- sudo su：切换到root用户

##### 删除用户

- sudo userdel -r 用户

#### 1.1.9. 网络管理

##### ifconfig

- 查看网卡信息：ifconfig
- 关闭网卡：sudo ifconfig eth0 down
- 开启网卡eth0：sudo ifconfig eth0 up
- 给eth0配置临时IP：sudo ifconfig eth0 IP

##### ping

- ping [选项] 主机名/IP地址

|选项| 含义 |
|--|--|
| -c | 数目，在发送指定数目的包后停止 |
| -d | 设置SO_DEBUG的选项 |
| -f | 大量且快速的送网络封包给一台机器，看他的回应 |
| -I（大写i） | 秒数，设定间隔几秒送一个网络封包给一台机器，预设值是一秒送一次 |
| -l | 次数，在指定次数内以最快的方式送封包数据到指定机器（只有超级用户才可以使用此选项） |
| -q | 不显示任何传送封包的信息，只显示最后的结果 |
| -r | 不经由网关，而直接送封包到一台机器，通常是查看本机的网络接口是否有问题 |
| -s | 字节数，指定发送的数据字节数，预设值是56，加上8字节的ICMP头，一共是64ICMP数据字节 |

##### netstat [选项]

显示网络连接、路由表和网络接口信息，可以让用户得知目前都有哪些网络连接正在工作。

| 选项 | 作用 |
|--|--|
| -a | 显示所有socket，包括正在监听的 |
| -c | 每隔一秒就重新显示一遍，知道用户中断它 |
| -i | 显示所有网络接口的信息，格式同“ifconfig -e” |
| -n | 以网络IP地址代替名称，显示出网络连接情形 |
| -r | 显示核心路由表，格式同“route -e” |
| -t | 显示TCP协议的连接情况 |
| -u | 显示UDP协议的连接情况 |
| -v | 显示正在进行的工作 |

##### nslookup name

查询一台机器IP地址和其对应的域名
给个域名找到IP

##### finger 用户名

显示用户的信息

#### 1.1.10. 常用服务器

##### ftp服务器

1. 安装vsftpd服务器：sudo apt-get install vsftpd
2. 配置vsftpd.conf文件：sudo vi /etc/vsftpd.conf
   - 添加设置：

```txt
anonymous_enable=YES     # 允许匿名用户访问
anon_root=/home/fittiger/ftp    # 匿名客户登录后所在的目录
no_anon_password=YES    # 匿名用户不需要密码
write_enable=YES               # 匿名用户可以写操作
anon_upload_enable=YES     # 匿名用户可以上传
anon_mkdir_write_enable=YES   # 匿名用户可以创建目录
```

3. 重启服务器，重新加载/etc/vsftpd.conf配置文件：sudo /etc/init.d/vsftpd restart
4. 测试上传功能，登录ftp服务器，进入anonymous目录
   - ftp IP
   - cd anonymous
5. 上传命令，把你当前目录下的文件上传到ftp服务器的anonymous目录：put somefile
6. 下载文件：get somefile

##### nfs服务器

1. 安装nfs服务器：sudo apt-get install nfs-kernel-server
2. 设置/etc/exports配置文件：sudo vi /etc/exports
   - 添加配置：/home/用户名/nfs *(rw,sync,no_root_squash)（就是添加了读写，同步以及不需用户登录）
3. 在用户目录下创建nfs目录：mkdir /home/用户名/nfs
4. 重启服务器，重新加载配置文件：sudo /etc/init.d/nfs-kernel-server restart
5. 在/home/用户名/nfs目录下创建测试文件hello：touch hello
6. 测试服务器，把服务器共享目录nfs挂载到/mnt节点：sudo mount -t nfs -o nolock -o tcp IP:/home/用户名/nfs  /mnt
7. 进入/mnt目录下可以看到hello文件表示构建
8. 卸载网络共享目录：sudo umount /mnt

##### ssh

1. 安装ssh服务器：sudo apt-get install openssh-server
2. 远程登录：ssh 用户名@IP

#### 1.1.11.其他命令

##### 终端翻页

- shift+pageup
- shift+pagedown

##### man

看每一个命令和系统函数的手册

- man read：查看read命令的man page
- man 2 read：查看read系统函数的man page（在第二个session中，表示为read（2））
- man -k read：以read为关键字查找相关的man page

##### clear 清屏（回到屏幕第一行，还可以往上翻）

快捷键Ctrl+l

##### alias

别名：alias [-p] name=value：给value起别名为name，命令行输入name，shell会自动翻译为value

-p显示当前定义的别名列表

##### echo

回显字符串

- echo [-n] 字符串

##### 关机重启

- poweroff：关闭电源
- shutdown -t 秒数 [-rknncfF] 时间 [警告讯息]
- reboot ：重启

| 选项 | 作用 |
|--|--|
| -t 秒数 | 设定在切换至不同的runlevel之前，警告和删除二讯号之间的延迟时间 |
| -k | 仅送出警告讯息文字，但不是真的要shutdown |
| -r | shutdown之后重新开机 |
| -h | shutdown之后关机 |
| -n | 不经过init，由shutdown指令本事来完成关机动作 |
| -f | 重新开机时，跳过fsck指令，不检查档案系统 |
| -F | 重新开机时，强迫做fsck检查 |
| -c | 将已经正在shutdown的动作取消 |

- uname -a：查看内核版本
- lsb_release -a：查看发行版信息
- free -m：查看内存空闲

### 1.2. Linux娱乐

### 1.3. 编辑器 vim

```text
它可以执行输出、删除、查找、替换、块操作等众多文本操作，vi没有菜单，只有命令。

vi的三种工作模式：
 命令模式：
   用户按下esc键，vi就会进入命令模式，用户可以输入各种合法的vi命令来管理自己的文档
 文本输入模式：
   只有在输入模式下，才可以对文本进行编辑。
   在命令模式下输入：插入命令i、附加命令a、打开命令o、修改命令c取代命令r或者替换命令s。都可以进入文本输入模式
 末行模式：
   也叫ex转义模式。用户在命令模式下可以按' ：' 键进入末行模式。vi会在窗口最后一行显示一个：作为末行模式的提示符

操作步骤（在终端操作）：
 创建文件：vi文件名 --> i进入编辑模式 --> 编辑文件 --> esc到命令模式 --> ：到末行模式 --> wq保存并退出
```

|进入编辑模式  |  |
|--|--|
|i和I  | i在光标前插入，I在行首插入 |
|a和A  | a在光标后插入，A在行末插入 |
|o和O  |o在光标所在下一行插入，O在光标所在上一行插入  |

| 进入命令模式 |  
|--|
| esc |

|进入末行模式：  |  |
|--|--|
| q | 退出 |
| w | 保存 |
| q！ | 强制退出，不保存 |
| qw！ | 强制退出并保存 |
|！  | 强制的意思，可以不加 |

|移动光标命令  |  |
|--|--|
| h | 光标向左移动 |
| j | 光标向下移动 |
| k | 光标向上移动 |
| I | 光标向右移动 |
|H、M、L  | 光标移动到可见屏幕的第一行、中间行、最后一行 |
| ^和$ | 移动到行首、行末 |
| G和gg | 移动到文档的最后一行、第一行 |
| ctrl+f、ctrl+b | 向前翻屏、向后翻屏 |
|ctrl+d、ctrl+u  |  向前半屏、向后半屏|
| { 和 } | 向上移动一段，向后移动一段 |
| w和b |w向前移动一个单词、b向后移动一个单词  |

|删除命令  |  |
|--|--|
| X和x | x删除光标所在字符，X删除光标前一个字符，包括光标所在字符 |
| dd和n dd | dd删除所在行，n dd删除n行 |
| d0和D | d0删除光标前文本行所有内容。D删除光标后文本行所有内容，包括光标位置字符 |
|dw  | 删除光标所在位置的字，包括光标所在位置字符 |

| 撤销命令 |  |
|--|--|
| u |一步步撤销  |
|ctrl+r  |反撤销（重做）  |

| 重复命令 |  |
|--|--|
| . | 重复执行上一次操作的命令 |

| 移动命令 |  |
|--|--|
| >> | 文本行向右移动 |
| << |文本行向左移动  |

| 复制粘贴 |  |
|--|--|
| yy、n yy、y$ |yy复制当前行，n yy复制n行，y$复制当前光标至行尾  |
|p  |在光标所在位置向下开辟一行粘贴  |

|可视模式|  |
|--|--|
| v | 按字符移动选中文本 |
| V | 按行移动选中文本 |
|  | 可视模式可以和d，y，<<，>>配合使用 |

| 分屏 |  |
|--|--|
| 执行shell下命令 | 末行模式里输入！，后面跟命令 |
| 启动分屏 | n是分屏数 |
|  | vim -On file1，file2... ：垂直分屏|
|  | vim -on file1，file2...：水平分屏 |
| 关闭分屏 |  |
|  | ctrl+w c ：关闭当前窗口 |
|  | ctrl+w q：如果当前窗口为最后一个就退出vim |
| 在编辑中分屏 |  |
|  | sp：上下分屏，后面可以跟文件名 |
|  | ctrl+w s：上下分割当前打开的文件 |
|  | vsp：左右分屏，后面可以跟文件名 |
|  | ctrl+w v：左右分割当前打开的文件 |
| 移动光标 |  |
| ctrl+w k | 把光标移动到上面的屏 |
| ctrl+w j | 把光标移动到下面的屏 |
| ctrl+w l | 把光标移动到右面的屏 |
| ctrl+w h | 把光标移动到左面的屏 |
| ctrl+w w | 把光标移动到下一个屏 |
| 移动屏幕 |  |
| ctrl+w K | 向上移动 |
| ctrl+w J | 向下移动 |
| ctrl+w L | 向右移动 |
| ctrl+w H | 向左移动 |
| 屏幕尺寸 |  |
| ctrl+w + | 增加高度 |
| ctrl+w - | 减少高度 |
| ctrl+w = | 让所有屏幕高度一致 |
| ctrl+w > | 左加宽度 |
| ctrl+w < | 右加宽度 |
| ctrl+w n> | 左加n倍宽度 |
| ctrl+w n< | 右加n倍宽度 |

| 查找替换 |  |
|--|--|
| 命令模式 |  |
| r和R | r替换当前字符，R替换光标后的字符 |
| / + str | 找到字符串str，n查找下一个，N查找上一个 |
| 查看 Man Page | 光标移动到函数上，使用K查看，或者3K查看第三章的Man Page |
| 查看宏定义 | [-d：可以查看宏定义，必须先包含此宏定义的头文件 |
| 排版 | gg=G：代码自动缩进排版 |
|  |  |
| 末行模式 |  |
| %s/abc/123/g | 将文件中所有abc替换成123 |
| 1,10s/abc/123/g | 将第一行至第十行之间的abc替换成123 |

***配置适合自己的vim编辑器***

```txt
1.找到配置文件存放的目录：/etc/vim
2.输入：sudo vi vimrc 进行配置配置文件
3.对文件进行配置，在网上就能找到
```

### 1.4. 编译器 Gnu工具链-gcc

***gcc***

| gcc |  |
|--|--|
| -I | 指定头文件目录（后面不加空格） |
| -g | 包含调试信息 |
| -c | 只编译，生成.o文件，不进行链接 |
| -On（n：0~3） | 编译优化，n越大优化的越多 |
| -Wall | 提示更多警告信息 |
| -D | 编译时宏定义（后面不加空格） |
| -E | 生成预处理文件 |
| -M | 生成.c文件与头文件依赖关系以用于Makefile，包括系统库的头文件 |
| -MM | 生成.c文件与头文件依赖关系以用于Makefile，不包括系统库的头文件 |

***toolchain***

| |  |
|--|--|
| gcc | 编辑器 |
| glibc | 该库实现Linux系统函数，例如open、read等，也实现标准c语言库，如printf等。几乎所有应用程序都需要安装与glibc链接 |

```txt
binutils一组用于编译、链接、汇编和其他调试目的的程序，包括ar、as、ld、nm、objcopy、objdump等
```

| toolchain中几种主要工具的作用 |  |
|--|--|
| ar | 打包生成静态库 |
| as | 汇编器 |
| ld | 链接器。gcc完成链接步骤中，其实是gcc调用链接器ld，将用户编译生成的目标文件连同系统的libc启动代码链接在一起形成最终的可执行文件 |
| nm | 查看目标文件中的符号（全局变量，全局符号等） |
| objcopy | 将原目标文件中的内容复制到新的目标文件中，可以通过不同的命令选项调整目标文件的格式，比如去除某些ELF文件头 |
| objdump | 用于生成反汇编文件，主要依赖objcopy实现，a.out编译时需要-g，objdump -dSsx a.out > file |
| ranlib | 为静态库文件创建索引相当于ar命令的s选项 |
| readelf | 解读ELF文件头 |

#### 程序库

```txt
就是包含了数据和执行码的文件。其不能单独执行，可以作为其他执行程序的一部分来完成某些功能，库的存在可以使得程序模块化，
可以加快程序的再编译，可以实现代码重用，可以使得程序便于升级。

程序库分为静态库和共享库
```

***静态库***

```txt
是在可执行程序运行前就已经加入到执行码中，成为执行程序的一部分。

静态库可以认为是一些目标代码的集合，按照习惯，一般按照.a作为文件后缀名。

使用ar(archiver)命令可以创建静态库。

创建一个静态库：
 ar rcs libmylib.a file1.o\
 rcs：创建一个索引
 libmylib：库名，一般以lib开头
```

***共享库***

```txt
是在执行程序启动时加载到执行程序中，可以被多个执行程序共享使用。

共享库有比较明显的优点：库是独立的，便于维护和更新

创建一个共享库：
 1.使用-fPIC或-fpic创建目标文件
 2.然后使用以下格式来创建共享库：
  gcc -share -Wl, -soname,your_soname -o library_name file_list library_list

创建案例：
 gcc -fPIC -c a.c   # 先生成.o文件
 gcc -fPIC -c b.c
 gcc -shared -Wl -o libmyab.so a.o b.o   # 再创建共享库
 # .so 共享库的后缀

 gcc -fPIC -c a.c 
 gcc -fPIC -c b.c
 gcc -shared -Wl,-soname, libmyab.so.1 -o libmyab.so.1.0.1 a.o b.o
 # 后面的1.0.1是库的版本号
```

| 三个库名 |  |
|--|--|
| real name | libmylib.so.1.10：小版本号，小版本差一些也可以运行 |
| so name | libmylib.so.1：这个是主版本号，主版本号一致才可以通用 |
| link name | libmylib.so ：不显示出版本来，是为了链接使用的，方便Makefile |
|| 这三者之间的关系：后两个都是链接指向第一个，第一个才是真正调用的 |

```txt
1.修改/etc/ld.so.conf：sudo vi /etc/ld.so.conf
 向里面添加你的共享库路径
2.更新查找共享库的路径
 sudo ldconfig -v
```

### 1.5. 调试器 GDB

### 1.6. Makefile

## 2. 系统编程

### 2.1. 文件I/O

#### c标准函数与系统函数

![C标准函数与系统函数](MD/assert/Linux系统编程/2-1-C标准函数与系统函数.png)

***I/O缓冲区***

每个文件流都有个缓冲区buffer，默认大小8192B
![IO缓冲区](MD/assert/Linux系统编程/2-1-IO缓冲区.png)
c标准函数的跨平台性要好，但是性能方面不如linuxAPI

#### PCB（进程控制块）

***task_struct结构体***

位置：/usr/src/linux-headers/include/linux/sched.h

***文件描述符***

```txt
一个文件默认打开三个文件描述符
 STDIN_FILENO  0
 STDOUT_FILENO  1
 STDERR_FILENO  2
```

![PCB进程控制块](MD/assert/Linux系统编程/2-1-PCB进程控制块.png)

#### open函数

```txt
int open(const char *pathname, int flags);
int open(const char *pathname, int flags, mode_t mode);
```

| 必选参数 | 访问模式 |
|--|--|
| O_RDONLY | 只读 |
| O_WRONLY | 只写 |
| O_RDWR | 读写 |

| 可选参数 | 可以同时指定另个或多个 |
|--|--|
| O_APPEND | 追加 |
| O_CREAT | 不存在文件则创建，需要加上参数mode，表示该文件的访问权限 |
| O_EXCL | 如果指定了O_CREAT，并且文件存在，则出错返回 |
| O_TRUNC | 如果文件存在并是可写状态打开，则将其长度截断为0字节 |
| O_NONBLOCK | 对于设备文件，以此方式打开可以做非阻塞I/O |

**文件最大打开数：1024（默认值，可改变）**
**查看系统用户所有限制值：ulimit -a**
**修改系统用户文件打开限制值：ulimit -n 4096**

#### read/write函数

```txt
ssize_t read(int fd, void *buf, size_t count);
 成功返回读到的字节数
 读到文件末尾返回0
 读失败返回-1
ssize_t write(int fd, const void *buf, size_t count);
```

#### 阻塞和非阻塞

读常规文件是不会阻塞的，read会在一定的时间内返回。从终端或网络读则不一定，有可能会一直阻塞。(写也一样)

***阻塞：当进程调用一个阻塞的系统函数时，该进程被置于睡眠（sleep）状态，这时内核调度其他进程运行，知道该进程的等待的事件发生了，它才有可能继续运行。（与睡眠状态相对应的是运行状态）***

linux内核中处于运行状态的进程分为两类：

- 正在被调度运行
- 就绪状态

***阻塞读终端***

```cpp
#include <unistd.h>
#include <stdlib.h>

int main(void)
{
 char buf[10];
 int n;
 n = read(STDIN_FILENO, buf, 10);
 if (n < 0) {
  perror("read STDIN_FILENO");
  exit(1);
 }
 write(STDOUT_FILENO, buf, n);
 return 0;
}

// 轮询(Poll),调用者只查询一下，并不会在这里阻塞等死，可以同时监听多个设备

while(1) {
 非阻塞read(设备1);
 if(设备1有数据到达)
  处理数据;

 非阻塞read(设备2);
 if(设备2有数据到达)
  处理数据;

 ...

 sleep(n); // 休息一段时间
}

```

***非阻塞读终端***

```cpp
// 非阻塞读终端
#include <stdio.h>
#include <stdlib.h>
#include <sys/types.h>
#include <sys/stat.h>
#include <unistd.h>
#include <string.h>
#include <stdlib.h>
#include <errno.h>
#include <fcntl.h>

#define MSG_TRY "try again\n"


int main(void)
{
 char buf[10];
 int fd, n;

 fd = open("/dev/tty", O_RDONLY|O_NONBLOCK); // 重新打开终端 // O_NONBLOCK：非阻塞标志，为打开的文件(终端)设置非阻塞标志
 if(fd<0){ // 打开失败提示失败信息
  perror("open /dev/tty");
  exit(1);
 }
 tryagain: // 如果打开成功就开始非阻塞读，如果读失败就一直重复读，成功就结束
 n = read(fd, buf, 10);
 if(n<0){
  if(errno == EAGAIN){
   sleep(1);
   write(STDOUT_FILENO, MSG_TRY, strlen(MSG_TRY));
   goto tryagain;
  }
  perror("read /dev/tty");
  exit(1);
 }
 write(STDOUT_FILENO,buf,n);
 close(fd);

 return 0;
}
```

```cpp
// 非阻塞读终端和等待超时
#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <string.h>
#include <stdlib.h>
#define MSG_TRY "try again\n"
#define MSG_TIMEOUT "timeout\n"

int main(void)
{
 char buf[10];
 int fd, n, i;
 
 fd = open("/dev/tty", O_RDONLY|O_NONBLOCK);
 
 if(fd<0) {
  perror("open /dev/tty");
  exit(1);
 }
 
 for(i=0; i<5; i++) { // 循环等待五次
  n = read(fd, buf, 10);
  if(n>=0)
   break;
  if(errno!=EAGAIN) {
   perror("read /dev/tty");
   exit(1);
  }
  sleep(1);
  write(STDOUT_FILENO, MSG_TRY, strlen(MSG_TRY));
 }
 
 if(i==5) // 如果五次都没有读到数据，就提示等待超时
  write(STDOUT_FILENO, MSG_TIMEOUT, strlen(MSG_TIMEOUT));
 else // 读到数据了，将数据写入进去
  write(STDOUT_FILENO, buf, n);
 close(fd);
 
 return 0;
}
```

#### lseek

lseek和标准I/O库的fseek函数类似，可以移动当前读写位置(偏移量)

偏移量允许超过文件末尾，这种情况下对该文件的下一次写操作将延长文件，中间空洞的部分读出来都是0。

```cpp
#include <sys/types.h>
#include <unistd.h>

off_t lseek(int fd, off_t offset, int whence);

// 若lseek成功执行，则返回新的偏移量
off_t currpos;
currpos = lseek(fd, 0, SEEK_CUR);
```

```cpp
#include <sys/types.h>
#include <sys/stat.h>
#include <fcntl.h>
#include <unistd.h>
#include <errno.h>
#include <stdlib.h>
#include <stdio.h>

int main(void)
{
   int fd = open("abc", O_RDWR);
   if(fd<0) {
      perror("open abc");
      exit(-1);
   }

   // 1.拓展文件
   // 扩展一个文件，一定要有一次写操作
   lseek(fd, 0x1000, SEEK_SET);
   write(fd, "a");

   close(fd);
   fd = open("hello", O_RDWR);
   if(fd<0) {
      perror("open hello");
      exit(-1);
   }
   // 2.获取文件大小
   // 指针位置移到到文件末尾，偏移量为0，返回值就是文件的大小
   printf("hello size = %d\n", lseek(fd, 0, SEEK_END));
   close(fd);

   // 3.它真正的工作是移动打开的文件的指针位置
   return 0;
}
```

#### fcntl

获取或设置文件访问控制属性

数改变一个已打开的文件的属性，可以重新设置读、写、追加、非阻塞等标志（这些标志称为File Status Flag），而不必重新open文件。

```cpp
#include <unistd.h>
#include <fcntl.h>
int fcntl(int fd, int cmd);
int fcntl(int fd, int cmd, long arg);
int fcntl(int fd, int cmd, struct flock *lock);


#include <unistd.h>
#include <fcntl.h>
#include <errno.h>
#include <string.h>
#include <stdlib.h>

#define MSG_TRY "try again\n"

int main(void)
{
   char buf[10];
   int n;
   int flags;
   // 它改变一个已经打开文件的访问控制属性
   flags = fcntl(STDIN_FILENO, F_GETFL);
   flags |= O_NONBLOCK;
   if (fcntl(STDIN_FILENO, F_SETFL, flags) == -1) {
      perror("fcntl");
      exit(1);
   }

tryagain:
   n = read(STDIN_FILENO, buf, 10);
   if (n < 0) {
      if (errno == EAGAIN) {
         sleep(1);
         write(STDOUT_FILENO, MSG_TRY, strlen(MSG_TRY));
         goto tryagain;
      }
      perror("read stdin");
      exit(1);
   }

   write(STDOUT_FILENO, buf, n);
   return 0;
}
```

#### ioctl

设置和获取设备文件的物理特性时使用.

用于向设备发控制和配置命令，有些命令也需要读写一些数据

```cpp
#include <sys/ioctl.h>

int ioctl(int d, int request, ...);
// d是某个设备的文件描述符。request是ioctl的命令，可变参数取决于request，通常是一个指向变量或结构体的指针。
// 若出错则返回-1，若成功则返回其他值，返回值也是取决于request。
```

```cpp
// 使用TIOCGWINSZ命令获得终端设备的窗口大小。
#include <stdio.h>
#include <stdlib.h>
#include <unistd.h>
#include <sys/ioctl.h>

int main(void)
{
   struct winsize size; // 终端大小
   
   if (isatty(STDOUT_FILENO) == 0) // isatty:根据文件描述符判断是否是终端文件
      exit(1);
   
   if(ioctl(STDOUT_FILENO, TIOCGWINSZ, &size)<0) {
      perror("ioctl TIOCGWINSZ error");
      exit(1);
   }
   
   printf("%d rows, %d columns\n", size.ws_row, size.ws_col);
   return 0;
}
```

### 2.2. 文件系统

#### 2.2.1. ext2文件系统

![文件系统](MD/assert/Linux系统编程/2-2-1-文件系统.png)

![文件系统](MD/assert/Linux系统编程/2-2-1-ext2文件系统.png)

- Boot Block:启动块，记录磁盘的分区情况，每个区的大小以及起始位置，记录里面装的什么系统...大小为1K
- Block 1 .... Block n:大小为4096Bytes(就是8个磁盘扇区)
- Block Group:文件系统将分区划分为若干大小相同的块组。
  - Super Block(超级块):***描述整个分区的文件系统信息***(块大小、文件系统版本号、上次mount的时间...超级块在每个块组的开头都有一份拷贝)
  - GDT(块组描述符表):由一系列块组描述符组成，分区有多少块组就有多少块组描述符。存储***一个块组的描述信息***(inode表开始位、数据块开始位、空闲的inode和数据块数量)***Super Block和GDT在每个块组的开头都有一份拷贝***
  - Block Bitmap(块位图):***用来描述整个块组中哪些块已用，哪些块仍空闲***，它本身占一个块，每个bit表示块组中的一个块，通过0、1标识是否被使用。
  - inode Bitmap(inode位图):类似块位图，标识inode是否使用
  - inode Table(inode表):每个文件一个inode，一个块组的全部inode构成了inode表，用来存储 ***文件描述信息(文件类型(常规、目录、符号链接)、权限、文件大小、创建/修改/访问时间等)*** inode表占多少块在格式化的时候就决定了，根据要存放的文件的大小来决定inode表的大小。
  - Data Block(数据块):
    - 常规文件：数据存储在数据块中
    - 目录：目录下所有的文件名以及目录名存放在数据块中
    - 符号链接：目标路径名较短就直接保存在inode中以便快速查找，如果目标路径名较长就分配一个数据块来保存。
    - 设备文件、FIFO和socket等特殊文件没有数据块，设备文件的主设备号和次设备号保存在inode中。

***目录中记录项文件类型***

|编码|文件类型|
|--|--|
|0|Unknown|
|1|Regular file|
|2|Directory|
|3|Character device|
|4|Block device|
|5|Named pipe|
|6|Socket|
|7|Symbolic link|

***数据块寻址***

![数据块寻址](MD/assert/Linux系统编程/2-2-1-数据块寻址.png)

- 直接寻址：
  - 直接寻址，它的块里面存放的就是数据本身
- 间接寻址
  - 间接寻址，它的块里面存放的是指针，指向另一个地址，一个Block里面可以存放Block/4个指针

#### 2.2.2. stat

### 2.3. 进程

#### 2.3.1. 进程控制原语

#### 2.3.2. 进程间通信

#### 2.3.3. 进程间关系

### 2.4. 信号

### 2.5. 线程

#### 2.5.1. 线程控制原语

#### 2.5.2. 线程同步机制

### 2.6. 网络编程

#### 2.6.1. socket套接字

#### 2.6.2. TCP/IP/UDP

#### 2.6.3. 并发服务器开发

##### 2.6.3.1. 多进程并发

##### 2.6.3.2. 多线程并发

##### 2.6.3.3. 异步I/O

###### 2.6.3.3.1. epoll

###### 2.6.3.3.2. select

###### 2.6.3.3.3. poll

## 3. shell编程

### 3.1. 正则表达式

### 3.2. 错误处理机制

***errno***

它是一个int型全局变量，是设置出错编号，perror是通过出错编号来打印出错信息。

vi /usr/include/asm-generic/errno-base.h

```txt
错误编号：

#define EPERM 1 /* Operation not permitted */
#define ENOENT 2 /* No such file or directory */
#define ESRCH 3 /* No such process */
#define EINTR 4 /* Interrupted system call */
#define EIO 5 /* I/O error */
#define ENXIO 6 /* No such device or address */
#define E2BIG 7 /* Argument list too long */
#define ENOEXEC 8 /* Exec format error */
#define EBADF 9 /* Bad file number */
#define ECHILD 10 /* No child processes */
#define EAGAIN 11 /* Try again */
#define ENOMEM 12 /* Out of memory */
#define EACCES 13 /* Permission denied */
#define EFAULT 14 /* Bad address */
#define ENOTBLK 15 /* Block device required */
#define EBUSY 16 /* Device or resource busy */
#define EEXIST 17 /* File exists */
#define EXDEV 18 /* Cross-device link */
#define ENODEV 19 /* No such device */
#define ENOTDIR 20 /* Not a directory */
#define EISDIR 21 /* Is a directory */
#define EINVAL 22 /* Invalid argument */
#define ENFILE 23 /* File table overflow */
#define EMFILE 24 /* Too many open files */
#define ENOTTY 25 /* Not a typewriter */
#define ETXTBSY 26 /* Text file busy */
#define EFBIG 27 /* File too large */
#define ENOSPC 28 /* No space left on device */
#define ESPIPE 29 /* Illegal seek */
#define EROFS 30 /* Read-only file system */
#define EMLINK 31 /* Too many links */
#define EPIPE 32 /* Broken pipe */
#define EDOM 33 /* Math argument out of domain of func */
#define ERANGE 34 /* Math result not representable */
```

***perror***

会输出自己设定的出错信息，加上errno所指向的错误信息。

```cpp
#include <stdio.h>

void perror(const char *s);

#include <errno.h>

const char *sys_errlist[];
int sys_nerr;
int errno;
```

***strerror***

```cpp
#include <string.h>

char *strerror(int errnum);

int strerror_r(int errnum, char *buf, size_t buflen);
/* XSI-compliant */

char *strerror_r(int errnum, char *buf, size_t buflen);
/* GNU-specific *
```

## 4. 数据库
