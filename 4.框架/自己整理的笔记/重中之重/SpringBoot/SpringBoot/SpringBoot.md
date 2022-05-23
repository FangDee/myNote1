#  SpringBoot入门

## 第一章 概述

### 1、今日内容

- SpringBoot概述、快速入门
- SpringBoot配置
- SpringBoot整合-重点

### 2、SpringBoot概述-面试

#### （1）官网： https://spring.io/

![image-20220119105911027](https://www.ydlclass.com/doc21xnv/assets/image-20220119105911027.f76857f8.png)

#### （2）spring缺点：

**a、配置繁琐**

搭建ssm项目，需要配置大量xml。 application.xml spring-mybatis.xml spring-mvc.xml,大量的bean。

**b、依赖繁琐**

pom.xml要写**大量依赖**。 pom.xml spring-core spring-bean spring-mvc spring-mybatis java-connector

**版本冲突** spring-core 4.0 spring-mvc 5.0

#### 3）SpringBoot概念

SpringBoot提供了一种快速使用Spring的方式，基于**约定优于配置**的思想，可以让开发人员不必在配置与逻辑业务之间进行思维的切换，全身心的投入到逻辑业务的代码编写中，从而大大提高了开发的效率。

#### （4）SpringBoot功能

**a、 自动配置**

Spring Boot的自动配置是一个运行时（更准确地说，是应用程序启动时）的过程，考虑了众多因素，才决定Spring配置应该用哪个，不该用哪个。该过程是SpringBoot自动完成的。

**b、起步依赖**

起步依赖本质上是一个Maven项目对象模型（Project Object Model，POM），定义了对其他库的**传递依赖**，这些东西加在一起即支持某项功能。 **依赖太多** **版本冲突**。

简单的说，起步依赖就是将具备某种功能的坐标打包到一起，并提供一些默认的功能。

**c、** **辅助功能**

提供了一些大型项目中常见的非功能性特性，如嵌入式服务器（tomcat）、安全、指标，健康检测、外部配置等。

**注意：Spring Boot 并不是对 Spring 功能上的增强，而是提供了一种快速使用 Spring 的方式。**

### 3、SpringBoot快速入门

官网： https://docs.spring.io/spring-boot/docs/current/reference/html

**（1）需求**：搭建SpringBoot工程，定义HelloController.hello()方法，返回”Hello SpringBoot!”。

**（2）实现步骤**：

①创建Maven项目 springboot-helloworld

②导入SpringBoot起步依赖

```xml
<!--springboot工程需要继承的父工程-->
    <parent>
      <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>2.1.8.RELEASE</version>
    </parent>

    <dependencies>
        <!--web开发的起步依赖-->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
    </dependencies>
```

③定义Controller编写引导类

```java
@SpringBootApplication//表示这个类 是springboot主启动类。
public class HelloApplication {
    public static void main(String[] args) {
        SpringApplication.run(HelloApplication.class,args);
    }
}
```

④定义Controller

```java
@RestController
@RequestMapping("/hello")
public class HelloController {

    @GetMapping("/test")
    public String test1(){
        return "Hello SpringBoot!";
    }
}
```

⑤启动测试 访问： http://localhost:8080/hello/test

**（3）总结**

1启动springboot一个web工程

 1pom 规定父工程，导入web的起步依赖

 2主启动类 @SpringBootApplication、main

 3业务逻辑 controller，service,dao

- SpringBoot在创建项目时，使用jar的打包方式。 java -jar xxx.jar
- SpringBoot的引导类，是项目入口，运行main方法就可以启动项目。
- 使用SpringBoot和Spring构建的项目，业务代码编写方式完全一样。

### 4、快速构建SpringBoot工程

![image-20220119171114892](https://www.ydlclass.com/doc21xnv/assets/image-20220119171114892.5265752f.png)

![image-20220119171223806](https://www.ydlclass.com/doc21xnv/assets/image-20220119171223806.6ceaed8c.png)

![image-20220119171317631](https://www.ydlclass.com/doc21xnv/assets/image-20220119171317631.5d8ee079.png)

![image-20220119171328347](https://www.ydlclass.com/doc21xnv/assets/image-20220119171328347.7bc66160.png)

编写conreoller

```java
@RestController
public class HelloController {
@RequestMapping("/hello")
public String hello(){
    return " hello Spring Boot !";
}
}
```

启动测试 访问： http://localhost:8080/hello/test

### 5、SpringBoot起步依赖原理分析-理解

- 在spring-boot-starter-parent中定义了各种技术的版本信息，组合了一套最优搭配的技术版本。
- 在各种starter中，定义了完成该功能需要的坐标合集，其中大部分版本信息来自于父工程。
- 我们的工程继承parent，引入starter后，通过**依赖传递**，就可以简单方便获得需要的jar包，并且不会存在版本冲突等问题。

## 第二章 配置文件

### **1、SpringBoot配置-配置文件分类**

SpringBoot是基于约定的，所以很多配置都有默认值，但如果想使用自己的配置替换默认配置的话，就可以使用application.properties或者application.yml（application.yaml）进行配置。

1. 默认配置文件名称：application
2. 在同一级目录下优先级为：properties>**yml** > yaml

例如：配置内置Tomcat的端口

properties：

```properties
server.port=8080
```

yml:

```yaml
server: 
	port: 8080
```

init工程：

修改application.properties

```text
server.port=8081
```

新建application.yml

```text
server: 
	port: 8082
```

新建application.yml

```text
server: 
	port: 8083
```

### )2、SpringBoot配置-yaml基本语法

#### （1）概念：

YAML是一种直观的能够被电脑识别的的数据数据序列化格式，并且容易被人类阅读，容易和脚本语言交互的，可以被支持YAML库的不同的编程语言程序导入。

#### （2）语法特点：

- 大小写敏感
- 数据值前边必须有空格，作为分隔符
- 使用缩进表示层级关系
- 缩进时不允许使用Tab键，只允许使用空格（各个系统 Tab对应的空格数目可能不同，导致层次混乱）。
- 缩进的空格数目不重要，只要相同层级的元素左侧对齐即可
- ''#" 表示注释，从这个字符一直到行尾，都会被解析器忽略。

```yaml
server: 
	port: 8080  
    address: 127.0.0.1
name: abc
```

### 3、SpringBoot配置-yaml数据格式

**对象(map)**：键值对的集合。

```yaml
person:  
   name: itlils
# 行内写法
person: {name: itlils}
```

**数组**：一组按次序排列的值

```text
address:
  - beijing
  - shanghai
# 行内写法
address: [beijing,shanghai]
```

**纯量**：单个的、不可再分的值

```yaml
msg1: 'hello \n world'  # 单引忽略转义字符
msg2: "hello \n world"  # 双引识别转义字符
```

**参数引用**

```yaml
name: itlils 
person:
  name: ${itlils} # 引用上边定义的name值
```

### 4、SpringBoot配置-获取数据

**1@Value**

```java
    #获取普通配置
    @Value("${name}")
    private String name;
    #获取对象属性
    @Value("${person.name}")
    private String name2;
   	#获取数组
    @Value("${address[0]}")
    private String address1;
  	#获取纯量
    @Value("${msg1}")
    private String msg1;
```

**2Evironment**

```java
@Autowired
 private Environment env;

 System.out.println(env.getProperty("person.name"));

 System.out.println(env.getProperty("address[0]"));
```

**3 @ConfigurationProperties**

**注意**：prefix一定要写

```java
@Component
@ConfigurationProperties(prefix = "person")
public class Person {

    private String name;
    private int age;
    private String[] address;

    public String getName() {
        return name;
    }

    public void setName(String name) {
        this.name = name;
    }

    public int getAge() {
        return age;
    }

    public void setAge(int age) {
        this.age = age;
    }

    public String[] getAddress() {
        return address;
    }

    public void setAddress(String[] address) {
        this.address = address;
    }

    @Override
    public String toString() {
        return "Person{" +
                "name='" + name + '\'' +
                ", age=" + age +
                '}';
    }
}
```

修改controller

```java
@Autowired
private Person person;

 		System.out.println(person);
        String[] address = person.getAddress();
        for (String s : address) {
            System.out.println(s);
        }
```

去掉报警提示：

```xml
 		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-configuration-processor</artifactId>
            <optional>true</optional>
        </dependency>
```

### 5、SpringBoot配置-profile-运维

1. **背景：profile是用来完成不同环境下，配置动态切换功能的**。
2. **profile配置方式**

 多profile文件方式：提供多个配置文件，每个代表一种环境。主配置文件application.properties配置：

```text
spring.profiles.active=dev
```

 application-dev.properties/yml 开发环境

 application-test.properties/yml 测试环境

 application-pro.properties/yml 生产环境

 yml多文档方式：

 在yml中使用 --- 分隔不同配置

```yaml
---
server:
  port: 8081
spring:
  profiles: dev
---
server:
  port: 8082
spring:
  profiles: pro
---
server:
  port: 8083
spring:
  profiles: test
---
spring:
  profiles:
    active: dev
```

1. **profile激活方式**

- 配置文件： 再配置文件中配置：spring.profiles.active=dev

- 虚拟机参数：在VM options 指定：-Dspring.profiles.active=pro

- 命令行参数： --spring.profiles.active=dev

  相当于上线时，运行jar包：java -jar xxx.jar --spring.profiles.active=dev

  测试：使用maven 打包此项目，在target包中出现springboot-profiles-0.0.1.jar

  cmd 输入

  ```text
  java -jar springboot-profiles-0.0.1.jar --spring.profiles.active=test
  ```

### 6、SpringBoot配置-项目内部配置文件加载顺序

加载顺序为下文的排列顺序，高优先级配置的属性会生效

- file:./config/：当前项目下的/config目录下
- file:./ ：当前项目的根目录
- classpath:/config/：classpath的/config目录
- classpath:/ ：classpath的根目录

测试：

新建springboot-config目录，分别在以上目录创建配置文件。

注意：1项目根目录为springboottest。

 2高级配置文件只覆盖低级配置文件的重复项。低级配置文件的独有项任然有效。最低级配置文件中增加：

```text
server.servlet.context-path = /test
```

访问： http://localhost:8084/test/hello

### 7、SpringBoot配置-项目外部配置加载顺序

外部配置文件的使用是为了不修改配置文件做的

1.命令行

```cmd
java -jar app.jar --name="Spring“ --server.port=9000
```

2.指定配置文件位置

```cmd
 java -jar myproject.jar --spring.config.location=d://application.properties
```

https://docs.spring.io/spring-boot/docs/current/reference/html/features.html#features.external-config

作用：生产环境，随时改变环境变量时，可以通过改变配置文件来做。不需重新打包项目。

## 第三章 整合框架-重要

### 1、SpringBoot整合Junit

1. 搭建SpringBoot工程 springboot-test。不用任何起步依赖。
2. 引入starter-test起步依赖

```xml
 <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

1. 编写service com.ydl.springboottest

   ```java
   
   ```

@Service public class UserService { public void add() { System.out.println("add..........."); } }

~~~text
4. 编写测试类 com.ydl.springboottest

```java
@SpringBootTest
@RunWith(SpringRunner.class)
public class UserServiceTest {
    @Autowired
    UserService userService;

    @Test
    public void testAdd() {
        userService.add();
    }
}
~~~

1. 测试

### 2、SpringBoot整合mybatis-最重点

①搭建SpringBoot工程 springboot-mybatis

②引入mybatis起步依赖，添加mysql驱动

```xml
	<dependencies>
        <dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.0</version>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <!--<scope>runtime</scope>-->
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

③定义表和实体类 com.ydl.springbootmybatis.domain

```java
public class User {
    private int id;
    private String username;
    private String password;


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

    public String getPassword() {
        return password;
    }

    public void setPassword(String password) {
        this.password = password;
    }

    @Override
    public String toString() {
        return "User{" +
                "id=" + id +
                ", username='" + username + '\'' +
                ", password='" + password + '\'' +
                '}';
    }
}
```

④编写DataSource和MyBatis相关配置

application.yml

```yaml
# datasource
spring:
  datasource:
    url: jdbc:mysql://127.0.0.1:3306/springboot?serverTimezone=UTC
    username: root
    password: root
    driver-class-name: com.mysql.jdbc.Driver
```

⑤纯注解开发 新建接口com.ydl.springbootmybatis.mapper

```java
@Mapper
public interface UserMapper {

    @Select("select * from t_user")
    public List<User> findAll();
}
```

测试

```java
@SpringBootTest
class SpringbootMybatisApplicationTests {
    @Autowired
    private UserMapper userMapper;
    
    @Test
    void testFindAll() {
        List<User> all = userMapper.findAll();
        System.out.println(all);
    }
}
```

⑥xml开发 新建接口 com.ydl.springbootmybatis.mapper

```java
@Mapper
public interface UserXmlMapper {
    public List<User> findAll();
}
```

resources下建立xml文件 UserMapper.xml

```xml
<?xml version="1.0" encoding="UTF8" ?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper namespace="com.ydl.mapper.UserXmlMapper">
    <select id="findAll" resultType="com.ydl.domain.User">
        select * from t_user
    </select>
</mapper>
```

修改application.yml 新增如下配置

```text
mybatis:
  mapper-locations: classpath:mapper/*
  type-aliases-package: com.ydl.springbootmybatis.domain
```

测试

```java
@Autowired
private UserXmlMapper userXmlMapper;

    @Test
    void testFindAllByXml() {
        List<User> all = userXmlMapper.findAll();
        System.out.println(all);
    }
```

### 3、SpringBoot整合redis

①搭建SpringBoot工程 springboot-redis

②引入redis起步依赖

```xml
  <dependencies>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-redis</artifactId>
        </dependency>

        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
    </dependencies>
```

③编写测试类

```java
@SpringBootTest
class SpringbootRedisApplicationTests {
    @Autowired
    private RedisTemplate redisTemplate;

    @Test
    void testSet() {
        redisTemplate.boundValueOps("name").set("zhangsan");
    }

    @Test
    void testGet() {
        Object name = redisTemplate.boundValueOps("name").get();
        System.out.println(name);
    }
}
```

④启动redis

⑤测试

⑥application.yml 配置redis相关属性

```yaml
spring:
  redis:
    host: 127.0.0.1 # redis的主机ip
    port: 6379
```

作业：ssm项目改造成boot

总结：上手

1boot不是spring的增强，快速使用 Spring 的方式。

 1.1pom parent starter

 1.2主启动类 SpringBootApplication

 1.3application.yml

快速构建

2配置文件

properties **yml** yaml

语法

自定义属性值，获取 @value Evironment 对象

3重要--其他技术整合

junit: starter

mybatis: starter 注解 xml

redis:starter redisTemplate 两套api

# SpringBoot高级

今日内容

- SpringBoot自动配置原理
- SpringBoot自定义starter
- SpringBoot事件监听
- SpringBoot流程分析
- SpringBoot监控
- SpringBoot部署

## 第一章 condition

### 1、 SpringBoot自动配置-Condition-1

Condition是Spring4.0后引入的条件化配置接口，通过实现Condition接口可以完成有条件的加载相应的Bean

@Conditional要配和Condition的实现类（ClassCondition）进行使用

- 创建模块 springboot-condition

- 一、观察spring自动创建bean过程

  改造启动类

  ```java
  @SpringBootApplication
  public class SpringbootConditionApplication {
  
      public static void main(String[] args) {
          //返回spring容器
          ConfigurableApplicationContext context= SpringApplication.run(SpringbootConditionApplication.class, args);
  
          // 获取redisTemplate 这个bean对象
          Object redisTemplate = context.getBean("redisTemplate");
          System.out.println(redisTemplate);
      }
  }
  ```

  启动：获取不到对象

  导入 redis依赖，再启动则可以获取到bean对象。

  ```xml
          <dependency>
              <groupId>org.springframework.boot</groupId>
              <artifactId>spring-boot-starter-data-redis</artifactId>
          </dependency>
  ```

- 二、自定义bean对象创建

  新建user实体类 com.ydlclass.springbootcondition.domain

  ```java
  public class User {
  }
  ```

  新建配置类 com.ydlclass.springbootcondition.config

  ```java
  @Configuration
  public class UserConfig {
  
      @Bean
      public User user() {
          return new User();
      }
  
  }
  ```

  启动类获取。测试可以获取到

  ```java
  Object user = context.getBean("user");
  System.out.println(user);
  ```

- 三、自定义bean 根据条件创建

  创建condition类 com.ydlclass.springbootcondition.condition

```java
public class ClassCondition implements Condition {
    @Override
    public boolean matches(ConditionContext conditionContext, AnnotatedTypeMetadata annotatedTypeMetadata) {
        return false;
    }
}
```

改造userConfig

```java
    @Bean
    @Conditional(ClassCondition.class)
    public User user() {
        return new User();
    }
```

测试不能自动创建user这个bean

- 四、改造ClassCondition。根据是否导入redis来决定是否创建userBean

  ```java
  package com.ydlclass.springbootcondition.condition;
  
  
  import org.springframework.context.annotation.Condition;
  import org.springframework.context.annotation.ConditionContext;
  import org.springframework.core.type.AnnotatedTypeMetadata;
  
  /**
   * @Created by IT李老师
   * 个人微 itlils
   */
  public class ClassCondition implements Condition {
  
      //通过boolean返回值，就能确定是否生成bean对象
      @Override
      public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
          //业务逻辑，返回true或false来决定某个bean对象是否生成
  
          //需求1：必须引入jedis,你的项目才生成user对象。
          try {
              Class.forName("redis.clients.jedis.Jedis");
              return true;
          } catch (ClassNotFoundException e) {
              return false;
          }
  
      }
  }
  ```

测试。获取不到userBean

```java
package com.ydlclass.springbootcondition;

import com.ydlclass.springbootcondition.domain.User;
import io.lettuce.core.output.StatusOutput;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

@SpringBootApplication
public class SpringbootConditionApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(SpringbootConditionApplication.class, args);

        //只要引入redis起步依赖，有redisTemplate对象。没引入，容器中没这个对象。
        //Object redisTemplate = context.getBean("redisTemplate");
        //System.out.println(redisTemplate);

        //通过名字拿bean对象
        //User user = (User) context.getBean("user");
        //System.out.println(user);

        //通过类型拿bean对象
        User user = context.getBean(User.class);
        System.out.println(user);
    }
}
```

导入依赖,再测试，可以获取到userBean

```xml
        <dependency>
            <groupId>redis.clients</groupId>
            <artifactId>jedis</artifactId>
        </dependency>
```

### 2、 SpringBoot自动配置-Condition-2

需求：将类的判断定义为动态的。判断哪个字节码文件存在可以动态指定。

自定义条件注解类

```java
import org.springframework.context.annotation.Conditional;

import java.lang.annotation.*;


@Target({ElementType.TYPE, ElementType.METHOD})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Conditional(ClassCondition.class)
public @interface ConditionOnClass {
    String[] value();
}
```

ClassCondition

```java
package com.ydlclass.springbootcondition.condition;


import org.springframework.context.annotation.Condition;
import org.springframework.context.annotation.ConditionContext;
import org.springframework.core.type.AnnotatedTypeMetadata;

import java.util.Map;

/**
 * @Created by IT李老师
 * 公主号 “IT李哥交朋友”
 * 个人微 itlils
 */
public class ClassCondition implements Condition {

    //通过boolean返回值，就能确定是否生成bean对象
    @Override
    public boolean matches(ConditionContext context, AnnotatedTypeMetadata metadata) {
        //业务逻辑，返回true或false来决定某个bean对象是否生成

        //需求1：必须引入jedis,你的项目才生成user对象。
        //try {
        //    Class.forName("redis.clients.jedis.Jedis");
        //    return true;
        //} catch (ClassNotFoundException e) {
        //    return false;
        //}


        try {
            //需求2：必须引入 动态传来的包名 ,你的项目才生成user对象。
            Map<String, Object> annotationAttributes = metadata.getAnnotationAttributes("com.ydlclass.springbootcondition.condition.ConditionalOnclass");
            System.out.println(annotationAttributes);
            String[] values = (String[]) annotationAttributes.get("value");
            for (String value : values) {
                Class.forName(value);
            }
            return true;
        } catch (Exception e) {
            return false;
        }


    }
}
```

**注意：**此处@ConditionOnClass为自定义注解

```java
package com.ydlclass.springbootcondition.config;

import com.ydlclass.springbootcondition.condition.ClassCondition;
import com.ydlclass.springbootcondition.condition.ConditionalOnclass;
import com.ydlclass.springbootcondition.domain.User;
import org.springframework.boot.autoconfigure.condition.ConditionalOnClass;
import org.springframework.boot.autoconfigure.condition.ConditionalOnProperty;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Conditional;
import org.springframework.context.annotation.Configuration;

/**
 * @Created by IT李老师
 * 个人微 itlils
 */
//配置类
@Configuration
public class UserConfig {

    @Bean
    //@Conditional(ClassCondition.class) //条件满足，new这个对象。条件不满足，不new这个bean这个对象
    @ConditionalOnclass({"redis.clients.jedis.Jedis"}) //这个注解，不用你写。springboot 已经写好了
    public User user(){
        return new User();
    }

    @Bean
    @ConditionalOnProperty(name = "ydlclass",havingValue = "itlils")
    //@ConditionalOnClass(name="redis.clients.jedis.Jedis")
    public User user2(){
        return new User();
    }

}
```

测试User对象的创建

```java
package com.ydlclass.springbootcondition;

import com.ydlclass.springbootcondition.domain.User;
import io.lettuce.core.output.StatusOutput;
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.context.ConfigurableApplicationContext;

@SpringBootApplication
public class SpringbootConditionApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(SpringbootConditionApplication.class, args);

        //只要引入redis起步依赖，有redisTemplate对象。没引入，容器中没这个对象。
        //Object redisTemplate = context.getBean("redisTemplate");
        //System.out.println(redisTemplate);

        //通过名字拿bean对象
        //User user = (User) context.getBean("user");
        //System.out.println(user);

        //通过类型拿bean对象
        //User user = context.getBean(User.class);
        //System.out.println(user);

        Object user2 = context.getBean("user2");
        System.out.println(user2);

    }

}
```

查看条件注解源码

![image-20220308110947447](https://www.ydlclass.com/doc21xnv/assets/image-20220308110947447.1f8a03da.png)

**SpringBoot 提供的常用条件注解：**

ConditionalOnProperty：判断配置文件中是否有对应属性和值才初始化Bean

ConditionalOnClass：判断环境中是否有对应字节码文件才初始化Bean

ConditionalOnMissingBean：判断环境中没有对应Bean才初始化Bean

### 3、 SpringBoot自动配置-切换内置web服务器

查看继承关系图

![1571306414687](https://www.ydlclass.com/doc21xnv/assets/1571306414687.a32a6428.png)

排除Tomcat

![1571306366201](https://www.ydlclass.com/doc21xnv/assets/1571306366201.64d62f01.png)

pom文件中的排除依赖效果

```xml
 		<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
            <!--排除tomcat依赖-->
            <exclusions>
                <exclusion>
                    <artifactId>spring-boot-starter-tomcat</artifactId>
                    <groupId>org.springframework.boot</groupId>
                </exclusion>
            </exclusions>
        </dependency>

        <!--引入jetty的依赖-->
        <dependency>
            <artifactId>spring-boot-starter-jetty</artifactId>
            <groupId>org.springframework.boot</groupId>
        </dependency>
```

问你：为什么引入了starter-data-redis，我们就能项目中，直接拿redistemplate?

springboot中的autoconfig工程里把常用的对象的配置类都有了，只要工程中，引入了相关起步依赖，这些对象在我们本项目的容器中就有了。

![image-20220308114104880](https://www.ydlclass.com/doc21xnv/assets/image-20220308114104880.6ada055c.png)

### 4、 SpringBoot自动配置-Enable注解原理-重点

- SpringBoot不能直接获取在其他工程中定义的Bean

  演示代码：

  springboot-enable工程

  ```java
  /**
   * @ComponentScan 扫描范围：当前引导类所在包及其子包
   *
   * com.ydlclass.springbootenable
   * com.ydlclass.config
   * //1.使用@ComponentScan扫描com.ydlclass.config包
   * //2.可以使用@Import注解，加载类。这些类都会被Spring创建，并放入IOC容器
   * //3.可以对Import注解进行封装。
   */
  
  //@ComponentScan("com.qiniu.config")
  //@Import(UserConfig.class)
  @EnableUser
  @SpringBootApplication
  public class SpringbootEnableApplication {
  
      public static void main(String[] args) {
          ConfigurableApplicationContext context = SpringApplication.run(SpringbootEnableApplication.class, args);
  
       //获取Bean
          Object user = context.getBean("user");
          System.out.println(user);
  
  	}
  
  }
  ```

  pom中引入springboot-enable-other

```java
 	<dependency>
          <groupId>com.ydlclass</groupId>
          <artifactId>springboot-enable-other</artifactId>
          <version>0.0.1-SNAPSHOT</version>
    </dependency>
```

springboot-enable-other工程

**UserConfig**

```java
@Configuration
public class UserConfig {

    @Bean
  public User user() {
     return new User();
  }

}
```

确实，本工程中没有这个第三方jar包中的bean对象

![image-20220308114747847](https://www.ydlclass.com/doc21xnv/assets/image-20220308114747847.51eb05bd.png)

**EnableUser注解类**

```java
import org.springframework.context.annotation.Import;

import java.lang.annotation.*;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Import(UserConfig.class)
public @interface EnableUser {
}
```

**原因**：@ComponentScan 扫描范围：当前引导类所在包及其子包

**三种解决方案：**

1.使用@ComponentScan扫描com.ydlclass.config包

2.可以使用@Import注解，加载类。这些类都会被Spring创建，并放入IOC容器

3.可以对Import注解进行封装。

**重点：Enable注解底层原理是使用@Import注解实现Bean的动态加载**

**重要**：**springbootapplication 由三个注解组成**

![image-20220308114911692](https://www.ydlclass.com/doc21xnv/assets/image-20220308114911692.84cdfb05.png)

```text
@SpringBootConfiguration 自动配置相关
@EnableAutoConfiguration
@ComponentScan 扫本包及子包
```

### 5、 SpringBoot自动配置-@Import详解

@Enable底层依赖于@Import注解导入一些类，使用@Import导入的类会被Spring加载到IOC容器中。而@Import提供4中用法：

①导入Bean。注意bean名字是全限定名。

![image-20220308151120063](https://www.ydlclass.com/doc21xnv/assets/image-20220308151120063.31f41f74.png)

②导入配置类

③导入 ImportSelector 实现类。一般用于加载配置文件中的类

④导入 ImportBeanDefinitionRegistrar 实现类。

- 导入Bean @Import(User.class)

- 导入配置类 @Import(UserConfig.class)

- 导入 ImportSelector 实现类 @Import(MyImportSelector.class)

  MyImportSelector

  ```java
  public class MyImportSelector implements ImportSelector {
      @Override
      public String[] selectImports(AnnotationMetadata importingClassMetadata) {
          return new String[]{"com.ydlclass.domain.User", "com.ydlclass.domain.Role"};
      }
  }
  ```

  ![image-20220308151811604](https://www.ydlclass.com/doc21xnv/assets/image-20220308151811604.c1db2a75.png)

- 导入 ImportBeanDefinitionRegistrar 实现类。@Import({MyImportBeanDefinitionRegistrar.class})

  ```java
  public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {
      @Override
      public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
          AbstractBeanDefinition beanDefinition = BeanDefinitionBuilder.rootBeanDefinition(User.class).getBeanDefinition();
          registry.registerBeanDefinition("user", beanDefinition);
  
  
      }
  }
  ```

  ![image-20220308152152225](https://www.ydlclass.com/doc21xnv/assets/image-20220308152152225.387350aa.png)

  SpringbootEnableApplication测试代码

```java
   Import4中用法：

*  1. 导入Bean
*  2. 导入配置类
*  3. 导入ImportSelector的实现类。
*  4. 导入ImportBeanDefinitionRegistrar实现类
      */
   
   //@Import(User.class)
  //@Import(UserConfig.class)
  //@Import(MyImportSelector.class)
  //@Import({MyImportBeanDefinitionRegistrar.class})

  @SpringBootApplication
  public class SpringbootEnableApplication {
 public static void main(String[] args) {
      ConfigurableApplicationContext context = SpringApplication.run(SpringbootEnableApplication.class, args);
  
      /*//获取Bean
      Object user = context.getBean("user");
      System.out.println(user);*/
  
      /*User user = context.getBean(User.class);
      System.out.println(user);
  
      Role role = context.getBean(Role.class);
      System.out.println(role);*/
  
    /*  Object user = context.getBean("user");
      System.out.println(user);*/
   Map<String, User> map = context.getBeansOfType(User.class);
      System.out.println(map);
  
  }
}
```

@EnableAutoConfiguration中使用的是第三种方式：@Import(AutoConfigurationImportSelector.class)

### 6、 SpringBoot自动配置-@EnableAutoConfiguration详解

面试题：springboot 自动配置原理？

![image-20220308152539680](https://www.ydlclass.com/doc21xnv/assets/image-20220308152539680.657155e8.png)

- @EnableAutoConfiguration 注解内部使用 @Import(AutoConfigurationImportSelector.**class**)来加载配置类。
- 配置文件位置：META-INF/spring.factories，该配置文件中定义了大量的配置类，当 SpringBoot 应用启动时，会自动加载这些配置类，初始化Bean
- 并不是所有的Bean都会被初始化，在配置类中使用Condition来加载满足条件的Bean

## 第二章 自定义starter -了解

### **1、SpringBoot自动配置-自定义starter步骤分析**

**需求：**自定义redis-starter。要求当导入redis-starter坐标时，SpringBoot自动创建Jedis的Bean。

**步骤：**

①创建 redis-spring-boot-autoconfigure 模块

②创建 redis-spring-boot-starter 模块,依赖 redis-spring-boot-autoconfigure的模块

③在 redis-spring-boot-autoconfigure 模块中初始化 Jedis 的 Bean。并定义META-INF/spring.factories 文件

④在测试模块中引入自定义的 redis-starter 依赖，测试获取 Jedis 的Bean，操作 redis。

### 2、 SpringBoot自动配置-自定义starter实现-1

1. **创建redis-spring-boot-starter工程**

 pom文件中引入redis-spring-boot-autoconfigure

```xml
 		<!--引入configure-->
        <dependency>
            <groupId>com.ydlclass</groupId>
            <artifactId>redis-spring-boot-autoconfigure</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
```

1. **创建redis-spring-boot-autoconfigure配置工程**

创建RedisProperties配置文件参数绑定类

```java
@ConfigurationProperties(prefix = "redis")
public class RedisProperties {

    private String host = "localhost";
    private int port = 6379;


    public String getHost() {
        return host;
    }

    public void setHost(String host) {
        this.host = host;
    }

    public int getPort() {
        return port;
    }

    public void setPort(int port) {
        this.port = port;
    }
}
```

创建RedisAutoConfiguration自动配置类

```java
@Configuration
@EnableConfigurationProperties(RedisProperties.class)
public class RedisAutoConfiguration {

    /**
     * 提供Jedis的bean
     */
    @Bean
    public Jedis jedis(RedisProperties redisProperties) {
        return new Jedis(redisProperties.getHost(), redisProperties.getPort());
    }
}
```

在resource目录下创建META-INF文件夹并创建spring.factories

注意：”\ “是换行使用的

```properties
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
  com.ydlclass.redis.config.RedisAutoConfiguration
```

1. **在springboot-enable工程中引入自定义的redis的starter**

```text
  <!--自定义的redis的starter-->
        <dependency>
            <groupId>com.ydlclass</groupId>
            <artifactId>redis-spring-boot-starter</artifactId>
            <version>0.0.1-SNAPSHOT</version>
        </dependency>
```

在SpringbootEnableApplication启动类中测试

```java
 Jedis jedis = context.getBean(Jedis.class);
 System.out.println(jedis);
```

### 3、 SpringBoot自动配置-自定义starter实现-2

测试springboot-enable工程中的application.properties中的配置参数

```properties
redis.port=6380
```

使用注解完成有条件加载配置类

```java
@Configuration
@EnableConfigurationProperties(RedisProperties.class)
@ConditionalOnClass(Jedis.class)
public class RedisAutoConfiguration {


    /**
     * 提供Jedis的bean
     */
    @Bean
    @ConditionalOnMissingBean(name = "jedis")
    public Jedis jedis(RedisProperties redisProperties) {
        System.out.println("RedisAutoConfiguration....");
        return new Jedis(redisProperties.getHost(), redisProperties.getPort());
    }
}
```

![image-20220308160934531](https://www.ydlclass.com/doc21xnv/assets/image-20220308160934531.fafad01b.png)

## 第三章 事件监听

### 1、 SpringBoot事件监听

Java中的事件监听机制定义了以下几个角色：

①事件：Event，继承 java.util.EventObject 类的对象

②事件源：Source ，任意对象Object

③监听器：Listener，实现 java.util.EventListener 接口 的对象

SpringBoot 在项目启动时，会对几个监听器进行回调，我们可以实现这些监听器接口，在项目启动时完成一些操作。

- ApplicationContextInitializer

- SpringApplicationRunListener

- CommandLineRunner

- ApplicationRunner

  自定义监听器的启动时机：MyApplicationRunner和MyCommandLineRunner都是当项目启动后执行，使用@Component放入容器即可使用

MyApplicationRunner

```java
/**
 * 当项目启动后执行run方法。
 */
@Component
public class MyApplicationRunner implements ApplicationRunner {
    @Override
    public void run(ApplicationArguments args) throws Exception {
        System.out.println("ApplicationRunner...run");
        System.out.println(Arrays.asList(args.getSourceArgs()));
    }
} 
```

MyCommandLineRunner

```java
@Component
public class MyCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... args) throws Exception {
        System.out.println("CommandLineRunner...run");
        System.out.println(Arrays.asList(args));
    }
}
```

MyApplicationContextInitializer的使用要在resource文件夹下添加META-INF/spring.factories

```properties
org.springframework.context.ApplicationContextInitializer=com.ydlclass.springbootlistener.listener.MyApplicationContextInitializer
```

```text
@Component
public class MyApplicationContextInitializer implements ApplicationContextInitializer {
    @Override
    public void initialize(ConfigurableApplicationContext applicationContext) {
        System.out.println("ApplicationContextInitializer....initialize");
    }
}
```

MySpringApplicationRunListener的使用要添加**构造器**

spring.factories加

```text
org.springframework.boot.SpringApplicationRunListener=com.ydlclass.springbootlistener.listener.MySpringApplicationRunListener
```

```java
public class MySpringApplicationRunListener implements SpringApplicationRunListener {

    public MySpringApplicationRunListener(SpringApplication application, String[] args) {
    }

    @Override
    public void starting() {
        System.out.println("starting...项目启动中");
    }

    @Override
    public void environmentPrepared(ConfigurableEnvironment environment) {
        System.out.println("environmentPrepared...环境对象开始准备");
    }

    @Override
    public void contextPrepared(ConfigurableApplicationContext context) {
        System.out.println("contextPrepared...上下文对象开始准备");
    }

    @Override
    public void contextLoaded(ConfigurableApplicationContext context) {
        System.out.println("contextLoaded...上下文对象开始加载");
    }

    @Override
    public void started(ConfigurableApplicationContext context) {
        System.out.println("started...上下文对象加载完成");
    }

    @Override
    public void running(ConfigurableApplicationContext context) {
        System.out.println("running...项目启动完成，开始运行");
    }

    @Override
    public void failed(ConfigurableApplicationContext context, Throwable exception) {
        System.out.println("failed...项目启动失败");
    }
}
```

## 第四章 SpringBoot启动流程

### 1、 SpringBoot流程分析-初始化

1. 配置启动引导类（判断是否有启动主类）
2. 判断是否是Web环境
3. 获取初始化类、监听器类

![1571369439416](https://www.ydlclass.com/doc21xnv/assets/1571369439416.e6fabc2c.png)

### [2、 SpringBoot流程分析-run

1. 启动计时器

2. 执行监听器

3. 准备环境

4. 打印banner：可以resource下粘贴自定义的banner

5. 创建context

   ```text
   refreshContext(context);
   ```

   执行refreshContext方法后才真正创建Bean

![1571373793325](https://www.ydlclass.com/doc21xnv/assets/1571373793325.e08dd78b.png)

## 第五章 监控-运维

### 1、 SpringBoot监控-actuator基本使用

①导入依赖坐标

```xml
<dependency>
  <groupId>org.springframework.boot</groupId>
  <artifactId>spring-boot-starter-actuator</artifactId>
</dependency>
```

②访问http://localhost:8080/acruator

```json
{
    "_links":{
        "self":{
            "href":"http://localhost:8080/actuator",
            "templated":false
        },
        "health":{
            "href":"http://localhost:8080/actuator/health",
            "templated":false
        },
        "health-component-instance":{
            "href":"http://localhost:8080/actuator/health/{component}/{instance}",
            "templated":true
        },
        "health-component":{
            "href":"http://localhost:8080/actuator/health/{component}",
            "templated":true
        },
        "info":{
            "href":"http://localhost:8080/actuator/info",
            "templated":false
        }
    }
}
```

http://localhost:8080/actuator/info

在application.properties中配置

```properties
info.name=lucy
info.age=99
```

http://localhost:8080/actuator/health

开启健康检查详细信息

```properties
management.endpoint.health.show-details=always
```

```json
{
    "status":"UP",
    "details":{
        "diskSpace":{
            "status":"UP",
            "details":{
                "total":159579508736,
                "free":13558104064,
                "threshold":10485760
            }
        },
        "redis":{
            "status":"UP",
            "details":{
                "version":"2.4.5"
            }
        }
    }
}
```

### 2、 SpringBoot监控-actuator开启所有endpoint

开启所有endpoint

在application.properties中配置：

```properties
management.endpoints.web.exposure.include=*
```

开启所有endpoint的返回结果：

```properties
{
    "_links":{
        "self":{
            "href":"http://localhost:8080/actuator",
            "templated":false
        },
        "auditevents":{
            "href":"http://localhost:8080/actuator/auditevents",
            "templated":false
        },
        "beans":{
            "href":"http://localhost:8080/actuator/beans",
            "templated":false
        },
        "caches-cache":{
            "href":"http://localhost:8080/actuator/caches/{cache}",
            "templated":true
        },
        "caches":{
            "href":"http://localhost:8080/actuator/caches",
            "templated":false
        },
        "health-component-instance":{
            "href":"http://localhost:8080/actuator/health/{component}/{instance}",
            "templated":true
        },
        "health":{
            "href":"http://localhost:8080/actuator/health",
            "templated":false
        },
        "health-component":{
            "href":"http://localhost:8080/actuator/health/{component}",
            "templated":true
        },
        "conditions":{
            "href":"http://localhost:8080/actuator/conditions",
            "templated":false
        },
        "configprops":{
            "href":"http://localhost:8080/actuator/configprops",
            "templated":false
        },
        "env":{
            "href":"http://localhost:8080/actuator/env",
            "templated":false
        },
        "env-toMatch":{
            "href":"http://localhost:8080/actuator/env/{toMatch}",
            "templated":true
        },
        "info":{
            "href":"http://localhost:8080/actuator/info",
            "templated":false
        },
        "loggers":{
            "href":"http://localhost:8080/actuator/loggers",
            "templated":false
        },
        "loggers-name":{
            "href":"http://localhost:8080/actuator/loggers/{name}",
            "templated":true
        },
        "heapdump":{
            "href":"http://localhost:8080/actuator/heapdump",
            "templated":false
        },
        "threaddump":{
            "href":"http://localhost:8080/actuator/threaddump",
            "templated":false
        },
        "metrics-requiredMetricName":{
            "href":"http://localhost:8080/actuator/metrics/{requiredMetricName}",
            "templated":true
        },
        "metrics":{
            "href":"http://localhost:8080/actuator/metrics",
            "templated":false
        },
        "scheduledtasks":{
            "href":"http://localhost:8080/actuator/scheduledtasks",
            "templated":false
        },
        "httptrace":{
            "href":"http://localhost:8080/actuator/httptrace",
            "templated":false
        },
        "mappings":{
            "href":"http://localhost:8080/actuator/mappings",
            "templated":false
        }
    }
}
```

### 3、 SpringBoot监控-springboot admin图形化界面使用

SpringBoot Admin 有两个角色，客户端(Client)和服务端(Server)。

![image-20220308171618412](https://www.ydlclass.com/doc21xnv/assets/image-20220308171618412.ba2b6c37.png)

以下为创建服务端和客户端工程步骤：

**admin-server：**

①创建 admin-server 模块

②导入依赖坐标 admin-starter-server

![image-20220308171740452](https://www.ydlclass.com/doc21xnv/assets/image-20220308171740452.14ff260c.png)

```xml
      <dependency>
            <groupId>de.codecentric</groupId>
            <artifactId>spring-boot-admin-starter-server</artifactId>
        </dependency>
```

③在引导类上启用监控功能@EnableAdminServer

```java
@EnableAdminServer
@SpringBootApplication
public class SpringbootAdminServerApplication {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootAdminServerApplication.class, args);
    }

}
```

**admin-client：**

①创建 admin-client 模块

②导入依赖坐标 admin-starter-client

```xml
  		<dependency>
            <groupId>de.codecentric</groupId>
            <artifactId>spring-boot-admin-starter-client</artifactId>
        </dependency>
```

③配置相关信息：server地址等

```properties
# 执行admin.server地址
spring.boot.admin.client.url=http://localhost:9000

management.endpoint.health.show-details=always
management.endpoints.web.exposure.include=*
```

④启动server和client服务，访问server

![image-20220308172552987](https://www.ydlclass.com/doc21xnv/assets/image-20220308172552987.fb0e8a06.png)

## 第六章 SpringBoot部署

SpringBoot 项目开发完毕后，支持两种方式部署到服务器：

①jar包(官方推荐)

②war包

**更改pom文件中的打包方式为war**

```text
<packaging>war</packaging>
```

修改启动类

```java
import org.springframework.boot.SpringApplication;
import org.springframework.boot.autoconfigure.SpringBootApplication;
import org.springframework.boot.builder.SpringApplicationBuilder;
import org.springframework.boot.web.servlet.support.SpringBootServletInitializer;

@SpringBootApplication
public class SpringbootDeployApplication extends SpringBootServletInitializer {

    public static void main(String[] args) {
        SpringApplication.run(SpringbootDeployApplication.class, args);
    }


    @Override
    protected SpringApplicationBuilder configure(SpringApplicationBuilder builder) {
        return builder.sources(SpringbootDeployApplication.class);
    }
}
```

指定打包的名称

```xml
   <build>
        <finalName>springboot</finalName>
        <plugins>
            <plugin>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-maven-plugin</artifactId>
            </plugin>
        </plugins>
    </build>
```