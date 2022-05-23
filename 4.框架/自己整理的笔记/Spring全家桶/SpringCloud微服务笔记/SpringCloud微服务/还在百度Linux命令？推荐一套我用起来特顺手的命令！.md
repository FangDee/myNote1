# [还在百度Linux命令？推荐一套我用起来特顺手的命令！](http://www.macrozheng.com/#/reference/linux_command?id=还在百度linux命令？推荐一套我用起来特顺手的命令！)

> 作为一位Java后端开发，怎能不会点Linux命令？总结了一套非常实用的Linux命令（基于CentOS 7.6），希望对大家有所帮助！

## [系统服务管理](http://www.macrozheng.com/#/reference/linux_command?id=系统服务管理)

### [systemctl](http://www.macrozheng.com/#/reference/linux_command?id=systemctl)

> `systemctl`命令是`service`和`chkconfig`命令的组合体，可用于管理系统。

- 输出系统中各个服务的状态：

```bash
systemctl list-units --type=serviceCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_01.png)

- 查看服务的运行状态：

```bash
systemctl status firewalldCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_02.png)

- 关闭服务：

```bash
systemctl stop firewalldCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_03.png)

- 启动服务：

```bash
systemctl start firewalldCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_04.png)

- 重新启动服务（不管当前服务是启动还是关闭）：

```bash
systemctl restart firewalldCopy to clipboardErrorCopied
```

- 重新载入配置信息而不中断服务：

```bash
systemctl reload firewalldCopy to clipboardErrorCopied
```

- 禁止服务开机自启动：

```bash
systemctl disable firewalldCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_05.png)

- 设置服务开机自启动：

```bash
systemctl enable firewalldCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_06.png)

## [文件管理](http://www.macrozheng.com/#/reference/linux_command?id=文件管理)

### [ls](http://www.macrozheng.com/#/reference/linux_command?id=ls)

列出指定目录下的所有文件，列出`/`目录下的文件：

```bash
ls -l /Copy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_07.png)

### [pwd](http://www.macrozheng.com/#/reference/linux_command?id=pwd)

获取目前所在工作目录的绝对路径：

![img](http://www.macrozheng.com/images/linux_command_08.png)

### [cd](http://www.macrozheng.com/#/reference/linux_command?id=cd)

改变当前工作目录：

```bash
cd /usr/localCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_09.png)

### [date](http://www.macrozheng.com/#/reference/linux_command?id=date)

显示或修改系统时间与日期；

```bash
date '+%Y-%m-%d %H:%M:%S'Copy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_10.png)

### [passwd](http://www.macrozheng.com/#/reference/linux_command?id=passwd)

用于设置用户密码：

```bash
passwd rootCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_11.png)

### [su](http://www.macrozheng.com/#/reference/linux_command?id=su)

改变用户身份（切换到超级用户）：

```bash
su -Copy to clipboardErrorCopied
```

### [clear](http://www.macrozheng.com/#/reference/linux_command?id=clear)

用于清除屏幕信息

### [man](http://www.macrozheng.com/#/reference/linux_command?id=man)

显示指定命令的帮助信息：

```bash
man lsCopy to clipboardErrorCopied
```

### [who](http://www.macrozheng.com/#/reference/linux_command?id=who)

- 查询系统处于什么运行级别：

```bash
who -rCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_12.png)

- 显示目前登录到系统的用户：

  ```bash
  who -buTCopy to clipboardErrorCopied
  ```

  

### [free](http://www.macrozheng.com/#/reference/linux_command?id=free)

显示系统内存状态（单位MB）：

```bash
free -mCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_14.png)

### [ps](http://www.macrozheng.com/#/reference/linux_command?id=ps)

- 显示系统进程运行动态：

```bash
ps -efCopy to clipboardErrorCopied
```

- 查看`sshd`进程的运行动态：

```bash
ps -ef | grep sshdCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_15.png)

### [top](http://www.macrozheng.com/#/reference/linux_command?id=top)

查看即时活跃的进程，类似Windows的任务管理器。

![img](http://www.macrozheng.com/images/linux_command_16.png)

### [mkdir](http://www.macrozheng.com/#/reference/linux_command?id=mkdir)

创建目录：

![img](http://www.macrozheng.com/images/linux_command_17.png)

### [more](http://www.macrozheng.com/#/reference/linux_command?id=more)

用于分页查看文件，例如每页10行查看`boot.log`文件：

```bash
more -c -10 /var/log/boot.logCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_18.png)

### [cat](http://www.macrozheng.com/#/reference/linux_command?id=cat)

用于查看文件，例如查看Linux启动日志文件文件，并标明行号：

```bash
cat -Ab /var/log/boot.logCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_19.png)

### [touch](http://www.macrozheng.com/#/reference/linux_command?id=touch)

用于创建文件，例如创建`text.txt`文件：

```bash
touch text.txtCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_20.png)

### [rm](http://www.macrozheng.com/#/reference/linux_command?id=rm)

- 删除文件：

```bash
rm text.txtCopy to clipboardErrorCopied
```

- 强制删除某个目录及其子目录：

```bash
rm -rf testdir/Copy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_21.png)

### [cp](http://www.macrozheng.com/#/reference/linux_command?id=cp)

用于拷贝文件，例如将`test1`目录复制到`test2`目录

```bash
cp -r /mydata/tes1 /mydata/test2Copy to clipboardErrorCopied
```

### [mv](http://www.macrozheng.com/#/reference/linux_command?id=mv)

用于移动或覆盖文件：

```bash
mv text.txt text2.txtCopy to clipboardErrorCopied
```

## [压缩与解压](http://www.macrozheng.com/#/reference/linux_command?id=压缩与解压)

### [tar](http://www.macrozheng.com/#/reference/linux_command?id=tar)

- 将`/etc`文件夹中的文件归档到文件`etc.tar`（并不会进行压缩）：

```bash
tar -cvf /mydata/etc.tar /etcCopy to clipboardErrorCopied
```

- 用`gzip`压缩文件夹`/etc`中的文件到文件`etc.tar.gz`：

```bash
tar -zcvf /mydata/etc.tar.gz /etcCopy to clipboardErrorCopied
```

- 用`bzip2`压缩文件夹`/etc`到文件`/etc.tar.bz2`：

```bash
tar -jcvf /mydata/etc.tar.bz2 /etcCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_22.png)

- 分页查看压缩包中内容（gzip）：

```bash
tar -ztvf /mydata/etc.tar.gz |more -c -10Copy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_24.png)

- 解压文件到当前目录（gzip）：

```bash
tar -zxvf /mydata/etc.tar.gzCopy to clipboardErrorCopied
```

- 解压文件到指定目录（gzip）：

```bash
tar -zxvf /mydata/etc.tar.gz -C /mydata/etcCopy to clipboardErrorCopied
```

## [磁盘和网络管理](http://www.macrozheng.com/#/reference/linux_command?id=磁盘和网络管理)

### [df](http://www.macrozheng.com/#/reference/linux_command?id=df)

查看磁盘空间占用情况：

```bash
df -hTCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_25.png)

### [dh](http://www.macrozheng.com/#/reference/linux_command?id=dh)

查看当前目录下的文件及文件夹所占大小：

```bash
du -h --max-depth=1 ./*Copy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_26.png)

### [ifconfig](http://www.macrozheng.com/#/reference/linux_command?id=ifconfig)

显示当前网络接口状态：

![img](http://www.macrozheng.com/images/linux_command_27.png)

### [netstat](http://www.macrozheng.com/#/reference/linux_command?id=netstat)

- 查看当前路由信息：

```bash
netstat -rnCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_28.png)

- 查看所有有效TCP连接：

```bash
netstat -anCopy to clipboardErrorCopied
```

- 查看系统中启动的监听服务：

```bash
netstat -tulnpCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/images/linux_command_29.png)

- 查看处于连接状态的系统资源信息：

```bash
netstat -atunpCopy to clipboardErrorCopied
```

### [wget](http://www.macrozheng.com/#/reference/linux_command?id=wget)

从网络上下载文件

![img](http://www.macrozheng.com/images/linux_command_30.png)

## [文件上传下载](http://www.macrozheng.com/#/reference/linux_command?id=文件上传下载)

- 安装上传下载工具`lrzsz`；

```bash
yum install -y lrzszCopy to clipboardErrorCopied
```

- 上传文件，输入以下命令`XShell`会弹出文件上传框；

```bash
rzCopy to clipboardErrorCopied
```

- 下载文件，输入以下命令`XShell`会弹出文件保存框；

```bash
sz fileNameCopy to clipboardErrorCopied
```

## [软件的安装与管理](http://www.macrozheng.com/#/reference/linux_command?id=软件的安装与管理)

### [rpm](http://www.macrozheng.com/#/reference/linux_command?id=rpm)

> RPM是`Red-Hat Package Manager`的缩写，一种Linux下通用的软件包管理方式，可用于安装和管理`.rpm`结尾的软件包。

- 安装软件包：

```bash
rpm -ivh nginx-1.12.2-2.el7.x86_64.rpmCopy to clipboardErrorCopied
```

- 模糊搜索软件包：

```bash
rpm -qa | grep nginxCopy to clipboardErrorCopied
```

- 精确查找软件包：

```bash
rpm -qa nginxCopy to clipboardErrorCopied
```

- 查询软件包的安装路径：

```bash
rpm -ql nginx-1.12.2-2.el7.x86_64Copy to clipboardErrorCopied
```

- 查看软件包的概要信息：

```bash
rpm -qi nginx-1.12.2-2.el7.x86_64Copy to clipboardErrorCopied
```

- 验证软件包内容和安装文件是否一致：

```bash
rpm -V nginx-1.12.2-2.el7.x86_64Copy to clipboardErrorCopied
```

- 更新软件包：

```bash
rpm -Uvh nginx-1.12.2-2.el7.x86_64Copy to clipboardErrorCopied
```

- 删除软件包：

```bash
rpm -e nginx-1.12.2-2.el7.x86_64Copy to clipboardErrorCopied
```

### [yum](http://www.macrozheng.com/#/reference/linux_command?id=yum)

> Yum是`Yellow dog Updater, Modified`的缩写，能够在线自动下载RPM包并安装，可以自动处理依赖性关系，并且一次安装所有依赖的软件包，非常方便！

- 安装软件包：

```bash
yum install nginxCopy to clipboardErrorCopied
```

- 检查可以更新的软件包：

```bash
yum check-updateCopy to clipboardErrorCopied
```

- 更新指定的软件包：

```bash
yum update nginxCopy to clipboardErrorCopied
```

- 在资源库中查找软件包信息：

```bash
yum info nginx*Copy to clipboardErrorCopied
```

- 列出已经安装的所有软件包：

```bash
yum info installedCopy to clipboardErrorCopied
```

- 列出软件包名称：

```bash
yum list nginx*Copy to clipboardErrorCopied
```

- 模糊搜索软件包：

```bash
yum search nginxCopy to clipboardErrorCopied
```

## [用户管理](http://www.macrozheng.com/#/reference/linux_command?id=用户管理)

### [用户信息查看](http://www.macrozheng.com/#/reference/linux_command?id=用户信息查看)

- 查看用户信息：

```bash
cat /etc/passwdCopy to clipboardErrorCopied
```

- 用户信息格式如下（密码已过滤）：

```bash
# 用户名:密码:用户标识号:组标识号:组注释性描述:主目录:默认shell
root:x:0:0:root:/root:/bin/bash
macro:x:1000:982:macro:/home/macro:/bin/bashCopy to clipboardErrorCopied
```

- 查看用户组信息：

```bash
cat /etc/groupCopy to clipboardErrorCopied
```

- 用户组信息格式如下：

```bash
# 组名:密码:组标识号:组内用户列表
root:x:0:
docker:x:982:macro,andyCopy to clipboardErrorCopied
```

### [passwd](http://www.macrozheng.com/#/reference/linux_command?id=passwd-1)

用于设置用户密码：

```bash
passwd rootCopy to clipboardErrorCopied
```

![img](http://www.macrozheng.com/reference/images/linux_command_11.png)

### [su](http://www.macrozheng.com/#/reference/linux_command?id=su-1)

改变用户身份（切换到超级用户）：

```bash
# 切换到root用户
su -
# 切换到macro用户
su macroCopy to clipboardErrorCopied
```

### [groupadd](http://www.macrozheng.com/#/reference/linux_command?id=groupadd)

添加用户组，使用`-g`可以设置用户组的标志号：

```bash
groupadd -g 1024 macrozhengCopy to clipboardErrorCopied
```

### [groupdel](http://www.macrozheng.com/#/reference/linux_command?id=groupdel)

删除用户组：

```bash
groupdel macrozhengCopy to clipboardErrorCopied
```

### [useradd](http://www.macrozheng.com/#/reference/linux_command?id=useradd)

添加用户，`-u`设置标志号，`-g`设置主用户组：

```bash
useradd -u 1024 -g macrozheng macroCopy to clipboardErrorCopied
```

### [usermod](http://www.macrozheng.com/#/reference/linux_command?id=usermod)

修改用户所属用户组：

```bash
usermod -g docker macroCopy to clipboardErrorCopied
```

### [userdel](http://www.macrozheng.com/#/reference/linux_command?id=userdel)

删除用户，使用`-r`可以删除用户主目录：

```bash
userdel macro -r
```