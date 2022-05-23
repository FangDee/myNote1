#  mysql进阶-上

## 第一章 mysql架构

### 一、mysql的系统架构

#### 1、 数据库和数据库实例

在MySQL的学习研究中，存在几个非常容易混淆的概念，即【数据库】、【数据库软件】和【数据库实例】：

- 数据库：按照数据结构来组织、存储和管理数据的仓库，通常由数据库管理系统进行管理。
- 数据库管理软件（RDBMS）：就是我们说的数据库管理系统软件，他强调软件。
- 数据库实例：启动数据库软件，在内存中运行一个独立进程，用来操作数据，这个正在运行的进程就是一个数据库实例，理论上可以在一台电脑上启动多个数据库实例，当然要监听在不同的端口。

#### 2、MySQL架构

复杂的架构是为了更好的工作，架构中的每一个角色都可以高效的单独处理一类事件，辅助整个系统的流畅运行，举个例子：

每天有很多人拜访市长，为了能合理的给市长安排拜访工作，需要对拜访流程做出复杂设计，比如先要在门卫处做身份认证、由传达室负责接通电话确认是否可以访问、市长办公室负责接待、你可能需要排队等候、你的事情如果办公室就能解决可能就不用见市长了，最后轮到你了，你才能见上市长，整个拜访流程就是设计的架构。

对于MySQL来说，虽然经历了多个版本迭代（MySQL5.5,MySQL 5.6,MySQL 5.7,MySQL 8）,但每次的迭代，都是基于MySQL基架的，MySQL基架大致包括如下几大模块组件，如下图：

![image-20220425170356906](https://www.ydlclass.com/doc21xnv/assets/image-20220425170356906.b2a52c35.png)

**（1）MySQL向外提供的交互接口（Connectors）**

Connectors组件，是MySQL向外提供的交互组件，如java,.net,php等语言可以通过该组件来操作SQL语句，实现与SQL的交互。通过客户端/服务器通信协议与MySQL建立连接。MySQL 客户端与服务端的通信方式是 “ 半双工 ”。对于每一个 MySQL 的连接，时刻都有一个线程状态来标识这个连接正在做什么。

**（2）管理服务组件和工具组件(Management Service & Utilities)**

提供MySQL的各项服务组件和管理工具，如备份(Backup)，恢复(Recovery)，安全管理(Security)等功能。

**（3）连接池组件(Connection Pool)**

负责监听客户端向MySQL Server端的各种请求，接收请求，转发请求到目标模块。每个成功连接MySQL Server的客户请求都会被创建或分配一个线程，该线程负责客户端与MySQL Server端的通信，接收客户端发送的命令，传递服务端的结果信息等。

**（4）SQL接口组件(SQL Interface)**

接收用户SQL命令，如DML,DDL和存储过程等，并将最终结果返回给用户。

**（5）查询分析器组件(Parser)**

首先分析SQL命令语法的合法性，并进行抽象语法树解析，如果sql有语法错误，会抛出异常信息。

**（6）优化器组件（Optimizer）**

对SQL命令按照标准流程进行优化分析，mysql会按照它认为的最优方式进行优化，选用成本最小的执行计划。

**（7）缓存主件（Caches & Buffers）**

缓存和缓冲组件，这里边的内容我们后边会详细的讲解。

**（8）MySQL存储引擎**

MySQL属于关系型数据库，而关系型数据库的存储是以表的形式进行的，对于表的创建，数据的存储，检索，更新等都是由MySQL存储引擎完成的。

MySQL存储引擎在MySQL中扮演着重要角色。研究过SQL Server和Oracle的读者可能很清楚，这两种数据库的存储引擎只有一个，而MySQL的存储引擎种类比较多，如MyIsam存储引擎，InnoDB存储引擎和Memory存储引擎。

因为mysql本身就是开源的，他允许第三方基于MySQL骨架，开发适合自己业务需求的存储引擎。从MySQL存储引擎种类上来说，可以分为官方存储引擎和第三方存储引擎，比较常用的存储引擎包括InnoDB存储引擎，MyIsam存储引擎和Momery存储引擎。

**查询流程大致如下：**

![image-20220424195802351](https://www.ydlclass.com/doc21xnv/assets/image-20220424195802351.3a69069f.png)

**小问题：MySQL8.0为什么取消了查询缓存？**

【MySQL缓存机制】简单的说就是缓存sql文本及查询结果，如果运行完全相同的SQL，服务器直接从缓存中取到结果，而不需要再去解析和执行SQL。但如果表中任何数据或是结构发生改变，包括INSERT、UPDATE、DELETE、TRUNCATE、ALTER TABLE、DROP TABLE或DROP DATABASE等，那么使用这个表的所有缓存查询将不再有效，查询缓存中相关条目被清空。缓存是对系统性能优化的重要手段。但是有经验的DBA都建议生产环境中把MySQL Query Cache关闭。MySQL8.0更是直接取消了查询缓存，其原因有下：

- MySQL会对每条接收到的SELECT类型的查询进行hash计算，然后查找这个查询的缓存结果是否存在。虽然hash计算和查找的效率已经足够高了，一条查询语句所带来的开销可以忽略，但一旦涉及到高并发，有成千上万条查询语句时，hash计算和查找所带来的开销就必须重视了。
- 查询语句的字符大小写、空格或者注释的不同，Query Cache都会认为是不同的查询（因为他们的hash值会不同）。
- 当向某个表写入数据的时候，必须将和这个表相关的所有缓存设置为失效，如果缓存内容很多，则消耗也会很大，可能使系统僵死，因为这个操作是靠全局锁操作来保护的。

当然还有一些其他原因，我们学习的过程中慢慢体会。

### 二、目录结构

#### 1、windows中的目录

在mysql启动的时候，会从【安装目录】加载软件数据，在运行过程中，会从【数据目录】中读取数据。这两个目录我们不要放在一起，避免重新安装软件导致数据丢失。

##### （1） mysql安装目录

默认安装目录：C:\Program Files\MySQL

![image-20220424195435853](https://www.ydlclass.com/doc21xnv/assets/image-20220424195435853.ffca9813.png)

- `bin`目录：用于放置一些可执行的工具文件，如mysql.exe、mysqld.exe、mysqlshow.exe等。
- `include`目录：用于放置一些头文件，如：mysql.h、mysql_ername.h等。
- `lib`目录：用于放置一系列库文件。

##### [#](https://www.ydlclass.com/doc21xnv/database/mysqladvance/mysqlAdvance1.html#_2-数据文件目录-重要)（2）数据文件目录（重要）

默认数据目录：C:\ProgramData\MySQL\MySQL Server 8.0，注意ProgramData是一个隐藏目录，需要设置为【显示隐藏文件】：

![image-20220424195619219](https://www.ydlclass.com/doc21xnv/assets/image-20220424195619219.9d7ce386.png)

`data`目录：用于放置一些日志文件以及数据库。

我们发现在data目录保存的是我们的真是的数据，每个数据库一个文件夹：

![image-20220424235111862](https://www.ydlclass.com/doc21xnv/assets/image-20220424235111862.444cb544.png)

每张表又是一个独立的文件，不同的存储引擎的文件存储方式不同：不仅能快没了，

![image-20220424235244545](https://www.ydlclass.com/doc21xnv/assets/image-20220424235244545.35dc0cdb.png)

- my.ini的部分截图如下：

  ![image-20220424234944753](https://www.ydlclass.com/doc21xnv/assets/image-20220424234944753.ede12acc.png)

配置文件很重要，所谓配置文件就是配置一下你的mysql让他成为你想要的的样子，这里可以配置大量的信息，我们放在文档最后的附录。

#### 2、linux中的文件目录

linux的各项目录可以在我们安装时自由选定，我们选取一个云服务器，看看里边的mysql的目录结构：

（1）我们可以通过配置文件查看当前mysql的一些基本信息，linux中的配置文件在etc目录 ，名称为my.cnf，如下：

![image-20220507095721892](https://www.ydlclass.com/doc21xnv/assets/image-20220507095721892.f5e6adbd.png)

（2）bin目录，一些可执行文件，包括

![image-20220507102512199](https://www.ydlclass.com/doc21xnv/assets/image-20220507102512199.e2f47841.png)

**bin目录工具汇总：**

> MySQL服务器端工具

- mysqld：SQL后台保护程序(MySQL服务器进程)。该程序必须运行之后。客户端才能通过连接服务器端程序访问和操作数据库。
- mysqld_safe：MySQL服务脚本。mysql_safe增加了一些安全特性，如当出现错误时重启服务器，向错误日志文件写入运行时间信息。
- mysql.server：MySQL服务启动服本。调用mysqld_safe来启动MySQL服务。
- mysql_multi：服务器启动脚本，可以启动或停止系统上安装的多个服务。
- myiasmchk：用来描述、检查、优化和维护MyISAM表的实用工具。
- mysqlbu：MySQL缺陷报告脚本。它可以用来向MySQL邮件系统发送缺陷报告。
- mysql_install_db：用于默认权限创建MySQ授权表。通常只是在系统上首次安装MySQL时执行一次。

> MySQL客户端工具

- mysql：交互式输入SQL语句或从文件以批处理模式执行SQL语句来操作数据库管理系统，就是我们的客户端。
- mysqldump：将MySQL数据库转储到一个文件，可以用来备份数据库。
- mysqladmin：用来检索版本、进程、以及服务器的状态信息。
- mysqlbinlog：用于从二进制日志读取语句。在二进制日志文件中包含执行的语句，可用来帮助系统从崩溃中恢复。
- mysqlcheck：检查、修复、分析以及优化表的表维护。
- mysqlhotcopy：当服务器在运行时，快速备份MyISAM或ISAM表的工具。
- mysql import：使用load data infile将文本文件导入相关表的客户程序。
- perror：显示系统或MySQL错误代码含义的工具。
- myisampack：压缩MyISAP表，产生更小的只读表。
- mysaqlaccess：检查访问主机名、用户名和数据库组合的权限。

mysql是一个很常用的【mysql客户端工具】，我们可以使用一下命令连接本机或者远程的mysql服务：

```bash
 mysql -h 124.220.197.17 -P 3306 -uroot -p
```

**tips**：我们看到在配置文件中有一个socket的配置，socket 即 Unix 域套接字文件，在类 unix 平台，客户端连接 MySQL 服务端的方式有两种，分别是 TCP/IP 方式与 socket 套接字文件方式。Unix 套接字文件连接的速度比 TCP/IP 快，但是只能连接到同一台计算机上的服务器使用。通过设置 socket 变量可配置套接字文件路径及名称，默认值为 /tmp/mysql.sock。本地客户端的连接默认会使用到该文件：

```bash
mysql -uroot -p -S /tmp/mysql.sock
```

如果mysql.sock文件误删的话，就需要重启mysql服务

![image-20220507102421872](https://www.ydlclass.com/doc21xnv/assets/image-20220507102421872.f0e9cd30.png)

接着给大家介绍一个【数据备份工具】，mysqldump可以用来实现轻量级的【快速迁移或恢复数据库】。 mysqldump是将数据表导成 SQL 脚本文件，在不同的 MySQL 版本之间升级时相对比较合适，这也是最常用的备份方法。mysqldump一般在数据量很小的时候（几个G）可以用于 备份。当数据量比较大的情况下，就不建议用mysqldump工具进行备份了。下边我们简单的使用mysqldump工具进行备份数据，命令如下：

```sql
-- 备份一个表
mysqldump -u root -p ydlclass ydl_user  > ~/dump.txt
-- 备份一个数据库
mysqldump -u root -p ydlclass  > ~/dump.txt
-- 备份所有数据库
mysqldump -u root -p --all-databases > dump.txt

备份完成之后使用
```

恢复数据，使用mysql指令：

```sql
mysql -u root -p ydl < ~/dump.txt
```

（3）数据库文件，该文件我们同样可以自由指定，该文件夹包含了日志文件，以及我们的数据文件，这些在后边的课程中会详细介绍：

![image-20220507112543589](https://www.ydlclass.com/doc21xnv/assets/image-20220507112543589.288eb8e4.png)

### 三、字符集和排序规则

mysql支持大量的字符集，但是我们通常使用的是utf8，【show collation】命令可以查看mysql支持的所有的排序规则和字符集，如下所示部分：

![image-20220427114132237](https://www.ydlclass.com/doc21xnv/assets/image-20220427114132237.2a65cd95.png)

由上图可知，一种字符集对应的很多比较规则。

举个例子：

- utf8-polish-ci，表示utf-8的字符集的波兰语的比较规则，ci代表忽略大小写。
- utf8-general-ci，就是通用的忽略大小写的utf8字符集比较规则。
- utf8mb4_0900_ai_ci中的0900指的是Unicode 9.0的规范，后边的后缀代表不区分重音也不区分大小写，他是utf8mb4字符集一个新的通用排序归则。

| 后缀 | 英文               | 描述                     |
| ---- | ------------------ | ------------------------ |
| _ai  | accent insensitive | 不区分重音（è，é，ê和ë） |
| _as  | accent sensitive   | 区分重音                 |
| _ci  | case insensitive   | 不区分大小写             |
| _cs  | case sensitive     | 区分大小写               |
| _bin | binary             | 以二进制的形式进行比较   |

utf8和utf8mb4的区别：

- utf8mb3(utf-8)：使用1~3个字节表示字符，utf8默认就是utf8mb3。
- utf8mb4：使用1~4个字节表示字符，他是utf8的超集，甚至可以存储很多【emoji表情😀😃😄😁😆】，mysql8.0已经默认字符集设置为utf8mb4。

【字符集】和【比较规则】可以作用在全局、数据库、表、甚至是列级别：

全局：

mysql提供两个变量，进行全局设置。【character_set_server】和【collate_server】对全局的字符集和排序规则进行设置。这两个设置可以在配置文件中修改。

![image-20220427160641490](https://www.ydlclass.com/doc21xnv/assets/image-20220427160641490.e72633a5.png)

在创建表时，可以对数据库、表、列指定字符集和排序规则：

```sql
-- 指定数据库
create database 数据库名 character set 字符集  collate 比较规则
-- 
create table 表名(
	列名 列类型 
) character set 字符集 collate 比较规则

create table 表名(
	列名 列类型 [character set 字符集] [collate 比较规则]
)
```

### 四、mysql修改配置的方法

在mysql中变量分为全局变量和会话变量，我们一一讲解：

#### 1、全局变量

（1）查看全局变量

我们查看一个全局变量wait_timeout的值（这个值代表，客户端和服务器的连接不生交互后多久自动断开连接），语法如下：

```sql
show global variables like '%wait_timeout%';
select @@global.wait_timeout;
```

（2）设置全局变量方法1，修改配置文件, 然后重启mysqld：

```ini
# vi /etc/my.cnf
[mysqld]
wait_timeout=10000
# service mysqld restart
```

（3）设置全局变量方法2（推荐），在命令行里通过SET来设置：

如果要修改全局变量, 必须要显示指定"GLOBAL"或者"@@global."，同时必须要有SUPER权限.：

```sql
set global wait_timeout=10000;
set @@global.wait_timeout=10000;
```

然后查看设置是否成功:

```sql
show global variables like 'wait_timeout'
select @@global.wait_timeout
```

#### 2、当前会话的变量

如果要修改会话变量值, 可以指定"SESSION"或者"@@session."或者"@@"或者"LOCAL"或者"@@local.", 或者什么都不使用。语法语法：

```sql
mysql> set wait_timeout=10000;
mysql> set session wait_timeout=10000;
mysql> set local wait_timeout=10000;
mysql> set @@wait_timeout=10000;
mysql> set @@session.wait_timeout=10000;
mysql> set @@local.wait_timeout=10000;
```

然后查看设置是否成功:

```sql
mysql> select @@wait_timeout;
mysql> select @@session.wait_timeout;
mysql> select @@local.wait_timeout;
mysql> show variables like 'wait_timeout';
mysql> show local variables like 'wait_timeout';
mysql> show session variables like 'wait_timeout';
```

### 五、内置数据库

- mysql：这个库很重要，他是mysql的核心数据库，负责存储数据库的用户、权限设置、关键字等mysql自己需要使用，控制和管理的信息。
- information_schema：这个数据库维护了数据库其他表的一些描述性信息，也称为元数据。比如，当前有哪些表，哪些视图，哪些触发器，哪些列等。
- performation_schema：这个数据库用来存储mysql服务器运行过程中的一些状态信息，是做性能监控的。比如最近执行了什么sql语句，内存使用情况等
- sys：结合information_schema和performation_schema的数据，能更方便的了解mysql服务器的性能信息。

## 第二章 I/O和存储

**谈谈性能：**mysql通常决定了一个系统的性能瓶颈，执行一条sql的成本主要在于以下两个方面：

- I/O成本：我们经常使用的MyIsam和InnoDB存储引擎都是将数据存储在磁盘上，当查询表中的记录时，需要先将数据加载到内存中，然后进行操作，这个从磁盘到内存的加载过程损耗的时间成为I/O成本。
- CPU成本：读取记录以及检测记录是否满足对应的搜索条件、对结果集进行排序等这些操作所消耗的时间称之为CPU成本。

本章节我们聊一聊I/O成本。

### 一、IO成本

我们想想要了解mysql的I/O成本，就需要知道计算机的磁盘是怎么工作的。

#### 1、磁盘的结构

![image-20201122150129276](https://www.ydlclass.com/doc21xnv/assets/image-20201122150129276.837e618f.png)

##### （1）盘片、片面和磁头

硬盘中一般会有多个【盘片】组成，每个盘片包含【两个面】，每个盘面都对应的有一个【读/写磁头】。受到硬盘整体体积和生产成本的限制，盘片数量都受到限制，一般都在5片以内。盘片的编号【自下向上】从0开始，如最下边的盘片有0面和1面，再上一个盘片就编号为2面和3面。

结构示意图，如图：

![image-20220424195930213](https://www.ydlclass.com/doc21xnv/assets/image-20220424195930213.55095177.png)

##### （2）扇区和磁道

下图显示的是一个盘面，盘面中一圈圈灰色同心圆为一条条磁道，从圆心向外画直线，可以将磁道划分为若干个弧段，每个磁道上一个弧段被称之为一个扇区（图践绿色部分）。扇区是磁盘的最小组成单元，通常是512字节。（由于不断提高磁盘的大小，部分厂商设定每个扇区的大小是4096字节）；

![image-20220424195949755](https://www.ydlclass.com/doc21xnv/assets/image-20220424195949755.9516f689.png)

##### （3）磁头和柱面

硬盘通常由重叠的一组盘片构成，每个盘面都被划分为数目相等的磁道，并从外缘的“0”开始编号，具有相同编号的磁道形成一个圆柱，称之为磁盘的柱面。磁盘的柱面数与一个盘面上的磁道数是相等的。由于每个盘面都有自己的磁头，因此，盘面数等于总的磁头数。 如下图

![image-20220425153537739](https://www.ydlclass.com/doc21xnv/assets/image-20220425153537739.a5e991d6.png)

#### 2、磁盘容量计算

存储容量 ＝ 磁头数 × 磁道(柱面)数 × 每道扇区数 × 每扇区字节数

图3中磁盘是一个 3个圆盘6个磁头，7个柱面（每个盘片7个磁道） 的磁盘，图3中每条磁道有12个扇区，所以此磁盘的容量为：

存储容量 6 * 7 * 12 * 512 = 258048

**tips：**有些【古老硬盘】每个磁道的扇区数一样，外圈的密度小，内圈的密度大，每圈可存储的数据量是一样的。现在的硬盘数据的密度都一致，这样磁道的周长越长，扇区就越多，存储的数据量就越大。

#### 3、磁盘读取响应时间

1. 寻道时间：磁头从开始移动到数据所在磁道所需要的时间，寻道时间越短，I/O操作越快，目前磁盘的平均寻道时间一般在3－15ms，一般都在10ms左右。
2. 旋转延迟：盘片旋转将请求数据所在扇区移至读写磁头下方所需要的时间，旋转延迟取决于磁盘转速。普通硬盘一般都是7200rpm，大概一圈8ms，慢的5400rpm。
3. 数据传输时间：完成传输所请求的数据所需要的时间。 小结一下：从上面的指标来看、其实最重要的、或者说、我们最关心的应该只有两个：寻道时间；旋转延迟。

读写一次磁盘信息所需的时间可分解为：寻道时间、延迟时间、传输时间。为提高磁盘传输效率，软件程序应着重考虑减少寻道时间和延迟时间。

下图是计算机硬件延迟的对比图，供大家参考：

![image-20220425153556197](https://www.ydlclass.com/doc21xnv/assets/image-20220425153556197.aef612b3.png)

#### 4、交换单位

【块】是操作系统中最小的逻辑存储单位，他是虚拟出来的一个单位。操作系统与磁盘打交道的最小单位是磁盘块。每个块可以包括2、4、8、16、32、64…2的n次方个扇区。

**为什么存在磁盘块？**

- 读取方便：由于扇区的容量比较小，数目众多，在寻址时比较困难，所以操作系统就将相邻的扇区组合在一起，形成一个块，再对块进行整体的操作。
- 分离对底层的依赖：操作系统忽略对底层物理存储结构的设计。通过虚拟出来磁盘块的概念，在系统中认为块是最小的单位。

**扇区、块/簇、page的关系：**

1. 扇区： 硬盘的最小读写单元
2. 块/簇： 是操作系统针对硬盘读写的最小单元
3. page： 是内存与操作系统之间操作的最小单元

扇区 <= 块/簇 <= page

#### 5、局部性原理与磁盘预读

由于存储介质的特性，磁盘本身存取就【比主存慢很多】，再加上机械运动耗费，磁盘的存取速度往往是主存的【十万分之一】，因此为了提高效率，要【尽量减少磁盘I/O】。也是因为这个原因，磁盘往往不是严格的【按需读取】，而是每次都会预读，即使只需要一个字节，磁盘也会从这个位置开始，顺序向后读取一定长度的数据放入内存。这样做的理论依据是计算机科学中著名的**局部性原理**。

**局部性原理：**当一个数据被用到时，其附近的数据也通常会马上被使用，程序运行期间所需要的数据通常比较集中。

由于磁盘【顺序读取】的效率很高（不需要寻道时间，只需很少的旋转时间），因此对于具有局部性的程序来说，预读可以提高I/O效率。

预读的长度一般为【页（page）】的整倍数（在许多操作系统中，页得大小通常为4k）。当程序要读取的数据不在主存中时，会触发一个缺页异常，此时系统会向磁盘发出读盘信号，磁盘会找到数据的起始位置并向后连续读取一页或几页载入内存中，然后异常返回，程序继续运行。

### 二、数据存储

对于mysql而言，数据是存储在文件系统中的，不同的存储存储引擎会有不同的文件格式和组织形式，我们还是以innodb为例给大家讲解。

#### 1、innodb数据存储

对于innodb而言，数据是存储在表空间（文件空间file space）内的，表空间是一个抽象的概念，他对应着硬盘上的一个或多个文件，如下图：

![image-20220427104757135](https://www.ydlclass.com/doc21xnv/assets/image-20220427104757135.00dc235b.png)

表空间存储数据的单位是【页】，我们可以这样类比，一个表空间就是个大大的本子，本子里是一页页的数据（innodb是以页为单位进行数据存储的），常用页面类型有很多，不同类型的页面可以存放【不同类型的数据】，这里不展开讲解，暂时统称为【数据页】、他的通用部分如下，每一页大概占用16k的空间：

![image-20220426220208824](https://www.ydlclass.com/doc21xnv/assets/image-20220426220208824.0063a836.png)

- file header：记录页面的一些通用信息，比如当前页的校验和、页号、上页号、下页号、所属表空间等。
- file trailer：主要的工作是检验页是否完整。
- 表空间中的每一个页，都有一个页号（File_PAGE_OFFSET），我们可以通过这个页号在表空间快速定位到指定的页面。这个页号由4个字节组成，也就是32位，所有最多能存放2的32次方页，如果按照一页16k计算，一个表空间最大支持【64TB】的数据。整体的排列中页是连续的，但是页有上下指针，不连续的页也能组成链表。

表空间的示意图如下：

![image-20220427154308986](https://www.ydlclass.com/doc21xnv/assets/image-20220427154308986.93b2bedb.png)

表空间可以分为系统表空间、独立表空间等：

##### （1）系统表空间（The System Tablespace）

- 系统表空间包含了很多【公共数据】，比如InnoDB的数据字典，回滚信息、系统事物信息、二次写缓冲等，老版本的mysql表中的数据也会存储在系统表空间。
- 系统表空间是一个共享的表空间因为它是被多个表共享的。
- 该空间的数据文件通过参数【innodb_data_file_path】控制，默认值是`ibdata1:12M:autoextend`（文件名为ibdata1、12MB、自动扩展）。
- 当然系统表空间也可以通过配置，修改文件的名称和个数。文件如下图：

![image-20220427181352706](https://www.ydlclass.com/doc21xnv/assets/image-20220427181352706.d43290e2.png)

相关变量的设置：

```sql
-- 如果1代表开启，0代表关闭
show variables like'innodb_file_per_table'
-- 设置对应的变量
set global innodb_file_per_table=0;
-- 查看系统表空间的配置
show variables like "innodb_data_file_path";
-- 配置文件的配置
innodb_data_file_path=data1:512M;data2:512M:autoextend
```

##### （2）独立表空间（File-Per-Table Tablespaces）

- 独立表空间是默认开启的，在5.6.6以后，Innodb不在默认将各个表的数据存储在【系统表空间】当中，而是会为每一个表建立一个独立表空间，innodb存储引擎的独立表空间为【.ibd】文件。
- 如果启用了【innodb_file_per_table】参数，需要注意的是每张表的表空间内存放的只是【数据】、【索引】和【插入缓冲Bitmap页】，其他数据如：回滚信息、系统事物信息、二次写缓冲（Double write buffer）等还是放在原来的系统表空间内。
- 同时说明了一个问题：即使启用了【innodb_file_per_table】参数，系统表空间还是会不断的增加其大小的。

![image-20220427163520350](data:image/png;base64,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)

##### （3）其他类型的表空间，

除了以上两种表空间，，innodb还提供了很多其他类型的表空间，比如通用表空间，undolog表空间、临时表空间等，这里不在赘述。

**tips：**MyIsam数据存储

MyIsam没有表空间的概念，他会在目录中产生2个文件【.MYD】（数据文件）、【 .MYI】（索引文件）三个文件。

![image-20220427164333413](https://www.ydlclass.com/doc21xnv/assets/image-20220427164333413.20982ce1.png)

**注意：**在5.7以前【数据文件】和【表信息文件】是分开的，相互独立的。会多一个【.frm】文件，8.0之后进行了合并。

#### 2、组织结构

- 区（extent）：：每一个表空间保存了大量的页，为了更好的管理这些页面，Innodb提出了【区】的概念，对于16k的页，连续64个页就是一个区，大概1M的空间，每一个表空间都是由若干个连续的区组成的，每256个区被划分为一组。
- 段（Segment）：分为索引段，数据段，回滚段等后边会将，段是为了区分不同的数据类型，相同的段存的数据类型是一致的。一个段包含256个区(256M大小)。

inndb表空间结构如下图所示：

![image-20220505142327089](https://www.ydlclass.com/doc21xnv/assets/image-20220505142327089.786fc468.png)

#### 2、Row Format(行记录格式)

- 一个表的【行记录格式】决定表中行的【物理存储模式】，决定了【DQL】和【DML】的操作性能。越多的行被匹配进独立的磁盘页，sql的性能会更好一些，需要的缓存及io操作就越少。
- 一条完整的信息记录分为：【记录的额外信息】和【记录的真实数据】两大部分，就如同一箱苹果里的苹果分为包装和苹果一样。
- 我们可以通过命令`SHOW TABLE STATUS LIKE 'table_name'`来查看当前表使用的行格式，其中row_format就代表了当前使用的行记录结构类型。

指定行格式的语法如下：

```sql
-- 创建数据表时,显示指定行格式
CREATE TABLE 表名 (列的信息) ROW_FORMAT=行格式名称;
-- 创建数据表时,修改行格式
ALTER TABLE 表名 ROW_FORMAT=行格式名称;

-- 具体如下：
CREATE TABLE `ydl_user`  (
  `user_id` bigint NOT NULL AUTO_INCREMENT COMMENT '用户ID',
  `user_name` varchar(30) CHARACTER SET utf8mb4 COLLATE utf8mb4_0900_ai_ci NOT NULL COMMENT '用户账号',
  ....
  PRIMARY KEY (`user_id`) USING BTREE
) ROW_FORMAT = DYNAMIC;
```

##### **（1）COMPACT**

【compact行记录】是在MySQL 5.0时被引入的，其设计目标是能高效存放数据。Compact行记录以如下方式进行存储：

![image-20220505142826773](https://www.ydlclass.com/doc21xnv/assets/image-20220505142826773.83792245.png)

- 第一个部分是一个非NULL【变长字段长度列表】。这个其实很好理解，我们的数据类型除了定长的char、int还有不定长的如varchar、text等，变长列的真实长度就保存在这个部分，他是按照列的顺序【逆序放置】的。当列的长度小于255字节，如（varchar(50)），用1字节表示；若大于255个字节（varchar(600)），用2个字节表示，这其实也就说明了为什么**varchar的最大长度是65536**。
- 第二个部分是【NULL标志位】，他指示了当前行数据中哪些为null值，用一个bitmap表示。举一个例子：假如该标志位为06(二进制：00000110)则表示第二三列（可以为空的列）的数据为NULL。需要注意的是，NULL值标志位仅仅针对可以为【NULL的字段】，如果某个字段被定义为not null，那么这个字段就不会进入NULL值标志位的BitMap中，这样可以节省很多空间，NULL值标志位也是逆序排列，占用空间按照字节数高位补零，如有九个字段可以为空（00000001 01010101）。
- 第三部分为记录头信息（record header），固定占用5个字节（40位），每位的含义见下表：

![image-20220505142851556](https://www.ydlclass.com/doc21xnv/assets/image-20220505142851556.e87bd07f.png)

- 第四部分就是实际存储的每个列的数据了，需要特别注意的是，NULL不占该部分任何数据，即NULL除了占有NULL标志位，实际存储不占有任何空间。Innodb存储变长列（VARCHAR, VARBINARY, BLOB, TEXT）的前768字节，剩下的部分存储在溢出页中。固定长度列，超过768字节的视为变长列。内部存储前768字节，20字节指针存储列的溢出页的地址，所以长度为768+20字节。

  ![image-20220505181612836](https://www.ydlclass.com/doc21xnv/assets/image-20220505181612836.38c0d037.png)

**注意：**另外有一点需要注意的是，每行数据除了用户定义的列外，**还有两个隐藏列，事务ID列和回滚指针列**，分别为6个字节和7个字节的大小。若InnoDB表没有定义Primary Key，每行还会增加一个【6字节的RowID列】。

**例子：**我们不妨举一个例子，看看硬盘的存储格式，以下表为准：

```sql
create table test (
　　t1 varchar(10),
　　t2 varchar(10),
　　t3 char(10),
　　t4 varchar(10)
) engine=innodb row_format=compact;

insert into row_test values('a','bb','bb','ccc'); 
insert into row_test values('d','ee','ee','fff'); 
insert into row_test values('d',NULL,NULL,'fff'); 
```

节选对应的【真实的表空间】中的的二进制结构表示：

```bash
                         03 02 01 00 00 00 10 00 
2c 00 00 00 00 2b 68 00  00 00 00 06 05 80 00 00 
00 32 01 10 61 62 62 62  62 20 20 20 20 20 20 20 
20 63 63 63 03 02 01 00  00 00 18 00 2b 00 00 00  
00 02 01 00 00 00 00 0f  62 c9 00 00 01 b2 01 10  
64 65 65 65 65 20 20 20  20 20 20 20 20 66 66 66  
03 01 06 00 00 20 ff 98  00 00 00 00 02 02 00 00 
00 00 0f 67 cc 00 00 01  b6 01 10 64 66 66 66 
```

第一行整理如下，需要注意，我们有三个变长列varchar：

```text
03 02 01 // 变长字段长度列表，逆序，t4列长度为3，t2列长度为2，t1列长度为1
00 // NULL标志位，第一行没有NULL值
00 00 10 00 2c // 记录头信息，固定5字节长度
00 00 00 00 2b 68 // RowID我们建的表没有主键，因此会有RowID，固定6字节长度
00 00 00 00 06 05 // 事务ID，固定6个字节
80 00 00 00 32 01 10 // 回滚指针，固定7个字节
61 // t1数据'a'
62 62 // t2'bb'
62 62 20 20 20 20 20 20 20 20 // t3数据'bb'  Ox20十进制是32对应ascii码是空字符
63 63 63 // t4数据'ccc'
```

第二行整理如下：

```bash
03 02 01 // 变长字段长度列表，逆序，t4列长度为3，t2列长度为2，t1列长度为1
00 // NULL标志位，第二行没有NULL值
00 00 18 00 2b // 记录头信息，固定5字节长度
00 00 00 00 02 01 // RowID我们建的表没有主键，因此会有RowID，固定6字节长度
00 00 00 00 0f 62 // 事务ID，固定6个字节
c9 00 00 01 b2 01 10 // 回滚指针，固定7个字节
64 // t1数据'd'
65 65 // t2数据'ee'
65 65 20 20 20 20 20 20 20 20 // t3数据'ee'
66 66 66 // t4数据'fff'
```

第三行整理如下：

```text
03 01 // 变长字段长度列表，逆序，t4列长度为3，t1列长度为1
06 // 00000110 NULL标志位，t2和t3列为空
00 00 20 ff 98  // 记录头信息，固定5字节长度
00 00 00 00 02 02 // RowID我们建的表没有主键，因此会有RowID，固定6字节长度
00 00 00 00 0f 67 // 事务ID，固定6个字节
cc 00 00 01 b6 01 10 // 回滚指针，固定7个字节
64 // t1数据'd'
66 66 66 // t4数据'fff'
```

##### （2）REDUNDANT

【Redundant】是MySQL 5.0版本之前InnoDB的行记录存储方式。Redundant行记录以如下方式存储：

![image-20220505142609716](https://www.ydlclass.com/doc21xnv/assets/image-20220505142609716.f073d2f3.png)

从上图可以看到，Redundant行格式如下：

1. 第一个部分保存了【字段长度偏移列表】，这个部分保存了该行数据所有列，包括隐藏列的长度偏移量。举一个例子说明一下偏移，假如第一个字段长度为x，第二个字段长度为y，那么列表中第一个字段就是x，第二个字段就是x+y。这个偏移列表是按照列的顺序【逆序排列】。

2. 第二个部分为记录头信息【record header】，Redundant行格式固定占用6个字节（48位），每位的含义如下图：

   有几个标志位我们可以注意一下，`n_fields`值代表一行中列的数量，占用10位，这也很好地解释了为什么MySQL【一个行支持最多的列为1023】。

![image-20220505142646478](https://www.ydlclass.com/doc21xnv/assets/image-20220505142646478.14643b59.png)

 3.第三个部分就是实际存储的每个列的数据了。

**null值的存储**，在【字段长度列表】的每个字段长度最高位标记 1 表示这个字段为 NULL。

- 对于一字节存储，通过【最高位标记字段】判断是否为 NULL，如果为 NULL，则最高位为1，否则为0。剩下的 7 位用来存储数据，所以最多是127。
- 对于两字节存储，通过【最高位标记字段】判断是否为 NULL，第二位标记这条记录是否在同一页，如果在则为0，如果不在则为1，这其实就涉及到了后面要说的溢出页。剩下的 14 位表示长度，所以最多是16383。
- 在这种类型的行格式中，无论字段是否为 NULL，或者长度是多少，**char(M) 都会占用 M \* 字节编码最大长度那么多字节**。为 NULL 的话，填充的是 0x00，不为 NULL，长度不够的情况下，末尾补充 0x20。 对于varchar来说，NULL 还是不占用空间的。

**小结：**

compact格式比redundant存储空间大约减少20%。如果受限于cache命中和磁盘速度，compact格式会快一些，若受限于CPU速度，compact格式会慢一些。

**（3）DYNAMIC**

InnoDB Plugin引入了两种新的文件格式（file format，可以理解为新的页格式），对于以前支持的Compact和Redundant格式将其称为Antelope文件格式，新的文件格式称为Barracuda。Barracuda文件格式下拥有两种新的行记录格式Compressed和Dynamic两种。

新的两种格式对于存放BLOB的数据采用了完全的行溢出的方式，在数据页中只存放20个字节的指针，实际的数据都存放在BLOB Page中，而之前的Compact和Redundant两种格式会存放768个前缀字节。mysql8.0默认此格式。

![image-20220505154503535](https://www.ydlclass.com/doc21xnv/assets/image-20220505154503535.312b70d6.png)

**（4）COMPRESSED** 基于dynamic格式，支持表和索引数据压缩。compressed行格式采用dynamic相同的页外存储细节，同时，存储在其中的行数据会以zlib的算法进行压缩，因此对于BLOB、TEXT、VARCHAR这类大长度类型的数据能进行非常有效的存储。

## 第三章 缓冲池 buffer pool

我们在之前的章节中已经介绍过了，innodb中的数据是以【页】的形式存储在磁盘上的表空间内，但是我们一再强调过，【磁盘的速度】和【内存】相比简直不值一提，而【内存的速度】和【cpu的速度】同样不可同日而语，对于数据库而言，I/O成本永远是不可忽略的一项成本，我们不妨思考下面的小问题：

**小问题：一个全表扫描会产生有多少次磁盘I/O?**

```sql
select * from user where id between 10 and 1000;
```

- 访问id为1的数据，需要访问当前表空间的第一行数据，一次I/O
- 访问id为2的数据，需要访问当前表空间的第二行数据，两次I/O
- 访问id为3的数据，需要访问当前表空间的第三行数据，三次I/O......

我们发现id为1，2，3...的数据都在同一个【数据页】，这会导致一个严重的问题，一次简单的查询，会访问【同一个页很多次】，可能产生很几百次I/O操作。所以为了解决快如闪电的【cpu】，和慢如蜗牛的【磁盘】之间的矛盾，innodb设计了buffer pool，有了缓存之后我们的执行过程如下：

- 访问id为1的数据，需要访问当前表空间的第一行数据，缓存当前页，一次I/O
- 访问id为2的数据，需要访问当前表空间的第二行数据，从缓存获取，无需I/O
- 访问id为3的数据，需要访问当前表空间的第三行数据，从缓存获取，无需I/O......

Innodb引擎会在mysql启动的时候，向操作系统申请一块连续的空间当做buffer pool，空间的大小由变量`innodb_buffer_pool_size`确定，我这台电脑他使用了8G，你的可能是128M。

![image-20220509110739628](https://www.ydlclass.com/doc21xnv/assets/image-20220509110739628.49731f16.png)这个缓冲区的大小可以结合自己服务器的性能而定，这就明白了内存大的好处了吧。

### 一、内部结构

整个buffer pool是由缓冲页和控制块组成的：

- **缓冲页：**buffer pool中存放的【数据页】我们称之为【缓冲页】，和磁盘上的数据页是一一对应的，都是16KB，缓冲页的数据，是从磁盘上加载到buffer pool当中的一个完整页。
- **控制块：**他是缓冲页【描述信息】，这一块区域保存的是数据页所属的表空间号，数据页编号，数据页地址，以及一些链表相关的节点信息等，每个控制块大小是缓存页的5%左右，大约是800个字节。

其内部结构如下，buffer pool的前一部分存储【控制块】，后一部分存储【缓冲页】，如果中间有未被利用的空间，就叫他【内存碎片】吧：

![image-20220509143231600](https://www.ydlclass.com/doc21xnv/assets/image-20220509143231600.b665b8b0.png)

**tips：buffer pool的初始化**

数据库会在启动的时候，按照配置中的Buffer Pool大小，去向操作系统申请一块内存，作为Buffer Pool的内存区域，然后会按照默认的缓存页的的大小【16KB】以及对应的【800个字节左右】的【控制块】的大小，在Buffer Pool中划分出一个一个的缓存页和一个一个与其对应的描述数据（控制块）。此时的buffer pool像一个干净的本子，没有书写任何内容。

### 二、free链

刚初始化的buffer pool，内存中都是【空白的缓冲页】，但是随着时间的推移，程序在执行过程中会不断的有新的页被缓存起来，那怎么来判断哪些缓冲页是【闲置状态】，可以被使用呢，此时就需要【控制块来进行标记和管理】了。innodb在设计之初，会将所有【空闲的缓冲页】所对应的【控制块】作为一个个的节点，形成一个链表，这个链表就是free链，翻译过来就是空闲链表，如下图：

![image-20220509153220595](https://www.ydlclass.com/doc21xnv/assets/image-20220509153220595.930514e2.png)

由上图可知，free链表是一个双向链表，链表上除了控制块以外，还有一个基础节点，存储了free链有多少个描述信息块，也就是有多少个空闲的缓存页，以及指向链表头尾的指针。

当我们加载数据的时候，会从free链中找到空闲的缓存页，把数据页的【表空间号和数据页】号写入【控制块】。

加载数据到缓存页后，会把缓存页对应的控制块从free链表中移除。

#### 1、怎么知道数据页是否被缓存？

我们已经有了free链表用来【保存空闲的页】，但是，当下一次访问时，要如何知道当前要访问的页是不是已经被缓存了，最直观的思路就是将buffer poll里的缓存数据【全部遍历一遍】。显然，这要做并不合理，本来设计buffer pool是为了提升效率，如果有人将buffer pool配置的很大，比如32个G，那扫描这一片区域的功夫都可以喝一杯茶了，反而成了累赘。

事实上，使用【表空间号+页号】就可以确定一个唯一的页，那么我们能不能设计一个hash表，使用【表空间号+页】号当做key，使用【控制块地址】做value，每次查询的时候只需要通过key进行查找即可，大家都知道hash的时间复杂度是O(1)，这样就能迅速定位缓存的页。（和hashmap很像）

结合我们的free链表，查询/缓存一个页的流程大致如下：

![image-20220509161002976](https://www.ydlclass.com/doc21xnv/assets/image-20220509161002976.2a06e1f4.png)

### 三、flush链表

#### 1、脏页

在sql的执行过程中，无论是增删改查，都是优先在buffer pool中进行的，这样可以极大的保证执行效率。但是同样会有一个问题，假如我们对缓存页的某些数据进行了修改（执行了一条update语句），就会导致buffer pool中的缓冲页和磁盘的数据页【数据不一致】，那么此时的缓冲页就称之为【脏页】。当然，这也就说明了，脏页的数据是要刷到磁盘上的。

我在看极客时间的专栏时，有一位老师的比喻很不错，在古代的酒楼中记账是个技术活，可能经常会有赊账行为。每次记账、赊账都会将相关记录记录在账本之上。但是当饭点高峰期，记账数据巨大，张三仗着自己的岳父是县令大人又来赊账一笔，掌柜一时间太忙没有时间翻阅账本，查看张三的历史记录，所以单独使用一个小黑板，记了一笔张三今天赊账10两银子，一会李四来赊账，再在上面记上一笔。等过了饭点，有时间了，再将记录誊抄至账本，计算张三和李四的总赊账额度，其实就是这么个原理。

#### 2、链表结构

![image-20220509153249620](https://www.ydlclass.com/doc21xnv/assets/image-20220509153249620.b602e7fa.png)

- flush链表同样是一个双向链表，链表结点是被【修改过的缓存页】的控制块。
- 和free链表一样，flush链表也有一个基础结点，链接首尾结点，并存储了有多少个控制块。

#### 3、刷盘时机

后台会有专门的线程每隔一段时间就把flush链表中的脏页刷入磁盘中，刷新的速率取决与当前系统是否繁忙。在这样的机制下，万一系统奔溃，是会产生数据不一致的问题的，没有刷入磁盘的数据就会丢失，而mysql通过日志系统解决了这个问题，以后的章节会详细讲解。

### 四、LRU链表

#### 1、概述

内存是有限的，buffer pool更是有限的，缓存只是数据的中转站，当我们的数据量很大以后，buffer pool其实是仅仅能容纳很少一部分数据，所以buffer pool的容量很有可能被使用殆尽，如果此时我们还想继续缓存数据页那该怎么办？

合理的做法就是，当需要更多的空间缓存【新的数据页】的时候，我们将最近使用最少的【缓冲页淘汰掉】就可以了，这就是典型的LRU（Least Recently Used）算法，我们在讲java的时候也手动实现过基于linkedhashmap的LRU算法。对于innodb而言，则是通过【LRU链表】来完成此功能的，他的结构和上边讲的free链表、flush链表基本相同，只是负责的功能不同而已。

于是，一个简单的思路诞生了，当客户端访问一条数据时，会加载对应的数据页到buffer pool，并会将缓冲页对应的控制块放置到【LRU链表的首位】。一旦buffer pool被占满，则从链表的末端开始淘汰数据，这是最简单的实现。

#### 2、优化

但是，实际的在使用场景中，我们需要对原有的LRU链表进行优化，因为他在一下场景可能会出现一些问题：

- 数据页预读：我们在讲多线程的时候是讲过【预读性原理】（当一个应用在访问一个数据时，很有可能会继续访问和他相邻的数据），cpu的高级缓冲区读取主存的数据也不是一个字节一个字节的读取，而是一下子会读取一个【缓存行】。同理，innodb从磁盘读取数据，也不一定是一页页读取，当mysql读取当前需要的页时，如果觉得后续操作会使用【附近的页】，就会将他们一起缓存到buffer pool，这样的作用是为了提升效率。但是，这也会导致大量的使用频率并不高的数据放置在LRU链表头部，反而将一些真正的【热点数据】淘汰。
- 全表扫描：一条【select * from user】 语句，会直接将一张表的全表数据缓存，并全部放在LRU链表头部，一样会淘汰很多热点数据。

所以，innodb对该链表进行了优化，将【LRU链表】分成了两个区域，分为【热数据区】和【冷数据区】，默认情况下冷数据区占了总链表的37%，机构如下：

![image-20220509180031426](https://www.ydlclass.com/doc21xnv/assets/image-20220509180031426.501d93db.png)

**tips：**一个select语句可能会多次访问一个页，因为你有【很多数据是保存在同一个页内】的。对于一个全表扫描的语句，每访问一条数据，就会访问一次相关的页，所以缓存确实能极大的提升效率：

- 对于预读的数据页，会在第一次访问时放入old区域，如果在sql执行的过程中访问相邻数据时，再次访问访问到该数据页，则把他加入如热数据区。

- 【大表的全表扫描】是个使用频率很低的操作（小表怎么操作都无所谓），但是如果按照上边的操作，首先全表数据会被放在【old区】，全表扫描必然会因为访问相邻数据而产生第二次、第三次、甚至数百次的访问，也就以为着这些页面会被全部放在young区。为了解决这个问题，INnodb提供了这样一个参数【innodb_old_blocks_time】，默认是1s，他的执行流程大致如下：

  1、页被首次访问时会记录访问的时间戳。

  2、以后访问都和首次访问的时间进行对比，如果时间大于1s，就讲当前页放入yong区。

  3、一个sql的扫描一个页的时间，哪怕在慢也不会低于1s，这样就解决了一个全表扫秒而导致全表成为热点数据的问题。

tips：也就意味着，热点数据要求首次访问时间和最后一次访问时间的时间差不能低于1s。

![image-20220509180909014](https://www.ydlclass.com/doc21xnv/assets/image-20220509180909014.c5327db9.png)

tips：使用以下的语句，可以查看innodb当前的状态：

```text
show engine innodb status
```

```bash
Total large memory allocated 8585216                  # 为innodb 分配的总内存数(byte)
Dictionary memory allocated 446370                    #为innodb数据字典分配的内存数(byte)
Buffer pool size   512                                #innodb_buffer_pool的大小(page)
Free buffers       101                                #innodb_buffer_pool lru列表中的空闲页面数量
Database pages     277                                #innodb_buffer_pool lru列表中的非空闲页面数
Old database pages 0                                  #innodb_buffer_pool old子列表的页面数量
Modified db pages  273                                #innodb_buffer_pool 中脏页的数量
Pending reads      1                                  #挂起读的数量
Pending writes: LRU 0, flush list 0, single page 0    #挂起写的数量
Pages made young 0, not young 0
0.00 youngs/s, 0.00 non-youngs/s
Pages read 8002054, created 766955, written 4652116
0.00 reads/s, 0.00 creates/s, 0.00 writes/s
Buffer pool hit rate 993 / 1000, young-making rate 0 / 1000 not 0 / 1000
Pages read ahead 0.00/s, evicted without access 0.00/s, Random read ahead 0.00/s
LRU len: 277, unzip_LRU len: 0
I/O sum[22009]:cur[1940], unzip sum[0]:cur[0]
```

## 第四章 MySQL临时表

### 一、临时表简介

MySQL临时表在很多场景中都会用到，比如用户自己创建的临时表用于【保存临时数据】，以及MySQL内部在执行【复杂SQL】时，需要借助临时表进行【分组、排序、去重】等操作，临时表具有一下几个特点：

1）临时表不能通过`show tables`查看，在服务器重启之后，所有的临时表将全部被销毁。

2）临时表是每个进程独享的，当前进程（客户端）创建的临时表，其他进程（客户端）是查不到临时表里面的数据的，所以不同客户端可以创建同名的临时表。

### 二、临时表分类

#### 1、外部临时表

通过create temporary table语句创建的临时表为外部临时表，创建语句如下：

```sql
create temporary table temp_table(
	id int,
	name varchar(10)
) ENGINE = InnoDB;
insert into temp_table values (1,'1');

select * from temp_table ;

-- 删除临时表
DROP TEMPORARY TABLE table_name;
```

#### 2、内部临时表

【内部临时表】用来存储某些操作的【中间结果】，这些操作可能包括在【优化阶段】或者【执行阶段】，这种内部表对用户来说是不可见的。通常在执行复杂SQL语句时，比如group by，distinct，union等语句时，MySQL内部将使用【自动生成的临时表】，以辅助SQL的执行。我们可以使用执行计划查看，如果一条sql语句的执行计划中【列extra】结果为Using temporary，那么也就说明这个查询要使用到临时表。执行计划，我们后边会详细讲解。

![image-20220512171531287](https://www.ydlclass.com/doc21xnv/assets/image-20220512171531287.af1986a5.png)

#### 3、group by执行流程

这个例子中我们使用，课程【mysql入门】中的一张学生表，执行以下sql，统计各个年龄的人数，并按照年龄大学排序：

```sql
select age,count(*) from student group by age order by age
```

**他的执行流程如下：**

（1）创建一个内部临时表，有两列，一列是age，一列是count(*)。

（2）全表扫描【原始表】（聚簇索引会在后边的内容讲解），每扫描一条数据进行一次判断，第一种情况，这条数据的年龄在临时表中不存在，则将年龄填入，count列填1。第二种情况，该条数据在临时表中存在，则将count列加1。临时表的存在是为了辅助计算。

（3）对临时表的数据按照年龄进行排序。

![image-20220512173136197](https://www.ydlclass.com/doc21xnv/assets/image-20220512173136197.67e95f7b.png)

#### 4、内部临时表创建时机

**MySQL在以下几种情况会创建临时表，大家也要思考为什么会产生临时表：**

1、使用GROUP BY分组，且分组字段没有索引时。

2、使用DISTINCT查询。

![image-20220512180936503](https://www.ydlclass.com/doc21xnv/assets/image-20220512180936503.7bb9f8be.png)

3、使用UNION进行结果合并，辅助去重。

![image-20220512181256543](https://www.ydlclass.com/doc21xnv/assets/image-20220512181256543.8c827827.png)

注意：【union all】不会使用零时表，因为他不需要去重

![image-20220512181433029](https://www.ydlclass.com/doc21xnv/assets/image-20220512181433029.019f7ad6.png)

复杂的sql中很容易产生临时表，这需要大家在工作中不断的学习和积累。

**tips：**其实临时表还可以分为【内存临时表】和【磁盘临时表】。内存临时表使用memery引擎（Memory引擎不支持BOLB和TEXT类型），磁盘临时表默认使用innodb引擎。在以下几种情况下，会创建磁盘临时表：

1、数据表中包含BLOB/TEXT列；

2、在 GROUP BY 或者 DSTINCT 的列中有超过 512字符的字符类型列；

3、在SELECT、UNION、UNION ALL查询中，存在最大长度超过512的列（对于字符串类型是512个字符，对于二进制类型则是512字节）；

## 第五章 MySQL事务

### 一、事务简介

首先给大家举一个例子：我们有如下的销售业务，一个销售业务可能包含很多步骤，比如记录订单、添加积分、管理库存、扣减金额等等，每一个操作都可能对应一条或多条sql语句，但是这个业务却是不可分割的，不能下了订单，不扣减库存。此时我们就需要事务来统一管理这个业务当中的一系列sql语句了。

（1）在 MySQL 中只有使用了 Innodb 数据库引擎的数据库或表才支持事务。

（2）事务处理可以用来维护数据库的完整性，保证成批的 SQL 语句要么全部执行，要么全部不执行。

### 二、事务分类

#### 1、显式事务和隐式事务

（1）mysql的事务可以分为【显式事务】和【隐式事务】。默认的事务是隐式事务，由变量【autocommit】控制。隐式事务的环境下，我们每执行一条sql都会【自动开启和关闭】事务，变量如下：

```text
SHOW VARIABLES LIKE 'autocommit';
```

![image-20220512184029517](https://www.ydlclass.com/doc21xnv/assets/image-20220512184029517.66be300a.png)

（2）显式事务由我们【自己控制】事务的【开启，提交，回滚】等操作，我们创建一个表，同时展示事务的基础语法，如下：

```sql
create database ydlTrx;
use ydlTrx;
-- UNSIGNED代表无符号数，不能是负数 
create table user(
	id int primary key auto_increment,
	name VARCHAR(20),
	balance DECIMAL(10,2) UNSIGNED
);

insert into user VALUES (1,'楠哥',200);
insert into user VALUES (2,'楠哥老婆',50000);

-- 转账业务，必须都成功，或者都失败，所以不能一句一句执行，万一执行了一半，断电了咋办
-- 所以要编程一个整体
-- 都成功
-- 开启事务;
start transaction;    
UPDATE user set balance = balance - 200 where id = 1;
UPDATE user set balance = balance + 200 where id = 2;
-- 提交事务
commit;

-- 都失败
start transaction;
UPDATE user set balance = balance - 200 where id = 1;
UPDATE user set balance = balance + 200 where id = 2;
-- 回滚事务
rollback;
```

我们可以使用begin或start transaction开启一个事务，使用commit提交事务，使用rollback回滚当前事务。

#### 2、只读事务和读写事务

我们可以使用read only开启只读事务，开启只读事务模式之后，事务执行期间任何【insert】或者【update】语句都是不允许的，具体语法如下：

```sql
start transaction read only
select * from ....
select * from ....
commit;
```

有人可能会问，这样和不开事务有什么区别呢？这个在下边学了隔离级别就知道了。

#### 3、保存点

我们可以使用savepoint 关键字在事务执行中新建【保存点】，之后可以使用rollback向任意保存点回滚。

```sql
start transaction;
UPDATE user set balance = balance - 200 where id = 1;
savepoint a;
UPDATE user set balance = balance + 200 where id = 2;
rollback to a;
```

**注意：**Mysql是不支持嵌套事务的，开启一个事务的情况下，若再开启一个事务，会隐式的提交上一个事务：

```sql
start transaction;
UPDATE user set balance = balance - 200 where id = 1;
    start transaction;    -- 这里会自动将第一个事务提交
    UPDATE user set balance = balance + 200 where id = 2;
    commit;
-- 回滚事务
rollback;
```

### 三、事务四大特征（ACID）

#### 1、原子性（Atomicity）

一个事务（transaction）中的所有操作，**要么全部完成，要么全部不完成**，不会结束在中间某个环节。如果事务在执行过程中发生错误，会被回滚（Rollback）到事务开始前的状态，就像这个事务从来没有执行过一样，这个很好理解。

#### 2、一致性（Consistency）

在事务【开始之前和结束以后】，数据库的完整性没有被破坏，数据库状态应该与业务规则保持一致。**举一个例子**：A向B转账，不可能A扣了钱，B却没有收到，也不可能A和B的总金额，在事务前后发生变化，产生数据不一致。其他的三个特性都在为他服务。

#### 3、隔离性（Isolation）

数据库【允许多个并发事务同时对其数据进行读取和修改】，隔离性可以防止多个事务在并发修改共享数据时产生【数据不一致】的现象，这里要联想到我们学习过的多线程。

事务隔离级别分为不同等级，包括读未提交（Read uncommitted）、读提交（read committed）、可重复读（repeatable read）和串行化（Serializable），后续会详细讲。

#### 4、持久性（Durability）

事务处理结束后，对数据的修改就是永久的，即便系统故障也不会丢失。

### 四、事务隔离级别

对于数据库的四大特性中的【隔离级别】是比较难理解的，我们在本小结中详细介绍。

在多个事务【并发操作】相同的表数据时，为了让多个事务都可以得到正确的结果，不会因为互相的交叉操作产生干扰，同时还要保证一定的执行效率，故而提出了不同的隔离级别。

**隔离级别分类如下，在不同的隔离级别下可能产生不同的问题，如脏读、不可重复度、幻读等，我们会在后边的课程中一一讲解：**

| 隔离级别                     | 脏读 | 不可重复读 | 幻读 | 解决方案                                   |
| ---------------------------- | ---- | ---------- | ---- | ------------------------------------------ |
| Read uncommitted（读未提交） | √    | √          | √    |                                            |
| Read committed（读已提交）   | ×    | √          | √    | undo log                                   |
| Repeatable read（可重复读）  | ×    | ×          | √    | MVCC版本控制+间隙锁（mysql的rr不存在幻读） |
| Serializable（串行化）       | ×    | ×          | ×    |                                            |

注意：传统意义上的rr级别是存在幻读问题的，但是mysql的rr级别不存在。

**在mysql中查看和设置【事务的隔离级别】，语法如下：**

```sql
-- 查看全局和当前事务的隔离级别
SELECT @@global.transaction_isolation, @@transaction_isolation_isolation;
show variables like 'transaction_isolation';
--5.7   tx_isolation
--8.0   transaction_isolation

-- 设置下一个事务的隔离级别
SET transaction isolation level read uncommitted;
SET transaction isolation level read committed;
set transaction isolation level repeatable read;
SET transaction isolation level serializable;
-- 设置当前会话的隔离级别
SET session transaction isolation level read uncommitted;
SET session transaction isolation level read committed;
set session transaction isolation level repeatable read;
SET session transaction isolation level serializable;
-- 设置全局事务的隔离级别
SET GLOBAL transaction isolation level read uncommitted;
SET GLOBAL transaction isolation level read committed;
set GLOBAL transaction isolation level repeatable read;
SET GLOBAL transaction isolation level serializable;


其中，SESSION 和 GLOBAL 关键字用来指定修改的事务隔离级别的范围：
SESSION：表示修改的事务隔离级别将应用于当前 session（当前 cmd 窗口）内的所有事务；
GLOBAL：表示修改的事务隔离级别将应用于所有 session（全局）中的所有事务，且当前已经存在的 session 不受影响；
如果省略 SESSION 和 GLOBAL，表示修改的事务隔离级别将应用于当前 session 内的下一个还未开始的事务。
```

#### 1、读未提交（RU）

【ru隔离级别】说的简单一点就是，一个事务可以读取其他【未提交的事务】修改的数据，这种隔离级别最低，一般情况下，数据库隔离级别都要高于该级别，该隔离级别下，可能会存在脏读、不可重复度，幻读的问题。

**脏读：**指的是一个事务读到了其他事务未提交的数据，未提交意味着这些数据可能会回滚，读到的数据不一定准确。

**案例：**

楠哥发工资了，老婆让楠哥把工资打到他老婆的账号上，但是该事务并未提交，就让老婆去查看，老婆一看真的打了钱了，高高兴兴关了网页，此时楠哥急中生智进行回滚，钱瞬间回来，一次蒙混了一个月工资。所以楠哥老婆看到的数据我们称之为“脏数据”。

第一步：使用两个窗口，开启两个事务，且将隔离级别设置为RU。

```sql
use ydlTrx;
SET transaction isolation level read uncommitted;
```

第二步：楠哥，给转账老婆转账10000元，但是不提交。

```sql
start transaction;
UPDATE user set balance = balance - 10000 where id = 1;
UPDATE user set balance = balance + 10000 where id = 2;
```

第三部步：楠哥通知老婆进行查账，确实读取到了楠哥未提交事务修改的数据。

```sql
start transaction;
select * from user where id = 2;
commit;
```

第四步：老婆查账结束，楠哥来一个回马枪，对自己的事务进行回滚操作。

```sql
rollback;
```

第五步：楠哥老婆某天查账，突然发现，哎，怎么少了一万。

```sql
start transaction;
select * from user where id = 2;
commit;
```

#### 2、读已提交（RC）

【RC读已提交】说的是当前事务只能读到别的事物已经提交的数据，该隔离级别可能会产生不可重复读和幻读。

【不可重复读】的官方解释是：【一个事务】（A事务）修改了【另一个未提交事务】（B事务）读取过的数据。那么B事务【再次读取】，会发现两次读取的数据不一致。也就是说在一个原子性的操作中一个事务两次读取相同的数据，却不一致，一行数据不能重复被读取。主要是【update】语句，会导致不可重复读。

**案例：**

楠哥拿着工资卡去消费，系统读取到卡里确实有10200元，而此时她的老婆也正好在网上转账，把楠哥工资卡的2000元转到另一账户，并在 楠哥之前提交了事务，当楠哥扣款时，系统检查到楠哥的工资卡和上次读取的不一样了，楠哥十分纳闷，明明卡里有钱，为何......

第一步：将事务的隔离级别设置为RC。

```sql
SET  transaction isolation level read committed;
```

第二步：楠哥准备去消费了，检查存款显示有余额，贼高兴。

```sql
start transaction;
select * from user where id = 1;
```

第三步：楠哥老婆在楠哥没有提交事务的时候，进行了一笔转账，并且提交了事务。

```sql
start transaction;
UPDATE user set balance = balance + 500 where id = 2;
UPDATE user set balance = balance - 500 where id = 1;
commit;
```

第四步：楠哥再次查账，同一个事务里，发现钱少了。

```sql
select * from user where id = 1;
```

当隔离级别设置为Read committed 时，避免了脏读，但是可能会造成不可重复读。

大多数数据库的默认级别就是Read committed，比如Sql Server , Oracle。如何解决不可重复读这一问题，请看下一个隔离级别。

#### 3、可重复读（RR）

学习完不可重复读，理解【可重复读】就简单多了，他的意思是，同一个事务中发出同一个SELECT语句【两次或更多次】，那么产生的结果数据集总是相同的，在RR隔离级别中可能出现幻读。

**幻读：**一个事务按照某些条件进行查询，事务提交前，有另一个事务插入了满足条件的其他数据，再次使用相同条件查询，却发现多了一些数据，就像出现了幻觉一样。幻读主要针对针对delete和insert语句。

不可重复读强调的是两次读取的数据【内容不同】，幻读前调的是两次读取的【行数不同】。

**案例**

楠哥的老婆在银行部门工作，她时常通过银行内部系统查看楠哥的账户信息。有一天，她正在查询到楠哥账户信息时发现楠哥只有一个账户，心想这家伙应该没有私房钱。此时楠哥在另外一家分行又开了一个账户，准备存私房钱。同时，楠哥老婆在同一个事务中又一次查询，结果显示出的楠哥账户居然多了一个，真实奇怪。

第一步：将数据库的隔离级别设置为RR。

```text
set  transaction isolation level repeatable read;
```

第二步：楠哥开启事务。

```text
start transaction;
```

第三步：老婆查账户。

```sql
start transaction;
select * from user where name = '楠哥';
```

第四步：楠哥趁机开户，并提交事务。

```sql
insert into user values(3,'楠哥',10000);
commit;
```

第五步：老婆再查询并打印，应该发现楠哥多了一个账户，但是没有，并没有出现幻读。

```sql
select * from user where name = '楠哥';
```

**注意：**在mysql中的RR隔离级别中，innodb使用mvcc+锁帮我们解决了绝大部分的幻读情况，

上边的例子稍微修改一下，我们就能看到幻读现象了。

第一步：将数据库的隔离级别设置为RR。

```sql
set  transaction isolation level repeatable read;
```

第二步：楠哥开启事务。

```sql
start transaction;
```

第三步：老婆查账户。

```sql
start transaction;
select * from user where name = '楠哥';
```

第四步：楠哥趁机开户，并提交事务。

```sql
insert into user values(3,'楠哥',10000);
commit;
```

第五步：老婆好心给所有的账户充值100，再查询并打印，结果发现楠哥多了一个账户，出现幻读。

```sql
-- 先给所有的账户钱充值一百
UPDATE user set balance = balance + 100;
select * from user where name = '楠哥';
```

这里边的原理过于复杂，目前我们先卖个关子，等我们学完【锁和mvcc】以后回来在看，就能明白了。

#### 4、串行化

- 事务A和事务B，事务A在操作数据库时，事务B只能排队等待
- 这种隔离级别很少使用，吞吐量太低，用户体验差
- 这种级别可以避免“幻读”，每一次读取的都是数据库中真实存在数据，事务A与事务B串行，而不并发。

第一步：修改数据库的隔离级别

```sql
SET  transaction isolation level serializable;
```

第一步：楠哥执行查询操作

```sql
begin;
select * from user;
```

第二步：楠哥老婆执行查询操作

```sql
begin;
select * from user;
```

第三部：楠哥老婆想执行一个删除操作，发现事务被阻塞了，需要等待

```sql
delete from user where id = 9;
```

第四部：楠哥这边一提交，老婆那边就能操作了

```sql
commit;
```