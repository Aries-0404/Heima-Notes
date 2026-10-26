# 第4章Linux实用操作

## 1、各类实用小技巧

### （1）`ctrl+c`命令（强制停止命令）

![image-20240705170602152](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705170602152.png)

### （2）`exit`命令（退出当前shell会话的命令）



### （3）history命令（查看历史记录中输入过的命令）

![image-20240705170858979](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705170858979.png)

### （4）clear命令（清屏命令）

![image-20240705171023372](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705171023372.png)



## 2、软件安装

### （1）Linux系统中软件安装的两种方式

- ==下载安装包安装==
- ==在应用商店中安装（这里主要介绍这一种）==

 ![image-20240705171135294](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705171135294.png)



### （2）yum命令（就是在linux系统的应用商店中安装）

![image-20240705171205091](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705171205091.png)



![image-20240705171218330](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705171218330.png)

### notes：ubuntu系统中的apt命令（了解）

![image-20240705171242323](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705171242323.png)





## 3、systemctl命令（控制应用软件的启动、关闭、开机自启等）

![image-20240705171656833](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705171656833.png)

### notes：如果应用软件没有自动集成到systemctl中，可以手动集成

![image-20240705171833133](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705171833133.png)



## 4、软链接（类似于快捷方式）

![image-20240705172108495](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705172108495.png)

## 5、日期、时区

### （1）date命令

![image-20240705172156646](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705172156646.png)

![image-20240705172234933](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705172234933.png)

#### notes：`date`命令的`-d`选项

![image-20240705172251338](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705172251338.png)

### （2）修改linux时区

![image-20240705172321528](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240705172321528.png)



## 6、IP地址、主机名

### （1）`ifconfig`命令（查看本机ip）

![image-20240727170616350](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240727170616350.png)



### （2）主机名

![image-20240727170657746](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240727170657746.png)

#### notes：hostname命令（查看主机名）

![image-20240727170842892](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240727170842892.png)

#### notes：hostnamectl set-hostname 新的主机名（设置新的主机名的命令）

![image-20240727170918603](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240727170918603.png)



### （3）域名解析

将ip地址和主机名之间建立映射关系从而可以通过主机名访问对应的ip地址（避免去记忆复杂的ip地址）

![image-20240727171631053](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240727171631053.png)

#### notes：在windows本地地址簿配置主机名映射的过程

![image-20240727171807094](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240727171807094.png)



### （4）给linux服务器配置固定ip

#### 为什么：

##### ①为什么要给linux服务器配置固定ip？

当前我们虚拟机的Linux操作系统，其IP地址是通过DHCP服务获取的。DHCP：动态获取IP地址，即每次重启设备后都会获取一次，可能导致IP地址频繁变更

原因1：办公电脑IP地址变化无所谓，但是我们要远程连接到Linux系统，如果IP地址经常变化我们就要频繁修改适配很麻烦

原因2：在刚刚我们配置了虚拟机IP地址和主机名的映射，如果IP频繁更改，我们也需要频繁更新映射关系

综上所述，我们需要IP地址固定下来，不要变化了。

#### 怎么办：

##### ①如何给linux服务器配置固定ip？

![image-20240727204940514](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240727204940514.png)

![image-20240727205040599](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240727205040599.png)



## 7、网络传输

### （1）ping命令（检查指定的网络服务器是否是可联通的状态的命令）

![image-20240727205512612](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240727205512612.png)

### （2）wget命令（用于从指定的url地址下载文件）

#### notes：`-b`选项表示后台下载

![image-20240728105918313](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728105918313.png)

### （3）curl命令（用于向指定的url地址发送http请求，可用于下载文件、获取信息）

![image-20240728110041181](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728110041181.png)

![image-20240728110401752](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728110401752.png)



### （4）端口

![image-20240728110636275](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728110636275.png)

![image-20240728110800070](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728110800070.png)

#### ①`nmap`命令（查看指定ip的端口占用情况）

![image-20240728110846197](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728110846197.png)

#### ②`netstat`命令（用于查看本机的端口占用情况）

##### notes：`netstat`命令有多个选项，常用的为以下三个

`-a` : 显示所有连接和监听端口。

`-n` : 以数字形式显示地址和端口号。

`-p` : 显示连接的程序名及PID（需要超级用户权限）。

![image-20240728112026710](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728112026710.png)



## 8、进程管理

### （1）`ps`命令（用于查看本主机的所有进程）

#### notes：`ps`命令的`-e -f`选项

![image-20240728112618318](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728112618318.png)

#### notes：`ps`命令配合管道符查看指定的进程

![image-20240728112801610](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728112801610.png)

### （2）`kill`命令（用于关闭指定的进程）

#### notes：`kill`命令的`-9`选项（表示强制关闭进程）

![image-20240728113103585](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728113103585.png)



## 9、主机状态

### （1）`top`命令（查看主机的系统资源占用情况）

![image-20240728114816377](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728114816377.png)

![image-20240728114849235](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728114849235.png)

#### notes：`top`命令的选项

![image-20240728115327754](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728115327754.png)

### （2）`df`命令（查看磁盘的使用情况）

![image-20240728115436944](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728115436944.png)

### （3）`iostat`命令（查看cpu、磁盘的相关信息）

![image-20240728165610096](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728165610096.png)

### （4）`sar`命令（用于实时监控系统性能数据）

#### notes：`sar`命令的众多选项

- `-u`：CPU使用情况
- `-r`：内存使用情况
- `-b`：I/O和传输速率
- `-q`：队列长度和系统负载平均值
- `-n`：网络统计信息
- `-B`：分页统计
- `-S`：交换空间使用情况
- `-P`：所有CPU的统计

![image-20240728170409540](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728170409540.png)



## 10、环境变量

### （1）`env`命令（用于显示当前系统所有的环境变量）

![image-20240728171532513](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728171532513.png)

### （2）`PATH`环境变量（是一个特定的环境变量，用于指定可执行文件的搜索路径）

![image-20240728172018108](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728172018108.png)

### （3）`$`符号（用于取变量的值）

![image-20240728172212613](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240728172212613.png)

### （4）自行设置环境变量

#### ①临时设置环境变量

这种方式仅在当前会话（终端）中有效，关闭终端后失效。

在当前终端会话中设置环境变量：

```shell
export VAR_NAME=value
```

#### ②永久设置环境变量

这种方式将环境变量写入到配置文件中，使其在每次启动终端时自动加载。

1. ==系统级别设置（对所有用户生效）==：编辑 `/etc/profile` 文件将环境变量添加到其中。例如：

```shell
#编辑/etc目录下的profile文件
sudo vim /etc/profile
#通过上述命令进入profile文件后，在其中添加自己想定义的环境变量
export VAR_NAME=value
#保存并关闭文件后，使更改生效：
source /etc/profile

#由此成功地在系统级别设置了一个新的环境变量，它将对所有用户和所有新的终端会话有效。如果你只想对当前用户设置环境变量，可以选择编辑 ~/.bashrc文件而不是 /etc/profile。
```

2. ==用户级别设置（仅对当前用户生效）==：编辑用户主目录下的 `.bashrc` 文件。例如：

```shell
#编辑当前用户主目录下的.bashrc文件
vim ~/.bashrc
#进入.bashrc文件以后，在其中添加自己想定义的环境变量
export VAR_NAME=value
#保存并关闭文件后，使更改生效：
source ~/.bashrc
```





## 11、文件的上传、下载

### （1）通过文件系统实现文件的上传下载

![image-20240803213627751](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240803213627751.png)



### （2）通过命令实现文件的上传下载

![image-20240803213843304](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240803213843304.png)



## 12、文件的压缩、解压

### notes：常见压缩格式介绍

![image-20240803214144903](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240803214144903.png)



### （1）`tar`命令（在`linux`系统中用于压缩和解压`.tar`和`.tar.gz`格式的文件的命令）

![image-20240803214253402](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240803214253402.png)

![image-20240803214429431](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240803214429431.png)

![image-20240803214602272](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240803214602272.png)



### （2）`zip`和`unzip`命令（在`linux`系统中用于压缩和解压`.zip`格式的文件的命令）

![image-20240803214658309](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240803214658309.png)

![image-20240803214936709](%E7%AC%AC4%E7%AB%A0Linux%E5%AE%9E%E7%94%A8%E6%93%8D%E4%BD%9C.assets/image-20240803214936709.png)
