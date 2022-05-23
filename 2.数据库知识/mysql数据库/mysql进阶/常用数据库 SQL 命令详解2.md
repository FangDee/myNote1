# 常用数据库 SQL 命令详解2

### **一、简介**

本文主要以 Mysql 数据库为基础，对常用 SQL 语句进行一次深度总结，由于篇幅较长，难免会有些遗漏的地方，欢迎网友留言指出！

整体内容，主要有以下几个部分：

- 库操作
- 表操作
- 数据操作
- 运算符
- 视图
- 函数
- 存储过程
- 触发器
- 序列
- 用户权限

在上篇《[常用数据库 SQL 命令详解（上）》](http://mp.weixin.qq.com/s?__biz=MzkzODE3OTI0Ng==&mid=2247502845&idx=2&sn=4c0eeca95de08967c8fd446a9a935616&chksm=c286a43cf5f12d2a440cf4b234c2c78bed4f985dbfada3b389b3582ede7e8ca9f6c8fc2e7927&scene=21#wechat_redirect)文章中，主要介绍上半部分内容，今天我们来介绍下半部分内容！

### **二、函数**

#### 2.1、常用函数列表

|           函数           |                          描述                          |                             实例                             |
| :----------------------: | :----------------------------------------------------: | :----------------------------------------------------------: |
|      char_length(s)      |                返回字符串 s 的字符长度                 |          `select char_length("hello") as content;`           |
|    concat(s1,s2...sn)    |       字符串 s1,s2 等多个字符串合并为一个字符串        |        `select concat("hello ", "world") as content;`        |
|       format(x,n)        | 将数字 x 进行格式化，到小数点后 n 位，最后一位四舍五入 |           `select format(500.5634, 2) as content;`           |
|         lower(s)         |                 将所有字母变成小写字母                 |                   `select lower('HELLO');`                   |
|   current_timestamp()    |                   返回当前日期和时间                   |                `select current_timestamp();`                 |
| DATE_FORMAT(date,format) |                   格式化时间或者日期                   | `select DATE_FORMAT(current_timestamp(),"%Y-%m-%d %H:%i:%s");` |
|      IFNULL(v1,v2)       |     如果 v1 的值不为 NULL，则返回 v1，否则返回 v2      |             `select IFNULL(null,'hello word');`              |

#### 2.2、自定义函数语法介绍

##### 2.2.1、创建函数

```
CREATE FUNCTION fn_name(func_parameter[,...])
RETURNS type
[characteristic...]
routine_body
```

参数说明：

- fn_name：自定义函数名称
- func_parameter:  param_name type
- type: 任何mysql支持的类型
- characteristic: LANGUAGE SQL
- routine_body: 函数体

##### 2.2.2、编辑函数

```
ALTER FUNCTION fn_name [characteristic...]
```

参数说明：

- fn_name：自定义函数名称
- func_parameter:  param_name type
- characteristic: LANGUAGE SQL

##### 2.2.3、删除函数

```
DROP FUNCTION  [IF EXISTS]  fn_name;
```

参数说明：

- fn_name：自定义函数名称
- func_parameter:  param_name type

##### 2.2.4、查看函数语法

```
SHOW FUNCTION STATUS [LIKE 'pattern']
```

参数说明：

- pattern：函数名称

示例：

```
SHOW FUNCTION STATUS LIKE 'user_function';
```

##### 2.2.5、查看函数的定义语法

```
SHOW CREATE FUNCTION fn_name;
```

参数说明：

- fn_name：自定义函数名称

#### 2.3、实例操作介绍

##### 2.3.1、创建一个表

```
CREATE TABLE `t_user` (
  `user_id` int(10) NOT NULL AUTO_INCREMENT COMMENT '用户id,作为主键',
  `user_name` varchar(5) DEFAULT NULL COMMENT '用户名',
  `age` int(3) DEFAULT NULL COMMENT '年龄',
  PRIMARY KEY (`user_id`)
) ENGINE=InnoDB AUTO_INCREMENT=1 DEFAULT CHARSET=utf8;
```

##### 2.3.2、插入数据

```
INSERT INTO t_user (user_name, age)
VALUES('张三',24),('李四',25),('王五',26),('赵六',27);
```

##### 2.3.3、创建函数

```
-- 创建一个函数
DELIMITER $$

-- 开始创建函数
CREATE FUNCTION user_function(v_id INT)
RETURNS VARCHAR(50)
READS SQL DATA
DETERMINISTIC
BEGIN
  -- 定义变量
  DECLARE userName VARCHAR(50);
  -- 给定义的变量赋值
  SELECT user_name INTO userName FROM t_user
  WHERE user_id = v_id;
  -- 返回函数处理结果
  RETURN userName;
END;

-- 函数创建定界符
DELIMITER ;
```

##### 2.3.4、调用函数

```
//查询用户ID为1的信息
SELECT user_function(1);
```

##### 2.3.5、删除函数

```
DROP FUNCTION  IF EXISTS  user_function;
```

### **三、存储过程**

#### 3.1、创建语法

```
CREATE PROCEDURE 存储过程名([[IN |OUT |INOUT ] 参数名 数据类形...])
```

过程与创建函数类似，其中的声明语句结束符，可以自定义:

```
DELIMITER $$
或
DELIMITER //
```

参数说明：

- **IN 输入参数**：表示该参数的值必须在调用存储过程时指定，在存储过程中修改该参数的值不能被返回，为默认值
- **OUT 输出参数**：该值可在存储过程内部被改变，并可返回
- **INOUT 输入输出参数**：调用时指定，并且可被改变和返回

创建一个查询用户信息的存储过程示例：

```
DELIMITER $$
CREATE PROCEDURE user_procedure(IN v_id int,OUT userName varchar(255))  
    BEGIN
    SELECT user_name as userName FROM t_user WHERE user_id = v_id;
    END $$ 
DELIMITER ;
```

#### 3.2、存储过程调用

```
-- @out为输出参数
CALL user_procedure(1, @out);
```

输出结果：

```
张三
```

#### 3.3、存储过程删除

```
DROP PROCEDURE [IF EXISTS]  proc_name;
```

删除示例：

```
DROP PROCEDURE IF EXISTS  user_procedure;
```

#### 3.4、存储过程和函数的区别

- 函数只能通过return语句返回单个值或者表对象。而存储过程不允许执行return，但是通过out参数返回多个值。
- 函数是可以嵌入在sql中使用的,可以在select中调用，而存储过程不行。
- 函数限制比较多，比如不能用临时表，只能用表变量，还有一些函数都不可用等等，而存储过程的限制相对就比较少
- 一般来说，存储过程实现的功能要复杂一点，而函数的实现的功能针对性比较强。
- 当存储过程和函数被执行的时候，SQL Manager会到procedure cache中去取相应的查询语句，如果在procedure cache里没有相应的查询语句，SQL Manager就会对存储过程和函数进行编译。

### **四、触发器**

触发器是与表有关的数据库对象，在满足定义条件时触发，并执行触发器中定义的语句集合。

#### 4.1、创建触发器

定义语法

```
CREATE
    [DEFINER = { user | CURRENT_USER }]
TRIGGER trigger_name
trigger_time trigger_event
ON tbl_name FOR EACH ROW
  [trigger_order]
trigger_body
trigger_time: { BEFORE | AFTER }
trigger_event: { INSERT | UPDATE | DELETE }
trigger_order: { FOLLOWS | PRECEDES } other_trigger_name
```

参数说明：

- FOR EACH ROW：表示任何一条记录上的操作满足触发事件都会触发该触发器，也就是说触发器的触发频率是针对每一行数据触发一次。
- trigger_time：BEFORE和AFTER参数指定了触发执行的时间，在事件之前或是之后。
- tigger_event详解：
- INSERT 型触发器：插入某一行时激活触发器，可能通过INSERT、LOAD DATA、REPLACE 语句触发(LOAD DAT语句用于将一个文件装入到一个数据表中，相当与一系列的INSERT操作)；
- UPDATE型触发器：更改某一行时激活触发器，可能通过UPDATE语句触发；
- DELETE型触发器：删除某一行时激活触发器，可能通过DELETE、REPLACE语句触发。
- trigger_order：是MySQL5.7之后的一个功能，用于定义多个触发器，使用follows(尾随)或precedes(在…之先)来选择触发器执行的先后顺序。

示例，创建了一个名为trig1的触发器，一旦在`t_user`表中有插入动作，就会自动往`t_time`表里插入当前时间。

```
CREATE TRIGGER trig1 AFTER INSERT
ON t_user FOR EACH ROW
INSERT INTO t_time VALUES(NOW());
```

创建有多个执行语句的触发器语法

```
CREATE TRIGGER 触发器名 BEFORE|AFTER 触发事件
ON 表名 FOR EACH ROW
BEGIN
        执行语句列表
END;
```

示例如下：

```
DELIMITER //
CREATE TRIGGER trig2 AFTER INSERT
ON t_user FOR EACH ROW
BEGIN
INSERT INTO t_time VALUES(NOW());
INSERT INTO t_time VALUES(NOW());
END//
DELIMITER ;
```

一旦插入成功，就会执行`BEGIN ...END`语句！

#### 4.2、查询触发器

- 查询所有触发器

```
SHOW TRIGGERS;
```

- 查询指定的触发器

```
select * from information_schema.triggers where trigger_name='trig1';
```

所有触发器信息都存储在information_schema数据库下的triggers表中，可以使用SELECT语句查询，如果触发器信息过多，最好通过TRIGGER_NAME字段指定查询。

#### 4.3、删除触发器

```
DROP TRIGGER [IF EXISTS] [schema_name.]trigger_name
```

示例如下：

```
DROP TRIGGER IF EXISTS trig1
```

删除触发器之后最好使用上面的方法查看一遍。

#### 4.4、总结

触发器尽量少的使用，因为不管如何，它还是很消耗资源，如果使用的话要谨慎的使用，确定它是非常高效的：触发器是针对每一行的；对增删改非常频繁的表上切记不要使用触发器，因为它会非常消耗资源。

### **五、序列**

在 MySQL 中，可以有如下几种途径实现唯一值：

- 自增序列
- 程序自定义
- UUID() 函数
- UUID_SHORT() 函数

#### 5.1、自增序列

在mysql中，一般我们可以给某个主键字段设置为自增模式，例如：

```
#创建一个表test_db，字段内容为id,name
create table test_db(id int,name char(10));

# 设置id主键
alter table test_db add primary key(id);

# 将id主键设置为自增长模式
alter table test_db modify id int auto_increment;
```

这种模式，在单库单表的时候，没啥问题，但是如果要对`test_db`表进行分库分表，这个时候问题就来了，如果水平分库，这个时候向`test_db_1`、`test_db_2`中插入数据，就会出现相同的`ID`！

#### 5.2、程序自定义

当然，为了避免出现这种情况，有的大神就自己单独创建了一张自增序列表，单独维护，这样就不会出现在分表的时候出现相同的ID！

实现过程也很简单！

- 1、创建一个序列表

```
CREATE TABLE `sequence` (
  `name` varchar(50) COLLATE utf8_bin NOT NULL COMMENT '序列的名字',
  `current_value` int(11) NOT NULL COMMENT '序列的当前值',
  `increment` int(11) NOT NULL DEFAULT '1' COMMENT '序列的自增值',
  PRIMARY KEY (`name`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin;
```

- 2、创建–取当前值的函数

```
BEGIN 
     DECLARE value INTEGER; 
     SET value = 0; 
     SELECT current_value INTO value 
          FROM sequence 
          WHERE name = seq_name; 
     RETURN value; 
END
```

- 3、创建–取下一个值的函数

```
DROP FUNCTION IF EXISTS nextval; 
DELIMITER $ 
CREATE FUNCTION nextval (seq_name VARCHAR(50)) 
     RETURNS INTEGER 
     LANGUAGE SQL 
     DETERMINISTIC 
     CONTAINS SQL 
     SQL SECURITY DEFINER 
     COMMENT '' 
BEGIN 
     UPDATE sequence 
          SET current_value = current_value + increment 
          WHERE name = seq_name; 
     RETURN currval(seq_name); 
END 
$ 
DELIMITER ;
```

- 4、创建–更新当前值的函数

```
DROP FUNCTION IF EXISTS setval; 
DELIMITER $ 
CREATE FUNCTION setval (seq_name VARCHAR(50), value INTEGER) 
     RETURNS INTEGER 
     LANGUAGE SQL 
     DETERMINISTIC 
     CONTAINS SQL 
     SQL SECURITY DEFINER 
     COMMENT '' 
BEGIN 
     UPDATE sequence 
          SET current_value = value 
          WHERE name = seq_name; 
     RETURN currval(seq_name); 
END 
$ 
DELIMITER ;
```

- 最后，直接通过函数调用，测试如下

```
# 添加一个sequence名称和初始值，以及自增幅度
INSERT INTO sequence VALUES ('testSeq', 0, 1);

#设置指定sequence的初始值
SELECT SETVAL('testSeq', 10);

#查询指定sequence的当前值
SELECT CURRVAL('testSeq');

#查询指定sequence的下一个值
SELECT NEXTVAL('testSeq');
```

这方案，某种情况下解决了分表的问题，但是如果分库还是会出现相同的ID！

#### 5.3、UUID() 函数

UUID 基于 16 进制，由 32 位小写的 16 进制数字组成，如下：

```
aaaaaaaa-bbbb-cccc-dddd-eeeeeeeeeeee
```

比如`d0c754a8-178e-11eb-ae3d-2a7bea22ed3d`就是一个典型的 UUID。

在 MySQL 的`UUID()`函数中，前三组数字从时间戳中生成，第四组数字暂时保持时间戳的唯一性，第五组数字是一个`IEEE 802`节点标点值，保证空间唯一。

使用 UUID() 函数，可以生成时间、空间上都独一无二的值。据说只要是使用了 UUID，都不可能看到两个重复的 UUID 值。当然，这个只是在理论情况下。

使用方法也很简单，在`sql`可以直接当成函数调用即可！

```
select uuid();
```

#### 5.4、UUID_SHORT() 函数

在 MySQL 5.1 之后的版本，提供`UUID_SHORT()`函数，生成一个`64`位无符号整数，在java中可以用`Long`类型接受。另外，需要注意的是，server_id 的范围必须为`0-255`，并且不支持 `STATEMENT`模式复制，否则有可能会产生重复的ID

```
select UUID_SHORT();
```

同时，需要注意的是，`UUID_SHORT()`返回的是`unsigned long long`类型，在字段类型设置的时候，一定要勾选`无符号`类型，否则有可能生成的ID超过`Long`类型最大长度！

### **六、用户权限**

#### 6.1、用户管理

- 查询所有用户

```
select * from mysql.user;
```

- 创建用户

```
# 格式
CREATE USER 'username'@'host' IDENTIFIED BY 'password';
# 例子，创建一个用户名为admin,密码123456，可以本地访问的用户
CREATE USER 'admin'@'localhost' IDENTIFIED BY '123456';
```

- 更改用户密码

```
# 格式
SET PASSWORD FOR 'username'@'host' = PASSWORD('newpassword');
# 例子，将用户名admin,密码修改为456789，可以本地访问的用户
SET PASSWORD FOR 'admin'@'localhost' = PASSWORD("456789");
```

- 删除用户

```
# 格式
DROP USER 'username'@'host';
# 例子，删除用户名为admin的用户
DROP USER 'admin'@'localhost';
```

- 最后刷新操作，使操作生效

```
#刷新操作使其生效
flush privileges
```

#### 6.2、用户权限管理

- 查询用户权限

```
# 格式
SHOW GRANTS FOR 'username'@'host'
# 查询用户名为 'root'@'%'的权限信息
SHOW GRANTS FOR 'root'@'%'
```

- 给用户授予某种权限

```
# 格式
GRANT privileges ON databasename.tablename TO 'username'@'host'
```

说明：

- **privileges**：用户的操作权限，如`SELECT`，`INSERT`，`UPDATE`、`DELETE`等，如果要授予所的权限则使用`ALL`
- **databasename**：数据库名
- **tablename**：表名，如果要授予该用户对所有数据库和表的相应操作权限则可用`*`表示，如`*.*`
- **username**：用户名
- **host**：可以访问的域名

在给其他授权前，请先用管理员账户登录！

##### 1、设置用户访问数据库权限

- 设置用户`testuser`，只能访问数据库`test_db`，其他数据库均不能访问

```
grant all privileges on test_db.* to 'testuser'@'localhost';
```

- 设置用户`testuser`，可以访问`mysql`上的所有数据库

```
grant all privileges on *.* to 'testuser'@'localhost';
```

- 设置用户`testuser`，只能访问数据库`testuser`的表`user_info`，数据库中的其他表均不能访问

```
grant all privileges on test_db.user_info to 'testuser'@'localhost';
```

##### 2、设置用户操作权限

- 设置用户`testuser`，拥有所有的操作权限，也就是管理员

```
grant all privileges on *.* to 'testuser'@'localhost';
```

- 设置用户`testuser`，只拥有【查询】操作权限

```
grant select on *.* to 'testuser'@'localhost';
```

- 设置用户`testuser`，只拥有【查询/插入/修改/删除】操作权限

```
grant select,insert,update,delete on *.* to 'testuser'@'localhost';
```

##### 3、设置用户远程访问权限

- 设置用户`testuser`，只能在客户端IP为`192.168.1.100`上才能远程访问`mysql`

```
grant all privileges on *.* to 'testuser'@'192.168.1.100';
```

- 设置所有用户可以远程访问`mysql`，修改`my.cnf`配置文件，将`bind-address = 127.0.0.1`前面加`#`注释掉

```
# bind-address = 127.0.0.1
```

**注意：用以上命令授权的用户不能给其它用户授权，如果想让该用户可以授权，用以下命令！**

```
GRANT privileges ON databasename.tablename TO 'username'@'host' WITH GRANT OPTION;
```

在结尾加上`WITH GRANT OPTION`就可以了！

#### 6.3、关于root用户的访问设置

可以使用如下命令，来一键设置`root`用户的密码，同时拥有所有的权限并设置为远程访问！

```
grant all privileges on *.* to 'root'@'%'  identified by '123456';
```

如果想关闭`root`用户远程访问权限，使用如下命令即可！

```
grant all privileges on *.* to 'root'@'localhost'  identified by '123456';
```

最后使用如下命令，使其生效！

```
flush privileges;
```

创建用户并进行授权，也可以使用如下快捷命令！

```
#例如，创建一个admin用户，密码为admin
grant all privileges on *.* to 'admin'@'%' identified by 'admin';

#刷新MySQL的系统权限相关表方可生效
flush privileges;
```

**最后需要注意的是：mysql8，使用强校验，所以，如果密码过于简单，会报错，密码尽量搞复杂些！**

### **七、总结**

本文主要围绕 Mysql 中常用的语法进行一次梳理和介绍，这些语法大部分也同样适用于其他的数据库，例如 oracle、sqlserver、postgres 等等，在数据操作栏，除了分页函数以外，基本都是通用的！