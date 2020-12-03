---
title: Writing a New Post
author: codelnn
date: 2020-12-02 14:10:00 +0800
categories: [linux,]
tags: [writing]
---

# linux 命令学习

## 1 用户和用户组管理

### 1.1 用户管理

**1、添加新的用户账号**

> useradd 选项 用户名

参数说明：

- 选项:

  - -c comment 指定一段注释性描述。
  - -d 目录 指定用户主目录，如果此目录不存在，则同时使用-m选项，可以创建主目录。
  - -g 用户组 指定用户所属的用户组。
  - -G 用户组，用户组 指定用户所属的附加组。
  - -s Shell文件 指定用户的登录Shell。
  - -u 用户号 指定用户的用户号，如果同时有-o选项，则可以重复使用其他用户的标识号。

- 用户名: 

  指定新账号的登录名

实例

```
useradd –d  /home/sam -m sam
```

此命令创建了一个用户sam，其中-d和-m选项用来为登录名sam产生一个主目录  /home/sam（/home为默认的用户主目录所在的父目录）。

**2、删除账号**

```
userdel 选项 用户名
```

常用的选项是 `-r`，它的作用是把用户的主目录一起删除。

```
userdel -r sam
```

此命令删除用户sam在系统文件中（主要是/etc/passwd, /etc/shadow, /etc/group等）的记录，同时删除用户的主目录。

**3、修改账号**

修改用户账号就是根据实际情况更改用户的有关属性，如用户号、主目录、用户组、登录Shell等。

修改已有用户的信息使用`usermod`命令，其格式如下：

```
usermod 选项 用户名
```

常用的选项包括`-c, -d, -m, -g, -G, -s, -u以及-o等`，这些选项的意义与`useradd`命令中的选项一样，可以为用户指定新的资源值。

另外，有些系统可以使用选项：-l 新用户名

这个选项指定一个新的账号，即将原来的用户名改为新的用户名。

```
 usermod -s /bin/ksh -d /home/z –g developer sam
```

 此命令将用户sam的登录Shell修改为ksh，主目录改为/home/z，用户组改为developer。

**4、用户口令的管理**

```
passwd 选项 用户名
```

可使用的选项：

- -l 锁定口令，即禁用账号。
- -u 口令解锁。
- -d 使账号无口令。
- -f 强迫用户下次登录时修改口令。

如果默认用户名，则修改当前用户的口令。

```
# 修改密码
passwd sam
```

### 1.2 用户组管理

**1、增加一个新的用户组**

```
groupadd 选项 用户组
```

可以使用的选项有：

- -g GID 指定新用户组的组标识号（GID）。
- -o 一般与-g选项同时使用，表示新用户组的GID可以与系统已有用户组的GID相同。

实例

```
 groupadd group1
```

**2、存在用户username 添加到group1**

```
usermod -a -G group1 username
```

**3、删除用户组**

```
groupdel 用户组
```

## 2 文件基本属性与目录管理

### 2.1 文件基本属性

**1、文件与目录权限**

在 Linux 中我们通常使用以下两个命令来修改文件或目录的所属用户与权限：

- chown (change ownerp) ： 修改所属用户与组。
- chmod (change mode) ： 修改用户的权限。

```shell
codelnn@codelnn-ubuntu:~$ ls -l
总用量 52
-rw-r--rw- 1 root    root      12 11月 29 12:52 1.txt
-rw-r--r-- 1 codelnn codelnn 8980 11月 22 22:08 examples.desktop
```

实例中，**1.txt** 文件的第一个属性用 d 表示。d 在 Linux 中代表该文件是一个目录文件。

在 Linux 中第一个字符代表这个文件是目录、文件或链接文件等等。

- 当为 d 则是目录
- 当为 - 则是文件；
- 若是 l 则表示为链接文档(link file)；
- 若是 b 则表示为装置文件里面的可供储存的接口设备(可随机存取装置)；
- 若是 c 则表示为装置文件里面的串行端口设备，例如键盘、鼠标(一次性读取装置)。

接下来的字符中，以三个为一组，且均为 rwx 的三个参数的组合。其中， r 代表可读(read)、 w 代表可写(write)、 x 代表可执行(execute)。 要注意的是，这三个权限的位置不会改变，如果没有权限，就会出现减号  - 而已。

![](/home/codelnn/文档/image/file-llls22.jpg)

**2、文件属主和属组**

- chgrp : 更改文件属组

  语法：

  ```
  chgrp [-R] 属组名 文件名
  ```

  -R：递归更改文件属组，就是在更改某个目录文件的属组时，如果加上-R的参数，那么该目录下的所有文件的属组都会更改。

- chown：更改文件属主，也可以同时更改文件属组

  语法

  ```
  chown [–R] 属主名 文件名
  chown [-R] 属主名：属组名 文件名
  ```

- chmod : 更改文件9个属性   

  Linux文件属性有两种设置方法，一种是数字，一种是符号。

  Linux 文件的基本权限就有九个，分别是 **owner/group/others(拥有者/组/其他)** 三种身份各有自己的 **read/write/execute** 权限。     

  **使用数字类型改变文件权限**

  - r:4
  - w:2
  - x:1
  - -:0

  ```
     chmod [-R] xyz 文件或目录
  ```

  选项与参数：

  - xyz : 就是刚刚提到的数字类型的权限属性，为 rwx 属性数值的相加。
  - -R : 进行递归(recursive)的持续变更，亦即连同次目录下的所有文件都会变更

  **使用符号类型改变文件权限**

  - user：用户
  - group：组
  - others：其他

  那么我们就可以使用 **u, g, o** 来代表三种身份的权限。

  此外， **a** 则代表 **all**，即全部的身份。读写的权限可以写成 r, w, x，也就是可以使用下表的方式来看：

  ![](/home/codelnn/文档/image/符号修改文件权限.png)

### 2.2 文件目录管理

  **常见的出来目录的命令**

- ls（英文全拼：list files）: 列出目录及文件名
- cd（英文全拼：change directory）：切换目录
- pwd（英文全拼：print work directory）：显示目前的目录
- mkdir（英文全拼：make directory）：创建一个新的目录
- rmdir（英文全拼：remove directory）：删除一个空的目录
- cp（英文全拼：copy file）: 复制文件或目录
- rm（英文全拼：remove）: 移除文件或目录
- mv（英文全拼：move file）: 移动文件与目录，或修改文件与目录的名称

你可以使用 *man [命令]* 来查看各个命令的使用文档，如 ：man cp。

移动文件

**1、ls(列出目录)**

语法

```
ls [-options]
```

选项与参数：

- -a  ：全部的文件，连同隐藏文件**( 开头为 . 的文件)** 一起列出来(常用)
- -d  ：仅列出目录本身，而不是列出目录内的文件数据(常用)
- -l  ：长数据串列出，包含文件的属性与权限等等数据；(常用)

**2、cd(切换目录)**

语法

```
cd [相对路径或绝对路径]
```

```shell
#使用 mkdir 命令创建 runoob 目录
[root@www ~]# mkdir runoob

#使用绝对路径切换到 runoob 目录
[root@www ~]# cd /root/runoob/

#使用相对路径切换到 runoob 目录
[root@www ~]# cd ./runoob/

# 表示回到自己的家目录，亦即是 /root 这个目录
[root@www runoob]# cd ~

# 表示去到目前的上一级目录，亦即是 /root 的上一级目录的意思；
[root@www ~]# cd ..
```

**3、pwd(显示目前所在的目录)**

语法

```
pwd [-P]
```

选项与参数：

- -P ：显示出确实的路径，而非使用连结 (link) 路径。

**4、mkdir(创建新目录)**

语法

```
mkdir [-mp] 目录名称
```

选项与参数：

- -m ：配置文件的权限喔！直接配置，不需要看默认权限 (umask) 的脸色～
- -p ：帮助你直接将所需要的目录(包含上一级目录)递归创建起来！

实例

```shell
codelnn@codelnn-ubuntu:~$ mkdir test
codelnn@codelnn-ubuntu:~$ mkdir test1/test2
mkdir: 无法创建目录"test1/test2": 没有那个文件或目录
codelnn@codelnn-ubuntu:~$ mkdir -p test1/test2 #加了这个 -p 的选项，可以自行帮你创建多层目录！
```

**5、rmdir(删除空的目录)**

语法

```
rmdir [-p] 目录名称
```

 选项与参数：

- **-p ：**连同上一级『空的』目录也一起删除

实例

```
rmdir test
rmdir -p test1/test2 # 删除上面创建的空目录
```

**6、cp(复制文件或目录)**

语法

```
cp [-adfilprsu] 来源档(source) 目标档(destination)
cp [options] source1 source2 source3 .... directory
```

选项与参数：

- **-a：**相当于 -pdr 的意思，至于 pdr 请参考下列说明；(常用)
- **-d：**若来源档为连结档的属性(link file)，则复制连结档属性而非文件本身；
- **-f：**为强制(force)的意思，若目标文件已经存在且无法开启，则移除后再尝试一次；
- **-i：**若目标档(destination)已经存在时，在覆盖时会先询问动作的进行(常用)
- **-l：**进行硬式连结(hard link)的连结档创建，而非复制文件本身；
- **-p：**连同文件的属性一起复制过去，而非使用默认属性(备份常用)；
- **-r：**递归持续复制，用于目录的复制行为；(常用)
- **-s：**复制成为符号连结档 (symbolic link)，亦即『捷径』文件；
- **-u：**若 destination 比 source 旧才升级 destination ！

```
cp 1.txt /usr/workspace # 复制文件到 workspace目录下
cp -r /usr/workspace /usr/soft/  # 复制workspace目录到soft目录下
```

**7、rm(移除文件或目录)**

语法

```
rm [-fir] 文件或目录
```

选项与参数：

- -f  ：就是 force 的意思，忽略不存在的文件，不会出现警告信息；

- -i  ：互动模式，在删除前会询问使用者是否动作

- -r  ：递归删除啊！最常用在目录的删除了！这是非常危险的选项！！！

- ```
  rm -rf /* #你懂得！！！！
  ```

**8、mv(移动文件与目录，或修改名称）**

语法

```
mv [-fiu] source destination
mv [options] source1 source2 source3 .... directory
```

选项与参数：

- -f  ：force 强制的意思，如果目标文件已经存在，不会询问而直接覆盖；
- -i  ：若目标文件 (destination) 已经存在时，就会询问是否覆盖！
- -u  ：若目标文件已经存在，且 source 比较新，才会升级 (update)

```
mv 1.txt 2.txt # 修改名称
```

**9、Linux文件内容查看**

- cat  由第一行开始显示文件内容

- tac  从最后一行开始显示，可以看出 tac 是 cat 的倒着写！

- nl  显示的时候，顺道输出行号！

  ```
  nl [-bnw] 文件
  ```

- more 一页一页的显示文件内容

- less 与 more 类似，但是比 more 更好的是，他可以往前翻页！

- head 只看头几行 head [-n number] 文件 

- tail 只看尾巴几行 tail [-n number] 文件 

**10、tar文件解压和压缩**

语法

```
tar [-options] filename
tar zxvf filename.tar.gz   // 解压文件 
tar zcvf filename.tar.gz filename  // 压缩文件
```

选项与参数：

```
-c 建立新的压缩文件
-f 指定压缩文件
-r 添加文件到已经压缩文件包中
-u 添加改了和现有的文件到压缩包中
-x 从压缩包中抽取文件
-t 显示压缩文件中的内容
-z 支持gzip压缩
-j 支持bzip2压缩
-Z 支持compress解压文件
-v 显示操作过程
```

## 3 软件包管理

### 3.1 yum

**1、yum 语法**

```
yum [options] [command] [package ...]
```

- **options：**可选，选项包括-h（帮助），-y（当安装过程提示选择全部为 "yes"），-q（不显示安装的过程）等等。
- **command：**要进行的操作。
- **package：**安装的包名。

**2、yum常用命令**

- 列出所有可更新的软件清单命令：yum check-update
- 更新所有软件命令：yum update
- 仅安装指定的软件命令：yum install <package_name>
- 仅更新指定的软件命令：yum update <package_name>
- 列出所有可安裝的软件清单命令：yum list
- 删除软件包命令：yum remove <package_name>
- 查找软件包命令：yum search <keyword>
- 清除缓存命令:
  - yum clean packages: 清除缓存目录下的软件包
  - yum clean headers: 清除缓存目录下的 headers
  - yum clean oldheaders: 清除缓存目录下旧的 headers
  - yum clean, yum clean all (= yum clean packages; yum clean oldheaders) :清除缓存目录下的软件包及旧的 headers

### 3.2 apt

**1、apt语法**

```
  apt [options] [command] [package ...]
```

- **options：**可选，选项包括 -h（帮助），-y（当安装过程提示选择全部为"yes"），-q（不显示安装的过程）等等。
- **command：**要进行的操作。
- **package**：安装的包名。

**2、apt常用命令**

- 列出所有可更新的软件清单命令：sudo apt update

- 升级软件包：sudo apt upgrade

  列出可更新的软件包及版本信息：apt list --upgradeable

  升级软件包，升级前先删除需要更新软件包：sudo apt full-upgrade

- 安装指定的软件命令：sudo apt install <package_name>

  安装多个软件包：sudo apt install  <package_1>  <package_2>  <package_3>

- 更新指定的软件命令：sudo apt update <package_name>

- 显示软件包具体信息,例如：版本号，安装大小，依赖关系等等：sudo apt show <package_name>

- 删除软件包命令：sudo apt remove <package_name>

- 清理不再使用的依赖和库文件: sudo apt autoremove

- 移除软件包及配置文件: sudo apt purge  <package_name>

- 查找软件包命令： sudo apt search <keyword>

- 列出所有已安装的包：apt list --installed

- 列出所有已安装的包的版本信息：apt list --all-versions

## 4 其他命令


**投影屏幕**

```
 # HDMI-1 第二个显示屏端口 DP-1 是主显示屏幕的端口， --auto 是自动设置最佳分辨率
 xrand --output HDMI-1 --same-as DP-1 --auto 
```

## 5 链接

https://blog.csdn.net/qq_44209336/article/details/110394884

