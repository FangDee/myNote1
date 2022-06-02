## MySQL数据库的卸载

### 步骤一：软件的卸载准备

学习网络编程时，TCP/IP协议程序有服务器端和客户端。mysql这个数据库管理软件是使用TCP/IP协议。我们现在要卸载的是mysql的服务器端，它没有界面。

 【计算】-->右键-->【管理】-->【服务】-->【mysql的服务】-->【停止】

### 步骤二：软件的卸载

**方式一：通过控制面板卸载**

![](imgs/MySQL卸载1.jpg)

**方式二：通过360或电脑管家等软件卸载**

![](imgs/MySQL卸载2.jpg)

![1575693828589](imgs/1575693828589.png)

 **方式三：通过安装包提供的卸载功能卸载**

![](imgs/MySQL卸载3.jpg)

或者

![1575693919722](imgs/1575693919722.png)

![1575693929346](imgs/1575693929346.png)

![1575693936722](imgs/1575693936722.png)

### 步骤三：清理残余文件

如果再次安装不成功，可以卸载后对残余文件进行清理后再安装。

（1）服务目录：mysql服务的安装目录

（2）数据目录：默认在C:\ProgramData\MySQL

如果自己单独指定过数据目录，就找到自己的数据目录，例如：

![](imgs\清理残余文件.jpg)

> 注意：请在卸载前做好数据备份
>
> 在操作完以后，需要重启计算机，然后进行安装即可。**如果仍然安装失败，需要继续操作如下步骤四。**

### 步骤四：清理注册表（选做）

如何打开注册表编辑器：在系统的搜索框中输入regedit

* HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\Eventlog\Application\MySQL服务 目录删除

* HKEY_LOCAL_MACHINE\SYSTEM\ControlSet001\Services\MySQL服务 目录删除

* HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Services\Eventlog\Application\MySQL服务 目录删除

* HKEY_LOCAL_MACHINE\SYSTEM\ControlSet002\Services\MySQL服务 目录删除

* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\Eventlog\Application\MySQL服务目录删除

* HKEY_LOCAL_MACHINE\SYSTEM\CurrentControlSet\Services\MySQL服务删除

> 注册表中的ControlSet001,ControlSet002,不一定是001和002,可能是ControlSet005、006之类

### 步骤五：清理服务列表中的服务名

![1575694218753](imgs/1575694218753.png)

如果在windows操作系统，删除注册表后，在服务列表中仍然存在原来的服务名，可以通过如下命令删除。

```
sc  delete  服务名
```

![1575694103122](imgs/1575694103122.png)

### 步骤六：清理原来的环境变量

找到path环境变量，将其中关于mysql的环境变量删除，**切记不要全部删除。**

例如：删除  D:\ProgramFiles\mysql\MySQLServer5.7\bin;  这个部分

![1575694476072](imgs/1575694476072.png)

## MySQL数据库的安装

* 双击`mysql-installer-community-5.7.28.0.msi`,进入安装界面后选择**Custom自定义安装**，点击**Next**

    ![](imgs/MySQL安装1.jpg)

* 在左侧目录中选择**MySQL Server 5.7.28 -X64**，点击`→`加载到右侧，在选择一下要安装的产品名，在右下角会出现**Advanced Options，单击它可以设置安装路径**

    ![](imgs/MySQL安装2.jpg)

* 例如将**安装路径**和**数据路径**修改到`D:\develop\`下，点击**OK**。注意，**安装目录避免空格和中文**

    ![](imgs/MySQL安装3.jpg)

* 点击**Next下一步**

    ![](imgs/MySQL安装4.jpg)

    

* 点击**Execute执行安装**

    有的可能会出现提示需要先安装C++动态库等，那就先安装C++动态库，没有提示说明操作系统中有你需要的动态库。

    ![image-20200921082804582](imgs/image-20200921082804582.png)

    ![](imgs/MySQL安装6.jpg)

* 安装完成，点击**Next下一步**

    ![](imgs/MySQL安装7.jpg)

* 点击**Next下一步**

    ![](imgs/MySQL安装8.jpg)

* 点击**Next下一步**

    ![](imgs/MySQL安装9.jpg)

* 将MySQL配置类型选择**Development Computer开发电脑设置**，点击**Next下一步**

    ![](imgs/MySQL安装10.jpg)

* 设置MySQL密码，因为管理员用户为root，学习初期建议将MySQL数据库密码也设置为root，设置完成后，点击**Next下一步**

    ![](imgs/MySQL安装11.jpg)

* 设置**Windows Service 系统服务**，设置完毕后点击**Next下一步**

    ![](imgs/MySQL安装12.jpg)

* 日志设置，不做任何修改，点击**Next下一步**

    ![](imgs/MySQL安装13.jpg)

* 高级设置，不做任何修改，点击**Next下一步**

    ![](imgs/MySQL安装14.jpg)

* 点击**Execute执行设置**

    ![](imgs/MySQL安装15.jpg)

* 安装完成，点击**Finish完成**

    ![](imgs/MySQL安装16.jpg)

## MySQL数据库服务的启动和停止

MySQL软件的服务器端必须先启动，客户端才可以连接和使用使用数据库。

### 方式一：图形化方式

* 计算机（点击鼠标右键）==》管理（点击）==》服务和应用程序（点击）==》服务（点击）==》MySQL57（点击鼠标右键）==》启动或停止（点击）
* 控制面板（点击）==》系统和安全（点击）==》管理工具（点击）==》服务（点击）==》MySQL57（点击鼠标右键）==》启动或停止（点击）
* 任务栏（点击鼠标右键）==》启动任务管理器（点击）==》服务（点击）==》MySQL57（点击鼠标右键）==》启动或停止（点击）

### 方式二：命令行方式

```cmd
启动 MySQL 服务命令：
net start MySQL57

停止 MySQL 服务命令：
net stop MySQL57
```

## MySQL数据库环境变量的配置

![1575694347758](imgs/1575694347758.png)

| 环境变量名 | 操作 |              环境变量值              |
| :--------: | :--: | :----------------------------------: |
| MYSQL_HOME | 新建 | D:\ProgramFiles\mysql\MySQLServer5.7 |
|    path    | 编辑 |           %MYSQL_HOME%\bin           |

或者直接

| 环境变量名 | 操作 |                环境变量值                |
| :--------: | :--: | :--------------------------------------: |
|    path    | 编辑 | D:\ProgramFiles\mysql\MySQLServer5.7\bin |



## MySQL数据库客户端的登录

### 方式一：MySQL自带客户端

开始菜单==》所有程序==》MySQL==》MySQL Server 5.7==》MySQL 5.7 Command Line Client

![1554910754482](imgs/1554910754482.png)

> 说明：仅限于root用户

### 方式二：命令行

**mysql -h 主机名 -P 端口号 -u 用户名 -p密码**

```sql
例如：mysql -hlocalhost -P3306 -uroot -proot   
```

注意：

（1）-p与密码之间不能有空格，其他参数名与参数值之间可以有空格也可以没有空格

```sql
mysql -hlocalhost -P3306 -uroot -proot
```

（2）密码建议在下一行输入

```sql
mysql -h localhost -P 3306 -u root -p
Enter password:****
```

（3）如果是连本机：-hlocalhost就可以省略，如果端口号没有修改：-P3306也可以省略

  简写成：

```sql
mysql -u root -p
Enter password:****
```

![1554910994523](imgs/1554910994523.png)

连接成功后，有关于MySQL Server服务版本的信息，还有第几次连接的id标识。

也可以在命令行通过以下方式获取MySQL Server服务版本的信息

![1554911047074](imgs/1554911047074.png)

或**登录**后，通过以下方式查看当前版本信息：

![1554911062159](imgs/1554911062159.png)

### 方式三：可视化工具

例如：Navicat Preminum，SQLyog 等工具

还有其他工具：mysqlfront,phpMyAdmin

#### SQLyog

![1554912367592](imgs/1554912367592.png)

![](imgs/1554912376445.png)

#### Navicat

![](imgs/1557378069584.png)

## MySQL数据库的编码配置(可以不做)

### 命令行操作sql乱码问题

```mysql
mysql> INSERT INTO t_stu VALUES(1,'张三','男');
ERROR 1366 (HY000): Incorrect string value: '\xD5\xC5\xC8\xFD' for column 'sname' at row 1
```

### 修改my.ini配置文件

```ini
在路径：D:\ProgramFiles\mysql\MySQLServer5.7Data 找到my.ini文件

修改内容1：
	找到[mysql]命令，大概在63行左右，在其下一行添加 
		default-character-set=utf8
修改内容2:
	找到[mysqld]命令，大概在76行左右，在其下一行添加
		character-set-server=utf8
		collation-server=utf8_general_ci

修改完毕后，重启MySQL57服务
```

查看编码命令

```sql
show variables like 'character_%';
show variables like 'collation_%';
```

![](imgs/MySQL编码1.jpg)

![](imgs/MySQL编码2.jpg)

* 如果是以上配置就说明对了

### 命令行操作sql仍然乱码问题

```mysql
mysql> INSERT INTO t_stu VALUES(1,'张三','男');
ERROR 1366 (HY000): Incorrect string value: '\xD5\xC5\xC8\xFD' for column 'sname' at row 1
```

原因：按照刚才的配置，服务器端认为你的客户端的字符集是utf-8，而实际上你的客户端的字符集是GBK。

![1557458006195](imgs/1557458006195.png)

查看所有字符集：**SHOW VARIABLES LIKE 'character_set_%';**

![1557458019409](imgs/1557458019409.png)

解决方案，设置当前连接的客户端字符集 **“SET NAMES GBK;”**

set names gbk;是为了告诉服务器,客户端用的GBK编码,防止乱码。

![1557458032334](imgs/1557458032334.png)

有的时候，这样还不能解决，例如，某些win10操作系统环境下，那么修改命令行的属性-》选项-》勾选使用旧版控制台，然后重启电脑

![1560996958604](imgs/1560996958604.png)