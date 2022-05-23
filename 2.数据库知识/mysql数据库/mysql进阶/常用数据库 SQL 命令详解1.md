# 常用数据库 SQL 命令详解1

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

由于整部内容过长，在本篇文章中，主要介绍上半部分内容！

### **二、库操作**

#### 2.1、新增库

创建数据库比较简单，在创建的时候直接指定字符集、排序规则即可！

```
CREATE DATABASE IF NOT EXISTS `库名` default charset utf8mb4 COLLATE utf8mb4_unicode_ci;
```

例子：

```
CREATE DATABASE IF NOT EXISTS test_db default charset utf8mb4 COLLATE utf8mb4_unicode_ci;
```

#### 2.2、修改库名

数据库修改库名的有三种方法，如果是`MyISAM`存储引擎，那么可以直接去数据库目录`mv`就可以了，如果是`Innodb`完全不行，会提示相关表不存在。

##### 方法一

```
RENAME database olddbname TO newdbname
```

这个语法在 `mysql-5.1.7` 中被添加进来，到了`mysql-5.1.23`又去掉了，官方不推荐，会有丢失数据的危险！

##### 方法二

思路是先创建一个新库，之后将旧库的数据导入到新库，即可完成修改库名！

- 1、创建需要改成新名的数据库。
- 2、mysqldum 导出要改名的数据库
- 3、删除原来的旧库（确定是否真的需要）

当然这种方法虽然安全，但是如果数据量大，会比较耗时，同时还需要考虑到磁盘空间等硬件成本。

例子：

```
# 将db1库备份到db1.sql文件
mysqldump -u root -p db1 > /usr/db1.sql;

# 导入备份文件到新库db2
mysql -u root -p db2 < /root/db1.sql;

# 删除旧库（如果真的需要）
DROP DATABASE db1;
```

##### 方法三

直接跑一个 shell 脚本！

```
#!/bin/bash
# 假设将db1数据库名改为db2
# MyISAM直接更改数据库目录下的文件即可

mysql -uroot -p123456 -e 'create database if not exists db2'
list_table=$(mysql -uroot -p123456 -Nse "select table_name from information_schema.TABLES where TABLE_SCHEMA='db1'")

for table in $list_table
do
    mysql -uroot -p123456 -e "rename table db1.$table to db2.$table"
done
```

其中`p123456`，`p`是`password`的简称，`123456`表示数据库密码值！

#### 2.3、删除库名

删除库，比较简单，直接删除即可！

```
DROP DATABASE db1;
```

#### 2.4、使用库

```
USE db2;
```

### **三、表操作**

#### 3.1、创建表

```
CREATE TABLE ts_user (
  id bigint(20) unsigned NOT NULL COMMENT '编码',
  name varchar(100) COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '' COMMENT '用户姓名',
  mobile varchar(11) COLLATE utf8mb4_unicode_ci DEFAULT '' COMMENT '手机号',
  create_userid varchar(32) COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '' COMMENT '创建人',
  create_time datetime NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
  update_userid varchar(32) COLLATE utf8mb4_unicode_ci NOT NULL DEFAULT '' COMMENT '更新人',
  update_time datetime NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
  PRIMARY KEY (id),
  KEY idx_create_time (create_time) USING BTREE
) ENGINE=InnoDB DEFAULT CHARSET=utf8mb4 COLLATE=utf8mb4_unicode_ci COMMENT='用户表';
```

#### 3.2、修改表名

```
ALTER  TABLE ts_user RENAME TO ts_new_user;
```

#### 3.3、删除表

```
DROP TABLE ts_new_user;
```

#### 3.4、字段操作

##### 3.4.1、查询表字段

```
show full columns from ts_user;
```

##### 3.4.2、新增字段

```
ALTER TABLE ts_user add column gender tinyint(4) NOT NULL DEFAULT '1' COMMENT '性别，1，男；2，女' AFTER mobile;
```

##### 3.4.3、修改字段

```
ALTER TABLE ts_user modify column mobile varchar(30) NOT NULL DEFAULT '' COMMENT '用户手机号';
```

##### 3.4.4、删除字段

```
ALTER TABLE ts_user drop column gender;
```

#### 3.5、索引操作

##### 3.5.1、查询表索引

```
 SHOW INDEXES FROM ts_user;
```

##### 3.5.2、新增普通索引

```
alter table ts_user add index idx_id (id);
```

##### 3.5.3、新增唯一索引

```
alter table ts_user add unique idx_id (id);
```

##### 3.5.4、新增主键索引

```
alter table ts_user add primary key idx_id (id) ;
```

##### 3.5.5、新增多列索引

```
alter table ts_user add index idx_id_name (id,name) ;
```

##### 3.5.6、新增全文索引

```
alter table ts_user add fulltext idx_id (id) ;
```

##### 3.5.7、删除索引

```
# 删除普通索引
alter table ts_user drop index idx_id;

# 删除主键索引
alter table ts_user drop primary key;
```

### **四、数据操作**

#### 4.1、查询操作

##### 4.1.1、单表查询

```
select * from ts_user;
```

或者

```
select id, name from ts_user;
```

##### 4.1.2、关键字查询

- and 查询

```
select id, name from ts_user where name = '张三'
```

- or 查询

```
select id, name from ts_user where name = '张三' or name = '李四'
```

- in 查询（参数个数不能超过1000）

```
select id, name from ts_user where name in ('张三', '李四')
```

- like 模糊查询（`%`属于通配符）

```
select id, name from ts_user where name like '张%'
```

- 非空查询

```
select id, name from ts_user where name is not null
```

- 区间字段查询

```
select id, name, age from ts_user where  age >= 18 and age <= 30
select id, name, age from ts_user where age between 18 and 30
```

- 多条件判断

```
select 
name,
(
case
when scope >= 90 then  '优'
when 80 <= scope < 90 then  '良'
when 80 > scope >= 70  then  '中'
else '差'
end
) as judge
from ts_user
```

##### 4.1.3、连表查询

- 左连接查询

```
select tu.id, tu.name,tr.role_name
from ts_user tu
left join ts_role tr on tu.id = tr.user_id
```

- 右连接查询

```
select tu.id, tu.name,tr.role_name
from ts_user tu
right join ts_role tr on tu.id = tr.user_id
```

- 内连接查询

```
select tu.id, tu.name,tr.role_name
from ts_user tu
inner join ts_role tr on tu.id = tr.user_id
```

- 满连接查询

```
select tu.id, tu.name,tr.role_name
from ts_user tu
full join ts_role tr on tu.id = tr.user_id
```

##### 4.1.4、分组查询

- 统计学生总数

```
select count(id) from ts_user
```

- 查询学生最大的年纪

```
select max(age) from ts_user
```

- 查询学生最大的年纪

```
select min(age) from ts_user
```

- 查询各个学生各项成绩的总和

```
select id, sum(score) from ts_user group by id
```

- 查询各个学生各项成绩的平均分

```
select id, avg(score) from ts_user group by id
```

- 查询各个学生各项成绩的平均分大于100的学生信息

```
select id, avg(score) from ts_user group by id having avg(score)  > 100
```

#### 4.2、插入操作

##### 4.2.1、单列插入

```
INSERT INTO ts_user(id, name) VALUES ('1', '张三');
```

##### 4.2.2、多列插入

```
INSERT INTO ts_user(id, name)
VALUES
('1', '张三'),
('2', '李四'),
('3', '王五');
```

#### 4.3、修改操作

```
update ts_user set name = '李四1', age = '18' where id = '1'
```

#### 4.4、 删除操作

```
# 删除表全部内容
delete from ts_user

# 根据判断条件进行删除
delete from ts_user where id = '1'
```

### **五、运算符**

MySQL 主要有以下几种运算符：

- 算术运算符
- 比较运算符
- 逻辑运算符
- 位运算符

#### 5.1、算术运算符

| 运算符 | 描述 |          实例          |
| :----: | :--: | :--------------------: |
|   +    | 加法 | `select 1+2;` 结果为3  |
|   -    | 减法 | `select 1-2;` 结果为-1 |
|   *    | 乘法 | `select 2*3;` 结果为6  |
|   /    | 除法 | `select 6/3;` 结果为2  |
|   %    | 取余 | `select 10%3;` 结果为1 |

说明：**在除法运算和模运算中，如果除数为0，将是非法除数，返回结果为NULL**。

#### 5.2、比较运算符

SELECT 语句中的条件语句经常要使用比较运算符。通过这些比较运算符，可以判断表中的哪些记录是符合条件的。比较结果为真，则返回 1，为假则返回 0，比较结果不确定则返回 NULL。

|      运算符      |              描述               |                             实例                             |
| :--------------: | :-----------------------------: | :----------------------------------------------------------: |
|        =         |              等于               | `select * from t_user where user_id = 1` 查询用户ID为1的信息 |
|        !=        |             不等于              | `select * from t_user where user_id != 1` 查询用户ID不为1的信息 |
|        >         |              大于               | `select * from t_user where user_id > 1` 查询用户ID大于1的信息 |
|        >=        |              大于               | `select * from t_user where user_id >= 1` 查询用户ID大于等于1的信息 |
|        <         |              大于               | `select * from t_user where user_id < 1` 查询用户ID小于1的信息 |
|        <=        |              大于               | `select * from t_user where user_id <= 1` 查询用户ID小于等于1的信息 |
|   BETWEEN AND    |           在两值之间            | `select * from t_user where user_id between 1 and 100` 查询用户ID在1和100之间的信息，类似`user_id >=1 and user_id <=100` |
| NOT  BETWEEN AND |          不在两值之间           | `select * from t_user where user_id not between 1 and 100` 查询用户ID不在1和100之间的信息，类似`user_id <1 and user_id >100` |
|        IN        |            在集合中             | `select * from t_user where user_id in ('1','2')` 查询用户ID为 1 或者 2 的信息 |
|      NOT IN      |           不在集合中            | `select * from t_user where user_id not in ('1','2')` 查询用户ID不为 1 和 2 的信息 |
|       LIKE       | 模糊匹配,`%`表示0个或者多个匹配 | `select * from t_user where user_name like '%张%'` 查询用户姓名包含`张`的信息 |
|     IS NULL      |              为空               | `select * from t_user where user_name is null` 查询用户姓名为空的信息 |
|   IS NOT NULL    |             不为空              | `select * from t_user where user_name not is null` 查询用户姓名不为空的信息 |

说明：**mysql中，IN 语句中参数个数是不限制的。不过对整段 sql 语句的长度有了限制，最大不超过 4M**！

#### 5.3、逻辑运算符

逻辑运算符用来判断表达式的真假。如果表达式是真，结果返回 1。如果表达式是假，结果返回 0。

|  运算符  |   描述   |            实例             |
| :------: | :------: | :-------------------------: |
| NOT 或 ! |  逻辑非  |   `select not 1;` 结果为0   |
|   AND    |  逻辑与  |  `select 2 and 0;` 结果为0  |
|    OR    |  逻辑或  |  `select 2 or 0;` 结果为1   |
|   XOR    | 逻辑异或 | `select null or 1;` 结果为1 |

#### 5.4、位运算符

位运算符是在二进制数上进行计算的运算符。位运算会先将操作数变成二进制数，进行位运算。然后再将计算结果从二进制数变回十进制数。

| 运算符 |   描述   |                  实例                   |
| :----: | :------: | :-------------------------------------: |
|   &    |  按位与  |          `select 3&5;` 结果为1          |
|   I    |  按位或  |          `select 3I5;` 结果为7          |
|   ^    | 按位异或 |          `select 3I5;` 结果为7          |
|   ^    | 按位异或 |          `select 3^5;` 结果为6          |
|   ~    | 按位取反 | `select ~18446744073709551612;` 结果为3 |
|   >>   | 按位右移 |         `select 3>>1;` 结果为1          |
|   <<   | 按位左移 |         `select 3<<1;` 结果为6          |

#### 5.5、运算符优先级

| 优先级(从高到底) |                         运算符                         |
| :--------------: | :----------------------------------------------------: |
|        1         |                           !                            |
|        2         |                -（负号）,~（按位取反）                 |
|        3         |                     ^（按位异或）                      |
|        4         |                    *,/(DIV),%(MOD)                     |
|        5         |                          +,-                           |
|        6         |                         >>,<<                          |
|        7         |                           &                            |
|        8         |                           I                            |
|        9         | =(比较运算),<=>,<,<=,>,>=,!=,<>,IN,IS NULL,LIKE,REGEXP |
|        10        |            BETWEEN AND,CASE,WHEN,THEN,ELSE             |
|        11        |                          NOT                           |
|        12        |                         &&,AND                         |
|        13        |                          XOR                           |
|        14        |                         II,OR                          |
|        15        |                     =(赋值运算),:=                     |

说明：**在无法确定优先级的情况下，可以使用圆括号`()`来改变优先级，并且这样会使计算过程更加清晰**。

### **六、视图**

视图（view）是一种虚拟存在的表，是一个逻辑表，本身并不包含数据。作为一个select语句保存在数据字典中的。

#### 6.1、创建视图

```
CREATE [OR REPLACE] [ALGORITHM = {UNDEFINED | MERGE | TEMPTABLE}]
    VIEW view_name [(column_list)]
    AS select_statement
   [WITH [CASCADED | LOCAL] CHECK OPTION]
```

参数说明：

- **OR REPLACE**：表示替换已有视图
- **ALGORITHM**：表示视图选择算法，默认算法是UNDEFINED(未定义的)：MySQL自动选择要使用的算法 ；merge合并；temptable临时表
- **select_statement**：表示select语句
- **[WITH [CASCADED | LOCAL] CHECK OPTION]**：表示视图在更新时保证在视图的权限范围之内
- **cascade**：是默认值，表示更新视图的时候，要满足视图和表的相关条件
- **local**：表示更新视图的时候，要满足该视图定义的一个条件即可

基本格式：

```
create view <视图名称>[(column_list)]
       as select语句
       with check option;
```

创建视图示例：

```
create view v_user(用户名,年龄)
as
select user_name,age from t_user
with check option;
```

#### 6.2、查看视图

- 使用show create view语句查看视图信息

```
show create view v_user;
```

- 视图一旦创建完毕，就可以像一个普通表那样使用，视图主要用来查询

```
select * from v_user;
```

#### 6.3、删除视图

删除视图是指删除数据库中已存在的视图，删除视图时，只能删除视图的定义，不会删除数据，也就是说不动基表：

```
DROP VIEW [IF EXISTS]   
view_name [, view_name] ...
```

删除示例：

```
drop view IF EXISTS v_user;
```

### **七、小结**

本文主要围绕 Mysql 中常用的语法进行一次梳理和介绍，这些语法大部分也同样适用于其他的数据库，例如 oracle、sqlserver、postgres 等等，在下篇文字中，我们会重点介绍剩下的部分。