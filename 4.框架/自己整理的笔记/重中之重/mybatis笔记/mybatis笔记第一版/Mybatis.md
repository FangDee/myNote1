# Mybatis

## 1 Mybatis概述

~~~markdown
   1.MyBatis是Apache的一个开源项目iBatis, 2010年6月这个项目由Apache Software 	Foundation 迁移到了Google Code，随着开发团队转投Google Code旗下， iBatis3.x	正式更名为MyBatis ，代码于2013年11月迁移到Github
   2.iBatis一词来源于“internet”和“abatis”的组合，是一个基于Java的持久层框架。 iBatis	提供的持久层框架包括SQL Maps和Data Access Objects（DAO）
~~~

~~~markdown
## 1.MyBatis 是支持定制化 SQL、存储过程以及高级映射的优秀的持久层框架
## 2.MyBatis 避免了几乎所有的 JDBC 代码和手动设置参数以及获取结果集
## 3.MyBatis可以使用简单的XML或注解用于配置和原始映射，将接口和Java的POJO（Plain Old Java        Objects，普通的Java对象）映射成数据库中的记录
## 4.Mybatis 是一个半自动的ORM（Object   Relation  Mapping）框架
~~~

**下载网址**

​     https://github.com/mybatis/mybatis-3/

### 1.1 现有持久化技术对比

~~~markdown
## **JDBC**
①  SQL夹在Java代码块里，耦合度高导致硬编码内伤
②  维护不易且实际开发需求中sql有变化，频繁修改的情况多见
## **Hibernate和JPA**
①  长难复杂SQL，对于Hibernate而言处理也不容易
②  内部自动生产的SQL，不容易做特殊优化
③  基于全映射的全自动框架，大量字段的POJO进行部分映射时比较困难。导致数据库性能下降
## **MyBatis**
①  对开发人员而言，核心sql还是需要自己优化
②  sql和java编码分开，功能边界清晰，一个专注业务、一个专注数据
~~~

## 2.Mybatis案例

~~~markdown
环境搭建步骤：
## 1.导入jar包
mybatis-3.4.1.jar
log4j.jar
mysql-connector-java-5.1.37-bin.jar
junit-4.13.2.jar
hamcrest-core-1.3.jar
## 2.创建mybatis的核心配置文件（全局配置文件）并且配置，一般叫mybatis-config.xml   -- 如何连接数据库
## 3.创建映射文件并且配置,一般叫XXXMappertaining.xml  -- 如何操作数据库
## 4.创建mapper接口与映射实体类，需要实现两个绑定才可以调用接口中的方法执行sql语句
    1.映射文件与接口进行绑定
    2.映射文件与接口的方法进行绑定，并且返回值类=类型与实体类进行绑定
## 5.获取mybatis操作数据的会话对象SqlSession,通过getMapper()方法获取接口的动态代理实现类
## 6.测试
~~~

 ### 2.1 编写2个配置文件

**log4j.xml**

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
 
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
 
 <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
   <param name="Encoding" value="UTF-8" />
   <layout class="org.apache.log4j.PatternLayout">
    <param name="ConversionPattern" value="%-5p %d{MM-dd HH:mm:ss,SSS} %m  (%F:%L) \n" />
   </layout>
 </appender>
 <logger name="java.sql">
   <level value="debug" />
 </logger>
 <logger name="org.apache.ibatis">
   <level value="info" />
 </logger>
 <root>
   <level value="debug" />
   <appender-ref ref="STDOUT" />
 </root>
</log4j:configuration>
~~~

**全局配置文件mybatis-config.xml**

核心配置文件体现了如何**连接数据库以及一些基本配置，并且绑定映射文件**

```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
    DOCTYPE后面的值一定是当前文件的根标签名
-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!--
           <mapper>:引入映射文件
                     指定映射文件的位置
        -->
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
```

**映射文件**

体现了如何操作数据库

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.UserMapper">
    <!--
      <select》标签：定义Sql语句中的查询语句
          id:设置sql语句的唯一标识，要与接口的方法名保持一致，说明方法执行该sql
          resultType:结果类型，即实体类的全限定名
    -->
    <select id="getUserById" resultType="com.atguigu.bean.User">
           select * from user where id = #{id}
    </select>
</mapper>
~~~

### 2.2 编写java类

**实体类**

~~~java
package com.atguigu.bean;

public class User {
    private int id;
    private String username;
    private String birthday;
    private String sex;
    private String address;

    public int getId() {
        return id;
    }

    public void setId(int id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getBirthday() {
        return birthday;
    }

    public void setBirthday(String birthday) {
        this.birthday = birthday;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday='" + birthday + '\'' +
                ", sex='" + sex + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}

~~~

**接口**

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.User;

public interface UserMapper {
    User getUserById(int id);
}

~~~

### 2.3 测试

**测试**

~~~java
package com.atguigu.test;

import com.atguigu.bean.User;
import com.atguigu.mapper.UserMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;

public class TestMybatis {
    @Test
    public void testMybatis(){
        try {
            //1. 通过Resources读取mybatis的核心配置文件，加载读取主配置文件(sqlMapConfig.xml)
            InputStream stream = Resources.getResourceAsStream("mybatis-config.xml");
            // 2.创建sqlSessionFactory工厂，它是框架的核心对象，很重要，根据读的取配置文件创建SqlSessionFactory
            SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(stream);
            // 3.获取SqlSession对象，它是与我们开发直接相关的一个对象
            SqlSession sqlSession = sqlSessionFactory.openSession();
            // 4.获取接口对象
            // getMapper()这是mybatis的核心方法，会通过代理模式创建接口的代理对象，动态生成UserMapper的代理实现类
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 5.调用接口方法
            User user = mapper.getUserById(10);
            System.out.println(user);
             // 6.释放资源
            sqlSession.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
~~~

### 2.4 总结

**一个接口、一个实体类、两个配置文件**

![](Mybatis.assets/Snipaste_2021-05-30_01-18-35-1622308725702.png)

![](Mybatis.assets/Snipaste_2021-05-30_01-15-24-1622308572933.png)

## 3.核心API

### 3.1 Resources

```java
org.apache.ibatis.io.Resources;
加载资源的工具类
核心方法：
    InputStream getResourceAsStream();  
```

### 3.2 SqlSessionFactoryBuilder

~~~java
org.apache.ibatis.session.SqlSessionFactoryBuilder
SqlSessionFactoryBuilder工具类：用于获取SqlSessionFactory工厂对象
核心方法：
   SqlSessionFactory build(InputStream inputStream) 
~~~

### 3.3 SqlSessionFactory

~~~java
org.apache.ibatis.session.SqlSessionFactory
SqlSessionFactory工厂对象：用于获取SqlSession构建者对象的接口，它是框架的核心对象
核心方法：
    SqlSession openSession()：获取SqlSession构建者对象，并且开启手动提交事务
    SqlSession openSession(boolean var1)：获取SqlSession构建者对象，并且如果参数为true，则开启手动提交事务
~~~

### 3.4 SqlSession

~~~java
org.apache.ibatis.session.SqlSession:构建者对象接口，它是与我们开发直接相关的一个对象
    用于执行sql，
    管理事务，
    接口代理
    第一个参数：接口名.方法名；第二个参数：参数
~~~

![](Mybatis.assets/Snipaste_2021-05-30_09-55-35-1622339748940.png)

## 4.★核心配置文件标签介绍

里面可以定义的内容

~~~xml
• properties：定义属性
• settings ：定义全局设置
• typeAliases：定义类型别名
• typeHandlers：定义类型处理的
• objectFactory：对象工厂
• plugins：定义插件
• environments
• databaseIdProvider：定义数据库产品标识，mysql，Oracle,...
• mappers:引入映射文件，注册映射文件
~~~

### 4.1 **properties**

>```
><properties>:设置或者引入资源文件
>```
>
>有两种用法：
>
>​         引入外部的资源文件
>
>​         通过子标签定义资源文件

~~~xml
用法1：
<?xml version="1.0" encoding="UTF-8" ?>
<!--
    DOCTYPE后面的值一定是当前文件的根标签名
-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <properties>
        <!-- 
            用法1：通过子标签<property>定义好属性值，其他地方引用
        -->
        <property name="driver" value="com.mysql.jdbc.Driver"/>
    </properties>
    
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${driver}"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
~~~

![](Mybatis.assets/Snipaste_2021-05-30_10-44-41-1622342699606.png)

~~~xml
用法2：
<?xml version="1.0" encoding="UTF-8" ?>
<!--
    DOCTYPE后面的值一定是当前文件的根标签名
-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!--
        用法二：
             可以通过properties标签的属性：
                url:指定网络地址或者磁盘路径的文件地址，网络路径或者磁盘文件路径下访问文件
                resource:指定类路径下的文件地址
    -->
    <properties resource="jdbc.properties"></properties>

    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
~~~

![](Mybatis.assets/Snipaste_2021-05-30_10-55-23-1622343340102.png)

### 4.2 **★settings**

>这是mybatis重要的一个调整设置，会改变mybatis的运行时行为

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
    DOCTYPE后面的值一定是当前文件的根标签名
-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <properties resource="jdbc.properties"></properties>
    
    <settings>
        <!--
            mapUnderscoreToCamelCase:将下划线转化为驼峰
            具体表现：
                假如数据库字段：
                    user_name
                实体类属性：
                    userName
            此时sql语句不需要将user_name设置别名userName，也可以将user_name字段的值映射到实体类的userName属性，就是因为我们设置了mapUnderscoreToCamelCase
        -->
        <!-- 开启驼峰命名自动映射-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
    </settings>
    
    
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
~~~

| 设置名                                      | 描述                                                         | 默认值                              |
| ------------------------------------------- | ------------------------------------------------------------ | ----------------------------------- |
| cacheEnabled                                | 缓存的全局开关                                               | true                                |
| lazyLoadingEnabled                          | 延迟加载的全局开关                                           | false                               |
| aggressiveLazyLoading（是否查询所有的信息） | 开启时，任意方法的调用都会加载该对象的所有延迟加载属性，否则，每个延迟加载属性会按需加载 | 3.4.1之前默认是true,现在默认是false |
| useGeneratedKeys                            | 允许JDBC支持自动生成主键，需要数据库驱动支持                 | false                               |
| autoMappingBehavior                         | 指定Mybatis应如何自动映射到字段或者属性。NONE表示关闭自动映射；PARTIAL只会自动映射没有定义嵌套结果映射的字段。FULL会自动映射任何复杂的结果集 | partial                             |
| mapUnderscoreToCamelCase                    | 是否开启驼峰命名自动映射，即从数据库列名A+COLUMN映射到属性aColumn | false                               |

### 4.3 **★typeAliases**

>用于给映射实体类起别名。
>
>子标签1：<typeAlias>
>
> 不区分大小写，别名的大小写不敏感！！！
>
>​        属性type：值为类的全限定名，java类型
>
>​        属性alias：指定别名，如果没有指定则是类名的首字母小写，但是别名的大小写不敏感
>
>子标签2：<package>,通过指定包名，给包下所有的类起别名

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
    DOCTYPE后面的值一定是当前文件的根标签名
-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <properties resource="jdbc.properties"></properties>

    <!--
        <typeAliases>:可以为java类型设置一个简短的别名
             子标签1：<typeAlias> ,这个子标签可以定义多个，意味着可以给多个类定义别名,为类型设置类型别名
             <typeAlias>标签有属性
                     属性type：值为类的全限定名，java类型
                     属性alias：可以不给值，此时默认值为类名，并且不区分大小写
                          如type="com.atguigu.bean.User" ，此时alias如果不给值，则默认值就是User或者user
                     此时映射文件的<mapper>标签的查询子标签的resultType属性值就是alias的值，而不用写映射的全类名
             子标签2：<package>,通过指定包名，给包下所有的类起别名
    -->
    <typeAliases>
        <typeAlias type="com.atguigu.bean.User" alias="U"></typeAlias>
        <package name="com.atguigu.bean"/>
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.UserMapper">
    <!--
      <select>标签：定义Sql语句中的查询语句
          id:设置sql语句的唯一标识，要与接口的方法名保持一致，说明方法执行该sql
          resultType:结果类型，即实体类的全限定名
    -->
    <select id="getUserById" resultType="U">
           select * from user where id = #{id}
    </select>
</mapper>
~~~

![](Mybatis.assets/Snipaste_2021-05-30_11-21-13-1622344891045.png)

![](Mybatis.assets/Snipaste_2021-06-01_08-58-17-1622509126252.png)

### 4.4 typeHandlers与 plugins

>typeHandlers:类型处理器，一般用来处理日期
>
>plugins：插件管理器：配置插件的

~~~xml
<!--
        <typeHandlers>标签：配置类处理器的
    -->
   <typeHandlers>
       <typeHandler handler="类处理器的全类名"></typeHandler>
   </typeHandlers>
<!--
    <plugins>标签：配置插件
     -->
    <plugins>
        <plugin interceptor="配置插件对应的类的全类名"></plugin>
    </plugins>
~~~

### 4.5 **★environments**

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
    DOCTYPE后面的值一定是当前文件的根标签名
-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <!--
        <environments>:用来设置连接数据的环境，
           里面有多个<environment>标签，说明可以设置多个数据库环境，每个环境使用id作为各自唯一的标识
           default属性：设置默认使用的数据库环境，与多个<environment>标签的其中某一个的id保持一致
    -->
    <environments default="development">
        <!--
            <environment>：设置某个具体的数据库环境
                  id:数据库环境的唯一标识
                  每个环境的事务管理器和数据源都是必须指定的！
                  子标签1：<transactionManager>:设置事务管理器，
                          有type属性有两个值：
                                   JDBC:使用原始的JDBC的方式来管理事务，即提交和回滚都需要手动处理
                                   MANAGED:谁能管理让谁管理，自己不管理，让容器来管理事务的整个生命周期，例如Spring有事务管理，可以给Spring
                  子标签2： <dataSource>:设置数据源，里面用来设置数据库的连接属性
                          有type属性有三个值：
                                   POOLED:默认使用mybatis自带的数据库连接池
                                   UNPOOLED:不使用连接池
                                   JNDI:调用上下文中的数据源
        -->
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <!--
           <mapper>:引入映射文件
                     指定映射文件的位置
        -->
        <mapper resource="UserMapper.xml"/>
    </mappers>
</configuration>
~~~

### 4.7 databaseIdProvider 数据库产品标识

>指定数据库厂商的别名：其中会和mapper映射文件的查询标签里里面的databaseId对应
>
>也就是配置了databaseIdProvider后，在SQL映射文件中增删改查标签中使用databaseId属性

~~~xml
核心配置文件：
 <databaseIdProvider type="DB_VENDOR">
        <!--
         name:数据库厂商标识
         value:为标识起一个别名
         -->
        <property name="MySQL" value="mysql"/>
        <property name="Oracle" value="oracle"/>
    </databaseIdProvider>

接口映射文件：
 <select id="getEmpByEid" resultType="com.atguigu.bean.Emp" databaseId="mysql">
         select eid,ename,age,sex from emp where eid=#{eid}
 </select>

 <select id="getEmpByEid" resultType="com.atguigu.bean.Emp" databaseId="oracle">
         select eid,ename,age,sex from emp where eid=#{eid}
 </select>

~~~

**这样就可以给相同的方法指定不同的数据库产品，来区分不同类型的数据库的SQL语句的问题**

### 4.8 ★mappers标签注册SQL映射文件标签

mybatis核心配置文件中的<mappers>里面可以设置多个<mapper>子标签，但是当子标签过多时，就显得臃肿。此时我们可以**通过package标签来将接口映射文件管理到一个文件夹下**

**前提：此时要求mapper接口和mapper映射文件必须在同一个包下，不同包不同名则通过在Mapper接口的方法上添加注解写sql语句！**

~~~xml
<!--
      <mappers>标签：注册mapper映射文件，也是SQL映射文件
    -->
    <mappers>
        <!--
         子标签1 mapper:
              resource属性：指定类路径下的sql映射文佳的路径
              url:指定磁盘或者网络下的sql映射文件的路径
              class属性：指定mapper接口的全类名，使用class属性注册sql映射文件时，映射文件必须与mapper接口同包且同名
                        或者不同包不同名则通过在Mapper接口的方法上添加注解写sql语句！
         子标签2 package:
              通过指定包名注册映射文件
              使用package属性注册sql映射文件时，映射文件必须与mapper接口同包且同名
                        或者不同包不同名则通过在Mapper接口的方法上添加注解写sql语句！
         -->
        <mapper resource="EmpMapper.xml"/>
    </mappers>
~~~

![](Mybatis.assets/Snipaste_2021-05-31_23-02-01-1622473337243.png)

**注意**

mybatis核心配置文件的标配是有先后顺序的，从上到下:<properties>-><mappers>

## 5.单表增删改查与手动提交事务

**映射实体类**

~~~java
package com.atguigu.bean;

public class Emp {
    private Integer eid;
    private String ename;
    private Integer age;
    private String sex;

    public Integer getEid() {
        return eid;
    }

    public void setEid(Integer eid) {
        this.eid = eid;
    }

    public String getEname() {
        return ename;
    }

    public void setEname(String ename) {
        this.ename = ename;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    @Override
    public String toString() {
        return "Emp{" +
                "eid=" + eid +
                ", ename='" + ename + '\'' +
                ", age=" + age +
                ", sex='" + sex + '\'' +
                '}';
    }
}

~~~

**接口**

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.Emp;

import java.util.List;

public interface EmpMapper {
    // 根据eid查询一个员工信息
    Emp getEmpByEid(String eid);
    // 获取所有员工信息
    List<Emp> getAllEmp();
    // 添加员工信息
    void addEmp(Emp emp);
    // 修改员工信息
    void updateEmp(Emp emp);
    // 删除员工信息
    void deteleEmp(String eid);
}

~~~

**映射文件**

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
    <!--
    Emp getEmpByEid(String eid)
    -->
    <select id="getEmpByEid" resultType="com.atguigu.bean.Emp">
        select eid,ename,aage,sex from emp where eid=#{eid}
    </select>

    <!--
    List<Emp> getAllEmp()
    -->
    <select id="getAllEmp" resultType="com.atguigu.bean.Emp">
        select eid,ename,aage,sex from emp
    </select>

    <!--
    void addEmp(Emp emp)  ---增
    -->
    <insert id="addEmp">
        insert into emp(eid,ename,age,sex) values(null,#{ename},#{age},#{sex})
    </insert>

    <!--
    void updateEmp(Emp emp) ---改
    -->
    <update id="updateEmp">
       update emp set ename=#{ename} ,age=#{age},sex=#{sex} where eid= #{eid}
    </update>

    <!--
   void deteleEmp(String eid) ---删
   -->
    <update id="deteleEmp">
       delete from emp where eid = #{eid}
    </update>
</mapper>
~~~

**主配置文件**

~~~Xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
    DOCTYPE后面的值一定是当前文件的根标签名
-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <properties resource="jdbc.properties"></properties>

    <!--
        <typeAliases>:可以为java类型设置一个简短的别名
             里面有子标签：<typeAlias> ,这个子标签可以定义多个，意味着可以给多个类定义别名,为类型设置类型别名
             <typeAlias>标签有属性
                     属性type：值为类的全限定名，java类型
                     属性alias：可以不给值，此时默认值为类名，并且不区分大小写
                          如type="com.atguigu.bean.User" ，此时alias如果不给值，则默认值就是User或者user
                     此时映射文件的<mapper>标签的查询子标签的resultType属性值就是alias的值，而不用写映射的全类名
    -->
    <typeAliases>
        <typeAlias type="com.atguigu.bean.User" alias="U"></typeAlias>
    </typeAliases>
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <mappers>
        <mapper resource="EmpMapper.xml"/>
    </mappers>
</configuration>
~~~

**测试**

~~~java
package com.atguigu.test;

import com.atguigu.bean.Emp;
import com.atguigu.mapper.EmpMapper;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis(){
        try {
            // 1.解析主配置文件
            InputStream stream = Resources.getResourceAsStream("mybatis-config.xml");
            // 2.获取SqlSessionFactory对象
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sqlSessionFactory = builder.build(stream);
            // 3.获取SqlSession对象
            SqlSession sqlSession = sqlSessionFactory.openSession();
            // 4.获取接口的代理对象
            EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
            // 5.执行数据库操作
            Emp emp = new Emp();
            emp.setAge(12);
            emp.setEid(6);
            emp.setEname("王二麻子");
            emp.setSex("不男不女");
            mapper.addEmp(emp);
            sqlSession.commit();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}

~~~

>```
>手动提交事务：
>SqlSession sqlSession = sqlSessionFactory.openSession(); // 这种情况下需要手动处理事务
>代码最后加着一行 sqlSession.commit();
>
>自动提交事务
>SqlSession sqlSession = sqlSessionFactory.openSession(true);// 这种情况下会自动处理事务
>```

![](Mybatis.assets/Snipaste_2021-05-31_19-53-37-1622462070154.png)



## 6.增删改的返回值为Integer或者Boolean

>增删改的返回值有两种：并且可以不在接口映射文件中注明要返回的类型
>
>​         数值：数值代表影响的行数
>
>​         boolean:boolean代表操作是否成功

~~~xml
方法
boolean updateUser(User user);

接口映射文件：
<update id="updateUser">
      update user set username=#{username},address=#{address}
</update>
~~~

~~~java
package com.atguigu.test;

import com.atguigu.bean.Emp;
import com.atguigu.bean.User;
import com.atguigu.mapper.EmpMapper;
import com.atguigu.mapper.UserMapper;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis(){
        try {
            // 1.解析主配置文件
            InputStream stream = Resources.getResourceAsStream("mybatis-config.xml");
            // 2.获取SqlSessionFactory对象
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sqlSessionFactory = builder.build(stream);
            // 3.获取SqlSession对象
            SqlSession sqlSession = sqlSessionFactory.openSession(true);// 这种情况下会手动处理事务
            // 4.获取接口的代理对象
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 5.执行数据库操作
            System.out.println(mapper.updateUser(new User(10,"张王八蛋","2014-07-10","1","朝阳区")));// true
            sqlSession.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
~~~



## 7.★主键生成方式

>支持主键自增：例如mysql
>
>不支持主键自增：例如oracle

**对于主键自增型数据库**

**方式1：**

~~~shell
# useGeneratedKeys(仅适用于inset和update)
这会令Mybatis使用JDBC的getGeneratedKeys方法来取出数据库内部自动生成的主键，默认值false
# keyProperty(仅适用于inset和update)
指定能够唯一识别对象的属性，mybatis会使用getGeneratedKeys的返回值或者insert语句的selectKey子元素
来设置返回的主键值设置为pojo的哪个属性
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.UserMapper">
    <!--
      <select>标签：定义Sql语句中的查询语句
          id:设置sql语句的唯一标识，要与接口的方法名保持一致，说明方法执行该sql
          resultType:结果类型，即实体类的全限定名
    -->
    <select id="getUserById" resultType="com.atguigu.bean.User">
           select * from user where id = #{id}
    </select>

    <!--
        useGeneratedKeys:用来设置是否让JDBC返回自动生成的主键
        如果为true，则返回主键
        keyProperty:用来设置返回的主键值设置为pojo的哪个属性
    -->
    <insert id="addUser" useGeneratedKeys="true" keyProperty="id" >
        insert into user(username,birthday,sex,address) values(#{username},#{birthday},#{sex},#{address})
    </insert>
</mapper>
~~~

![](Mybatis.assets/Snipaste_2021-06-01_11-10-06-1622517030105.png)

**方式2**

~~~xml
selectKey:查询主键值
      属性：
          keyColumn:主键字段
          keyProperty：主键属性
          resultType:主键字段类型
          order:在insert语句执行前，还是执行后，获取主键值。
          取值：
              BEFORE:在insert语句执行前
              AFTER:在insert语句执行后

<insert id="insertEmployee" 
		parameterType="com.atguigu.mybatis.beans.Employee" >
		<selectKey keyColumn="id" keyProperty="id" resultType="int" order="AFTER">
            select LAST_INSERT_ID()
       </selectKey>	
		insert into orcl_employee(id,last_name,email,gender) values(#{id},#{lastName},#{email},#{gender})
</insert>

~~~

~~~java
 mapper.insertUser(new User(null,"玄心","2018/02/01","1","广州市提南岸区"));
~~~

**对于不支持主键自增的数据库**

~~~shell
可以使用selectKey子元素：selectKey标签中的语句会首先运行，id会被设置，然后插入语句会被调用
~~~

![](Mybatis.assets/Snipaste_2021-06-01_11-14-27-1622517279107.png)

## 8 ★Mybatis获取参数值的两种方式:#{}&${}

>#{}:用来给预编译对象PreparedStatement的参数赋值(通配符可以自动给字符串加单引号）
>
>可以使用通配符操作sql,可以防止sql注入，再为string赋值时，可以自动为String加单引号
>
>```xml
>Preparing: insert into User (username,birthday,sex,address) values( ?,?,?,?)  
>
><insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
>   insert into User (username,birthday,sex,address) values( #{username},#{birthday},#{sex},#{address})
></insert>
>```
>
>${}:用来给Ststement对象的参数赋值(无法自动给字符串加单引号）
>
>必须使用字符串拼接的方式操作Sql语句，不能防止sql注入，一定要注意单引号问题
>
>```xml
>Preparing: insert into User (username,birthday,sex,address) values( 'root','2020/02/01','1','灌南市提南岸区')
>
><insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
>   insert into User (username,birthday,sex,address) values( '${username}','${birthday}','${sex}','${address}')
></insert>
>```

## 9 ★入参的参数传递方式

~~~markdown
## 1.单个普通类型参数传递
    可以接受普通类型，包装类型，字符串类型等，这种情况下mybatis可以直接使用，不需要经过任何处理
    #{}:可以以任意的名字获取参数值
    ${}:只能以${value}或者${_parameter}获取
## 2.多个参数
    任意多个参数，都会被MyBatis重新包装成一个Map传入。Map的key是param1，param2，或者0，1…，值就是参数的值，要注意${}的单引号问题
     #{}:#{0}，#{1}||#{parme1}，#{parme2}
     ${}:${parme1}， ${parme2}
## 3.命名参数
    为参数使用@Param起一个名字，MyBatis就会将这些参数封装进map中，key就是我们自己指定的名字
    此时param1，param2也仍然可以用
## 4.POJO
    当这些参数属于我们业务POJO时，我们直接传递POJO，
    #{}和${}里面都可以写属性名获取参数值，但是要注意${}的单引号问题
## 5.Map
    我们也可以封装多个参数为map，#{}和${}里面都可以写集合键的名获取参数值，但是要注意${}的单引号问题
## 6.Collection/Array
    会被MyBatis封装成一个map传入, Collection对应的key是collection,Array对应的key是array. 如果确定是List集合，key还可以是list.
~~~

![](Mybatis.assets/Snipaste_2021-06-01_21-13-02-1622553192294.png)

### 9.1 单个参数

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致， namespace：是mapper接口的全类名或者接口的全限定名的别名
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.UserMapper">

    <!-- User getUserById(int id); -->
    <select id="getUserById" resultType="com.atguigu.bean.User">
        select * from user where id=${value}
        select * from user where id=${_parameter}
    </select>


</mapper>
~~~

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.User;

public interface UserMapper {
   User getUserById(int id);

}
~~~

### 9.2**多个参数**

~~~Xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<!--
     namespace：必须是mapper接口的全类名
 -->
<mapper namespace="com.atguigu.mapper.UserMapper">
    <!--
      <select> 标签：定义Sql语句中的查询语句
          id:设置sql语句的唯一标识，要与接口的方法名保持一致，说明方法执行该sql
          resultType:结果类型，即实体类的全限定名
    -->

    <!--
        参数处理：
            1.单个参数：
                 Mybatis不做任何处理，
                 #{}:可以以任意的名字获取参数值，通过#{key}填充占位符时的keu可以任意指定
                 ${}:只能以${value}或者${_parameter}获取
            2.多个参数：
                 Mybatis获取以后会转化为一个Map，填充占位符时的key是0、1...或者是parme1、parm2...
                 #{}:#{0}，#{1}||#{parme1}，#{parme2}
                 ${}:${parme1}， ${parme2}
     -->
    <select id="getUserByUserIdAndAddress" resultType="com.atguigu.bean.User">
           select * from user where username = #{param1} and address=#{param2}
    </select>
</mapper>
~~~

~~~java

package com.atguigu.test;

import com.atguigu.bean.Emp;
import com.atguigu.bean.User;
import com.atguigu.mapper.EmpMapper;
import com.atguigu.mapper.UserMapper;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis(){
        try {
            // 1.解析主配置文件
            InputStream stream = Resources.getResourceAsStream("mybatis-config.xml");
            // 2.获取SqlSessionFactory对象
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sqlSessionFactory = builder.build(stream);
            // 3.获取SqlSession对象
            SqlSession sqlSession = sqlSessionFactory.openSession(true);// 这种情况下会手动处理事务
            // 4.获取接口的代理对象
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 5.执行数据库操作
            System.out.println(mapper.getUserByUserIdAndAddress("陈小明","河南郑州"));
            sqlSession.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
User{id=22, username='陈小明', birthday='null', sex='1', address='河南郑州'}
~~~

### 9.3**命名参数**

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.User;
import org.apache.ibatis.annotations.Param;

public interface UserMapper {

    User getUserByUserIdAndAddress(@Param("username") String username,@Param("address") String address);
}

~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<!--
     namespace：必须是mapper接口的全类名
 -->
<mapper namespace="com.atguigu.mapper.UserMapper">
    <!--
      <select> 标签：定义Sql语句中的查询语句
          id:设置sql语句的唯一标识，要与接口的方法名保持一致，说明方法执行该sql
          resultType:结果类型，即实体类的全限定名
    -->

    <!--
        参数处理：
            1.单个参数：
                 Mybatis不做任何处理，
                 #{}:可以以任意的名字获取参数值，通过#{key}填充占位符时的keu可以任意指定
                 ${}:只能以${value}或者${_parameter}获取
            2.多个参数：
                 Mybatis获取以后会转化为一个Map，填充占位符时的key是arg0、arg1...或者是parme1、parm2...
            3.命名参数：
                 Mybatis获取以后会转化为一个Map，填充占位符时的key是arg0、arg1...或者是parme1、parm2...
                 如果接口方法入参前面添加了@Param注解，那么填充占位符的key就是注解中指定的值
     -->
    <select id="getUserByUserIdAndAddress" resultType="com.atguigu.bean.User">
           select * from user where username = #{username} and address=#{address}
    </select>
</mapper>
~~~

~~~java
package com.atguigu.test;

import com.atguigu.bean.Emp;
import com.atguigu.bean.User;
import com.atguigu.mapper.EmpMapper;
import com.atguigu.mapper.UserMapper;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis(){
        try {
            // 1.解析主配置文件
            InputStream stream = Resources.getResourceAsStream("mybatis-config.xml");
            // 2.获取SqlSessionFactory对象
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sqlSessionFactory = builder.build(stream);
            // 3.获取SqlSession对象
            SqlSession sqlSession = sqlSessionFactory.openSession(true);// 这种情况下会手动处理事务
            // 4.获取接口的代理对象
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 5.执行数据库操作
            System.out.println(mapper.getUserByUserIdAndAddress("陈小明","河南郑州"));
            sqlSession.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}

~~~

![](Mybatis.assets/Snipaste_2021-06-01_12-45-34-1622522748414.png)

### 9.4**POJO**

~~~Xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<!--
     namespace：必须是mapper接口的全类名
 -->
<mapper namespace="com.atguigu.mapper.UserMapper">
    <!--
      <select> 标签：定义Sql语句中的查询语句
          id:设置sql语句的唯一标识，要与接口的方法名保持一致，说明方法执行该sql
          resultType:结果类型，即实体类的全限定名
    -->

    <!--
        参数处理：
            1.单个参数：
                 Mybatis不做任何处理
                    #{}:可以以任意的名字获取参数值，通过#{key}填充占位符时的keu可以任意指定
                    ${}:只能以${value}或者${_parameter}获取
            2.多个参数：
                 Mybatis获取以后会转化为一个Map，填充占位符时的key是arg0、arg1...或者是parme1、parm2...
            3.命名参数：
                 Mybatis获取以后会转化为一个Map，填充占位符时的key是arg0、arg1...或者是parme1、parm2...
                 如果入参前面添加了@Param注解，那么填充占位符的key就是注解中指定的值
            4.pojo参数
                 如果传递的参数是一个pojo,那么可以传入pojo,此时占位符是pojo的属性，但是要注意${}的单引号问题

     -->
    <select id="getUserByUserIdAndAddress" resultType="com.atguigu.bean.User">
           select * from user where username = #{username} and address=#{address}
       
    </select>
    <insert id="insertUser" useGeneratedKeys="true" keyProperty="id">
   insert into User (username,birthday,sex,address) values(    '${username}','${birthday}','${sex}','${address}')
    </insert>
</mapper>
~~~

### 9.5**Map**

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.User;
import org.apache.ibatis.annotations.Param;

import java.util.Map;

public interface UserMapper {

    User getUserByUserIdAndAddress(Map<String,String> map);
}

~~~

~~~java
package com.atguigu.test;

import com.atguigu.bean.Emp;
import com.atguigu.bean.User;
import com.atguigu.mapper.EmpMapper;
import com.atguigu.mapper.UserMapper;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis(){
        try {
            // 1.解析主配置文件
            InputStream stream = Resources.getResourceAsStream("mybatis-config.xml");
            // 2.获取SqlSessionFactory对象
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sqlSessionFactory = builder.build(stream);
            // 3.获取SqlSession对象
            SqlSession sqlSession = sqlSessionFactory.openSession(true);// 这种情况下会手动处理事务
            // 4.获取接口的代理对象
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 5.执行数据库操作
            HashMap<String, String> map = new HashMap<>();
            map.put("aa","陈小明");
            map.put("bb","河南郑州");
            System.out.println(mapper.getUserByUserIdAndAddress(map));
            sqlSession.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}

~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<!--
     namespace：必须是mapper接口的全类名
 -->
<mapper namespace="com.atguigu.mapper.UserMapper">
    <!--
      <select> 标签：定义Sql语句中的查询语句
          id:设置sql语句的唯一标识，要与接口的方法名保持一致，说明方法执行该sql
          resultType:结果类型，即实体类的全限定名
    -->

    <!--
        参数处理：
            1.单个参数：
                 Mybatis不做任何处理，
                     #{}:可以以任意的名字获取参数值，通过#{key}填充占位符时的keu可以任意指定
                     ${}:只能以${value}或者${_parameter}获取
            2.多个参数：
                 Mybatis获取以后会转化为一个Map，填充占位符时的key是arg0、arg1...或者是parme1、parm2...
            3.命名参数：
                 Mybatis获取以后会转化为一个Map，填充占位符时的key是arg0、arg1...或者是parme1、parm2...
                 如果入参前面添加了@Param注解，那么填充占位符的key就是注解中指定的值
            4.pojo参数
                 如果传递的参数是一个pojo,那么可以传入pojo,此时占位符是pojo的属性名
            5.map参数
                 如果传递的参数是一个map,那么1填充占位符的可以是map指定的key，#{}和${}里面都可以写集合键的名获取参数值，但是要注意${}的单引号问题
     -->
    <select id="getUserByUserIdAndAddress" resultType="com.atguigu.bean.User">
           select * from user where username = #{aa} and address=#{bb}
    </select>


</mapper>
~~~

![](Mybatis.assets/Snipaste_2021-06-01_12-59-11-1622523569084.png)

## 10 ★Select 查询的几种情况

~~~markdown
## 1.查询单行数据返回一个map
Employee getEmployeeById(Integer id);
~~~

~~~markdown
## 2.查询多行数据返回对象的集合
List<employee> getEmployees();
~~~

~~~java
接口映射文件
package com.atguigu.mapper;

import com.atguigu.bean.User;
import java.util.List;

public interface UserMapper {

    List<User> getUsers();
}
~~~

~~~xml
接口配置文件
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<!--
     namespace：必须是mapper接口的全类名
 -->
<mapper namespace="com.atguigu.mapper.UserMapper">
    <!--
      <select> 标签：定义Sql语句中的查询语句
          id:设置sql语句的唯一标识，要与接口的方法名保持一致，说明方法执行该sql
          resultType:结果类型，即实体类的全限定名
    -->

    <!--
       List<User> getUsers();
       当方法的返回值是一个List时，resultType的值指定的是泛型的类型
    -->
    <select id="getUsers" resultType="com.atguigu.bean.User">
        select  id,username,birthday,sex,address from user
    </select>


</mapper>
~~~

~~~java
测试
package com.atguigu.test;

import com.atguigu.bean.Emp;
import com.atguigu.bean.User;
import com.atguigu.mapper.EmpMapper;
import com.atguigu.mapper.UserMapper;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis(){
        try {
            // 1.解析主配置文件
            InputStream stream = Resources.getResourceAsStream("mybatis-config.xml");
            // 2.获取SqlSessionFactory对象
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sqlSessionFactory = builder.build(stream);
            // 3.获取SqlSession对象
            SqlSession sqlSession = sqlSessionFactory.openSession(true);// 这种情况下会自动处理事务
            // 4.获取接口的代理对象
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 5.执行数据库操作
            System.out.println(mapper.getUsers());
            sqlSession.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}

~~~

~~~markdown
## 3.查询单行数据返回map集合
此时数据库的字段名作为key
数据库的字段值作为value
Map<String,String> getEmployeeByIdReturnMap(Integer id);
~~~

~~~java
接口
package com.atguigu.mapper;

import com.atguigu.bean.User;
import java.util.Map;

public interface UserMapper {

   Map<String,User> getUserById(Integer id);
}

~~~

~~~xml
配置文件
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<!--
     namespace：必须是mapper接口的全类名
 -->
<mapper namespace="com.atguigu.mapper.UserMapper">
    <!--
      <select> 标签：定义Sql语句中的查询语句
          id:设置sql语句的唯一标识，要与接口的方法名保持一致，说明方法执行该sql
          resultType:结果类型，即实体类的全限定名
    -->

    <!--
       Map<String,User> getUserById(Integer id);
       当方法的返回值是一个map时，resultType的值是map
    -->
    <select id="getUserById" resultType="map">
        select  id,username,birthday,sex,address from user where id=#{Id}
    </select>


</mapper>
~~~

~~~java
package com.atguigu.test;

import com.atguigu.bean.Emp;
import com.atguigu.bean.User;
import com.atguigu.mapper.EmpMapper;
import com.atguigu.mapper.UserMapper;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis(){
        try {
            // 1.解析主配置文件
            InputStream stream = Resources.getResourceAsStream("mybatis-config.xml");
            // 2.获取SqlSessionFactory对象
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sqlSessionFactory = builder.build(stream);
            // 3.获取SqlSession对象
            SqlSession sqlSession = sqlSessionFactory.openSession(true);// 这种情况下会手动处理事务
            // 4.获取接口的代理对象
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 5.执行数据库操作
            System.out.println(mapper.getUserById(10));//  {birthday=2014-07-10, address=北京市, sex=1, id=10, username=张三}
            sqlSession.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}

~~~

~~~markdown
## 4.查询多行数据返回map集合
@MapKey("id") // 指定使用数据库中的哪个字段作为map的key
Map<Integer,Employee> getEmployeesReturnMap();
~~~

~~~java
接口
package com.atguigu.mapper;

import com.atguigu.bean.User;
import org.apache.ibatis.annotations.MapKey;

import java.util.Map;

public interface UserMapper {

    @MapKey注解指定返回的多个User对象("id")// 需要通过@MapKey注解指定返回的多个User对象中的key是数据库中的哪个字段
    Map<Integer,User> getEmployeesReturnMap();
}

~~~

~~~java
映射文件
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<!--
     namespace：必须是mapper接口的全类名
 -->
<mapper namespace="com.atguigu.mapper.UserMapper">
    <!--
      <select> 标签：定义Sql语句中的查询语句
          id:设置sql语句的唯一标识，要与接口的方法名保持一致，说明方法执行该sql
          resultType:结果类型，即实体类的全限定名
    -->

    <!--
       Map<String,User> getUserById(Integer id);
       当方法的返回值是一个map时，resultType的值是map
    -->
    <select id="getEmployeesReturnMap" resultType="map">
        select  id,username,birthday,sex,address from user
    </select>


</mapper>
~~~

~~~java
测试
package com.atguigu.test;

import com.atguigu.bean.Emp;
import com.atguigu.bean.User;
import com.atguigu.mapper.EmpMapper;
import com.atguigu.mapper.UserMapper;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis(){
        try {
            // 1.解析主配置文件
            InputStream stream = Resources.getResourceAsStream("mybatis-config.xml");
            // 2.获取SqlSessionFactory对象
            SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
            SqlSessionFactory sqlSessionFactory = builder.build(stream);
            // 3.获取SqlSession对象
            SqlSession sqlSession = sqlSessionFactory.openSession(true);// 这种情况下会手动处理事务
            // 4.获取接口的代理对象
            UserMapper mapper = sqlSession.getMapper(UserMapper.class);
            // 5.执行数据库操作
            System.out.println(mapper.getEmployeesReturnMap());// {1={sex=2, id=1, username=王五}, 99={birthday=2019-11-09, id=99, username=你好}, 102={birthday=2019-11-14, address=广州, sex=1, id=102, username=用户0}, 103={birthday=2019-11-14, address=广州, sex=1, id=103, username=用户1}, 104={birthday=2021-03-04, address=广州市天河区岗顶, sex=男, id=104, username=狗蛋}, 10={birthday=2014-07-10, address=北京市, sex=1, id=10, username=张三}, 16={address=河南郑州, sex=1, id=16, username=张小明}, 22={address=河南郑州, sex=1, id=22, username=陈小明}, 24={address=河南郑州, sex=1, id=24, username=张三丰}, 88={address=广州, sex=1, id=88, username=小李}, 25={address=河南郑州, sex=1, id=25, username=陈小明2}, 26={id=26, username=王五}, 90={birthday=2019-11-08, address=广州, sex=1, id=90, username=小小李飞到}, 91={birthday=2019-11-08, address=广州7777, sex=2, id=91, username=小李飞}, 92={birthday=2019-11-08, address=广西, sex=2, id=92, username=男人婆}}

            sqlSession.close();
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}

~~~

## 11 ★高级映射ResultMap

>resultType:结果类型
>
>resultMap:结果映射，用来定义自定义映射，针对复杂映射需要用到这个标签

### 11.1 多对一

此时将1放到多中

~~~java
package com.atguigu.bean;

public class Dept {
    private Integer did;
    private String dname;

    public String getDname() {
        return dname;
    }

    public void setDname(String dname) {
        this.dname = dname;
    }

    public Integer getDid() {
        return did;
    }

    public void setDid(Integer did) {
        this.did = did;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "did=" + did +
                ", dname='" + dname + '\'' +
                '}';
    }
}

~~~

~~~java
package com.atguigu.bean;

public class Emp {
    private Integer eid;
    private String ename;
    private Integer age;
    private String sex;
    private Dept dept;   // 这个属性是一个对象

    public Dept getDept() {
        return dept;
    }

    public void setDept(Dept dept) {
        this.dept = dept;
    }

    public Integer getEid() {
        return eid;
    }

    public void setEid(Integer eid) {
        this.eid = eid;
    }

    public String getEname() {
        return ename;
    }

    public void setEname(String ename) {
        this.ename = ename;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    @Override
    public String toString() {
        return "Emp{" +
                "eid=" + eid +
                ", ename='" + ename + '\'' +
                ", age=" + age +
                ", sex='" + sex + '\'' +
                ", dept=" + dept +
                '}';
    }
}

~~~

~~~java
public interface EmpDeptMapper {
    // 获取所有的员工信息
    List<Emp> getAllEmp();
}
~~~

#### 11.1.1 方式1 

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致， namespace：是mapper接口的全类名或者接口的全限定名的别名
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpDeptMapper">
    <!--
       resultMap:自定义映射，处理复杂的表关系
       子标签<id>：设置主键的映射关系
            column：设置数据库字段名
            property：设置实体类属性名
       子标签<result>:设置非主键的映射关系，不是主键的都通过这个标签设置
    -->
    <resultMap id="han" type="com.atguigu.bean.Emp">
        <id column="eid" property="eid"></id>
        <result column="ename" property="ename"></result>
        <result column="age" property="age"></result>
        <result column="sex" property="sex"></result>
        <result column="did" property="dept.did"></result>
        <result column="dname" property="dept.dname"></result>
    </resultMap>

  <select id="getAllEmp" resultMap="han">
      select emp.eid,emp.ename,emp.age,emp.sex,emp.did
          ,dep.dname from emp,dep where emp.did=dep.did
  </select>
</mapper>
~~~

#### 11.1.2★ 方式2 association

>POJO中的属性可能会是一个对象,使用association标签定义对象的封装规则

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致， namespace：是mapper接口的全类名或者接口的全限定名的别名
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpDeptMapper">
    <!--
       resultMap:自定义映射，处理复杂的表关系
       子标签<id>：设置主键的映射关系
            column：设置数据库字段名
            property：设置实体类属性名
       子标签<result>:设置非主键的映射关系，不是主键的都通过这个标签设置


    -->
    <resultMap id="han" type="com.atguigu.bean.Emp">
        <id column="eid" property="eid"></id>
        <result column="ename" property="ename"></result>
        <result column="age" property="age"></result>
        <result column="sex" property="sex"></result>
        <!--
        通过级联方式映射部门信息
        子标签association：用来设置多对一和一对一关系的标签
                 属性property：给哪个属性进行映射，指定属性名
                 属性javaType：这个属性对应的java类型，也就是指定property标签中的属性的类型的全类名
        mybatis会根据给的java类型创建一个对象，
           然后把id字段的值赋值给id对应的属性
           把result字段的值赋值给result对应的属性
           最后把这个对应赋值给property属性

        -->
        <association property="dept" javaType="com.atguigu.bean.Dept">
            <id column="did" property="did"></id>
            <result column="dname" property="dname"></result>
        </association>
    </resultMap>

  <select id="getAllEmp" resultMap="han">
      select emp.eid,emp.ename,emp.age,emp.sex,emp.did
          ,dep.dname from emp,dep where emp.did=dep.did
  </select>
</mapper>
~~~

#### 11.1.3 ★方式3 分步查询

~~~java
在上述案例中，一个员工对应一个部门，而一个部门有多个员工
我们可以换种思考方式，先查询出员工的信息，再对应查询出部门的信息，将部门信息设到员工信息中，这也就是分步查询
~~~

**这样可以提高sql的复用性**

~~~java
员工类
package com.atguigu.bean;

public class Emp {
    private Integer eid;
    private String ename;
    private Integer age;
    private String sex;
    private Dept dept;

    public Dept getDept() {
        return dept;
    }

    public void setDept(Dept dept) {
        this.dept = dept;
    }

    public Integer getEid() {
        return eid;
    }

    public void setEid(Integer eid) {
        this.eid = eid;
    }

    public String getEname() {
        return ename;
    }

    public void setEname(String ename) {
        this.ename = ename;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    @Override
    public String toString() {
        return "Emp{" +
                "eid=" + eid +
                ", ename='" + ename + '\'' +
                ", age=" + age +
                ", sex='" + sex + '\'' +
                ", dept=" + dept +
                '}';
    }
}

~~~

~~~xml
员工接口映射文件
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致， namespace：是mapper接口的全类名或者接口的全限定名的别名
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpDeptMapper">
    <!--
     <resultMap>标签：实现高级结果集的映射
          id属性“指定唯一的一个标识，以便被引用
          子标签：
             <id>:映射主键字段  column属性：指定数据库字段名 property属性：指定pojo类的属性名
             <result>:映射非主键字段  column属性：指定数据库字段名 property属性：指定pojo类的属性名
     -->
    <resultMap id="empMapStep" type="com.atguigu.bean.Emp">
        <id column="eid" property="eid"></id>
        <result column="ename" property="ename"></result>
        <result column="age" property="age"></result>
        <result column="sex" property="sex"></result>
        <!--
           property属性：指定属性的名字
           select属性：分步查询的sql的id，即接口的全限定名.方法名或者namespace.SQL的id，查询出来的结果为dept属性赋值，指定调用那个接口的哪个方法查询部门信息
           column：分布查询的条件，注意：此条件必须是从数据库查询过的！！！！
        -->
        <association property="dept" select="com.atguigu.mapper.DeptMapper.getDeptById" column="did"></association>
    </resultMap>
    
    
   <select id="getEmpStep" resultMap="empMapStep">
       select eid,ename,age,sex,did from emp where eid=#{eid}
   </select>
</mapper>
~~~

~~~java
部门类
package com.atguigu.bean;

public class Dept {
    private Integer did;
    private String dname;

    public String getDname() {
        return dname;
    }

    public void setDname(String dname) {
        this.dname = dname;
    }

    public Integer getDid() {
        return did;
    }

    public void setDid(Integer did) {
        this.did = did;
    }

    @Override
    public String toString() {
        return "Dept{" +
                "did=" + did +
                ", dname='" + dname + '\'' +
                '}';
    }
}

~~~

~~~java
部门类接口
package com.atguigu.mapper;

import com.atguigu.bean.Dept;

public interface DeptMapper {
    Dept getDeptById(int id);
}

~~~

~~~xml
部门类接口映射文件
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.DeptMapper">
 <select id="getDeptById" resultType="com.atguigu.bean.Dept" >
     select did,dname from dep where did =#{ll}
 </select>

</mapper>
~~~

**测试**

~~~java
package com.atguigu.test;


import com.atguigu.bean.Emp;
import com.atguigu.bean.User;
import com.atguigu.mapper.EmpDeptMapper;
import com.atguigu.mapper.UserMapper;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.Map;


public class TestMybatis {
    @Test
    public void testMybatis(){
        InputStream resourceAsStream = null;
        try {
            resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
            SqlSessionFactoryBuilder builder  = new SqlSessionFactoryBuilder();
            SqlSessionFactory build = builder.build(resourceAsStream);
            SqlSession sqlSession = build.openSession(true);// 自动开启事务
            EmpDeptMapper mapper = sqlSession.getMapper(EmpDeptMapper.class);
            Emp empStep = mapper.getEmpStep(3);
            System.out.println(empStep);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
-----------------------------------------------------------------
DEBUG 06-03 21:55:02,877 ==>  Preparing: select eid,ename,age,sex,did from emp where eid=?   (BaseJdbcLogger.java:145) 
DEBUG 06-03 21:55:02,898 ==> Parameters: 3(Integer)  (BaseJdbcLogger.java:145) 
DEBUG 06-03 21:55:02,908 ====>  Preparing: select did,dname from dep where did =?   (BaseJdbcLogger.java:145) 
DEBUG 06-03 21:55:02,909 ====> Parameters: 4(Integer)  (BaseJdbcLogger.java:145) 
DEBUG 06-03 21:55:02,910 <====      Total: 1  (BaseJdbcLogger.java:145) 
DEBUG 06-03 21:55:02,911 <==      Total: 1  (BaseJdbcLogger.java:145) 
Emp{eid=3, ename='赵六', age=15, sex='男', dept=Dept{did=4, dname='市场部'}}
~~~

**可以看到，分步以后确实是sql分步查询的！**

![](Mybatis.assets/Snipaste_2021-06-03_22-31-16-1622730707454.png)

#### 11.1.4 分布查询的延迟加载

>先查询员工表的信息，按照上面的分布查询，直到我用到了部门表的信息，再分步查询部门表
>
> 延迟加载只用于分步查询中

我们需要通过全局配置文件的setting标签来设置

~~~markdown
## 1.lazyLoadingEnabled ：是否开启懒加载，默认是false
## 2.aggressiveLazyLoading:是否查询所有的信息,3.4.1之前默认是true,现在默认是false
我们不用懒加载，则lazyLoadingEnabled属性设置为false，aggressiveLazyLoading属性设置为true
我们开启懒加载以后，则lazyLoadingEnabled属性设置为true，需要将aggressiveLazyLoading属性设置为false
~~~

~~~xml
## 全局配置文件
 <settings>
        <!-- 将下划线设置为驼峰，user_name映射为userName-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <!-- 开启延迟加载-->
        <setting name="lazyLoadingEnabled" value="true"/>
        <!-- 是否查询所有数据-->
        <setting name="aggressiveLazyLoading" value="false"/>
    </settings>
~~~

### 11.2一对多

>在上面的多对一案例中，实际上是多个员工对应一个部门，但是对于一个员工而言，他还是一个员工对应一个部门。
>
>而反过来我们从部门类考虑，则变成一个部门里面有多个员工啦，这就是一对多！

#### 11.2.1 collection

**实体类** 

~~~java
package com.atguigu.bean;

import java.util.List;

public class Dept {
    private Integer did;
    private String dname;
    private List<Emp> emps;

    @Override
    public String toString() {
        return "Dept{" +
                "did=" + did +
                ", dname='" + dname + '\'' +
                ", emps=" + emps +
                '}';
    }

    public List<Emp> getEmps() {
        return emps;
    }

    public void setEmps(List<Emp> emps) {
        this.emps = emps;
    }

    public String getDname() {
        return dname;
    }

    public void setDname(String dname) {
        this.dname = dname;
    }

    public Integer getDid() {
        return did;
    }

    public void setDid(Integer did) {
        this.did = did;
    }


}

~~~

**接口**

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.Dept;

public interface EmpDeptMapper {
   Dept getDeptEmpsByDid(int did);
}

~~~

**接口映射文件**

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致， namespace：是mapper接口的全类名或者接口的全限定名的别名
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpDeptMapper">
    <!--
       resultMap:用来处理复杂映射关系的 id属性：唯一标识  type属性：映射的类型的全类名
           子标签id：映射主键字段 column对应数据库字段  property对应pojo属性名
           子标签result：用来映射非主键字段  column对应数据库字段  property对应pojo属性名
           子标签collection：用来处理一对多的映射的关系
    -->
    <resultMap id="lala" type="com.atguigu.bean.Dept">
        <id column="did" property="did"></id>
        <result column="dname" property="dname"></result>
        <!--
           <collection>处理一对多和多对多关系的，它不需要指定javaType
              property属性：要映射的属性字段名
              ofType属性:这个属性指的是emps中的类型，也就是集合中的类型，
        -->
        <collection property="emps" ofType="com.atguigu.bean.Emp">
            <id property="eid" column="eid"></id>
            <result column="ename" property="ename"></result>
            <result column="age" property="age"></result>
            <result column="sex" property="sex"></result>
        </collection>
    </resultMap>

    <select id="getDeptEmpsByDid" resultMap="lala">
        SELECT dep.did,dep.dname,emp.eid,emp.ename,emp.age,emp.sex FROM dep LEFT JOIN emp  on dep.did =emp.did  where dep.did=#{id}
    </select>

</mapper>
~~~

**测试**

~~~java
package com.atguigu.test;


import com.atguigu.bean.Dept;
import com.atguigu.bean.Emp;
import com.atguigu.bean.User;
import com.atguigu.mapper.EmpDeptMapper;
import com.atguigu.mapper.UserMapper;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.Map;


public class TestMybatis {
    @Test
    public void testMybatis(){
        InputStream resourceAsStream = null;
        try {
            resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
            SqlSessionFactoryBuilder builder  = new SqlSessionFactoryBuilder();
            SqlSessionFactory build = builder.build(resourceAsStream);
            SqlSession sqlSession = build.openSession(true);// 自动开启事务
            EmpDeptMapper mapper = sqlSession.getMapper(EmpDeptMapper.class);
            Dept deptEmpsByDid = mapper.getDeptEmpsByDid(1);
            System.out.println(deptEmpsByDid);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}
-----------------------------------------------------
Dept{did=1, dname='人事部门', emps=[Emp{eid=0, ename='张三', age=13, sex='男', dept=null}, Emp{eid=1, ename='李四', age=16, sex='男', dept=null}]}
~~~

#### 11.2.2 分步查询

>先查询部门信息，在根据部门信息查询出员工信息

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.Dept;
import com.atguigu.bean.Emp;

import java.util.List;

public interface EmpDeptMapper {
  Dept getOnlyDeptByDid(int did);
  List<Emp> getEmpListByDid(int did);
}

~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致， namespace：是mapper接口的全类名或者接口的全限定名的别名
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpDeptMapper">
    <resultMap id="hehe" type="com.atguigu.bean.Dept">
        <id column="did" property="dname"></id>
        <result column="dname" property="dname"></result>
        <collection property="emps" select="com.atguigu.mapper.EmpDeptMapper.getEmpListByDid" column="did"></collection>
    </resultMap>

   <select id="getOnlyDeptByDid" resultMap="hehe">
       select did,dname from dep where did=#{did}
   </select>

    <select id="getEmpListByDid" resultType="com.atguigu.bean.Emp">
        select eid,ename,sex,age from emp where emp.did=#{did}
    </select>
</mapper>
~~~

**测试**

~~~java
package com.atguigu.test;


import com.atguigu.bean.Dept;
import com.atguigu.bean.Emp;
import com.atguigu.bean.User;
import com.atguigu.mapper.EmpDeptMapper;
import com.atguigu.mapper.UserMapper;
import com.sun.scenario.effect.impl.sw.sse.SSEBlend_SRC_OUTPeer;
import org.apache.ibatis.annotations.Param;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.Map;


public class TestMybatis {
    @Test
    public void testMybatis(){
        InputStream resourceAsStream = null;
        try {
            resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
            SqlSessionFactoryBuilder builder  = new SqlSessionFactoryBuilder();
            SqlSessionFactory build = builder.build(resourceAsStream);
            SqlSession sqlSession = build.openSession(true);// 自动开启事务
            EmpDeptMapper mapper = sqlSession.getMapper(EmpDeptMapper.class);
            Dept deptEmpsByDid = mapper.getOnlyDeptByDid(1);
            System.out.println(deptEmpsByDid);

        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}

~~~

### 11.3 细节

#### 11.3.1 分布查询传递多个值

如果分步查询需要传递多个值，此时我们考虑用map集合来传递参数，格式是

~~~markdown
## 1.column="{key1=value1,key2=value2,...}"
~~~

~~~java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致， namespace：是mapper接口的全类名或者接口的全限定名的别名
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpDeptMapper">
    <resultMap id="hehe" type="com.atguigu.bean.Dept">
        <id column="did" property="dname"></id>
        <result column="dname" property="dname"></result>
        <collection property="emps" select="com.atguigu.mapper.EmpDeptMapper.getEmpListByDid" column="{key1=did}"></collection>
    </resultMap>

   <select id="getOnlyDeptByDid" resultMap="hehe">
       select did,dname from dep where did=#{did}
   </select>

    <select id="getEmpListByDid" resultType="com.atguigu.bean.Emp">
        select eid,ename,sex,age from emp where emp.did=#{key1}
    </select>
</mapper>
~~~

![](Mybatis.assets/Snipaste_2021-06-05_11-32-47-1622863985788.png)

#### 11.3.2 设置某个sql的延迟加载fetchType

全局配置文件中配置的延迟加载属性是针对整个配置文件的，我们还可以考虑只给某个sql配置延迟加载

>在association或者collection标签中有属性 fetchType
>
>它有两种取值：
>
>eager：不开启延迟加载
>
>lazy：开启延迟加载
>
>```
><collection property="emps" select="com.atguigu.mapper.EmpDeptMapper.getEmpListByDid" column="{key1=did}" fetchType="eager"></collection>
> <collection property="emps" select="com.atguigu.mapper.EmpDeptMapper.getEmpListByDid" column="{key1=did}" fetchType="lazy"></collection>
>```

## 12 ★动态SQL

>动态 SQL是MyBatis强大特性之一。极大的简化我们拼装SQL的操作

### 12.1 if标签用于多条件查询

>```xml
><if test=""></if>:通过test表达式来拼接Sql
>```

**实体类**

~~~java
package com.atguigu.bean;

public class Emp {
    private Integer eid;
    private String ename;
    private Integer age;
    private String sex;
    private Dept dept;

    public Dept getDept() {
        return dept;
    }

    public void setDept(Dept dept) {
        this.dept = dept;
    }

    public Integer getEid() {
        return eid;
    }

    public void setEid(Integer eid) {
        this.eid = eid;
    }

    public String getEname() {
        return ename;
    }

    public void setEname(String ename) {
        this.ename = ename;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    @Override
    public String toString() {
        return "Emp{" +
                "eid=" + eid +
                ", ename='" + ename + '\'' +
                ", age=" + age +
                ", sex='" + sex + '\'' +
                ", dept=" + dept +
                '}';
    }
}

~~~

**接口**

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.Emp;

import java.util.List;

public interface EmpMapper {
   // 根据eid，ename,sex,age多条件查询员工信息
    List<Emp> getEmpListByMoreTJ(Emp emp);
}

~~~

**映射文件**

~~~java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
    <!--
         List<Emp> getEmpListByMoreTJ();
     -->
    <select id="getEmpListByMoreTJ" resultType="com.atguigu.bean.Emp" >
select eid,ename,age,sex,did from emp
where
    eid=#{eid}
    and ename =#{ename}
    and age = #{age}
    and sex= #{sex}
    </select>
</mapper>
~~~

**测试**

~~~java
package com.atguigu.test;



import com.atguigu.bean.Emp;
import com.atguigu.mapper.EmpMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis() throws Exception{
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory build = builder.build(resourceAsStream);
        SqlSession sqlSession = build.openSession(true);
        EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
        Emp emp = new Emp();
        emp.setEid(0);
        emp.setEname("张三");
        emp.setAge(13);
        emp.setSex("男");
        List<Emp> empListByMoreTJ = mapper.getEmpListByMoreTJ(emp);
        System.out.println(empListByMoreTJ);


    }
}

~~~

当我们给查询条件的某一个值设置为空的时候，此时多条件查询会出现某个字段等于null的情况，于是会查询不出想要的结果。

**我们可以用if标签进行过滤**

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
    <!--
       多条件查询：若页面中没有设置此条件，SQL语句中一定不能有该条件
     -->
    <select id="getEmpListByMoreTJ" resultType="com.atguigu.bean.Emp">
        select eid,ename,age,sex,did from emp
        where
        <if test="eid != null">
            eid=#{eid}
        </if>
        <if test="ename != null and ename != ''">
            and ename =#{ename}
        </if>
        <if test="age != null ">
            and age = #{age}
        </if>
            and sex= #{sex}
    </select>
</mapper>
~~~

>最终的过滤效果是实现啦，可是还是有一个问题，当我们的 eid条件为空时，
>
> 此时 sql: Preparing: select eid,ename,age,sex,did from emp where and age = ? and sex= ? 
>
>为此，我们可以使用在where后面拼接  1=1
>
><mapper namespace="com.atguigu.mapper.EmpMapper">
>    <!--
>       多条件查询：若页面中没有设置此条件，SQL语句中一定不能有该条件
>     -->
>
>    <select id="getEmpListByMoreTJ" resultType="com.atguigu.bean.Emp">
>        select eid,ename,age,sex,did from emp
>        where 1=1   
>        <if test="eid != null">
>            eid=#{eid}
>        </if>
>        <if test="ename != null and ename != ''">
>            and ename =#{ename}
>        </if>
>        <if test="age != null ">
>            and age = #{age}
>        </if>
>            and sex= #{sex}
>    </select>
></mapper>

### 12.2 where 标签

上面出现的多余的and的情况我们是通过在sql语句where后面拼接sql where 1-1来实现的解决多余and的情况，我们还可以通过<where>标签来解决这个问题！

>```xml
><where>:为我们添加关键字where并且去掉多余的and，
>where 元素只会在至少有一个子元素的条件返回 SQL 子句的情况下才去插入“WHERE”子句。而且，若语句的开头为“AND”或“OR”，where 元素也会将它们去除
>```

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
    <!--
       多条件查询：若页面中没有设置此条件，SQL语句中一定不能有该条件
     -->
    <select id="getEmpListByMoreTJ" resultType="com.atguigu.bean.Emp">
        <!--
           <if test=""></if>:通过test表达式来拼接Sql
           <where>:为我们添加关键字where并且去掉多余的and
        -->
        select eid,ename,age,sex,did from emp
        <where>
            <if test="eid != null">
                eid=#{eid}
            </if>
            <if test="ename != null and ename != ''">
                and ename =#{ename}
            </if>
            <if test="age != null ">
                and age = #{age}
            </if>
                and sex= #{sex}
        </where>


    </select>
</mapper>
~~~

### 12.3 trim标签

上面的where虽然可以去掉多余的and,但是特殊情况下它无法去掉。也就是where标签只能去掉前面的and或者or

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
    <!--
       多条件查询：若页面中没有设置此条件，SQL语句中一定不能有该条件
     -->
    <select id="getEmpListByMoreTJ" resultType="com.atguigu.bean.Emp">
        <!--
           <if test=""></if>:通过test表达式来拼接Sql
           <where>:为我们添加关键字where并且去掉多余的and
        -->
        select eid,ename,age,sex,did from emp
        <where>
            <if test="eid != null">
                eid=#{eid} and
            </if>
            <if test="ename != null and ename != ''">
                ename =#{ename} and
            </if>
            <if test="age != null ">
                age = #{age}  and
            </if>
            <if test="sex != null">
                sex= #{sex}
            </if>
        </where>


    </select>
</mapper>
~~~

此时我们发现如果sex为null，则sql语句会多出一个and

![](Mybatis.assets/Snipaste_2021-06-05_17-59-23-1622887185181.png)

>```
><trim prefix="" suffix="" prefixOverrides="" suffixOverrides=""></trim>:截取并拼接
>     属性prefix：在操作的语句前加入某些内容
>     属性suffix：在操作的语句后加入某些内容
>     属性prefixOverrides：把操作SQL语句前的某些内容去掉
>     属性suffixOverrides：把操作SQL语句后的某些内容去掉
>```

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
    <!--
       多条件查询：若页面中没有设置此条件，SQL语句中一定不能有该条件
     -->
    <select id="getEmpListByMoreTJ" resultType="com.atguigu.bean.Emp">
        <!--
           <if test=""></if>:通过test表达式来拼接Sql
           <where>:为我们添加关键字where并且去掉多余的and
           <trim prefix="" suffix="" prefixOverrides="" suffixOverrides=""></trim>:截取并拼接
                属性prefix：在操作的语句前加入某些内容
                属性suffix：在操作的语句后加入某些内容
                属性prefixOverrides：把操作SQL语句前的某些内容去掉
                属性suffixOverrides：把操作SQL语句后的某些内容去掉
        -->
        select eid,ename,age,sex,did from emp
        <trim prefix="" suffix="" prefixOverrides="" suffixOverrides="and|or"></trim>
        <where>
            <if test="eid != null">
                eid=#{eid} and
            </if>
            <if test="ename != null and ename != ''">
                ename =#{ename} and
            </if>
            <if test="age != null ">
                age = #{age}  and
            </if>
            <if test="sex != null">
                sex= #{sex}
            </if>
        </where>


    </select>
</mapper>
~~~

### 12.4 set标签

>set 主要是用于解决修改操作中SQL语句中可能多出逗号的问题。
>
>会帮我们创建set关键字并且智能的去掉多余的逗号

~~~xml
<update id="updateEmpByConditionSet">
		update  tbl_employee  
		<set>
			<if test="lastName!=null &amp;&amp; lastName!=&quot;&quot;">
				 last_name = #{lastName},
			</if>
			<if test="email!=null and email.trim()!=''">
				 email = #{email} ,
			</if>
			<if test="&quot;m&quot;.equals(gender) or &quot;f&quot;.equals(gender)">
				gender = #{gender} 
			</if>
		</set>
		 where id =#{id}
	</update>
~~~

### 12.5 choose(when,otherwise)

>choose 主要是用于分支判断，类似于java中的switch case,只会满足所有分支中的一个

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.Emp;

import java.util.List;

public interface EmpMapper {
   // 根据eid，ename,age,sex中的其中一个查询一个员工信息
    List<Emp> getEmpListByChoose(Emp emp);
}
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
    <!--
       多条件查询：若页面中没有设置此条件，SQL语句中一定不能有该条件
     -->
    <!--
        List<Emp> getEmpListByChoose(Emp emp);
    -->
    <select id="getEmpListByChoose" resultType="com.atguigu.bean.Emp">
        select eid,ename,age,sex from emp
        where
        <!--
            <choose>标签：选择某一个when或者otherwise执行，其中<otherwise>标签不是必须的！！！
                <when test=""></when>：通过test表达式判断并且拼接SQL
                。
                、
                、
                <otherwise></otherwise>：当when都不符合条件，就会选择otherwise拼接SQL
        -->
        <choose>
            <when test="eid != null">
                eid =#{eid}
            </when>
            <when test="ename != null and ename != ''">
                ename =#{ename}
            </when>
            <when test="age != null">
                age =#{age}
            </when>
            <otherwise>
                sex=#{sex}
            </otherwise>
        </choose>
    </select>
</mapper>
~~~

案例2：

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.Emp;

import java.util.List;

public interface EmpMapper {
    // 添加员工信息  0|1 --》 女|男
    void insertEmp(Emp emp);

}

~~~

~~~java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
  <!--
     void insertEmp(Emp emp)
  -->
    <insert id="insertEmp" >
        insert into emp (eid,ename,age,sex,did)values(
           #{eid},
           #{ename},
           #{age},
           <choose>
               <when test="sex==0">'女'</when>
               <when test="sex==1">'男'</when>
               <otherwise>'不详'</otherwise>
           </choose>
           ,1
        )
    </insert>
</mapper>
~~~

~~~java
package com.atguigu.test;



import com.atguigu.bean.Emp;
import com.atguigu.mapper.EmpMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;


import java.io.InputStream;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis() throws Exception{
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory build = builder.build(resourceAsStream);
        SqlSession sqlSession = build.openSession(true);
        EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
        Emp emp = new Emp();
        emp.setEid(4);
        emp.setEname("张三三");
        emp.setAge(13);
        emp.setSex("0");
        mapper.insertEmp(emp);
    }
}
--------------------------------------
DEBUG 06-06 11:49:06,781 ==>  Preparing: insert into emp (eid,ename,age,sex,did)values( ?, ?, ?, '女' ,1 )   (BaseJdbcLogger.java:145) 
~~~

### 12.6 foreach批量操作

~~~markdown
##  foreach 主要用于循环迭代,用于批量操作
      collection: 要迭代的集合
      item: 当前从集合中迭代出的元素
      open: 开始字符
      close:结束字符
      separator: 元素与元素之间的分隔符
      index:
	       迭代的是List集合: index表示的当前元素的下标
		   迭代的Map集合:  index表示的当前元素的key
~~~

**案例：批量删除**

>实现的方式1：
>
>delete from emp where eid in();

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
  <!--
  void deleteMoreByList(List<Integer> eids);
  -->
    <delete id="deleteMoreByList">
        delete from emp where eid in
        <!--
           <foreach>:对一个数组或者集合进行遍历
           属性：
              1.collection：指定遍历的集合或者数组
              传入的参数是集合或者数组，会被MyBatis封装成一个map传入, Collection对应的key是collection,Array对应的key是array.
              如果确定是List集合，key还可以是list
              2.item：设置别名
              3.close：设置循环体的结束内容 如in 后面的()的(
              4.open：设置循环体的开始内容  如in 后面的()的)
              5.separator：设置每一次循环之间的分隔符
              6.index：
        -->

        <foreach collection="list" item="eid"  separator="," open="(" close=")">
            #{eid}
        </foreach>

    </delete>
</mapper>
~~~

>实现的方式2：
>
>delete from emp where eid  =  or eid = or eid = 

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
  <!--
  void deleteMoreByList(List<Integer> eids);
  -->
    <delete id="deleteMoreByList">
        delete from emp where 
        <!--
           <foreach>:对一个数组或者集合进行遍历
           属性：
              1.collection：指定遍历的集合或者数组
              传入的参数是集合或者数组，会被MyBatis封装成一个map传入, Collection对应的key是collection,Array对应的key是array.
              如果确定是List集合，key还可以是list
              2.item：设置别名
              3.close：设置循环体的结束内容 如in 后面的()的(
              4.open：设置循环体的开始内容  如in 后面的()的)
              5.separator：设置每一次循环之间的分隔符
              6.index：
        -->

        <foreach collection="list" item="eid"  separator="or" open="" close="">
           eid= #{eid} 
        </foreach>

    </delete>
</mapper>
~~~

~~~markdown
## 1.delete 
    delete from emp where eid in ();
    delete from emp where eid =1 or eid =2 or eid =3
## 2.select 
    select * from emp where eid in ();
    select * from emp where eid =1 or eid =2 or eid =3
## 3.update
    把每条数据都修改为相同的内容
    update emp set ... where eid in ();
    update emp set ... where eid =1 or eid =2 or eid =3
## 4.insert
    insert into emp values(),(),()
~~~

#### 12.6.1 批量添加

~~~
package com.atguigu.mapper;

import com.atguigu.bean.Emp;
import org.apache.ibatis.annotations.Param;


public interface EmpMapper {
    void insertMoreByArray(@Param("emps") Emp[] emp);

}
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
    <!--
      void insertMoreByArray(Emp[] emp);
     -->
    <insert id="insertMoreByArray" >
        insert into emp values 
        <foreach collection="emps" item="emp" separator=",">
            (#{emp.eid},#{emp.ename},#{emp.age},#{emp.sex},2)
        </foreach>
    </insert>
</mapper>
~~~

~~~java
package com.atguigu.test;



import com.atguigu.bean.Emp;
import com.atguigu.mapper.EmpMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;


import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis() throws Exception{
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory build = builder.build(resourceAsStream);
        SqlSession sqlSession = build.openSession(true);
        EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
        //  Emp[] emp = new Emp[3];
        //  Emp[] emp = new Emp[]{};
        Emp emp1 = new Emp(5,"a",23,"男",2);
        Emp emp2 = new Emp(6,"a",234,"女",2);
        Emp emp3 = new Emp(7,"bb",99,"女",1);
        Emp[] emp = {emp1,emp2,emp3};
        mapper.insertMoreByArray(emp);

    }
}

~~~

#### 12.6.2 批量更新

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.Emp;
import org.apache.ibatis.annotations.Param;


public interface EmpMapper {
    void updateMoreById(@Param("emps")Emp[] array);

}

~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
    <!--
       void updateMoreById(@Param("emps")Emp[] array);
     -->
    <update id="updateMoreById">
        <foreach collection="emps" item="emp" >
            update emp set ename =#{emp.ename},sex=#{emp.sex},age=#{emp.age},did=#{emp.did} where eid=#{emp.eid};
        </foreach>
    </update>
</mapper>
~~~

~~~java
package com.atguigu.test;



import com.atguigu.bean.Emp;
import com.atguigu.mapper.EmpMapper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;


import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis() throws Exception{
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory build = builder.build(resourceAsStream);
        SqlSession sqlSession = build.openSession(true);
        EmpMapper mapper = sqlSession.getMapper(EmpMapper.class);
        //  Emp[] emp = new Emp[3];
        //  Emp[] emp = new Emp[]{};
        Emp emp1 = new Emp(5,"a1",233,"男",1);
        Emp emp2 = new Emp(6,"a1",244,"女",2);
        Emp emp3 = new Emp(7,"bb1",255,"女",3);
        Emp[] emp = {emp1,emp2,emp3};
        mapper.updateMoreById(emp);

    }
}

~~~

**注意：我们在批量更新的时候是预编译了多条sql，而默认不支持多条sql预编译执行，故会出现下面的异常**

~~~java
DEBUG 06-06 16:58:35,235 ==>  Preparing: update emp set ename =?,sex=?,age=?,did=? where eid=?; update emp set ename =?,sex=?,age=?,did=? where eid=?; update emp set ename =?,sex=?,age=?,did=? where eid=?;   (BaseJdbcLogger.java:145) 
DEBUG 06-06 16:58:35,256 ==> Parameters: a1(String), 男(String), 233(Integer), 1(Integer), 5(Integer), a1(String), 女(String), 244(Integer), 2(Integer), 6(Integer), bb1(String), 女(String), 255(Integer), 3(Integer), 7(Integer)  (BaseJdbcLogger.java:145) 

org.apache.ibatis.exceptions.PersistenceException: 
### Error updating database.  Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'update emp set ename ='a1',sex='女',age=244,did=2 where eid=6;
          
     ' at line 3
### The error may involve defaultParameterMap
### The error occurred while setting parameters
### SQL: update emp set ename =?,sex=?,age=?,did=? where eid=?;                        update emp set ename =?,sex=?,age=?,did=? where eid=?;                        update emp set ename =?,sex=?,age=?,did=? where eid=?;
### Cause: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'update emp set ename ='a1',sex='女',age=244,did=2 where eid=6;
          
     ' at line 3

	at org.apache.ibatis.exceptions.ExceptionFactory.wrapException(ExceptionFactory.java:30)
	at org.apache.ibatis.session.defaults.DefaultSqlSession.update(DefaultSqlSession.java:200)
	at org.apache.ibatis.binding.MapperMethod.execute(MapperMethod.java:62)
	at org.apache.ibatis.binding.MapperProxy.invoke(MapperProxy.java:53)
	at com.sun.proxy.$Proxy4.updateMoreById(Unknown Source)
	at com.atguigu.test.TestMybatis.testMybatis(TestMybatis.java:33)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.junit.runners.model.FrameworkMethod$1.runReflectiveCall(FrameworkMethod.java:59)
	at org.junit.internal.runners.model.ReflectiveCallable.run(ReflectiveCallable.java:12)
	at org.junit.runners.model.FrameworkMethod.invokeExplosively(FrameworkMethod.java:56)
	at org.junit.internal.runners.statements.InvokeMethod.evaluate(InvokeMethod.java:17)
	at org.junit.runners.ParentRunner$3.evaluate(ParentRunner.java:306)
	at org.junit.runners.BlockJUnit4ClassRunner$1.evaluate(BlockJUnit4ClassRunner.java:100)
	at org.junit.runners.ParentRunner.runLeaf(ParentRunner.java:366)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:103)
	at org.junit.runners.BlockJUnit4ClassRunner.runChild(BlockJUnit4ClassRunner.java:63)
	at org.junit.runners.ParentRunner$4.run(ParentRunner.java:331)
	at org.junit.runners.ParentRunner$1.schedule(ParentRunner.java:79)
	at org.junit.runners.ParentRunner.runChildren(ParentRunner.java:329)
	at org.junit.runners.ParentRunner.access$100(ParentRunner.java:66)
	at org.junit.runners.ParentRunner$2.evaluate(ParentRunner.java:293)
	at org.junit.runners.ParentRunner$3.evaluate(ParentRunner.java:306)
	at org.junit.runners.ParentRunner.run(ParentRunner.java:413)
	at org.junit.runner.JUnitCore.run(JUnitCore.java:137)
	at com.intellij.junit4.JUnit4IdeaTestRunner.startRunnerWithArgs(JUnit4IdeaTestRunner.java:68)
	at com.intellij.rt.junit.IdeaTestRunner$Repeater.startRunnerWithArgs(IdeaTestRunner.java:33)
	at com.intellij.rt.junit.JUnitStarter.prepareStreamsAndStart(JUnitStarter.java:230)
	at com.intellij.rt.junit.JUnitStarter.main(JUnitStarter.java:58)
Caused by: com.mysql.jdbc.exceptions.jdbc4.MySQLSyntaxErrorException: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near 'update emp set ename ='a1',sex='女',age=244,did=2 where eid=6;
          
     ' at line 3
	at sun.reflect.NativeConstructorAccessorImpl.newInstance0(Native Method)
	at sun.reflect.NativeConstructorAccessorImpl.newInstance(NativeConstructorAccessorImpl.java:62)
	at sun.reflect.DelegatingConstructorAccessorImpl.newInstance(DelegatingConstructorAccessorImpl.java:45)
	at java.lang.reflect.Constructor.newInstance(Constructor.java:423)
	at com.mysql.jdbc.Util.handleNewInstance(Util.java:404)
	at com.mysql.jdbc.Util.getInstance(Util.java:387)
	at com.mysql.jdbc.SQLError.createSQLException(SQLError.java:941)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3870)
	at com.mysql.jdbc.MysqlIO.checkErrorPacket(MysqlIO.java:3806)
	at com.mysql.jdbc.MysqlIO.sendCommand(MysqlIO.java:2470)
	at com.mysql.jdbc.MysqlIO.sqlQueryDirect(MysqlIO.java:2617)
	at com.mysql.jdbc.ConnectionImpl.execSQL(ConnectionImpl.java:2550)
	at com.mysql.jdbc.PreparedStatement.executeInternal(PreparedStatement.java:1861)
	at com.mysql.jdbc.PreparedStatement.execute(PreparedStatement.java:1192)
	at sun.reflect.NativeMethodAccessorImpl.invoke0(Native Method)
	at sun.reflect.NativeMethodAccessorImpl.invoke(NativeMethodAccessorImpl.java:62)
	at sun.reflect.DelegatingMethodAccessorImpl.invoke(DelegatingMethodAccessorImpl.java:43)
	at java.lang.reflect.Method.invoke(Method.java:498)
	at org.apache.ibatis.logging.jdbc.PreparedStatementLogger.invoke(PreparedStatementLogger.java:59)
	at com.sun.proxy.$Proxy7.execute(Unknown Source)
	at org.apache.ibatis.executor.statement.PreparedStatementHandler.update(PreparedStatementHandler.java:46)
	at org.apache.ibatis.executor.statement.RoutingStatementHandler.update(RoutingStatementHandler.java:74)
	at org.apache.ibatis.executor.SimpleExecutor.doUpdate(SimpleExecutor.java:50)
	at org.apache.ibatis.executor.BaseExecutor.update(BaseExecutor.java:117)
	at org.apache.ibatis.executor.CachingExecutor.update(CachingExecutor.java:76)
	at org.apache.ibatis.session.defaults.DefaultSqlSession.update(DefaultSqlSession.java:198)
	... 29 more


Process finished with exit code -1

~~~

**解决办法：修改配置文件allowMultiQueries=true**

```java
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql://localhost:3306/mybatis?allowMultiQueries=true
username=root
password=123456
```

### 12.7 sql标签与include标签

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.Emp;
import org.apache.ibatis.annotations.Param;


public interface EmpMapper {
   Emp getEmpByEid(Integer eid);

}

~~~

~~~java
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.EmpMapper">
    <!--
        <sql id=""></sql>：设置一段sql片段，即公共sql,可以被当前当前映射文件中的所有sql语句所访问 id标记唯一的sql片段
        <include refid=""></include>:用来引用sql片段 refid与对应 <sql>中的id标签
    -->
    <sql id="empColumn">
        select eid,ename,age,sex,did from emp
    </sql>
    <select id="getEmpByEid" resultType="com.atguigu.bean.Emp">
        <include refid="empColumn"></include>
        where eid = #{eid};
    </select>

</mapper>
~~~

## 13.Mybatis的缓存

缓存一般将数据存储在内存中。可以更快的读取！

### 13.1 一级缓存

一级缓存是SqlSession级别的，针对同一个sqlSession

>mybatis一级缓存默认开启，它是SqlSession级别的！
>
>即同一个SqlSession对于一个sql语句，执行之后就会存储在缓存中，下次执行相同的sql，直接从缓存中取结果

**注意：一级缓存只针对同一个SqlSession**

~~~markdown
## 一级缓存失效的几种情况
    不同的SqlSession对应不同的一级缓存
    同一个SqlSession但是查询条件不同
    同一个SqlSession两次查询期间执行了任何一次增删改操作
    同一个SqlSession两次查询期间手动清空了缓存 -- sqlSession.clearCache(); //清空缓存的方法
~~~

### 13.2 二级缓存

**二级缓存是映射文件级别的**,**注意：**二级缓存在SqlSession 关闭或提交之后才会生效！！！！

~~~shell
# 1.二级缓存(second level cache)，全局作用域缓存
# 2.二级缓存默认不开启，需要手动配置
# 3.MyBatis提供二级缓存的接口以及实现，缓存实现要求POJO实现Serializable接口
# 4.二级缓存在SqlSession 关闭或提交之后才会生效
# 5.二级缓存使用的步骤:
  ①对于全局配置文件：全局配置文件中开启二级缓存<setting name="cacheEnabled" value="true"/>
  ②对于接口映射文件：需要使用二级缓存的映射文件处使用cache配置缓存<cache />
  ③注意：POJO需要实现Serializable接口
# 6.二级缓存<cache>标签相关的属性
  ①eviction=“FIFO”：缓存回收策略：
    LRU – 最近最少使用的：移除最长时间不被使用的对象。
    FIFO – 先进先出：按对象进入缓存的顺序来移除它们。
    SOFT – 软引用：移除基于垃圾回收器状态和软引用规则的对象。内存足够不移除，内存不够移除
    WEAK – 弱引用：更积极地移除基于垃圾收集器状态和弱引用规则的对象。垃圾回收器一运行，就移除
    默认的是 LRU。
  ②flushInterval：刷新间隔，单位毫秒
    默认情况是不设置，也就是没有刷新间隔，缓存仅仅调用语句时刷新
  ③size：引用数目，正整数
    代表缓存最多可以存储多少个对象，太大容易导致内存溢出
  ④readOnly：只读，true/false
    true：只读缓存；会给所有调用者返回缓存对象的相同实例。因此这些对象不能被修改。这提供了很重要的性能优势。
    false：读写缓存；会返回缓存对象的拷贝（通过序列化）。这会慢一些，但是安全，因此默认是 false。
~~~

**开启步骤1：**

![](Mybatis.assets/Snipaste_2021-06-07_21-20-58-1623072078378.png)

**开启步骤2**

#### 13.2.1 二级缓存相关属性

~~~markdown
## 缓存的相关属性设置
  1)全局setting的cacheEnable：
    配置二级缓存的开关，一级缓存一直是打开的。
  2)select标签的useCache属性：是否使用二级缓存
    配置这个select是否使用二级缓存。一级缓存一直是使用的 设置为false则二级缓存无法使用
  3)sql标签的flushCache属性：一级缓存和二级缓存的开关
        对应查询来说默认是false,如果设置为true,则一级缓存和二级缓存都不可用
        对应增删改来说默认是true.
    增删改默认flushCache=true。sql执行以后，会同时清空一级和二级缓存。
    查询默认 flushCache=false。不要轻易改！
  4)sqlSession.clearCache()：只是用来清除一级缓存。
~~~

### 13.3 第三方缓存

为了提高扩展性。MyBatis定义了缓存接口Cache。我们可以通过实现Cache接口来自定义二级缓存。

EhCache 是一个纯Java的进程内缓存框架，具有快速、精干等特点，是Hibernate中默认的CacheProvider

**整合EhCache缓存的步骤:**

~~~markdown
## 1.导入ehcache包，以及整合包，日志包
     ehcache-core-2.6.8.jar、mybatis-ehcache-1.0.3.jar
     slf4j-api-1.6.1.jar、slf4j-log4j12-1.6.2.jar
## 2.编写ehcache.xml配置文件
<?xml version="1.0" encoding="UTF-8"?>
<ehcache xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
 xsi:noNamespaceSchemaLocation="../config/ehcache.xsd">
 <!-- 磁盘保存路径 -->
 <diskStore path="D:\atguigu\ehcache" />
 
 <defaultCache 
   maxElementsInMemory="1000"    内存中缓存的最大数
   maxElementsOnDisk="10000000"  硬盘中缓存的最大数
   eternal="false" 
   overflowToDisk="true" 
   timeToIdleSeconds="120"
   timeToLiveSeconds="120" 
   diskExpiryThreadIntervalSeconds="120"
   memoryStoreEvictionPolicy="LRU">
 </defaultCache>
</ehcache>
 
<!-- 
属性说明：
l diskStore：指定数据在磁盘中的存储位置。
l defaultCache：当借助CacheManager.add("demoCache")创建Cache时，EhCache便会采用<defalutCache/>指定的的管理策略
 
以下属性是必须的：
l maxElementsInMemory - 在内存中缓存的element的最大数目 
l maxElementsOnDisk - 在磁盘上缓存的element的最大数目，若是0表示无穷大
l eternal - 设定缓存的elements是否永远不过期。如果为true，则缓存的数据始终有效，如果为false那么还要根据timeToIdleSeconds，timeToLiveSeconds判断
l overflowToDisk - 设定当内存缓存溢出的时候是否将过期的element缓存到磁盘上
 
以下属性是可选的：
l timeToIdleSeconds - 当缓存在EhCache中的数据前后两次访问的时间超过timeToIdleSeconds的属性取值时，这些数据便会删除，默认值是0,也就是可闲置时间无穷大
l timeToLiveSeconds - 缓存element的有效生命期，默认是0.,也就是element存活时间无穷大
 diskSpoolBufferSizeMB 这个参数设置DiskStore(磁盘缓存)的缓存区大小.默认是30MB.每个Cache都应该有自己的一个缓冲区.
l diskPersistent - 在VM重启的时候是否启用磁盘保存EhCache中的数据，默认是false。
l diskExpiryThreadIntervalSeconds - 磁盘缓存的清理线程运行间隔，默认是120秒。每个120s，相应的线程会进行一次EhCache中数据的清理工作
l memoryStoreEvictionPolicy - 当内存缓存达到最大，有新的element加入的时候， 移除缓存中element的策略。默认是LRU（最近最少使用），可选的有LFU（最不常使用）和FIFO（先进先出）
 -->
 ## 3.配置cache标签
<cache type="org.mybatis.caches.ehcache.EhcacheCache"></cache>
type的值就是实现MyBatis缓存接口Cache的实现类
~~~

##   14 Mybatis逆向工程

~~~shell
MyBatis Generator:  
    简称MBG，是一个专门为MyBatis框架使用者定制的代码生成器，可以快速的根据表生成对应的映射文件，接口，以及bean类。支持基本的增删改查，以及QBC风格的条件查询。但是表连接、存储过程等这些复杂sql的定义需要我们手工编写,就是代码生成器!
~~~

官方文档地址

http://www.mybatis.org/generator/

官方工程地址

https://github.com/mybatis/generator/releases

逆向工程配置

**1.导入逆向工程jar包**

mybatis-generator-core-1.3.2.jar

**编写MBG的配置文件**

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE generatorConfiguration
        PUBLIC "-//mybatis.org//DTD MyBatis Generator Configuration 1.0//EN"
        "http://mybatis.org/dtd/mybatis-generator-config_1_0.dtd">

<generatorConfiguration>
    <!--
            targetRuntime: 执行生成的逆向工程的版本
                    MyBatis3Simple: 生成基本的CRUD,最终生成的接口中只包含最基本的CRUD
                    MyBatis3: 生成带条件的CRUD，最终生成的接口中除了包含最基本的CRUD，还包含带条件的查询
     -->
    <context id="DB2Tables" targetRuntime="MyBatis3">
        <!--设置连接数据库的相关信息 -->
        <jdbcConnection driverClass="com.mysql.jdbc.Driver"
                        connectionURL="jdbc:mysql://localhost:3306/mybatis"
                        userId="root"
                        password="123456">
        </jdbcConnection>
        <!--
           javaBean的生成策略
                   属性：targetPackage：指定将生成的javaBean放在哪个包下
                   属性：targetProject：指定工程的路径
        -->
        <javaModelGenerator targetPackage="com.atguigu.mbg.entities" targetProject="src">
            <property name="enableSubPackages" value="true" />
            <property name="trimStrings" value="true" />
        </javaModelGenerator>

        <!-- SQL映射文件的生成策略 -->
        <sqlMapGenerator targetPackage="com.atguigu.mbg.mapper"  targetProject="src">
            <property name="enableSubPackages" value="true" />
        </sqlMapGenerator>

        <!-- Mapper接口的生成策略 -->
        <javaClientGenerator type="XMLMAPPER" targetPackage="com.atguigu.mbg.mapper"  targetProject="src">
            <property name="enableSubPackages" value="true" />
        </javaClientGenerator>

        <!--
          配置通过逆向分析表生成javaBean的类
             属性tableName：配置表名
             属性domainObjectName：配置要生成的类名
        -->
        <table tableName="emp" domainObjectName="Employee"></table>
        <table tableName="dep" domainObjectName="Department"></table>
    </context>
</generatorConfiguration>
~~~

**编写测试：运行代码生成器生成代码**

~~~java
package com.atguigu.mbg.test;

import org.junit.Test;
import org.mybatis.generator.api.MyBatisGenerator;
import org.mybatis.generator.config.Configuration;
import org.mybatis.generator.config.xml.ConfigurationParser;
import org.mybatis.generator.internal.DefaultShellCallback;
import java.io.File;
import java.util.ArrayList;
import java.util.List;

public class MBGTest {
    /**
     * 测试逆向工程，生成对应的代码
     */
    @Test
    public void test() throws Exception {
        List<String> warnings = new ArrayList<String>();
        boolean overwrite = true;
        File configFile = new File("mbg.xml");
        ConfigurationParser cp = new ConfigurationParser(warnings);
        Configuration config = cp.parseConfiguration(configFile);
        DefaultShellCallback callback = new DefaultShellCallback(overwrite);
        MyBatisGenerator myBatisGenerator = new MyBatisGenerator(config,
                callback, warnings);
        myBatisGenerator.generate(null);

    }
}

~~~

**复杂查询**

![](Mybatis.assets/Snipaste_2021-06-06_23-34-05-1622993671238.png)

![](Mybatis.assets/Snipaste_2021-06-06_23-38-13-1622993905337.png)

## 15 Mybatis分页插件

1)    PageHelper是MyBatis中非常方便的第三方分页插件

2)    官方文档：

https://github.com/pagehelper/Mybatis-PageHelper/blob/master/README_zh.md

### 15.1 使用步骤

~~~markdown
## 1.导入jar包
    pagehelper-x.x.x.jar 和 jsqlparser-0.9.5.jar
## 2.在Mybatis全局配置文件中配置分页插件
    <plugins>
	  <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
    </plugins>
## 3.使用PageHelper提供的方法进行分页
## 4.可以使用更强大的PageInfo封装返回结果
~~~

### 15.2 具体实现

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!--
    DOCTYPE后面的值一定是当前文件的根标签名
-->
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

<configuration>
    <properties resource="jdbc.properties"></properties>
    <settings>
        <!-- 将下划线设置为驼峰，user_name映射为userName-->
        <setting name="mapUnderscoreToCamelCase" value="true"/>
        <!-- 开启延迟加载-->
        <setting name="lazyLoadingEnabled" value="true"/>
        <!-- 是否查询所有数据-->
        <setting name="aggressiveLazyLoading" value="false"/>
    </settings>
    <plugins>
        <plugin interceptor="com.github.pagehelper.PageInterceptor"></plugin>
    </plugins>
    <environments default="development">
        <environment id="development">
            <!-- JDBC:使用原始的JDBC的方式来管理事务，即提交和回滚都需要手动处理-->
            <transactionManager type="JDBC"/>
            <dataSource type="POOLED">
                <property name="driver" value="${jdbc.driver}"/>
                <property name="url" value="${jdbc.url}"/>
                <property name="username" value="${username}"/>
                <property name="password" value="${password}"/>
            </dataSource>
        </environment>
    </environments>
    <!--
      <mappers>标签：注册mapper映射文件，也是SQL映射文件
    -->
    <mappers>
        <!--
         子标签1 mapper:
              resource属性：指定类路径下的sql映射文佳的路径
              url:指定磁盘或者网络下的sql映射文件的路径
              class属性：指定mapper接口的全类名，使用class属性注册sql映射文件时，映射文件必须与mapper接口同包且同名
                        或者不同包不同名则通过在Mapper接口的方法上添加注解写sql语句！
         子标签2 package:
              通过指定包名注册映射文件
              使用package属性注册sql映射文件时，映射文件必须与mapper接口同包且同名
                        或者不同包不同名则通过在Mapper接口的方法上添加注解写sql语句！
         -->
        <mapper resource="TbAreasMapper.xml"/>
    </mappers>
</configuration>
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<!--
   两个绑定：
   1.映射文件的命名空间namespace的属性值为接口的全类名：映射文件与接口完成绑定
     接口全限定名要与映射文件的namespace保持一致
   2.sql标签的id属性值与方法名进行绑定
     接口的方法名要与sql标签的ID保持一致
   resultType与实体类全类名进行绑定
-->
<mapper namespace="com.atguigu.mapper.TbAreasMapper">
  <select id="getAllTbAreas" resultType="com.atguigu.bean.TbAreas">
      select  * FROM tb_areas
  </select>

</mapper>
~~~

~~~java
package com.atguigu.bean;

public class TbAreas {
    private Integer id;
    private String areaid;
    private String area;
    private String cityid;

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getAreaid() {
        return areaid;
    }

    public void setAreaid(String areaid) {
        this.areaid = areaid;
    }

    public String getArea() {
        return area;
    }

    public void setArea(String area) {
        this.area = area;
    }

    public String getCityid() {
        return cityid;
    }

    public void setCityid(String cityid) {
        this.cityid = cityid;
    }

    @Override
    public String toString() {
        return "TbAreas{" +
                "id=" + id +
                ", areaid='" + areaid + '\'' +
                ", area='" + area + '\'' +
                ", cityid='" + cityid + '\'' +
                '}';
    }
}

~~~

~~~java
package com.atguigu.mapper;

import com.atguigu.bean.TbAreas;

import java.util.List;

public interface TbAreasMapper {
    List<TbAreas> getAllTbAreas();
}

~~~

**测试1：获取基本信息**

~~~java
package com.atguigu.test;



import com.atguigu.bean.TbAreas;
import com.atguigu.mapper.TbAreasMapper;
import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;


import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis() throws Exception{
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory build = builder.build(resourceAsStream);
        SqlSession sqlSession = build.openSession();
        TbAreasMapper mapper = sqlSession.getMapper(TbAreasMapper.class);
        // 添加分页信息，分页设置放在查询之前 
        Page<Object> page = PageHelper.startPage(1, 15);
        List<TbAreas> allTbAreas = mapper.getAllTbAreas();
        // 获取相关信息
        System.out.println("当前页是："+page.getPageNum());
        System.out.println("每页显示的条数是："+page.getPageSize());
        System.out.println("总页数是："+page.getPages());
        System.out.println("总记录数："+page.getTotal());
    }
}

~~~

**测试2：使用pageInfo**

~~~java
package com.atguigu.test;



import com.atguigu.bean.TbAreas;
import com.atguigu.mapper.TbAreasMapper;
import com.github.pagehelper.Page;
import com.github.pagehelper.PageHelper;
import com.github.pagehelper.PageInfo;
import com.sun.xml.internal.ws.addressing.WsaActionUtil;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import org.junit.Test;


import java.io.InputStream;
import java.util.ArrayList;
import java.util.List;


public class TestMybatis {
    @Test
    public void testMybatis() throws Exception{
        InputStream resourceAsStream = Resources.getResourceAsStream("mybatis-config.xml");
        SqlSessionFactoryBuilder builder = new SqlSessionFactoryBuilder();
        SqlSessionFactory build = builder.build(resourceAsStream);
        SqlSession sqlSession = build.openSession();
        TbAreasMapper mapper = sqlSession.getMapper(TbAreasMapper.class);
        // 添加分页信息,分页设置放在查询之前
        Page<Object> page = PageHelper.startPage(2, 15);
        List<TbAreas> allTbAreas = mapper.getAllTbAreas();
        // 创建PageInfo对象,并且设置每页显示多少页码
        PageInfo<TbAreas> pageInfo = new PageInfo<>(allTbAreas,20);
        // 获取相关信息
        System.out.println("当前页是："+pageInfo.getPageNum());
        System.out.println("每页显示的条数是："+pageInfo.getPageSize());
        System.out.println("总页数是："+pageInfo.getPages());
        System.out.println("总记录数："+pageInfo.getTotal());
        System.out.println("是否有上一页："+pageInfo.isHasPreviousPage());
        System.out.println("获取上一页："+pageInfo.getPrePage());
        System.out.println("是否有下一页："+pageInfo.isHasNextPage());
        System.out.println("获取下一页："+pageInfo.getNextPage());
        System.out.println("是否是第一页："+pageInfo.isIsFirstPage());
        System.out.println("是否是最后一页："+pageInfo.isIsLastPage());
    }
}
--------------------------------------------------------------
当前页是：2
每页显示的条数是：15
总页数是：210
总记录数：3144
是否有上一页：true
获取上一页：1
是否有下一页：true
获取下一页：3
是否是第一页：false
是否是最后一页：false
~~~

## 16.自定义mybatis框架

### 1.Dom4j

#### 1.1 依赖

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>ToolsDemo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <packaging>jar</packaging>

    <properties>
        <!--dom4j版本-->
        <dom4j.vesrion>1.6.1</dom4j.vesrion>
        <!--dom4j依赖包版本-->
        <jaxen.version>1.1.6</jaxen.version>
    </properties>

    <dependencies>
        <!--dom4j依赖-->
        <dependency>
            <groupId>dom4j</groupId>
            <artifactId>dom4j</artifactId>
            <version>${dom4j.vesrion}</version>
        </dependency>
        <dependency>
            <groupId>jaxen</groupId>
            <artifactId>jaxen</artifactId>
            <version>${jaxen.version}</version>
        </dependency>
    </dependencies>
</project>
~~~

#### 1.2 需要解析的xml文件

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--运行环境的配置，
        default="mysql"：指定使用哪一个运行环境
    -->
    <environments default="mysql">
        <!--运行环境配置，
           id="mysql" ：配置运行环境，id属性是唯一标识的名称
        -->
        <environment id="mysql">
            <!--transactionManager：事务配置，
                type="JDBC"：直接使用jdbc事务
            -->
            <transactionManager type="JDBC"/>
            <!--dataSource：配置数据源（连接数据库的信息），说明：
                type=POOLED：mybatis框架提供的连接池
            -->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/86_mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="admin"/>
            </dataSource>
        </environment>
    </environments>

    <!--加载具体的接口映射文件-->
    <mappers>
    <!--加载UserDao.xml-->
    <mapper resource="sqlmap/UserDao.xml"></mapper>
    <mapper resource="sqlmap/OrderDao.xml"></mapper>
    <mapper resource="sqlmap/ProductDao.xml"></mapper>
   </mappers>

</configuration>
~~~

#### 1.3 核心程序

~~~java
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.io.InputStream;
import java.util.List;

public class Dom4jDemo {
    public static void main(String[] args) throws DocumentException {
        // 1.加载配置文件
        InputStream resourceAsStream
                = Dom4jDemo.class.getClassLoader().getResourceAsStream("sqlMapConfig.xml");
        // 2.dom4j解析配置文件内容
        SAXReader reader = new SAXReader();
        Document document = reader.read(resourceAsStream);
        // 3.获取xml完整字符串
        String xmlStr = document.asXML();
        System.out.println(xmlStr);
        System.out.println("***************获取xml完整字符串结束************************");
        // 4.获取根元素
        Element rootElement = document.getRootElement();
        System.out.println(rootElement.getName());
        System.out.println("*****************获取根元素结束**********************");
        // 5.获取根元素的所有子元素
        List<Element> elements = rootElement.elements();
        for (Element element : elements) {
            System.out.println(element.getName());
        }
        System.out.println("****************获取根元素的所有子元素结束***********************");
        // 6.获取根元素的指定子元素
        List<Element> mappers = rootElement.elements("mappers");
        for (Element mapper : mappers) {
            System.out.println(mapper.getName());
        }
        System.out.println("*****************获取根元素的指定子元素结束**********************");
        // 7.查找任意深层次的后被元素
        List<Element> propertys = rootElement.selectNodes("//property");
        for (Element property : propertys) {
            // 获取属性值
            String attr1 = property.attributeValue("name");
            String attr2 = property.attributeValue("value");
            System.out.println("attr1="+attr1+",attr2"+attr2);
        }
    }
}
~~~

### 2.自定义框架

#### 2.1 架构分析

![](Mybatis.assets/Snipaste_2021-05-30_18-17-17-1622369859294.png)

#### 2.2 技术分析

~~~shell
# 1.解析xml配置文件：dom4j
# 2.操作数据库：jdbc
# 3.连接池技术：c3p0
# 4.单元测试框架：junit
# 5.工厂设计模式：就是代替我们new操作，创建对象的设计模式
# 6.动态代理技术
# 7.反射技术：运行时获取信息
# 8.在编写代码的时候不确定类型，在使用的时候才确定类型
~~~

![](Mybatis.assets/Snipaste_2021-05-30_18-24-44-1622370304838.png)

#### 2.3 搭建基础环境

~~~shell
# 1.创建maven项目
# 2.导入依赖
# 3.编写核心配置文件和映射配置文件
# 4.编写接口和实体映射类
~~~

~~~xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>ToolsDemo</artifactId>
    <version>1.0-SNAPSHOT</version>

    <packaging>jar</packaging>

    <properties>
        <!--dom4j版本-->
        <dom4j.vesrion>1.6.1</dom4j.vesrion>
        <!--dom4j依赖包版本-->
        <jaxen.version>1.1.6</jaxen.version>
        <!--mysql驱动版本-->
        <mysql.version>5.1.30</mysql.version>
        <!--c3p0版本-->
        <c3p0.version>0.9.2.1</c3p0.version>
        <!-- junit版本 -->
        <junit.version>4.12</junit.version>
    </properties>

    <dependencies>
        <!--dom4j依赖-->
        <dependency>
            <groupId>dom4j</groupId>
            <artifactId>dom4j</artifactId>
            <version>${dom4j.vesrion}</version>
        </dependency>
        <dependency>
            <groupId>jaxen</groupId>
            <artifactId>jaxen</artifactId>
            <version>${jaxen.version}</version>
        </dependency>
        <!-- mysql数据库依赖 -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>${mysql.version}</version>
        </dependency>
        <!--c3p0依赖-->
        <dependency>
            <groupId>com.mchange</groupId>
            <artifactId>c3p0</artifactId>
            <version>${c3p0.version}</version>
        </dependency>
        <!-- junit依赖 -->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>${junit.version}</version>
        </dependency>
    </dependencies>

</project>
~~~

**核心配置文件sqlMapConfig.xml**

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>

    <!--配置运行环境-->
    <environments default="development">
        <environment id="development">
            <transactionManager type="JDBC"/>

            <!--配置连接数据库信息-->
            <dataSource type="POOLED">
                <property name="driver" value="com.mysql.jdbc.Driver"/>
                <property name="url" value="jdbc:mysql://127.0.0.1:3306/mybatis"/>
                <property name="username" value="root"/>
                <property name="password" value="123456"/>
            </dataSource>

        </environment>
    </environments>

    <!--配置接口映射文件-->
    <mappers>
        <!--加载UserDao.xml文件-->
        <mapper resource="sqlmap/UserDao.xml"></mapper>
    </mappers>

</configuration>
~~~

**UserDao.xml**

~~~xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace属性值，必须指定为接口的全路径-->
<mapper namespace="com.itheima.dao.UserDao">

    <!--查询全部用户列表数据
        1.配置java对象属性名称，与sql语句字段的对应关系

        属性：
            id：唯一标识名称，与接口方法名称一致
            resultType：返回值。与接口方法返回值类型一致（暂时需要指定为全路径）

    -->
    <select id="findAllUsers" resultType="com.itheima.po.User">
    select * from `user`
</select>
</mapper>
~~~

**接口**

~~~java
package com.itheima.dao;

import com.itheima.po.User;

import java.util.List;

/**
 * 持久层dao接口
 */
public interface UserDao {

    /**
     * 查询全部用户列表数据
     */
    List<User> findAllUsers();
}

~~~

**实体类**

~~~java
package com.itheima.po;

import java.util.Date;

/**
 * 用户实体类
 */
public class User {

    private Integer id;// int(11) NOT NULL AUTO_INCREMENT,
    private String username;// varchar(32) NOT NULL COMMENT '用户名称',
    private Date birthday;// date DEFAULT NULL COMMENT '生日',
    private String sex;// char(1) DEFAULT NULL COMMENT '性别',
    private String address;// varchar(256) DEFAULT NULL COMMENT '地址',

    public Integer getId() {
        return id;
    }

    public void setId(Integer id) {
        this.id = id;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public String getSex() {
        return sex;
    }

    public void setSex(String sex) {
        this.sex = sex;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", birthday=" + birthday +
                ", sex='" + sex + '\'' +
                ", address='" + address + '\'' +
                '}';
    }
}

~~~

#### 2.4 解析配置文件

##### 2.4.1**Mapper类：用于封装接口映射文件**

~~~java
package com.itheima.config;

/**
 * 解析配置文件1：Mapper,用于封装接口映射文件的内容
 */
public class Mapper {
    // 名称空间
    private String nameSpace;
    // sql语句标签的id属性
    private String id;
    // sql语句标签的resultType
    private String resultType;
    // 执行的sql语句
    private String sql;

    public String getNameSpace() {
        return nameSpace;
    }

    public void setNameSpace(String nameSpace) {
        this.nameSpace = nameSpace;
    }

    public String getId() {
        return id;
    }

    public void setId(String id) {
        this.id = id;
    }

    public String getResultType() {
        return resultType;
    }

    public void setResultType(String resultType) {
        this.resultType = resultType;
    }

    public String getSql() {
        return sql;
    }

    public void setSql(String sql) {
        this.sql = sql;
    }
}

~~~

![](Mybatis.assets/Snipaste_2021-05-30_19-33-17-1622374418417.png)

##### 2.4.2**Configuration类：封装主配置文件sqlMapConfig.xml文件内容**

~~~java
package com.itheima.config;

import com.mchange.v2.c3p0.ComboPooledDataSource;
import org.dom4j.Document;
import org.dom4j.DocumentException;
import org.dom4j.Element;
import org.dom4j.io.SAXReader;

import java.beans.PropertyVetoException;
import java.io.InputStream;
import java.util.HashMap;
import java.util.List;
import java.util.Map;

/**
 * 解析配置文件二：Configuration，用于封装sqlMapConfig.xml
 */
public class Configuration {

    // 1.连接数据库的四个基本要素
    private String driver;
    private String url;
    private String username;
    private String password;

    // 2.创建数据源对象（c3p0）
    private ComboPooledDataSource dataSource;

    // 3.封装接口映射文件(选择Map)
    private Map<String,Mapper> mappers;

    /**
     * 1.思考：
     *      1.1.解析主配置文件sqlMapConfig.xml
     *      1.2.获取连接数据库的四个基本要素
     *      1.3.根据四个基本要素，创建数据源对象
     *
     *      1.4.解析接口映射文件
     */
    public Configuration(){
        // 1.2.获取连接数据库的四个基本要素
        loadSqlMapConfig();

        // 1.3.根据四个基本要素，创建数据源对象
        createDataSource();
    }

    // 加载解析主配置文件
    private void loadSqlMapConfig(){

        try {
            // 1.加载主配置文件sqlMapConfig.xml
            InputStream inputStream = this.getClass().
                    getClassLoader().getResourceAsStream("sqlMapConfig.xml");

            // 2.通过Dom4j解析配置文件
            SAXReader reader = new SAXReader();
            Document document = reader.read(inputStream);

            // 获取根元素
            Element rootElement = document.getRootElement();

            // 查找property标签后辈元素
            /**
             *  <property name="driver" value="com.mysql.jdbc.Driver"/>
             * <property name="url" value="jdbc:mysql://127.0.0.1:3306/111_mybatis"/>
             * <property name="username" value="root"/>
             * <property name="password" value="admin"/>
             */
            List<Element> list = rootElement.selectNodes("//property");
            for(Element e:list){
                // 获取到name和value属性值
                String name = e.attributeValue("name");
                String value = e.attributeValue("value");

                if("driver".equals(name)){
                    driver = value;
                }
                if("url".equals(name)){
                    url = value;
                }
                if("username".equals(name)){
                    username = value;
                }
                if("password".equals(name)){
                    password = value;
                }
                // 3.获取连接数据库的四个基本要素

            }

            // 4.解析接口映射文件，并且封装
            //  <mapper resource="sqlmap/UserDao.xml"></mapper>
            List<Element> list1 = rootElement.selectNodes("//mapper");
            for(Element e:list1){
                // 获取resource属性值
                String path = e.attributeValue("resource");

                // 解析接口映射文件
                loadMapperConfig(path);
            }

        } catch (DocumentException e) {
            e.printStackTrace();
        }


    }

    // 创建数据源对象
    private void  createDataSource(){
        // 创建c3p0数据源对象
        dataSource = new ComboPooledDataSource();

        try {
            // 设置连接数据库的四个要素
            dataSource.setDriverClass(driver);
            dataSource.setJdbcUrl(url);
            dataSource.setUser(username);
            dataSource.setPassword(password);
        } catch (PropertyVetoException e) {
            e.printStackTrace();
        }
    }

    // 解析接口映射文件
    private void loadMapperConfig(String path){

        try {
            // 1.加载接口映射文件
            InputStream inputStream = this.getClass().
                    getClassLoader().getResourceAsStream(path);

            // 2.通过dom4j解析配置文件内容
            SAXReader reader = new SAXReader();
            Document document = reader.read(inputStream);

            // 获取根元素
            Element rootElement = document.getRootElement();

            // 获取名称空间
            String namespace = rootElement.attributeValue("namespace");

            // 获取根元素的所有子元素
            List<Element> list = rootElement.elements();
            for(Element e:list){
                /**
                 * <select id="findAllUsers" resultType="com.itheima.po.User">
                 * select * from `user`
                 * </select>
                 */

                // 获取id和resultType属性值
                String id = e.attributeValue("id");
                String resultType = e.attributeValue("resultType");

                // 获取sql语句
                String sql = e.getText();

                // 封装Mapper
                putMappers(namespace,id,resultType,sql);

            }


        } catch (DocumentException e) {
            e.printStackTrace();
        }
    }

    // 封装Mapper
    private void  putMappers(String namespace,String id,String resultType,String sql){

        // 创建Mapper
        Mapper mapper = new Mapper();
        mapper.setNameSpace(namespace);
        mapper.setId(id);
        mapper.setResultType(resultType);
        mapper.setSql(sql);

        // 把Mapper对象放入Map容器中
        if(mappers == null){
            mappers = new HashMap<String, Mapper>();
        }

        String key = namespace+"."+id;
        mappers.put(key,mapper);

    }

    //==========================getter/setter==================================
    public String getDriver() {
        return driver;
    }

    public void setDriver(String driver) {
        this.driver = driver;
    }

    public String getUrl() {
        return url;
    }

    public void setUrl(String url) {
        this.url = url;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    public ComboPooledDataSource getDataSource() {
        return dataSource;
    }

    public void setDataSource(ComboPooledDataSource dataSource) {
        this.dataSource = dataSource;
    }

    public Map<String, Mapper> getMappers() {
        return mappers;
    }

    public void setMappers(Map<String, Mapper> mappers) {
        this.mappers = mappers;
    }
}

~~~

![](Mybatis.assets/Snipaste_2021-05-30_19-48-53-1622375357836.png)

#### 2.5 编写框架核心组件

##### 2.5.1 SqlSession 创建代理对象

~~~java
package com.itheima.core;

import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Proxy;

/**
 * 核心组件，提供一个getMapper方法
 */
public class SqlSession {
    /**
     *  getMapper方法：获取接口的代理对象
     *  需要用到的技术：
     *      1.动态代理技术
     *            涉及到的类：Proxy
     *            涉及到的方法：newProxyInstance();
     *            涉及到的参数：
     *                ClassLoader:类加载器
     *                Interfaces:实现的接口列表
     *                InvocationHandler:如何增强
     *      2.泛型技术
     */
     public <T> T getMapper(Class<T> type){
         // 创建代理对象
         T proxy = (T)Proxy.newProxyInstance(
                 this.getClass().getClassLoader(),
                 new Class[]{type},
                 new MapperProxyFacory()
         );
         return proxy;
     }
}

~~~

##### 2.5.2 MapperProxyFacory 代理要增强的方法写在这个里面

~~~java
package com.itheima.core;

import com.itheima.config.Configuration;
import com.itheima.config.Mapper;
import com.itheima.util.Executor;
import com.mchange.v2.c3p0.ComboPooledDataSource;

import javax.sql.DataSource;
import java.lang.reflect.InvocationHandler;
import java.lang.reflect.Method;
import java.security.spec.RSAOtherPrimeInfo;
import java.util.Map;

public class MapperProxyFacory implements InvocationHandler {
    /**
     *  回调实现的方法：invoke
     *  参数:
     *     proxy:代理对象的引用，不需要关心
     *     method：当前执行的方法
     *     args：当前方法执行的参数列表
     *   思考：
     *     1.解析配置文件，获取配置文件内容
     *     2.根据配置文件内容，获取执行的sql
     *     3.根据配置文件内容，获取数据源对象（数据库连接的对象）
     *     4.最终执行数据库操作，返回结果集
     */
    public Object invoke(Object proxy, Method method, Object[] args) throws Throwable {
        //  1.解析配置文件，获取配置文件内容
        Configuration configuration = new Configuration();

        // 2.根据配置文件内容，获取执行的sql
        Map<String, Mapper> mappers = configuration.getMappers();

        // 获取当前的mapper对象
        /**
         *  key=namespace+","+id
         *  namespace:类的全限定名
         *  id:方法名
         */
        String namespace = method.getDeclaringClass().getName();
        String id = method.getName();
        String key=namespace+"."+id;

        Mapper mapper = mappers.get(key);

        // 3.根据配置文件内容，获取数据源对象（数据库连接的对象）
        ComboPooledDataSource dataSource = configuration.getDataSource();

        System.out.println("当前执行的操作"+key);
        System.out.println("数据源对象"+dataSource);

        // 4.执行数据库操作，返回结果集
        return Executor.selectList(mapper, dataSource.getConnection());
    }
}
~~~

##### 2.5.3 SqlSessionFactory

~~~java
package com.itheima.core;

/**
 * 提供openSession方法，获取SqlSession对象
 */
public class SqlSessionFactory {
   public  SqlSession  openSession(){
       return new SqlSession();
   }
}
~~~

### 2.6 数据库操作工具类

~~~java
package com.itheima.util;

import com.itheima.config.Mapper;

import java.beans.PropertyDescriptor;
import java.lang.reflect.Method;
import java.sql.*;
import java.util.ArrayList;
import java.util.Arrays;
import java.util.List;

/**
 * 封装执行数据操作的工具类
 */
public class Executor {
    /**
     * 封装执行数据库操作的工具类
     * 思考用到的技术
     *     1.jdbc连接数据库
     *     2.泛型技术
     */
    public static <T> List<T> selectList(Mapper mapper, Connection connection){
        // 定义返回值
        List<T> list = new ArrayList<T>();

        // 定义PreparedStatement和ResultSet
        PreparedStatement psml = null;
        ResultSet rs = null;

        try {
            // 获取执行的sql和返回值类型
            String sql = mapper.getSql();
            System.out.println(sql);
            String resultType = mapper.getResultType();

            // 通过反射技术加载返回值类型
            Class<?> className = Class.forName(resultType);

            // jdbc操作
            psml = connection.prepareStatement(sql);
            rs = psml.executeQuery();

            // 封装结果集数据
            handleResult(list,className,rs);
        } catch (Exception e) {
            e.printStackTrace();
        }finally {
            //释放资源
            close(connection,psml,rs);
        }
        return list;
    }

    private static <T> void handleResult(List list, Class<?> className, ResultSet rs) throws Exception {
        // 1.获取元数据
        /**
         * ResultSetMetaData里面包含那当前执行的sqld的字段数量和字段名称
         */
        ResultSetMetaData metaData = rs.getMetaData();
        int columnCount = metaData.getColumnCount();

        // 字段名称数组
        String[] columnNames = new String[columnCount];
        for (int i = 1; i <= columnCount; i++) {
            columnNames[i-1] = metaData.getColumnName(i);
        }
        // 循环遍历取出结果集数据进行封装
        while(rs.next()){
            // 1.通过反射技术实例化目标对象
            Object o = className.newInstance();
            // 2.从rs中取出结果数据
            for (int i = 0; i < columnNames.length; i++) {
                // 根据字段名称取出数据
                Object rv = rs.getObject(columnNames[i]);
                // 通过反射技术赋值
                PropertyDescriptor pd = new PropertyDescriptor(columnNames[i],className);
                // 获取Set方法
                Method writeMethod = pd.getWriteMethod();
                writeMethod.invoke(o,rv);
            }
            // 3.把对象添加到list
            list.add(o);
        }
    }

    // 释放资源
    private static void close(Connection connection, PreparedStatement psml, ResultSet rs) {
            try {
                if(rs!=null){
                    rs.close();
                }
                if(psml!=null){
                    psml.close();
                }
                if(connection!=null){
                    connection.close();
                }
            } catch (SQLException e) {
                e.printStackTrace();
            }
    }
}
~~~



