## <font style="color:rgb(0, 0, 0);">Linux简介</font>
<font style="color:rgb(51, 51, 51);">Linux 内核最初只是由芬兰人林纳斯·托瓦兹（Linus Torvalds）在赫尔辛基大学上学时出于个人爱好而编写的。</font>

<font style="color:rgb(51, 51, 51);">Linux 是一套免费使用和自由传播的类 Unix 操作系统，是一个基于 POSIX 和 UNIX 的多用户、多任务、支持多线程和多 CPU 的操作系统。</font>

<font style="color:rgb(51, 51, 51);">Linux 能运行主要的 UNIX 工具软件、应用程序和网络协议。它支持 32 位和 64 位硬件。Linux 继承了 Unix 以网络为核心的设计思想，是一个性能稳定的多用户网络操作系统。</font>



linux仿真环境：

+ **<font style="color:rgb(85, 85, 85);">jslinux</font>**<font style="color:rgb(85, 85, 85);">:：</font>[http://bellard.org/jslinux/](http://bellard.org/jslinux/)
+ **<font style="color:rgb(85, 85, 85);">JS/UIX</font>**<font style="color:rgb(85, 85, 85);"> ：</font>[http://www.masswerk.at/jsuix/index.html](http://www.masswerk.at/jsuix/index.html)
+ <font style="color:rgb(85, 85, 85);">cb.vu：</font>[http://cb.vu/](http://cb.vu/)



## Linux主要命令
### <font style="color:rgb(31, 35, 40);">Linux 文件目录管理</font>
#### <font style="color:rgb(31, 35, 40);">Linux 目录结构</font>
![](https://cdn.nlark.com/yuque/0/2024/png/26411187/1717654929307-fe5a8dda-a238-44b8-9834-1021b0087491.png)



#### <font style="color:rgb(31, 35, 40);">Linux 文件属性</font>
<font style="color:rgb(31, 35, 40);">Linux 系统是一种典型的多用户系统，不同的用户处于不同的地位，拥有不同的权限。为了保护系统的安全性，Linux 系统对不同的用户访问同一文件（包括目录文件）的权限做了不同的规定。 在 Linux 中我们可以使用 ll 或者 ls –l 命令来显示一个文件的属性以及文件所属的用户和组</font>

```java
➜  /tmp ll
total 0
drwxr-xr-x@ 2 evan  wheel    64B  6  7 15:15 1
-rw-r--r--@ 1 evan  wheel     0B  6  7 15:15 1.txt
```

<font style="color:rgb(31, 35, 40);">接下来的字符中，以三个为一组，且均为『rwx』 的三个参数的组合。其中，</font><font style="color:rgb(31, 35, 40);">r</font><font style="color:rgb(31, 35, 40);"> 代表可读(read)、</font><font style="color:rgb(31, 35, 40);">w</font><font style="color:rgb(31, 35, 40);"> 代表可写(write)、</font><font style="color:rgb(31, 35, 40);">x</font><font style="color:rgb(31, 35, 40);"> 代表可执行(execute)。 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号 </font><font style="color:rgb(31, 35, 40);">-</font><font style="color:rgb(31, 35, 40);"> 而已。</font>

![](https://cdn.nlark.com/yuque/0/2024/png/26411187/1717744636121-e4502299-273c-428b-ad70-de6401f41257.png)



+ <font style="color:rgb(31, 35, 40);">对于文件来说，它都有一个特定的拥有者，也就是对该文件具有所有权的用户。</font>
+ <font style="color:rgb(31, 35, 40);">同时，在 Linux 系统中，用户是按组分类的，一个用户属于一个或多个组。</font>
+ <font style="color:rgb(31, 35, 40);">文件拥有者以外的用户又可以分为文件拥有者的同组用户和其他用户。</font>

<font style="color:rgb(31, 35, 40);"></font>

#### <font style="color:rgb(31, 35, 40);">文件目录命令</font>
`cd`, `ls`, `pwd`, `mkdir`,  `tree`, `touch`, `ln`, `chmod`, `chown`, `find`, `cp`, `scp`, `mv`, `rm`





### Linux文件查看编辑
+ 连接文件并打印到标准输出设备 - 使用 cat
+ 显示指定文件的开头若干行 - 使用 head
+ 显示指定文件的末尾若干行，常用于实时打印日志文件内容 - 使用 tail
+ 显示文件内容，每次显示一屏 - 使用 more
+ 显示文件内容，每次显示一屏 - 使用 less
+ 自动编辑一个或多个文件；简化对文件的反复操作；编写转换程序等 - 使用 sed, awk
+ 文本编辑器 - 使用 vi/vim
+ 使用正则表达式搜索文本，并把匹配的行打印出来 - 使用 grep

<font style="color:rgb(31, 35, 40);"></font>

### <font style="color:rgb(31, 35, 40);">Linux 文件压缩和解压</font>
#### tar
```java
tar -cvf 1.tar 1.txt            # 仅打包，不压缩
tar -zcvf 1.tar.gz 1.txt        # 打包后，以 gzip 压缩
tar -jcvf 1.tar.bz2 1.txt       # 打包后，以 bzip2 压缩

tar -ztvf 1.tar.gz                    # 查阅上述 tar 包内有哪些文件
tar -zxvf 1.tar.gz                    # 将 tar 包解压缩
tar -zxvf 1.tar.gz       # 只将 tar 内的部分文件解压出来
```



#### gzip
gzip 命令用来压缩文件。gzip 是个使用广泛的压缩程序，文件经它压缩过后，其名称后面会多出“.gz”扩展名。

gzip 是在 Linux 系统中经常使用的一个对文件进行压缩和解压缩的命令，既方便又好用。gzip 不仅可以用来压缩大的、较少使用的文件以节省磁盘空间，还可以和 tar 命令一起构成 Linux 操作系统中比较流行的压缩文件格式。据统计，gzip 命令对文本文件有 60%～ 70%的压缩率。减少文件大小有两个明显的好处，一是可以减少存储空间，二是通过网络传输文件时，可以减少传输的时间。

```java
gzip * # 将所有文件压缩成 .gz 文件
gzip -l * # 详细显示压缩文件的信息，并不解压
gzip -dv * # 解压上例中的所有压缩文件，并列出详细的信息
gzip -r log.tar     # 压缩一个 tar 备份文件，此时压缩文件的扩展名为.tar.gz
gzip -rv 1/      # 递归的压缩目录
gzip -dr 1/      # 递归地解压目录
```



#### zip
zip 命令可以用来解压缩文件，或者对文件进行打包操作。zip 是个使用广泛的压缩程序，文件经它压缩后会另外产生具有“.zip”扩展名的压缩文件。

```plain

zip -q -r 1.zip *
```

<font style="color:rgb(31, 35, 40);"></font>

<font style="color:rgb(31, 35, 40);"></font>

### <font style="color:rgb(31, 35, 40);">Linux 系统管理</font>
+ 查看 CPU 信息 - 使用 cat /proc/cpuinfo
+ 重新启动 Linux 操作系统 - 使用 reboot
+ 退出 shell，并返回给定值 - 使用 exit
+ 关闭系统 - 使用 shutdown
+ 查看或设置系统时间与日期 - 使用 date
+ 查看系统当前进程状态 - 使用 ps
+ 删除当前正在运行的进程 - 使用 kill

#### date
```plain
# 格式化输出
date +"%Y-%m-%d"
2024-06-07

# 输出昨天日期
date -d "1 day ago" +"%Y-%m-%d"
2024-06-06
```



### Linux网络管理
+ 下载文件 - 使用 curl、wget
+ telnet 方式登录远程主机，对远程主机进行管理 - 使用 telnet
+ 查看和设置系统的主机名 - 使用 hostname
+ 查看和配置 Linux 内核中网络接口的网络参数 - 使用 ifconfig
+ ssh 方式连接远程主机 - 使用 ssh
+ 为 ssh 生成、管理和转换认证密钥 - 使用 ssh-keygen
+ 查看域名信息 - 使用 host, nslookup
+ 测试主机之间网络是否连通 - 使用 ping
+ 追踪数据在网络上的传输时的全部路径 - 使用 traceroute
+ 查看当前工作的端口信息 - 使用 netstat

#### hostname
hostname 命令用于查看和设置系统的主机名称。环境变量 HOSTNAME 也保存了当前的主机名。在使用 hostname 命令设置主机名后，系统并不会永久保存新的主机名，重新启动机器之后还是原来的主机名。如果需要永久修改主机名，需要同时修改 /etc/hosts 和 /etc/sysconfig/network 的相关内容。

<font style="color:rgb(99, 108, 118);"></font>

#### <font style="color:rgb(99, 108, 118);">host/nslookup</font>
host 命令是常用的分析域名查询工具，可以用来测试域名系统工作是否正常。

<font style="color:rgb(31, 35, 40);"></font>

#### <font style="color:rgb(31, 35, 40);">traceroute</font>
<font style="color:rgb(99, 108, 118);">traceroute 命令用于追踪数据包在网络上的传输时的全部路径，它默认发送的数据包大小是 40 字节。</font>

<font style="color:rgb(99, 108, 118);"></font>

#### <font style="color:rgb(99, 108, 118);">netstat</font>
netstat 命令用来打印 Linux 中网络系统的状态信息，可让你得知整个 Linux 系统的网络情况。

```plain
# 列出所有端口 (包括监听和未监听的)
netstat -a     #列出所有端口
netstat -at    #列出所有tcp端口
netstat -au    #列出所有udp端口

# 列出所有处于监听状态的 Sockets
netstat -l        #只显示监听端口
netstat -lt       #只列出所有监听 tcp 端口
netstat -lu       #只列出所有监听 udp 端口
netstat -lx       #只列出所有监听 UNIX 端口

# 显示每个协议的统计信息
netstat -s   显示所有端口的统计信息
netstat -st   显示TCP端口的统计信息
netstat -su   显示UDP端口的统计信息
```

<font style="color:rgb(99, 108, 118);"></font>

### <font style="color:rgb(99, 108, 118);">Linux 硬件管理</font>
+ 查看磁盘空间 - 使用 df
+ 查看文件或目录的磁盘空间 - 使用 du
+ 实时查看系统整体运行状态（如：CPU、内存） - 使用 top
+ 查看已使用和未使用的内存 - 使用 free



#### `df`（Disk Free）
`df` 命令用于显示文件系统的磁盘空间使用情况。它报告的是整个文件系统的使用情况，而不是单个目录或文件的使用情况。

常用选项：

+ `-h`：以人类可读的格式显示，自动使用适当的单位（如KB、MB、GB）。
+ `-T`：显示文件系统类型。
+ `-i`：显示inode使用情况。

使用示例：

```bash
df -h
```



输出示例：

```plain
Filesystem      Size  Used Avail Use% Mounted on
/dev/sda1        50G   15G   33G  31% /
tmpfs           3.9G  1.1M  3.9G   1% /run
/dev/sda2       100G   45G   56G  45% /home
```



#### `du`（Disk Usage）
`du` 命令用于显示特定目录或文件的磁盘使用情况。它可以递归地显示每个子目录的磁盘使用情况，非常适合查找哪些目录占用了大量空间。

常用选项：

+ `-h`：以人类可读的格式显示，自动使用适当的单位（如KB、MB、GB）。
+ `-s`：显示指定目录或文件的总计。
+ `-a`：显示所有文件和目录的磁盘使用情况。
+ `--max-depth=N`：限制递归显示的深度。



使用示例：

```bash
du -h --max-depth=1 /home
```

输出示例：

```plain
1.1G    /home/user1
500M    /home/user2
20G     /home/user3
21.6G   /home
```

区别总结

+  **df**： 
    - 用于查看整个文件系统的磁盘空间使用情况。
    - 通常用来检查整体磁盘空间和文件系统的使用状态。
    - 输出信息包括文件系统类型、总空间、已用空间、可用空间和挂载点。
+  **du**： 
    - 用于查看特定目录或文件的磁盘使用情况。
    - 通常用来查找哪个目录或文件占用了大量空间。
    - 可以递归地显示子目录和文件的使用情况，提供详细的目录大小信息。

#### top
top 命令可以实时动态地查看系统的整体运行情况，是一个综合了多方信息监测系统性能和运行信息的实用工具。通过 top 命令所提供的互动式界面，用热键可以管理。



#### free
free 命令可以显示当前系统未使用的和已使用的内存数目，还可以显示被内核使用的内存缓冲区。

```plain
free -t    # 以总和的形式显示内存的使用信息
free -s 10 # 周期性的查询内存使用信息，每10s 执行一次命令

# 显示内存使用情况

free -m
             total       used       free     shared    buffers     cached
Mem:          2016       1973         42          0        163       1497
-/+ buffers/cache:        312       1703
Swap:         4094          0       4094
```

#### 
### Linux 软件管理
+ rpm (red-hat)
+ yum (Fedora 和 RedHat)
+ apt-get (Debian) 



---

<font style="color:rgb(24, 25, 28);">大家学习Java碰到问题欢迎加入知识星球「风筑的编程圈子」，答疑所有碰到的编程问题，面试问题以及职业发展问题，欢迎加入</font>

<font style="color:rgb(24, 25, 28);"></font>

![](https://cdn.nlark.com/yuque/0/2024/jpeg/26411187/1719459784632-b665952c-7260-492c-af91-faa772aa4c88.jpeg)

<font style="color:rgb(24, 25, 28);"></font>

