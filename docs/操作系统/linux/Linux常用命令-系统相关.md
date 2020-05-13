# Linux常用命令

[TOC]

### 查看当前日历：cal

参数：`-y`显示整年日历

实例：

```shell
[root@izgdvgz30p94oaz ~]# cal
     April 2020     
Su Mo Tu We Th Fr Sa
          1  2  3  4
 5  6  7  8  9 10 11
12 13 14 15 16 17 18
19 20 21 22 23 24 25
26 27 28 29 30
```

### 显示或设置时间：date

设置时间格式（需要管理员权限）：

`date [MMDDhhmm[[CC]YY][.ss]] +format`

CC为年前两位 yy为年的后两位，前两位的mm为月，后两位的mm为分钟，dd为天，hh为小时，ss为秒。如： date 010203042016.55。

显示时间格式（date '+%y,%m,%d,%H,%M,%S'）：

| format格式 | 含义 |
| :--------- | ---- |
| %Y，%y     | 年   |
| %m         | 月   |
| %d         | 日   |
| %H         | 时   |
| %M         | 分   |
| %S         | 秒   |

### 查看进程信息：ps

ps命令可以查看进程的详细状况，常用选项(选项可以不加“-”)如下：

| 选项 | 含义                                     |
| :--- | ---------------------------------------- |
| -a   | 显示终端上的所有进程，包括其他用户的进程 |
| -u   | 显示进程的详细状态                       |
| -x   | 显示没有控制终端的进程                   |
| -w   | 显示加宽，以便显示更多的信息             |
| -r   | 只显示正在运行的进程                     |

实例：

```shell
ps axo pid,comm,pcpu # 查看进程的PID、名称以及CPU 占用率
ps aux | sort -rnk 4 # 按内存资源的使用量对进程进行排序
ps aux | sort -nk 3  # 按 CPU 资源的使用量对进程进行排序
ps -A # 显示所有进程信息
ps -u root # 显示指定用户信息
ps -efL # 查看线程数
ps -e -o "%C : %p :%z : %a"|sort -k5 -nr # 查看进程并按内存使用大小排列
ps -ef # 显示所有进程信息，连同命令行
ps -ef | grep ssh # ps 与grep 常用组合用法，查找特定进程
ps -C nginx # 通过名字或命令搜索进程
ps aux --sort=-pcpu,+pmem # CPU或者内存进行排序,-降序，+升序
ps -f --forest -C nginx # 用树的风格显示进程的层次关系
ps -o pid,uname,comm -C nginx # 显示一个父进程的子进程
ps -e -o pid,uname=USERNAME,pcpu=CPU_USAGE,pmem,comm # 重定义标签
ps -e -o pid,comm,etime # 显示进程运行的时间
ps -aux | grep named # 查看named进程详细信息
ps -o command -p 91730 | sed -n 2p # 通过进程id获取服务名称
```

### 动态显示进程：top

top命令用来动态显示运行中的进程。top命令能够在运行后，在指定的时间间隔更新显示信息。可以在使用top命令时加上-d 来指定显示信息更新的时间间隔。

参数：

```shell
-b：以批处理模式操作；
-c：显示完整的治命令；
-d：屏幕刷新间隔时间；
-I：忽略失效过程；
-s：保密模式；
-S：累积模式；
-i<时间>：设置间隔时间；
-u<用户名>：指定用户名；
-p<进程号>：指定进程；
-n<次数>：循环显示的次数。
```

在top命令执行过程中可以使用的一些交互命令。这些命令都是单字母的，如果在命令行中使用了-s选项， 其中一些命令可能会被屏蔽。

```shell
h：显示帮助画面，给出一些简短的命令总结说明；
k：终止一个进程；
i：忽略闲置和僵死进程，这是一个开关式命令；
q：退出程序；
r：重新安排一个进程的优先级别；
S：切换到累计模式；
s：改变两次刷新之间的延迟时间（单位为s），如果有小数，就换算成ms。输入0值则系统将不断刷新，默认值是5s；
f或者F：从当前显示中添加或者删除项目；
o或者O：改变显示项目的顺序；
l：切换显示平均负载和启动时间信息；
m：切换显示内存信息；
t：切换显示进程和CPU状态信息；
c：切换显示命令名称和完整命令行；
M：根据驻留内存大小进行排序；
P：根据CPU使用百分比大小进行排序；
T：根据时间/累计时间进行排序；
w：将当前设置写入~/.toprc文件中。
```

### 终止进程：kill

kill命令指定进程号的进程，需要配合 ps 使用。

使用格式：

```shell
kill [-signal] pid
```

选项

```shell
-s sig    信号名称。
-n sig    信号名称对应的数字。
-l        列出信号名称。如果在该选项后提供了数字那么假设它是信号名称对应的数字。
-L        等价于-l选项。
```

信号值从0到15，其中9为绝对终止，可以处理一般信号无法终止的进程。

kill 9133 ：9133 为应用程序所对应的进程号

实例

```shell
[user2@pc] kill -l 9
KILL

[user2@pc] kill -l # 列出所有信号名称：

# 下面是常用的信号。
# 只有第9种信号(SIGKILL)才可以无条件终止进程，其他信号进程都有权利忽略。

HUP     1    终端挂断
INT     2    中断（同 Ctrl + C）
QUIT    3    退出（同 Ctrl + \）
KILL    9    强制终止
TERM   15    终止
CONT   18    继续（与STOP相反，fg/bg命令）
STOP   19    暂停（同 Ctrl + Z）
```

### 关机重启：reboot、shutdown、init

| 命令              | 含义                                       |
| :---------------- | ------------------------------------------ |
| reboot            | 重新启动操作系统                           |
| shutdown –r now   | 重新启动操作系统，shutdown会给别的用户提示 |
| shutdown -h now   | 立刻关机，其中now相当于时间为0的状态       |
| shutdown -h 20:25 | 系统在今天的20:25 会关机                   |
| shutdown -h +10   | 系统再过十分钟后自动关机                   |
| init 0            | 关机                                       |
| init 6            | 重启                                       |

### 检测磁盘空间：df

检测文件系统的磁盘空间占用和空余情况，可以显示所有文件系统对节点和磁盘块的使用情况。

| 选项 | 含义                                 |
| :--- | ------------------------------------ |
| -a   | 显示所有文件系统的磁盘使用情况       |
| -m   | 以1024字节为单位显示                 |
| -t   | 显示各指定文件系统的磁盘空间使用情况 |
| -T   | 显示文件系统                         |

### 检测目录所占磁盘空间：du

统计目录或文件所占磁盘空间的大小，该命令的执行结果与df类似，du更侧重于磁盘的使用状况。

du命令的使用格式如下： du [选项] 目录或文件名

| 选项 | 含义                                               |
| :--- | -------------------------------------------------- |
| -a   | 递归显示指定目录中各文件和子目录中文件占用的数据块 |
| -s   | 显示指定文件或目录占用的数据块                     |
| -b   | 以字节为单位显示磁盘占用情况                       |
| -l   | 计算所有文件大小，对硬链接文件计算多次             |

### 查看或配置网卡信息：ifconfig

如果，我们只是敲：ifconfig，它会显示所有网卡的信息：

```shell
[root@localhost ~]# ifconfig
eth0      Link encap:Ethernet  HWaddr 00:16:3E:00:1E:51  
          inet addr:10.160.7.81  Bcast:10.160.15.255  Mask:255.255.240.0
          UP BROADCAST RUNNING MULTICAST  MTU:1500  Metric:1
          RX packets:61430830 errors:0 dropped:0 overruns:0 frame:0
          TX packets:88534 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:1000
          RX bytes:3607197869 (3.3 GiB)  TX bytes:6115042 (5.8 MiB)

lo        Link encap:Local Loopback  
          inet addr:127.0.0.1  Mask:255.0.0.0
          UP LOOPBACK RUNNING  MTU:16436  Metric:1
          RX packets:56103 errors:0 dropped:0 overruns:0 frame:0
          TX packets:56103 errors:0 dropped:0 overruns:0 carrier:0
          collisions:0 txqueuelen:0
          RX bytes:5079451 (4.8 MiB)  TX bytes:5079451 (4.8 MiB)

```

**说明**

**eth0** 表示第一块网卡，其中`HWaddr`表示网卡的物理地址，可以看到目前这个网卡的物理地址(MAC地址）是`00:16:3E:00:1E:51`。

**inet addr** 用来表示网卡的IP地址，此网卡的IP地址是`10.160.7.81`，广播地址`Bcast:10.160.15.255`，掩码地址`Mask:255.255.240.0`。

**lo** 是表示主机的回坏地址，这个一般是用来测试一个网络程序，但又不想让局域网或外网的用户能够查看，只能在此台主机上运行和查看所用的网络接口。比如把 httpd服务器的指定到回坏地址，在浏览器输入127.0.0.1就能看到你所架WEB网站了。但只是您能看得到，局域网的其它主机或用户无从知道。

- 第一行：连接类型：Ethernet（以太网）HWaddr（硬件mac地址）。
- 第二行：网卡的IP地址、子网、掩码。
- 第三行：UP（代表网卡开启状态）RUNNING（代表网卡的网线被接上）MULTICAST（支持组播）MTU:1500（最大传输单元）：1500字节。
- 第四、五行：接收、发送数据包情况统计。
- 第七行：接收、发送数据字节数统计信息。

**参数说明**

| add<地址>                   | 设置网络设备IPv6的ip地址；     |
| --------------------------- | ------------------------------ |
| del<地址>                   | 删除网络设备IPv6的IP地址；     |
| down                        | 关闭指定的网络设备             |
| <hw<网络设备类型><硬件地址> | 设置网络设备的类型与硬件地址； |
| up                          | 启动指定的网络设备；           |

**启动关闭指定网卡：**

```shell
ifconfig eth0 up
ifconfig eth0 down
```

`ifconfig eth0 up`为启动网卡eth0，`ifconfig eth0 down`为关闭网卡eth0。ssh登陆linux服务器操作要小心，关闭了就不能开启了，除非你有多网卡。

**为网卡配置和删除IPv6地址：**

```shell
ifconfig eth0 add 33ffe:3240:800:1005::2/64    #为网卡eth0配置IPv6地址
ifconfig eth0 del 33ffe:3240:800:1005::2/64    #为网卡eth0删除IPv6地址
```

**用ifconfig修改MAC地址：**

```shell
ifconfig eth0 hw ether 00:AA:BB:CC:dd:EE
```

**配置IP地址：**

```shell
[root@localhost ~]# ifconfig eth0 192.168.2.10
[root@localhost ~]# ifconfig eth0 192.168.2.10 netmask 255.255.255.0
[root@localhost ~]# ifconfig eth0 192.168.2.10 netmask 255.255.255.0 broadcast 192.168.2.255
```

**启用和关闭arp协议：**

```shell
ifconfig eth0 arp    #开启网卡eth0 的arp协议
ifconfig eth0 -arp   #关闭网卡eth0 的arp协议
```

**设置最大传输单元：**

```shell
ifconfig eth0 mtu 1500    #设置能通过的最大数据包大小为 1500 bytes
```

**其它实例**

```shell
ifconfig   #处于激活状态的网络接口
ifconfig -a  #所有配置的网络接口，不论其是否激活
ifconfig eth0  #显示eth0的网卡信息
```

### 测试远程主机连通性：ping

用来测试主机之间网络的连通性。执行ping指令会使用ICMP传输协议，发出要求回应的信息，若远端主机的网络功能没有问题，就会回应该信息，因而得知该主机运作正常。

```shell
[root@izgdvgz30p94oaz ~]# ping www.baidu.com
PING www.a.shifen.com (14.215.177.39) 56(84) bytes of data.
64 bytes from 14.215.177.39 (14.215.177.39): icmp_seq=1 ttl=52 time=6.05 ms
64 bytes from 14.215.177.39 (14.215.177.39): icmp_seq=2 ttl=52 time=6.05 ms
64 bytes from 14.215.177.39 (14.215.177.39): icmp_seq=3 ttl=52 time=6.05 ms
64 bytes from 14.215.177.39 (14.215.177.39): icmp_seq=4 ttl=52 time=6.04 ms
64 bytes from 14.215.177.39 (14.215.177.39): icmp_seq=5 ttl=52 time=6.04 ms
^C
--- www.a.shifen.com ping statistics ---
5 packets transmitted, 5 received, 0% packet loss, time 4012ms
rtt min/avg/max/mdev = 6.042/6.052/6.058/0.005 ms
```

### 防火墙管理

#### service方式

**查看防火墙状态**：

```shell
service iptables status
```

iptables：未运行防火墙。

**开启防火墙**：

```shell
 service iptables start
```

**关闭防火墙**：

```shell
service iptables stop
```

CentOS 7.0默认使用的是firewall作为防火墙

**查看防火墙状态**

```shell
firewall-cmd --state
```

**停止firewall**

```shell
systemctl stop firewalld.service
```

**禁止firewall开机启动**

```shell
systemctl disable firewalld.service
```

#### iptables方式

**先进入init.d目录，命令如下**：

```shell
[root@centos6 ~]# cd /etc/init.d/
[root@centos6 init.d]#
```

**查看防火墙状态**：

```shell
[root@centos6 init.d]# /etc/init.d/iptables status
```

**暂时关闭防火墙**：

```shell
[root@centos6 init.d]# /etc/init.d/iptables stop
```

**重启iptables**：

```shell
[root@centos6 init.d]# /etc/init.d/iptables restart
```

