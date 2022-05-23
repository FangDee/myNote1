# [Spring框架](https://jshand.gitee.io/#/course/12_spring/spring?id=spring框架)

# [1. Spring概述](https://jshand.gitee.io/#/course/12_spring/spring?id=_1-spring概述)

官网： https://spring.io/

Lean: https://spring.io/projects/spring-framework#learn

User Guide ：https://docs.spring.io/spring/docs/5.2.4.RELEASE/spring-framework-reference/

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B0380.png)

## [1.1. 介绍](https://jshand.gitee.io/#/course/12_spring/spring?id=_11-介绍)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B0385.png)

## [1.2. 特点](https://jshand.gitee.io/#/course/12_spring/spring?id=_12-特点)

## [1.3. 优势](https://jshand.gitee.io/#/course/12_spring/spring?id=_13-优势)

Spring可以称之为J2EE比较流行的规范。

## [1.4. 版本变更](https://jshand.gitee.io/#/course/12_spring/spring?id=_14-版本变更)

从1.x到现在的5.x 上课使用 5.2.4

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B0447.png)

## [1.5. 架构](https://jshand.gitee.io/#/course/12_spring/spring?id=_15-架构)

1、核心容器由spring-beans、spring-core、spring-context 和 spring-expression（Spring Expression Language, SpEL） 4 个模块组成。

spring-beans 和 spring-core 模块是 Spring 框架的核心模块，包含了控制反转（Inversion of Control, IOC）和依赖注入（Dependency Injection, DI）。

spring-context 模块构架于核心模块之上，他扩展了 BeanFactory，为她添加了 Bean 生命周期 控制、框架事件体系以及资源加载透明化等功能。此外该模块还提供了许多企业级支持，如邮件访问、 远程访问、任务调度等。

spring-expression 模块是统一表达式语言（EL）的扩展模块，可以查询、管理运行中的对象， 同时也方便的可以调用对象方法、操作数组、集合等。

2、AOP和设备支持由spring-aop、spring-aspects 和 spring-instrument 3 个模块组成。

spring-aop 是 Spring 的另一个核心模块，是 AOP 主要的实现模块。

spring-aspects 模块集成自 AspectJ 框架，主要是为 Spring AOP 提供多种 AOP实现方法。

spring-instrument 模块是基于 JAVA SE 中的“java.lang.instrument”进行设计的，应该算是AOP 的一个支援模块，主要作用是在 JVM 启用时，生成一个代理类，程序员通过代理类在运行时修改类的字节，从而改变一个类的功能，实现 AOP 的功能。

3、数据访问及集成：由spring-jdbc、spring-tx、spring-orm、spring-jms 和 spring-oxm 5 个模块组成。

spring-jdbc 模块是 Spring 提供的 JDBC 抽象框架的主要实现模块，用于简化 Spring JDBC。

spring-tx 模块是 Spring JDBC 事务控制实现模块。

spring-orm 模块是 ORM 框架支持模块，主要集成 Hibernate, Java Persistence API (JPA) 和Java Data Objects (JDO) 用于资源管理、数据访问对象(DAO)的实现和事务策略。

spring-jms 模块（Java Messaging Service）能够发送和接受信息，自 Spring Framework 4.1 以后，他还提供了对 spring-messaging 模块的支撑。

spring-oxm 模块主要提供一个抽象层以支撑 OXM（OXM 是 Object-to-XML-Mapping 的缩写，它是一个 O/M-mapper，将 java 对象映射成 XML 数据，或者将 XML 数据映射成 java 对象），例如：JAXB,Castor, XMLBeans, JiBX 和 XStream 等。

4、Web由 spring-web、spring-webmvc、spring-websocket 和 spring-webflux 4 个模块组成。

spring-web 模块为 Spring 提供了最基础 Web 支持，主要建立于核心容器之上，通过 Servlet 或 者 Listeners 来初始化 IOC 容器，也包含一些与 Web 相关的支持。

spring-webmvc模块为web应用提供了模型视图控制（MVC）和REST Web服务的实现。Spring的MVC框架可以使领域模型代码和web表单完全地分离，且可以与Spring框架的其它所有功能进行集成。

spring-websocket 模块为WebSocket-based 提供了支持，而且在 web 应用程序中提供了客户端和服务器端之间通信的两种方式。

spring-webflux 是一个新的非堵塞函数式 Reactive Web 框架，可以用来建立异步的，非阻塞，事件驱动的服务，并且扩展性非常好。

5、报文发送：即 spring-messaging 模块。

spring-messaging 是从 Spring4 开始新加入的一个模块，主要职责是为 Spring 框架集成一些基础的报文传送应用。

6、Test：即 spring-test 模块。

spring-test 模块主要为测试提供支持。

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B02342.png)

## [1.6. 依赖关系](https://jshand.gitee.io/#/course/12_spring/spring?id=_16-依赖关系)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B02350.png)

# [2. Spring入门程序](https://jshand.gitee.io/#/course/12_spring/spring?id=_2-spring入门程序)

Spring中的Bean：使用容器管理的对象统一称之为Bean

## [2.1. 类与类的依赖关系](https://jshand.gitee.io/#/course/12_spring/spring?id=_21-类与类的依赖关系)

IUserDao，dao层的接口类

List selectUsers();

UserDaoImpl，dao层的实现类

List selectUsers(){

}

IUserService 定义一个方法

List queryUsers();

UserServiceImpl 实现类,依赖（属性）IUserDao

List queryUsers(){

调用Dao层的方法

}

## [2.2. 创建工程](https://jshand.gitee.io/#/course/12_spring/spring?id=_22-创建工程)

### [2.2.1. Spring的工程(父工程)](https://jshand.gitee.io/#/course/12_spring/spring?id=_221-spring的工程父工程)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B02616.png)

### [2.2.2. Spring-01-hello(子项目)](https://jshand.gitee.io/#/course/12_spring/spring?id=_222-spring-01-hello子项目)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B02639.png)

### [2.2.3. 上述四个类实现（代码创建完）](https://jshand.gitee.io/#/course/12_spring/spring?id=_223-上述四个类实现（代码创建完）)

#### [2.2.3.1. IUserDao 接口](https://jshand.gitee.io/#/course/12_spring/spring?id=_2231-iuserdao-接口)

```java
/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/19  16:18 19
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : dao层接口
 */
public interface IUserDao {
    List selectUser();
}
```

#### [2.2.3.2. UserDaoImpl 实现类](https://jshand.gitee.io/#/course/12_spring/spring?id=_2232-userdaoimpl-实现类)

```java
import java.util.List;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/19  16:18 19
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : dao层的实现类
 */
public class UserDaoImpl implements IUserDao {
    @Override
    public List selectUser() {
        System.out.println("执行dao层的方法......");
        return null;
    }
}
```

#### [2.2.3.3. IUserService 接口](https://jshand.gitee.io/#/course/12_spring/spring?id=_2233-iuserservice-接口)

```java
import java.util.List;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/19  16:19 19
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : Service层接口
 */
public interface IUserService {

    List queryUser();
}
```

#### [2.2.3.4. UserServiceImpl 实现类](https://jshand.gitee.io/#/course/12_spring/spring?id=_2234-userserviceimpl-实现类)

```java
import com.neuedu.dao.IUserDao;

import java.util.List;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/19  16:20 19
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : service层的实现类
 */
public class UserServiceImpl implements  IUserService {

    IUserDao userDao ;


public UserServiceImpl(IUserDao userDao) {
    this.userDao = userDao;
}




    @Override
    public List queryUser() {
        userDao.selectUser();
        return null;
    }
}
```

## [2.3. 依赖Spring的类库](https://jshand.gitee.io/#/course/12_spring/spring?id=_23-依赖spring的类库)

```xml
<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>


<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-expression</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>
```

## [2.4. 由Spirng维护依赖关系](https://jshand.gitee.io/#/course/12_spring/spring?id=_24-由spirng维护依赖关系)

### [2.4.1. 使用java代码的形式维护类的创建和依赖](https://jshand.gitee.io/#/course/12_spring/spring?id=_241-使用java代码的形式维护类的创建和依赖)

#### [2.4.1.1. AppConfig](https://jshand.gitee.io/#/course/12_spring/spring?id=_2411-appconfig)

```java
@Configuration //让当前类作为配置类存在（管理bean对象）
public class AppConfig {

    //管理Dao的创建
    @Bean
    public IUserDao getDao(){
       return  new UserDaoImpl();
    }

    //管理service的创建
    @Bean
    public IUserService getService(){
        return  new UserServiceImpl();
    }
}
```

#### [2.4.1.2. 单元测试](https://jshand.gitee.io/#/course/12_spring/spring?id=_2412-单元测试)

```java
/**
 * Unit test for simple App.
 */
public class AppTest {

    @Test
    public void test1(){
        //1 获取工厂
        ApplicationContext context = new AnnotationConfigApplicationContext(AppConfig.class);

        //由工厂创建Service、Dao
        //测试从工厂获取Dao
        IUserDao userDao = context.getBean(IUserDao.class);
        //userDao.selectUser();//从工厂中获取到 dao对象

        //测试从工厂获取service
        IUserService userService = context.getBean(IUserService.class);
        userService.queryUser();//从工厂中获取到 dao对象
    }
}
```

### [2.4.2. 使用xml的形式维护类的创建和依赖](https://jshand.gitee.io/#/course/12_spring/spring?id=_242-使用xml的形式维护类的创建和依赖)

#### [2.4.2.1. 准备xml配置Service和Dao的申明依赖](https://jshand.gitee.io/#/course/12_spring/spring?id=_2421-准备xml配置service和dao的申明依赖)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 使用bean标签声明bean对象

        id : 在容器中的名字，不能重复
        class : 对象的类型
    -->
    <bean id="userDao" class="com.neuedu.dao.UserDaoImpl">
    </bean>

    <bean id="userService" class="com.neuedu.service.UserServiceImpl">
        <!-- service 依赖（需要）userDao，使用构造器的方法传递进入
            constructor-arg
                name属性 构造器中的参数名
                value：给参数赋值（常量）
                ref: 引用容器中的 其他bean的id
        -->
        <constructor-arg name="userDao" ref="userDao" ></constructor-arg>
    </bean>

</beans>
```

#### [2.4.2.2. 单元测试](https://jshand.gitee.io/#/course/12_spring/spring?id=_2422-单元测试)

```java
  /**
     * 使用xml的方式管理bean的创建
     */
    @Test
    public void test2(){
        //1 获取工厂context (容器)
        String configLocation = "spring-beans.xml";
        ApplicationContext context = new ClassPathXmlApplicationContext(configLocation);

        //由工厂创建Service、Dao
        //测试从工厂获取Dao
//        IUserDao userDao = context.getBean(IUserDao.class);
//        userDao.selectUser();//从工厂中获取到 dao对象

        //测试从工厂获取service
        IUserService userService = context.getBean(IUserService.class);
        userService.queryUser();//从工厂中获取到 service对象

    }
```

### [2.4.3. 使用注解扫描的形式维护类的创建和依赖](https://jshand.gitee.io/#/course/12_spring/spring?id=_243-使用注解扫描的形式维护类的创建和依赖)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B07254.png)

#### [2.4.3.1. 在需要使用容器管理的类上声明注解](https://jshand.gitee.io/#/course/12_spring/spring?id=_2431-在需要使用容器管理的类上声明注解)

@Component代表需要让容器给我创建对象，spring额外提供了3个同样功能的注解

@Service、@Repository、@Controller作用一样，只是从语义上进行区分。

- dao

```java
import org.springframework.stereotype.Component;

import java.util.List;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/19  16:18 19
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : dao层的实现类
 */
@Component
public class UserDaoImpl implements IUserDao {
    @Override
    public List selectUser() {
        System.out.println("执行dao层的方法......");
        return null;
    }
}
```

- Service

```java
import com.neuedu.dao.IUserDao;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.stereotype.Component;

import java.util.List;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/19  16:20 19
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : service层的实现类
 */

@Component
public class UserServiceImpl implements  IUserService {

    @Autowired // 自动的将容器中IUserDao的类注入到 userDao属性
    IUserDao userDao ;

    //声明无参的构造器
    public UserServiceImpl(){
    }


    public UserServiceImpl(IUserDao userDao) {
        this.userDao = userDao;
    }

    @Override
    public List queryUser() {
        System.out.println("执行serive层的方法");
        userDao.selectUser();
        return null;
    }
}
```

#### [2.4.3.2. 在xml中声明需要扫描的包](https://jshand.gitee.io/#/course/12_spring/spring?id=_2432-在xml中声明需要扫描的包)

创建spring-beans-annotation.xml配置文件

使用 <context:component-scan base-package="com.neuedu"/>进行扫描

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
      xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
      xmlns:context="http://www.springframework.org/schema/context"
      xsi:schemaLocation="http://www.springframework.org/schema/beans
       https://www.springframework.org/schema/beans/spring-beans.xsd
       http://www.springframework.org/schema/context
       https://www.springframework.org/schema/context/spring-context.xsd">

   <!--指定包进行扫描 -->
  <context:component-scan base-package="com.neuedu.dao,com.neuedu.service"/>
</beans>
```

#### [2.4.3.3. 单元测试](https://jshand.gitee.io/#/course/12_spring/spring?id=_2433-单元测试)

```java
/**
     * 使用注解扫描的形式的方式管理bean的创建
     */
    @Test
    public void test3(){
        //1 获取工厂context (容器)
        String configLocation = "spring-beans-annotation.xml";
        ApplicationContext context = new ClassPathXmlApplicationContext(configLocation);

        //由工厂创建Service、Dao
        //测试从工厂获取Dao
//        IUserDao userDao = context.getBean(IUserDao.class);
//        userDao.selectUser();//从工厂中获取到 dao对象

        //测试从工厂获取service
        IUserService userService = context.getBean(IUserService.class);
        userService.queryUser();//从工厂中获取到 service对象

    }
```

## [2.5. 整合Junit单元测试](https://jshand.gitee.io/#/course/12_spring/spring?id=_25-整合junit单元测试)

### [2.5.1. 1修改junit的库变为4.12+](https://jshand.gitee.io/#/course/12_spring/spring?id=_251-1修改junit的库变为412)

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
```

### [2.5.2. 1添加Spring的test模块](https://jshand.gitee.io/#/course/12_spring/spring?id=_252-1添加spring的test模块)

```xml
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>
```

#### [2.5.2.1. 单元测试](https://jshand.gitee.io/#/course/12_spring/spring?id=_2521-单元测试)

```java
/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/20  10:06 20
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : junit集成Spring的测试库
 */
@RunWith(SpringJUnit4ClassRunner.class) //对junit的扩展
@ContextConfiguration(locations="classpath:spring-beans-annotation.xml")
public class SpringTest {


    @Autowired
    IUserService userService;

    @Test
    public void test(){
        userService.queryUser();
    }
} 
```

注意，以后的测试SpringJunit4ClassRunner ,但是底层还是ApplicationContext。

# [3. IOC容器和Bean的装填](https://jshand.gitee.io/#/course/12_spring/spring?id=_3-ioc容器和bean的装填)

- IOC:控制翻转,

简单说由原来的自己控制对象的创建等过程，现在交给ioc容器进行创建对象。

AOP、事务的都是依赖于IOC。

- DI：(Dependency Injection)依赖注入：

比如service依赖dao（没有实例化的），如果设置一个实例化的dao对象过程（方式），称之为DI

## [3.1. BeanFactory与ApplicationContext接口](https://jshand.gitee.io/#/course/12_spring/spring?id=_31-beanfactory与applicationcontext接口)

都是IOC容器,ApplicationContext是对BeanFactory的扩展，有两种子类型

1. ClassPathXMLApplicationContext：

 从类路径中查找配置文件

1. FileSystemXMLApplicationContext

 从文件系统中查找配置文件

BeanFactory与ApplicationContext区别：

BeanFactory，在使用的时候创建对象.如果配置出错问题会滞后报出。

ApplicationContext，在启动的时候创建的对象,能够在执行之初暴露一些问题。

## [3.2. 依赖注入(DI)](https://jshand.gitee.io/#/course/12_spring/spring?id=_32-依赖注入di)

Spring提供了两种实现方案构造函数的注入，属性的注入

### [3.2.1. 创建一个新工程（quickstart）](https://jshand.gitee.io/#/course/12_spring/spring?id=_321-创建一个新工程（quickstart）)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B011173.png)

### [3.2.2. 四个类实现（代码创建完）](https://jshand.gitee.io/#/course/12_spring/spring?id=_322-四个类实现（代码创建完）)

#### [3.2.2.1. IUserDao 接口](https://jshand.gitee.io/#/course/12_spring/spring?id=_3221-iuserdao-接口)

```java
/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/19  16:18 19
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : dao层接口
 */
public interface IUserDao {
    List selectUser();
} 
```

#### [3.2.2.2. UserDaoImpl 实现类](https://jshand.gitee.io/#/course/12_spring/spring?id=_3222-userdaoimpl-实现类)

```java
import java.util.List;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/19  16:18 19
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : dao层的实现类
 */
public class UserDaoImpl implements IUserDao {
    @Override
    public List selectUser() {
        System.out.println("执行dao层的方法......");
        return null;
    }
}
```

#### [3.2.2.3. IUserService 接口](https://jshand.gitee.io/#/course/12_spring/spring?id=_3223-iuserservice-接口)

```java
import java.util.List;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/19  16:19 19
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : Service层接口
 */
public interface IUserService {

    List queryUser();
} 
```

#### [3.2.2.4. UserServiceImpl 实现类](https://jshand.gitee.io/#/course/12_spring/spring?id=_3224-userserviceimpl-实现类)

```java
import com.neuedu.dao.IUserDao;

import java.util.List;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/19  16:20 19
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : service层的实现类
 */
public class UserServiceImpl implements  IUserService {

    IUserDao userDao ;


public UserServiceImpl(IUserDao userDao) {
    this.userDao = userDao;
}




    @Override
    public List queryUser() {
        userDao.selectUser();
        return null;
    }
}
```

## [3.3. 依赖Spring的类库](https://jshand.gitee.io/#/course/12_spring/spring?id=_33-依赖spring的类库)

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>



<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>


<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-expression</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency> 
```

## [3.4. 在xml中声明需要管理的Bean](https://jshand.gitee.io/#/course/12_spring/spring?id=_34-在xml中声明需要管理的bean)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="userDao" class="com.neuedu.dao.UserDaoImpl">
    </bean>

    <bean id="userService" class="com.neuedu.service.UserServiceImpl">
    </bean>

</beans>
```

### [3.4.1. 构造函数的注入](https://jshand.gitee.io/#/course/12_spring/spring?id=_341-构造函数的注入)

#### [3.4.1.1. 声明带参数的构造器](https://jshand.gitee.io/#/course/12_spring/spring?id=_3411-声明带参数的构造器)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B014291.png)

#### [3.4.1.2. 使用constructor-arg标签调用带参的构造器注入](https://jshand.gitee.io/#/course/12_spring/spring?id=_3412-使用constructor-arg标签调用带参的构造器注入)

Name ：构造器中参数的名字

Index：构造器中参数的位置 从0开始

Value代表注入的值（常量类型）

Ref：注入的引用对象的id或者name![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B014398.png)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B014400.png)

### [3.4.2. 属性注入](https://jshand.gitee.io/#/course/12_spring/spring?id=_342-属性注入)

需求：UserController依赖于UserService

#### [3.4.2.1. UserCongtroller](https://jshand.gitee.io/#/course/12_spring/spring?id=_3421-usercongtroller)

```java
import com.neuedu.service.IUserService;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/20  13:52 20
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : 控制层的代码（模拟）
 */
public class UserController {

    //1 声明属性
    private IUserService userService;
    private String address;

    //2 设置setter 和getter 方法
    public IUserService getUserService() {
        return userService;
    }

    public void setUserService(IUserService userService) {
        this.userService = userService;
    }

    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public void queryUsers(){
        System.out.println("调用Controller.."+address);
        userService.queryUser();
    }
}
```

#### [3.4.2.2. Xml中Bean的声明](https://jshand.gitee.io/#/course/12_spring/spring?id=_3422-xml中bean的声明)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <!--声明Dao层-->
    <bean id="userDao"  class="com.neuedu.dao.UserDaoImpl">
    </bean>

    <!--声明Service层-->
    <bean id="userService" class="com.neuedu.service.UserServiceImpl">
        <constructor-arg name="userDao" ref="userDao"/>
        <constructor-arg name="age" value="50"/>
<!--        <constructor-arg index="0" ref="userDao"/>-->
<!--        <constructor-arg index="1" value="30"/>-->
    </bean>

    <!--声明Controller层
        property 向属性中注入
            name : 属性的名字
            ref:   注入的bean对象的id
            value ： 常量值
    -->
    <bean id="userController"  class="com.neuedu.controller.UserController">
        <property name="userService"  ref="userService" />
        <property name="address"  value="黑龙江哈尔滨市" />
    </bean>

</beans>
```

## [3.5. 依赖注入的参数类型](https://jshand.gitee.io/#/course/12_spring/spring?id=_35-依赖注入的参数类型)

1 ) 常量值 字面量（字符串、数字、布尔类型等）

2）引用类型

3）集合类型List\Set、Properties、Map

### [3.5.1. Bean的Java代码](https://jshand.gitee.io/#/course/12_spring/spring?id=_351-bean的java代码)

```java
package com.neuedu;

import com.neuedu.controller.UserController;

import java.util.List;
import java.util.Map;
import java.util.Properties;
import java.util.Set;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/20  14:16 20
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : 声明一个普通的bean
 */
public class Person {
    //字面量的类型
    private String address;
    private Integer age;

    //引用类型
    private UserController userController;

    //集合类型
    private List<String> names;
    private Set<Integer> ages;
    private Map<String,String> stus;

    private Properties props;

    public void testDI(){
        System.out.println("address:"+address);
        System.out.println("age:"+age);
       // System.out.println("address:"+address);

        System.out.println("names:"+names);
        System.out.println("ages:"+ages);
        System.out.println("stus:"+stus);
        System.out.println("props:"+props);

        userController.queryUsers();
    }


    public String getAddress() {
        return address;
    }

    public void setAddress(String address) {
        this.address = address;
    }

    public Integer getAge() {
        return age;
    }

    public void setAge(Integer age) {
        this.age = age;
    }

    public UserController getUserController() {
        return userController;
    }

    public void setUserController(UserController userController) {
        this.userController = userController;
    }

    public List<String> getNames() {
        return names;
    }

    public void setNames(List<String> names) {
        this.names = names;
    }

    public Set<Integer> getAges() {
        return ages;
    }

    public void setAges(Set<Integer> ages) {
        this.ages = ages;
    }

    public Map<String, String> getStus() {
        return stus;
    }

    public void setStus(Map<String, String> stus) {
        this.stus = stus;
    }

    public Properties getProps() {
        return props;
    }

    public void setProps(Properties props) {
        this.props = props;
    }
}
```

### [3.5.2. Xml注入](https://jshand.gitee.io/#/course/12_spring/spring?id=_352-xml注入)

```xml
<!--声明bean对象-->
<bean id="person" class="com.neuedu.Person">

    <property name="address" value="南岗区" >  </property>
    <property name="age" value="30" >  </property>
    <property name="userController" ref="userController" >  </property>

    <property name="names" >
        <list>
            <value>张三</value>
            <value>李四</value>
            <value>王五</value>
            <value>张三</value>
        </list>
    </property>

    <property name="ages" >
        <set>
            <value>30</value>
            <value>32</value>
            <value>30</value>
        </set>
    </property>

    <property name="stus" >
        <map>
            <entry>
                <key><value>key00001</value></key>
                <value>val0001</value>
            </entry>
            <entry>
                <key><value>key00002</value></key>
                <value>val0002</value>
            </entry>
        </map>
    </property>

    <property name="props" >
        <props>
            <prop key="proKey0001" >proVal0001</prop>
            <prop key="proKey0002" >proVal0002</prop>
        </props>
    </property>

</bean>
```

### [3.5.3. 单元测试](https://jshand.gitee.io/#/course/12_spring/spring?id=_353-单元测试)

```java
import static org.junit.Assert.assertTrue;

import com.neuedu.controller.UserController;
import com.neuedu.service.IUserService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

/**
 * Unit test for simple App.
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:spring-beans.xml")
public class AppTest
{
    /**
     * Rigorous Test :-)
     */

    @Autowired
    IUserService userService;
    @Test
    public void test1() {

        userService.queryUser();

    }




    @Autowired
    UserController userController;
    @Test
    public void test2() {
        userController.queryUsers();
    }


    /**
     * 测试各种数据类型的 注入
     */
    @Autowired
    Person person;
    @Test
    public void test3() {
        person.testDI();
    }

}
```

## [3.6. 自动注入](https://jshand.gitee.io/#/course/12_spring/spring?id=_36-自动注入)

> 配置自动注入的方式

　　配置自动注入的方式有两种，一种是全局配置，另一种是局部单独配置。

　　全局配置：只配置一次，之后配置文件中的所有bean，都按照全局配置进行注入，全局配置是在标签中配置default-autowire="Xxx"；

　　局部单独配置：对于每一个bean，单独设置注入方式，单独配置是在单独的标签中配置autowire="xxx"。

对于全局配置和局部单独配置，都有5个值可以选择：

| Mode          | Explanation                                                  |
| ------------- | ------------------------------------------------------------ |
| `no`          | (Default) No autowiring. Bean references must be defined by `ref` elements. Changing the default setting is not recommended for larger deployments, because specifying collaborators explicitly gives greater control and clarity. To some extent, it documents the structure of a system. |
| `byName`      | Autowiring by property name. Spring looks for a bean with the same name as the property that needs to be autowired. For example, if a bean definition is set to autowire by name and it contains a `master` property (that is, it has a `setMaster(..)` method), Spring looks for a bean definition named `master` and uses it to set the property. |
| `byType`      | Lets a property be autowired if exactly one bean of the property type exists in the container. If more than one exists, a fatal exception is thrown, which indicates that you may not use `byType` autowiring for that bean. If there are no matching beans, nothing happens (the property is not set). |
| `constructor` | Analogous to `byType` but applies to constructor arguments. If there is not exactly one bean of the constructor argument type in the container, a fatal error is raised. |

### [3.6.1. Java代码](https://jshand.gitee.io/#/course/12_spring/spring?id=_361-java代码)

```java
import com.neuedu.controller.UserController;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/20  14:44 20
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     :
 */
public class MyApplication {

    private UserController userController;

    public UserController getUserController() {
        return userController;
    }

    public void setUserController(UserController userController) {
        this.userController = userController;
    }


    public void testController(){
        System.out.println(userController);
    }
}
```

### [3.6.2. Xml的声明](https://jshand.gitee.io/#/course/12_spring/spring?id=_362-xml的声明)

使用autowire属性根据名字、类型、构造器自动的注入。

```xml
<!--autowire 自动注入：
    byName：查找相同的 bean的id和属性名相同的，自动注入
    byType：查找bean类型 和属性类型相同的自动注入-->
<bean id="myApp" class="com.neuedu.MyApplication" autowire="byType">
</bean>
```

跟标签beans：default-autowire="no"，可以指定装填的方式，在生产中保持不变，使用注解扫描的形式bean，通过@Autowired注解自动的装填,@Resource默认按照类型装填，可以指定name或者type。

## [3.7. 作用域](https://jshand.gitee.io/#/course/12_spring/spring?id=_37-作用域)

自定义一个MyBean的类(空)

Spring在声明Bean可以指定Scope用于设置Bean的作用域

### [3.7.1. Singleton(默认)](https://jshand.gitee.io/#/course/12_spring/spring?id=_371-singleton默认)

单例模式的对象，全局唯一.

### [3.7.2. prototype](https://jshand.gitee.io/#/course/12_spring/spring?id=_372-prototype)

prototype:原型模式，每次从容器获取bean时，容器都将创建一个新的Bean实例。

### [3.7.3. request](https://jshand.gitee.io/#/course/12_spring/spring?id=_373-request)

request：每一次Http请求都会创建一个新的Bean，仅在当前Http Request内有效。

### [3.7.4. session](https://jshand.gitee.io/#/course/12_spring/spring?id=_374-session)

session：同一个Http Session共享一个Bean,不同的Session请求则会创建新的实例，该bean实例仅在当前Session内有效。

### [3.7.5. session](https://jshand.gitee.io/#/course/12_spring/spring?id=_375-session)

global Session：在一个全局的Http Session中，容器会返回该Bean的同一个实例，仅适用于Portlet应用环境。

## [3.8. 整合多文件](https://jshand.gitee.io/#/course/12_spring/spring?id=_38-整合多文件)

### [3.8.1. Spirng-service.xml](https://jshand.gitee.io/#/course/12_spring/spring?id=_381-spirng-servicexml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd"
>
   <!--模拟一大堆的service -->
   <bean id="userService" class="com.neuedu.service.UserService"/>


</beans>
```

### [3.8.2. UserService](https://jshand.gitee.io/#/course/12_spring/spring?id=_382-userservice)

```java
/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/23  10:02 23
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : service层
 */
public class UserService {
}
```

### [3.8.3. Spring-controller.xml](https://jshand.gitee.io/#/course/12_spring/spring?id=_383-spring-controllerxml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd"
>
    <!-- 模拟一堆的 Controller-->
   <bean id="userController" class="com.neuedu.controller.UserController" />

</beans>
```

### [3.8.4. UserController 依赖service](https://jshand.gitee.io/#/course/12_spring/spring?id=_384-usercontroller-依赖service)

```java
/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/23  10:02 23
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : 控制器
 */
public class UserController {
}
```

### [3.8.5. 整合后配合文件](https://jshand.gitee.io/#/course/12_spring/spring?id=_385-整合后配合文件)

通过import引入其他配置文件。

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd"
       >
       <!--包含子配置文件-->
       <import resource="spring-controller.xml"/>
       <import resource="spring-service.xml"/>

       <bean id="myBean" class="com.neuedu.MyBean" scope="singleton"/>

       <bean id="myBean2" class="com.neuedu.MyBean" scope="prototype"/>
</beans>
```

## [3.9. 使用注解声明Bean及自动注入](https://jshand.gitee.io/#/course/12_spring/spring?id=_39-使用注解声明bean及自动注入)

声明Bean的注解

> @Component

> @Controlle

> @Service

> @Repository

自动注入的注解

@Autowired 不需要声明setter、getter方法

@Qualifier 注解用于获取同类型有多个的情况，用于区分Bean的名字

@Scope(value="prototype") 用于指定Bean的作用域

@Lazy让类的加载（初始化延迟，需要用到属性的时在加载）在属性、类上面同时声明

### [3.9.1. 定义Bean](https://jshand.gitee.io/#/course/12_spring/spring?id=_391-定义bean)

#### [3.9.1.1. IAccountService 接口](https://jshand.gitee.io/#/course/12_spring/spring?id=_3911-iaccountservice-接口)

```java
/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/23  10:59 23
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     :
 */
public interface IAccountService {
}
```

#### [3.9.1.2. AccountServiceImpl 实现类](https://jshand.gitee.io/#/course/12_spring/spring?id=_3912-accountserviceimpl-实现类)

```java
/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/23  10:59 23
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     :
 */
//@Component("accountServiceImpl")
@Component
public class AccountServiceImpl implements IAccountService {
}
```

#### [3.9.1.3. AccountServiceImpl2实现类](https://jshand.gitee.io/#/course/12_spring/spring?id=_3913-accountserviceimpl2实现类)

```java
import org.springframework.stereotype.Component;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/23  10:59 23
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     :
 */
@Component
public class AccountServiceImpl2 implements IAccountService {
}
```

#### [3.9.1.4. AccountController （依赖Service）](https://jshand.gitee.io/#/course/12_spring/spring?id=_3914-accountcontroller-（依赖service）)

```java
import com.neuedu.service.IAccountService;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.beans.factory.annotation.Qualifier;
import org.springframework.stereotype.Component;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/23  10:33 23
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     :
 */
@Component
public class AccountController {


    @Autowired  //按照类型匹配
    @Qualifier("accountServiceImpl2")
    private IAccountService iAccountService;

    public void testService(){
        System.out.println(iAccountService);
    }

} 
```

### [3.9.2. 使用xml声明扫描的包](https://jshand.gitee.io/#/course/12_spring/spring?id=_392-使用xml声明扫描的包)

使用context标签指定扫描的包

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/context
        https://www.springframework.org/schema/context/spring-context.xsd"
>


       <!--使用scan 进行扫描Bean-->
       <context:component-scan
               base-package="com.neuedu.service,com.neuedu.controller"/>

</beans>
```

### [3.9.3. 使用注解声明类为IOC中Bean](https://jshand.gitee.io/#/course/12_spring/spring?id=_393-使用注解声明类为ioc中bean)

如果出现多个同类型的Bean @AutoWried是默认按照类型匹配的，需要通过Qualifier注解指定名字

### [3.9.4. 单元测试](https://jshand.gitee.io/#/course/12_spring/spring?id=_394-单元测试)

```
@Test
    public void test3(){
        String config = "spring-annotation.xml";
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext(config);
        AccountController accountController = applicationContext.getBean(AccountController.class);
//        AccountService accountService = applicationContext.getBean(AccountService.class);

        System.out.println(accountController);
//        System.out.println(accountService);
        accountController.testService();//测试Controller是否依赖成功 service

    }
```

## [3.10. 使用Java类的形式进行配置Bean并自动注入](https://jshand.gitee.io/#/course/12_spring/spring?id=_310-使用java类的形式进行配置bean并自动注入)

1）在配置类上申明@Configuration注解，

2）在需要创建Bean的方法上声明@Bean注解

3）使用AnnotationConfigApplicationContext类初始化IOC容器

4）使用@import注解引入其他配置类。

### [3.10.1. 主配置类JavaConfig](https://jshand.gitee.io/#/course/12_spring/spring?id=_3101-主配置类javaconfig)

```java
import com.neuedu.MyBean;
import com.neuedu.controller.UserController;
import com.neuedu.service.UserService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;
import org.springframework.context.annotation.Import;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/23  11:29 23
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     :
 */
@Configuration
@Import(OtherConfig.class)
public class JavaConfig {

    @Bean
    public UserService createService(){
        return new UserService();
    }


    @Bean
    public UserController createController(UserService userService){
        System.out.println("bean工厂 声明Controller时，注入的 "+userService);//
        return new UserController();
    }

    public static void main(String[] args) {
        ApplicationContext context = new AnnotationConfigApplicationContext(JavaConfig.class);
        UserController userController = context.getBean(UserController.class);
        MyBean myBean = context.getBean(MyBean.class);
        System.out.println(userController);
        System.out.println(myBean);
    }
}
```

### [3.10.2. 其他配置类](https://jshand.gitee.io/#/course/12_spring/spring?id=_3102-其他配置类)

```java
import com.neuedu.MyBean;
import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/23  11:36 23
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : 另外一个配置类
 */
@Configuration
public class OtherConfig {


    @Bean
    public MyBean createBean(){
        return new MyBean();
    }
}
```

## [3.11 Bean的声明周期](https://jshand.gitee.io/#/course/12_spring/spring?id=_311-bean的声明周期)

# [4. AOP 面向切面编程](https://jshand.gitee.io/#/course/12_spring/spring?id=_4-aop-面向切面编程)

## [4.1. 介绍](https://jshand.gitee.io/#/course/12_spring/spring?id=_41-介绍)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B028509.png)

## [4.2. 代理机制](https://jshand.gitee.io/#/course/12_spring/spring?id=_42-代理机制)

Spring AOP使用了两种代理机制：一种是基于JDK的动态代理；另一种是基于CGLib的动态代理。之所以需要两种代理机制，是因为JDK本身只提供接口的代理，而不支持类的代理。

### [4.2.1. JDK动态代理](https://jshand.gitee.io/#/course/12_spring/spring?id=_421-jdk动态代理)

Spring的AOP的默认实现就是采用jdk的动态代理机制实现的。

自Java1.3以后，Java提供了动态代理技术，允许开发者在运行期创建接口的代理实例。

### [4.2.2. CGLib代理](https://jshand.gitee.io/#/course/12_spring/spring?id=_422-cglib代理)

JDK只能为接口创建代理实例，对于那些没有通过接口定义业务方法的类，可以通过CGLib创建代理实例。

CGLib采用底层字节码技术，可以为一个类创建子类，并在子类中采用方法拦截技术拦截所有父类方法的调用，这时可以顺势织入横切逻辑。

## [4.3. AOP的概念](https://jshand.gitee.io/#/course/12_spring/spring?id=_43-aop的概念)

### [4.3.1. 切面（Aspect）](https://jshand.gitee.io/#/course/12_spring/spring?id=_431-切面（aspect）)

切面是切点和通知组成，通知和切点共同定义了切面的全部内容即：它是什么，在何时何处完成其功能；

举例：将pointcut和Advice配置整合的过程切面

### [4.3.2. 连接点（Joinpoint）](https://jshand.gitee.io/#/course/12_spring/spring?id=_432-连接点（joinpoint）)

连接点是在应用执行过程中能够插入切面的一个点，Spring仅支持方法的连接点，即仅能在方法调用前，方法调用后，方法抛出异常时及方法调用前后插入切面代码。 .

举例：如果在insert之前执行扩展（增强、通知），简单理解insert这个方法可以称之为连接点。

连接点: 1 Com.neuedu.service.StuServiceImpl.insert()

连接点: 2 Com.neuedu.service.UserServiceImpl.insert()

### [4.3.3. 切点(Pointcut）](https://jshand.gitee.io/#/course/12_spring/spring?id=_433-切点pointcut）)

##### [Supported Pointcut Designators](https://jshand.gitee.io/#/course/12_spring/spring?id=supported-pointcut-designators)

Spring AOP supports the following AspectJ pointcut designators (PCD) for use in pointcut expressions:

- `execution`: For matching method execution join points. This is the primary pointcut designator to use when working with Spring AOP.
- `within`: Limits matching to join points within certain types (the execution of a method declared within a matching type when using Spring AOP).
- `this`: Limits matching to join points (the execution of methods when using Spring AOP) where the bean reference (Spring AOP proxy) is an instance of the given type.
- `target`: Limits matching to join points (the execution of methods when using Spring AOP) where the target object (application object being proxied) is an instance of the given type.
- `args`: Limits matching to join points (the execution of methods when using Spring AOP) where the arguments are instances of the given types.
- `@target`: Limits matching to join points (the execution of methods when using Spring AOP) where the class of the executing object has an annotation of the given type.
- `@args`: Limits matching to join points (the execution of methods when using Spring AOP) where the runtime type of the actual arguments passed have annotations of the given types.
- `@within`: Limits matching to join points within types that have the given annotation (the execution of methods declared in types with the given annotation when using Spring AOP).
- `@annotation`: Limits matching to join points where the subject of the join point (the method being run in Spring AOP) has the given annotation.

切点定义了在何处应用切面，AOP通过“切点”定位特定的连接点。切点相当于查询条件，一个切点可以匹配多个连接点。

举例：多个insert方法都需要进行扩展，统一一次配置，将多个连接点一起匹配称之为切点

切点匹配：Com.neuedu.service.*Impl.insert(..);

表达式:

Execution表达式语法

execution(<修饰符模式>?<返回类型模式><方法名模式>(<参数模式>)<异常模式>?)

除了返回类型模式、方法名模式和参数模式外，其它项都是可选的。

execution(public * *(..))

匹配所有目标类的public方法，第一个*代表返回类型，第二个*代表方法名，而..代表任意入参的方法；

execution(* *To(..))

匹配目标类所有以To为后缀的方法，第一个*代表返回类型，而*To代表任意以To为后缀的方法；

execution(* com.neuedu.UserService.*(..))

匹配UserService接口的所有方法，第一个*代表返回任意类型，com.neuedu.UserService.*代表UserService接口中的所有方法；

execution(* com.neuedu.*(..))

匹配com.neuedu包下所有类的所有方法

execution(* com.neuedu..*(..))

匹配com.neuedu包、子孙包下所有类的所有方法，“..”出现在类名中时，后面必须跟“*”，表示包、子孙包下的所有类；

execution(* com..*.*Dao.find*(..))

匹配包名前缀为com的任何包下类名后缀为Dao的方法，方法名必须以find为前缀

### [4.3.4. 通知（Advice）、增强](https://jshand.gitee.io/#/course/12_spring/spring?id=_434-通知（advice）、增强)

切面的工作被成为通知，定义了切面是什么及何时使用。除了描述切面要完成的工作，通知还解决了何时执行这个工作的问题，它应该在某个方法被调用之前？之后？等。

举例：额外定义一个方法想要在连接点上进行扩展的功能

#### [4.3.4.1. Spring切面可以应用5种类型的通知：](https://jshand.gitee.io/#/course/12_spring/spring?id=_4341-spring切面可以应用5种类型的通知：)

前置通知（Before）在目标方法被调用之前调用通知功能；

后置通知（After）在目标方法被完成之后调用通知功能，不关心方法的输出是什么；

环绕通知（Around advice）通知包裹了目标方法，在目标方法调用之前和之后执行自定义的行为；

异常通知（After-throwing)在目标方法抛出异常后调用通知；

返回通知（After-returning）在目标方法成功执行之后调用通知；

### [4.3.5. 目标对象(Target)](https://jshand.gitee.io/#/course/12_spring/spring?id=_435-目标对象target)

通知逻辑的织入目标类。如果没有AOP，那么目标业务类需要自己实现所有的逻辑，在AOP的帮助下，目标类只需要实现那些非横切逻辑的程序逻辑，而比如事务管理等这些横切逻辑就可以使用AOP动态织入特定的连接点上。

举例：两个Service即是目标对象

### [4.3.6. 引入（Introduction）](https://jshand.gitee.io/#/course/12_spring/spring?id=_436-引入（introduction）)

引介是一种特殊的通知，为类添加一些属性和方法。这样，即使一个业务类原本没有实现某个接口，通过AOP的引介功能，也可以动态地为该业务类添加接口的实现逻辑，使业务类成为这个接口的实现类。

几乎由框架完成，我们可以暂时透明

### [4.3.7. 代理（Proxy）](https://jshand.gitee.io/#/course/12_spring/spring?id=_437-代理（proxy）)

一个类被AOP织入通知后，就产生一个结果类，它是融合了原类和通知逻辑的代理类。根据不同的代理方式，代理类既可能是和原类具有相同接口的类，也可能就是原类的子类，所以可以采用与调用原类相同的方式调用代理类。

举例：IOC容器创建的两个Service类型对象

### [4.3.8. 织入(Weaving)](https://jshand.gitee.io/#/course/12_spring/spring?id=_438-织入weaving)

织入是将通知添加到目标类的具体连接点上的过程.AOP就像一台织布机，将目标类，通知或引介编织到一起。 AOP有三种织入方式： ①编译期织入：切面在目标类编译时被织入，需要特殊的编译器； ②类装载期织入：切面在目标类加载到JVM时被织入，需要特殊的类装载器； ③动态代理织入：切面在应用运行的某个时刻呗织入，AOP容器会为目标对象动态创建一个代理对象；

对用户透明，由框架完成

![img](https://jshand.gitee.io/imgs/spring/2021-03-10_112809.png)

## [4.4. AOP实战](https://jshand.gitee.io/#/course/12_spring/spring?id=_44-aop实战)

### [4.4.1. 需求：](https://jshand.gitee.io/#/course/12_spring/spring?id=_441-需求：)

在service层的方法上添加功能：

1）在insert方法之前开启事务（使用System.out.println(‘开启事务’)代替）

### [4.4.2. 实现的过程](https://jshand.gitee.io/#/course/12_spring/spring?id=_442-实现的过程)

1. 添加依赖

```xml
<!--AOP依赖 -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-aop</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>

<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.6</version>
</dependency>
```

1. 定义两个类UserService、StuService 接口和实现类

1. 使用IOC容器管理Bean

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <!--虽然声明的是UserServiceImpl,ioc容器创建的时候 可能创建的是子类型(代理对象)  -->
    <bean id="userService" class="com.neuedu.service.UserServiceImpl"/>

    <bean id="stuService" class="com.neuedu.service.StuServiceImpl"/>

</beans>
```

### [4.4.3. 使用注解定义AOP的增强（通知）AopAdvice](https://jshand.gitee.io/#/course/12_spring/spring?id=_443-使用注解定义aop的增强（通知）aopadvice)

#### [4.4.3.1. 切点（PointCut）](https://jshand.gitee.io/#/course/12_spring/spring?id=_4431-切点（pointcut）)

```java
//定义一个切点，insert方法
@Pointcut("execution(* com.neuedu.service.*.insert(..))")
public void pointcut(){}
```

#### [4.4.3.2. 增强（通知）-扩展代码](https://jshand.gitee.io/#/course/12_spring/spring?id=_4432-增强（通知）-扩展代码)

定义一个方法（想要在insert之前或者之后执行的代码）

前置通知

```xml
//定义一个需要在insert方法之前指定的逻辑
@Before("pointcut()")
public void before(){
    System.out.println("开启事务..代表了一些逻辑");
}
```

#### [4.4.3.3. 切面（类最终的代码）](https://jshand.gitee.io/#/course/12_spring/spring?id=_4433-切面（类最终的代码）)

使用@Aspectj注解让类中的切点和通知整合到一起

```java
import org.aspectj.lang.annotation.Aspect;
import org.aspectj.lang.annotation.Before;
import org.aspectj.lang.annotation.Pointcut;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/24  9:05 24
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : AOP的逻辑
 */
@Aspect  //切面 整合pointcut 和 @Before
public class AopAdvice {

    //定义一个切点，insert方法
    @Pointcut("execution(* com.neuedu.service.*.insert(..))")
    public void pointcut(){}


    //定义一个需要在insert方法之前指定的逻辑
    @Before("pointcut()")
    public void before(){
        System.out.println("开启事务..代表了一些逻辑");
    }
}
```

#### [4.4.3.4. 准备织入的逻辑](https://jshand.gitee.io/#/course/12_spring/spring?id=_4434-准备织入的逻辑)

1 ) 将切面类配置到IOC容器中，让IOC管理Bean,注解或者bean标签

```
<bean id="aopAdvice" class="com.neuedu.aop.AopAdvice"/>
```

1. 配置IOC使用切面生成对应的代理对象

```xml
<!--使用aop:aspectj-autoproxy 对应的目标对象生成代理对象-->
<!--用于扫描 Aspect 切面注解-->
<aop:aspectj-autoproxy/>
```

#### [4.4.3.5 完整的xml](https://jshand.gitee.io/#/course/12_spring/spring?id=_4435-完整的xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">

    <bean id="aopAdvice" class="com.neuedu.aop.AopAdvice"/>
    <!--使用aop:aspectj-autoproxy 对应的目标对象生成代理对象-->
    <!--用于扫描 Aspect 切面注解-->
    <aop:aspectj-autoproxy/>


    <!--虽然声明的是UserServiceImpl,ioc容器创建的时候 可能创建的是子类型(代理对象)  -->
    <bean id="userService" class="com.neuedu.service.UserServiceImpl"/>

    <bean id="stuService" class="com.neuedu.service.StuServiceImpl"/>

</beans>
```

#### [4.4.3.6. 单元测试](https://jshand.gitee.io/#/course/12_spring/spring?id=_4436-单元测试)

```java
import static org.junit.Assert.assertTrue;

import com.neuedu.service.IStuService;
import com.neuedu.service.IUserService;
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

/**
 * Unit test for simple App.
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:spring-aop.xml")
public class AppTest
{

    @Autowired
    IUserService userService;

    @Autowired
    IStuService stuService;

    @Test
    public void test1(){
        userService.insert();
        stuService.insert();
    }

}
```

### [4.4.4. Xml的形式配置AOP](https://jshand.gitee.io/#/course/12_spring/spring?id=_444-xml的形式配置aop)

创建新的工程，并将上一个工程的Service四个类拿过来。并使用bean工厂管理service对象

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B034840.png)

#### [4.4.4.1. 定义增强（通知）](https://jshand.gitee.io/#/course/12_spring/spring?id=_4441-定义增强（通知）)

定义类，并准备方法

```java
/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/24  9:58 24
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : 增强的通知
 */
public class AopAdvice {

    public void before(){
        System.out.println("使用xml的形式，声明的通知");
    }
}
```

在xml中配置AOP

Pointcut和advice整合的切面 aspectj

1）在xml中声明通知的Bean

2）整合切面

#### [4.4.4.2. 最终Spring-aop-xml的配置文件](https://jshand.gitee.io/#/course/12_spring/spring?id=_4442-最终spring-aop-xml的配置文件)

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:context="http://www.springframework.org/schema/context"
       xmlns:aop="http://www.springframework.org/schema/aop"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd http://www.springframework.org/schema/context https://www.springframework.org/schema/context/spring-context.xsd http://www.springframework.org/schema/aop https://www.springframework.org/schema/aop/spring-aop.xsd">


    <!--虽然声明的是UserServiceImpl,ioc容器创建的时候 可能创建的是子类型(代理对象)  -->
    <bean id="userService" class="com.neuedu.service.UserServiceImpl"/>

    <bean id="stuService" class="com.neuedu.service.StuServiceImpl"/>

    <bean id="aopAdvice" class="com.neuedu.aop.AopAdvice"/>

<!--    配置切面-->
   <!-- <aop:config>
        <!–配置pointcut–>
        <aop:pointcut id="serviceInsert" expression="execution(* com.neuedu.service.*.insert(..))"/>
        <!–配置切面的应用–>
        <aop:aspect ref="aopAdvice">
            <!–将通知应用到切点（pointcut）中–>
            <!–aop:before将切点和通知进行整合–>
            <aop:before method="before"  pointcut-ref="serviceInsert"/>
        </aop:aspect>
    </aop:config>-->



    <aop:config>
        <!--配置pointcut-->
<!--        <aop:pointcut id="serviceInsert" expression="execution(* com.neuedu.service.*.insert(..))"/>-->
        <!--配置切面的应用-->
        <aop:aspect ref="aopAdvice">
            <!--将通知应用到切点（pointcut）中-->
            <!--aop:before将切点和通知进行整合-->
            <aop:before method="before"  pointcut="execution(* com.neuedu.service.*.insert(..))"/>
        </aop:aspect>
    </aop:config>

</beans>
```

## [4.5. 各种类型的通知](https://jshand.gitee.io/#/course/12_spring/spring?id=_45-各种类型的通知)

### [4.5.1. 通知的代码](https://jshand.gitee.io/#/course/12_spring/spring?id=_451-通知的代码)

```java
import org.aspectj.lang.ProceedingJoinPoint;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/24  9:58 24
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : 增强的通知
 */
public class AopAdvice {

    public void before(){
        System.out.println("使用xml的形式，[前置]声明的通知");
    }


    public void after(){ System.out.println("使用xml的形式，[后置]声明的通知");
    }

    public void afterReturn(){ System.out.println("使用xml的形式，[返回通知]声明的通知");
    }


    public void afterEx(Exception ex){
        System.out.println(ex);
        System.out.println("使用xml的形式，[异常通知]声明的通知");
    }


//    环绕通知  
    public void around(ProceedingJoinPoint pjp){
        //之前
        //insert 切点
        System.out.println("aroud    start ...........");
        try {
            pjp.proceed();  //insert方法
        } catch (Throwable throwable) {
            throwable.printStackTrace();
        }
        System.out.println("aroud    end ...........");
        //之后
    }
}
```

### [4.5.2. Xml中的声明](https://jshand.gitee.io/#/course/12_spring/spring?id=_452-xml中的声明)

```xml
<aop:config>
        <!--配置pointcut-->
<!--        <aop:pointcut id="serviceInsert" expression="execution(* com.neuedu.service.*.insert(..))"/>-->
        <!--配置切面的应用-->

        <aop:aspect ref="aopAdvice">
            <!--将通知应用到切点（pointcut）中-->
            <!--aop:before将切点和通知进行整合-->
<!--            <aop:before method="before"  pointcut="execution(* com.neuedu.service.*.insert(..))"/>-->
<!--            <aop:after method="after"  pointcut="execution(* com.neuedu.service.*.insert(..))"/>-->
<!--            <aop:after-returning method="afterReturn"  pointcut="execution(* com.neuedu.service.*.insert(..))"/>-->
<!--            <aop:after-throwing method="afterEx" throwing="ex"   pointcut="execution(* com.neuedu.service.*.insert(..))"/>-->
            <aop:around method="around"  pointcut="execution(* com.neuedu.service.*.insert(..))"/>
        </aop:aspect>
    </aop:config>
```

# [5. SpringJDBC](https://jshand.gitee.io/#/course/12_spring/spring?id=_5-springjdbc)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B038917.png)

JDBCTemplate JDBC的模板类

## [5.1. 搭建入门程序](https://jshand.gitee.io/#/course/12_spring/spring?id=_51-搭建入门程序)

### [5.1.1. 创建项目](https://jshand.gitee.io/#/course/12_spring/spring?id=_511-创建项目)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B038953.png)

### [5.1.2. Pom的依赖](https://jshand.gitee.io/#/course/12_spring/spring?id=_512-pom的依赖)

```xml
<!--Spring对JDBC封装-->
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>

<!-- 数据源： https://mvnrepository.com/artifact/org.apache.commons/commons-dbcp2 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-dbcp2</artifactId>
    <version>2.7.0</version>
</dependency>

<!--msyql数据库的驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.46</version>
</dependency>
```

### [5.1.3. 配置数据源](https://jshand.gitee.io/#/course/12_spring/spring?id=_513-配置数据源)

1. 使用代码自己new
2. Spring-jdbc.xml IOC容器管理数据源 √

#### [5.1.3.1. 创建xml文件](https://jshand.gitee.io/#/course/12_spring/spring?id=_5131-创建xml文件)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd ">

    <!-- 声明数据源-->
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource">
        <property name="username" value="root"/>
        <property name="password" value="root"/>
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
    </bean>

</beans>
```

#### [5.1.3.2. 测试获取连接](https://jshand.gitee.io/#/course/12_spring/spring?id=_5132-测试获取连接)

```java
import static org.junit.Assert.assertTrue;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.sql.DataSource;
import java.sql.SQLException;

/**
 * Unit test for simple App.
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:spring-jdbc.xml")
public class AppTest
{

    @Autowired
    DataSource dataSource;


    /**
     * 测试获取连接对象
     * @throws SQLException
     */
    @Test
    public void shouldAnswerWithTrue() throws SQLException {

        System.out.println(dataSource.getConnection());

    }
}
```

### [5.1.4. 使用JDBCTemplate类做测试查询](https://jshand.gitee.io/#/course/12_spring/spring?id=_514-使用jdbctemplate类做测试查询)

#### [5.1.4.1. 自定义创建JDBCTemplate类](https://jshand.gitee.io/#/course/12_spring/spring?id=_5141-自定义创建jdbctemplate类)

```java
@Test
public void test2() throws SQLException {

    JdbcTemplate jt = new JdbcTemplate(dataSource);

    List<Map<String,Object>> userList = jt.queryForList("select * from t_user");
    for (Map<String, Object> userMap : userList) {
        Set<String> columns = userMap.keySet();

        for (String column : columns) {
            System.out.printf("column:%s,value:%s ",column,userMap.get(column));
        }
        System.out.println();
    }

}
```

#### [5.1.4.2. 在IOC容器中定义JDBCTemplate类](https://jshand.gitee.io/#/course/12_spring/spring?id=_5142-在ioc容器中定义jdbctemplate类)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">


    <!-- 声明数据源-->
    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" p:username="root">
<!--        <property name="username" value="root"/>-->
        <property name="password" value="root"/>
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>
    </bean>


<!--    <bean id="jt" class="org.springframework.jdbc.core.JdbcTemplate">-->
<!--        <property name="dataSource" ref="dataSource"/>-->
<!--    </bean>-->


    <bean id="jt" class="org.springframework.jdbc.core.JdbcTemplate"
        p:dataSource-ref="dataSource" >
    </bean>

</beans>
```

单元测试类

```java
@Autowired
JdbcTemplate jt ;
@Test
public void test3() throws SQLException {

    List<Map<String,Object>> userList = jt.queryForList("select * from t_user");
    for (Map<String, Object> userMap : userList) {
        Set<String> columns = userMap.keySet();

        for (String column : columns) {
            System.out.printf("column:%s,value:%s ",column,userMap.get(column));
        }
        System.out.println();
    }
}
```

## [5.2. 使用jt封装Dao](https://jshand.gitee.io/#/course/12_spring/spring?id=_52-使用jt封装dao)

### [5.2.1. 数据库脚本](https://jshand.gitee.io/#/course/12_spring/spring?id=_521-数据库脚本)

```sql
/*

SQLyog Ultimate v12.08 (64 bit)

MySQL - 5.7.18 : Database - mybatis

*********************************************************************

*/





/*!40101 SET NAMES utf8 */;



/*!40101 SET SQL_MODE=''*/;



/*!40014 SET @OLD_UNIQUE_CHECKS=@@UNIQUE_CHECKS, UNIQUE_CHECKS=0 */;

/*!40014 SET @OLD_FOREIGN_KEY_CHECKS=@@FOREIGN_KEY_CHECKS, FOREIGN_KEY_CHECKS=0 */;

/*!40101 SET @OLD_SQL_MODE=@@SQL_MODE, SQL_MODE='NO_AUTO_VALUE_ON_ZERO' */;

/*!40111 SET @OLD_SQL_NOTES=@@SQL_NOTES, SQL_NOTES=0 */;

CREATE DATABASE /*!32312 IF NOT EXISTS*/`mybatis` /*!40100 DEFAULT CHARACTER SET utf8 COLLATE utf8_bin */;



USE `mybatis`;



/*Table structure for table `t_account` */



DROP TABLE IF EXISTS `t_account`;



CREATE TABLE `t_account` (

  `account_id` int(11) NOT NULL AUTO_INCREMENT,

  `username` varchar(100) COLLATE utf8_bin DEFAULT NULL,

  `displayname` varchar(100) COLLATE utf8_bin DEFAULT NULL,

  `email` varchar(100) COLLATE utf8_bin DEFAULT NULL,

  `address` varchar(100) COLLATE utf8_bin DEFAULT '哈尔滨',

  `phone` varchar(100) COLLATE utf8_bin DEFAULT NULL,

  `createtime` decimal(10,0) DEFAULT NULL,

  PRIMARY KEY (`account_id`)

) ENGINE=InnoDB AUTO_INCREMENT=16 DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='京东账户';



/*Data for the table `t_account` */



insert  into `t_account`(`account_id`,`username`,`displayname`,`email`,`address`,`phone`,`createtime`) values (1,'admin','郭靖','guojing@neusoft.com','襄阳城','18888888888','9999999999'),(2,'jshand','黄老邪','huanglaoxie@neusoft.com','桃花岛','13888888888','9999999999'),(4,'bbbb','梅超风','meichaofeng@163.com','骨髅岛','phone:138',NULL),(5,NULL,'黄蓉',NULL,'襄阳城',NULL,NULL),(6,NULL,'华筝',NULL,NULL,NULL,NULL),(7,NULL,'祥龙','insertSelective','哈尔滨',NULL,NULL),(8,'jjqqkk','伏虎','fuhu@163.com',NULL,NULL,NULL),(9,'jjqqkk',NULL,NULL,NULL,NULL,NULL),(10,NULL,'达摩',NULL,NULL,NULL,NULL),(11,'u-aaaaaaa','dis-aaaaaaa',NULL,'哈尔滨',NULL,NULL),(12,'u-bbbbbbbb',NULL,NULL,'哈尔滨',NULL,NULL),(13,'u-cccccccccc',NULL,NULL,'哈尔滨',NULL,NULL),(14,'u-cccccccccc','dis-aaaaaaa',NULL,'哈尔滨',NULL,NULL),(15,'u-dddddddddd--uuuu','dis-dddd--uuu','15@163.com','哈尔滨',NULL,NULL);



/*Table structure for table `t_order` */



DROP TABLE IF EXISTS `t_order`;



CREATE TABLE `t_order` (

  `order_id` int(11) NOT NULL AUTO_INCREMENT,

  `account_id` int(11) DEFAULT NULL,

  `order_amount` varchar(100) COLLATE utf8_bin DEFAULT NULL,

  `order_address` varchar(100) COLLATE utf8_bin DEFAULT NULL,

  `createtime` datetime DEFAULT NULL,

  PRIMARY KEY (`order_id`),

  KEY `FK_Reference_1` (`account_id`),

  CONSTRAINT `FK_Reference_1` FOREIGN KEY (`account_id`) REFERENCES `t_account` (`account_id`)

) ENGINE=InnoDB AUTO_INCREMENT=5 DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='订单表';



/*Data for the table `t_order` */



insert  into `t_order`(`order_id`,`account_id`,`order_amount`,`order_address`,`createtime`) values (1,1,'5000','襄阳城','2019-03-04 09:00:37'),(2,1,'6000','桃花岛','2019-03-12 09:00:54'),(3,2,'4000','桃花岛-岛主','2019-03-26 09:01:30'),(4,2,'8000','西域',NULL);



/*Table structure for table `t_order_item` */



DROP TABLE IF EXISTS `t_order_item`;



CREATE TABLE `t_order_item` (

  `pid` varchar(10) COLLATE utf8_bin DEFAULT NULL,

  `order_id` int(10) DEFAULT NULL,

  `itemid` varchar(10) COLLATE utf8_bin NOT NULL,

  PRIMARY KEY (`itemid`),

  KEY `FK_Reference_6` (`pid`),

  KEY `FK_Reference_5` (`order_id`),

  CONSTRAINT `FK_Reference_4` FOREIGN KEY (`pid`) REFERENCES `t_product` (`pid`),

  CONSTRAINT `FK_Reference_5` FOREIGN KEY (`order_id`) REFERENCES `t_order` (`order_id`)

) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='订单详情\n';



/*Data for the table `t_order_item` */



insert  into `t_order_item`(`pid`,`order_id`,`itemid`) values ('1',1,'1'),('2',1,'2'),('3',1,'3'),('1',2,'4'),('4',2,'5'),('2',3,'6'),('3',3,'7');



/*Table structure for table `t_product` */



DROP TABLE IF EXISTS `t_product`;



CREATE TABLE `t_product` (

  `pid` varchar(10) COLLATE utf8_bin NOT NULL,

  `name` varchar(10) COLLATE utf8_bin DEFAULT NULL,

  `price` decimal(10,2) DEFAULT NULL,

  PRIMARY KEY (`pid`)

) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin COMMENT='商品信息';



/*Data for the table `t_product` */



insert  into `t_product`(`pid`,`name`,`price`) values ('1','男士腰带','180.00'),('2','香奈儿','50.00'),('3','生日蛋糕','120.00'),('4','联想笔记本电脑','5000.00'),('5','水杯','5.50');



/*Table structure for table `t_stu` */



DROP TABLE IF EXISTS `t_stu`;



CREATE TABLE `t_stu` (

  `stu_id` bigint(100) NOT NULL,

  `stu_name` varchar(100) NOT NULL COMMENT '学生姓名',

  `stu_birthday` date DEFAULT NULL COMMENT '学生出生日期',

  `stu_cardno` varchar(100) DEFAULT NULL COMMENT '学生身份证号码',

  `stu_gender` varchar(100) DEFAULT '男',

  PRIMARY KEY (`stu_id`)

) ENGINE=InnoDB DEFAULT CHARSET=utf8;



/*Data for the table `t_stu` */



insert  into `t_stu`(`stu_id`,`stu_name`,`stu_birthday`,`stu_cardno`,`stu_gender`) values (1,'张国荣','1956-09-12','zgr0000000000001','男'),(2,'郭靖','1987-08-07','gj0000000000002','男'),(3,'黄蓉','1988-09-08','hr0000000000003','女'),(4,'黄老邪','2020-03-13','666556548465165','男');



/*Table structure for table `t_stu_account` */



DROP TABLE IF EXISTS `t_stu_account`;



CREATE TABLE `t_stu_account` (

  `account_id` bigint(50) NOT NULL COMMENT '账户id',

  `amount` double(10,2) DEFAULT '0.00' COMMENT '账户金额',

  `createtime` date DEFAULT NULL COMMENT '账户创建时间',

  `status` varchar(50) DEFAULT NULL COMMENT '账户状态',

  `stu_id` bigint(50) NOT NULL COMMENT '学生id',

  PRIMARY KEY (`account_id`),

  KEY `fk_account_stu` (`stu_id`),

  CONSTRAINT `fk_account_stu` FOREIGN KEY (`stu_id`) REFERENCES `t_stu` (`stu_id`)

) ENGINE=InnoDB DEFAULT CHARSET=utf8;



/*Data for the table `t_stu_account` */



insert  into `t_stu_account`(`account_id`,`amount`,`createtime`,`status`,`stu_id`) values (1,15.15,'2018-01-01','销户',1),(2,80.15,'2018-02-03','正常',2),(3,190.15,'2015-06-10','挂失',3);



/*Table structure for table `t_user` */



DROP TABLE IF EXISTS `t_user`;



CREATE TABLE `t_user` (

  `user_id` varchar(100) COLLATE utf8_bin NOT NULL,

  `user_name` varchar(100) COLLATE utf8_bin DEFAULT NULL,

  `gender` varchar(100) COLLATE utf8_bin DEFAULT NULL,

  `birthday` datetime DEFAULT NULL,

  `status` varchar(10) COLLATE utf8_bin DEFAULT NULL,

  PRIMARY KEY (`user_id`)

) ENGINE=InnoDB DEFAULT CHARSET=utf8 COLLATE=utf8_bin;



/*Data for the table `t_user` */



insert  into `t_user`(`user_id`,`user_name`,`gender`,`birthday`,`status`) values ('1','刘备','男','2008-02-02 09:57:43','0'),('10','典韦使用别名','男','2020-03-11 15:01:03','0'),('11','典韦使用别名','男','2020-03-12 08:45:53','0'),('12','root','男','2020-03-12 08:47:02','0'),('13','root','男','2020-03-12 08:50:53','0'),('14','曹操','男','2020-03-12 09:04:19','0'),('15','曹操1','男','2020-03-12 09:07:31','0'),('20','使用JT添加的',NULL,NULL,NULL),('3','关云长','男','2008-02-02 09:57:43','0'),('5','三国-典韦','男','2020-03-11 13:39:39','0'),('6','典韦使用别名','男','2020-03-11 14:47:12','0'),('7','典韦使用别名','男','2020-03-11 14:55:42','0'),('8','典韦使用别名','男','2020-03-11 14:56:24','0'),('9','吕布',NULL,NULL,NULL);



/*!40101 SET SQL_MODE=@OLD_SQL_MODE */;

/*!40014 SET FOREIGN_KEY_CHECKS=@OLD_FOREIGN_KEY_CHECKS */;

/*!40014 SET UNIQUE_CHECKS=@OLD_UNIQUE_CHECKS */;

/*!40111 SET SQL_NOTES=@OLD_SQL_NOTES */;
```

### [5.2.2. 实体类User](https://jshand.gitee.io/#/course/12_spring/spring?id=_522-实体类user)

```java
import java.util.Date;

/**
 * 用户的实体对象
 */
public class User {

    private String userId;
    private String userName;
    private String gender;
    private Date birthday;
    private Integer status;

    public String getUserId() {
        return userId;
    }

    public void setUserId(String userId) {
        this.userId = userId;
    }

    public String getUserName() {
        return userName;
    }

    public void setUserName(String userName) {
        this.userName = userName;
    }

    public String getGender() {
        return gender;
    }

    public void setGender(String gender) {
        this.gender = gender;
    }

    public Date getBirthday() {
        return birthday;
    }

    public void setBirthday(Date birthday) {
        this.birthday = birthday;
    }

    public Integer getStatus() {
        return status;
    }

    public void setStatus(Integer status) {
        this.status = status;
    }

    @Override
    public String toString() {
        return "User{" +
                "userId='" + userId + '\'' +
                ", userName='" + userName + '\'' +
                ", gender='" + gender + '\'' +
                ", birthday=" + birthday +
                ", status=" + status +
                '}';
    }
}
```

### [5.2.3. 定义Dao](https://jshand.gitee.io/#/course/12_spring/spring?id=_523-定义dao)

```java
import org.springframework.jdbc.core.JdbcTemplate;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/24  13:40 24
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : 用户的Dao层操作
 */
public class UserDao {


    //让Dao层持有一个 jt
    private JdbcTemplate jdbcTemplate;


    public JdbcTemplate getJdbcTemplate() {
        return jdbcTemplate;
    }

    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }

    //写增删改查的代码
}
```

### [5.2.4. 声明Bean并注入jt](https://jshand.gitee.io/#/course/12_spring/spring?id=_524-声明bean并注入jt)

```xml
<bean id="userDao" class="com.neuedu.UserDao">
    <property name="jdbcTemplate" ref="jt"/>
</bean>
```

### [5.2.5. 单元测试UserDao的实例化对象是否声明完成](https://jshand.gitee.io/#/course/12_spring/spring?id=_525-单元测试userdao的实例化对象是否声明完成)

```java
//使用jdbcTemplate测试CRUD
@Autowired
UserDao userDao;

@Test
public  void testUserDao(){
    System.out.println(userDao.getJdbcTemplate());
}
```

### [5.2.6. Insert](https://jshand.gitee.io/#/course/12_spring/spring?id=_526-insert)

#### [5.2.6.1. Dao中声明的方法](https://jshand.gitee.io/#/course/12_spring/spring?id=_5261-dao中声明的方法)

```java
/**
 * 插入用户信息
 * @param user
 * @return
 */
public boolean insert(User user){
    String sql = "INSERT INTO t_user (  user_id,  user_name, gender,  birthday , status )  VALUES  (  ?,?,?,?,? ) ";

    Object[] params = new Object[]{
            user.getUserId(),
            user.getUserName(),
            user.getGender(),
            user.getBirthday(),
            user.getStatus(),
    };
    int count = jdbcTemplate.update(sql,params);

    return count>0;
}
```

#### [5.2.6.2. 单元测试类](https://jshand.gitee.io/#/course/12_spring/spring?id=_5262-单元测试类)

```java
@Test
public  void testInsert(){

    User user = new User();
    user.setUserId("20");
    user.setUserName("使用JT添加的");
    boolean success = userDao.insert(user);
    System.out.println(success);

}
```

### [5.2.7. delete](https://jshand.gitee.io/#/course/12_spring/spring?id=_527-delete)

#### [5.2.7.1. Dao中声明的方法](https://jshand.gitee.io/#/course/12_spring/spring?id=_5271-dao中声明的方法)

```java
/**
 * 删除
 * @param id
 * @return
 */
public boolean delete(String id){
    String sql = "delete from  t_user where  user_id = ? ";

    int count = jdbcTemplate.update(sql,id);

    return count>0;
}
```

#### [5.2.7.2. 单元测试类](https://jshand.gitee.io/#/course/12_spring/spring?id=_5272-单元测试类)

```java
@Test
public  void testDelete(){

   String id = "16";
   boolean success = userDao.delete(id);
   System.out.println(success);
}
```

### [5.2.8. selectById](https://jshand.gitee.io/#/course/12_spring/spring?id=_528-selectbyid)

#### [5.2.8.1. Dao中声明的方法](https://jshand.gitee.io/#/course/12_spring/spring?id=_5281-dao中声明的方法)

```java
/**
     * 根据主键查询一条
     * @param id
     * @return
     */
    public User selectById(String id){

        String sql = "select * from t_user where user_id = ?";

        //聚合函数的时候使用并且是单行、单列的
//        User user = jdbcTemplate.queryForObject(sql,User.class,id);
        User user = jdbcTemplate.queryForObject(sql, new RowMapper<User>() {
            @Override
            public User mapRow(ResultSet resultSet, int i) throws SQLException {
                String userId = resultSet.getString("user_id");
                String userName = resultSet.getString("user_name");
                //其他属性照抄 String userName = resultSet.getString("user_name");


                User user = new User();
                user.setUserId(userId);
                user.setUserName(userName);
                return user;
            }
        },id);

        return user;

    }
```

#### [5.2.8.2. 单元测试类](https://jshand.gitee.io/#/course/12_spring/spring?id=_5282-单元测试类)

```java
@Test
public  void testSelectById(){

    String id = "1";
    User user =  userDao.selectById(id);
    System.out.println(user);
} 
```

### [5.2.9. selectList](https://jshand.gitee.io/#/course/12_spring/spring?id=_529-selectlist)

#### [5.2.9.1. Dao中声明的方法](https://jshand.gitee.io/#/course/12_spring/spring?id=_5291-dao中声明的方法)

```java
/**
 * 查询多条
 * @param
 * @return
 */
public List selectList(){

    String sql = "select * from t_user ";

    List<User> userList= jdbcTemplate.queryForObject(sql, new RowMapper<List>() {
        @Override
        public List<User> mapRow(ResultSet resultSet, int i) throws SQLException {

            List<User> list = new ArrayList();
            do {
                String userId = resultSet.getString("user_id");
                String userName = resultSet.getString("user_name");
                //其他属性照抄 String userName = resultSet.getString("user_name");

                User user = new User();
                user.setUserId(userId);
                user.setUserName(userName);
                list.add(user);
            }while(resultSet.next());

            return list;

        }
    });
    return userList;
}
```

#### [5.2.9.2. 单元测试类](https://jshand.gitee.io/#/course/12_spring/spring?id=_5292-单元测试类)

```java
@Test
public  void testSelectList(){
    List<User> list =  userDao.selectList();

    //应该在页面列表展示
    System.out.println(list.size());

    for (User user : list) {
        System.out.println(user);
    }
} 
```

### [5.2.10. update](https://jshand.gitee.io/#/course/12_spring/spring?id=_5210-update)

#### [5.2.10.1. Dao中声明的方法](https://jshand.gitee.io/#/course/12_spring/spring?id=_52101-dao中声明的方法)

```java
/**
 * 根据主键更新
 * @param user
 * @return
 */
public boolean update(User user){
    StringBuffer sql = new StringBuffer();

    sql.append(" UPDATE          ");
    sql.append("  t_user            ");
    sql.append(" SET                ");
    sql.append("   user_name=?,       ");
    sql.append("   gender=?,      ");
    sql.append("   birthday=?,    ");
    sql.append("   status=?           ");
    sql.append(" WHERE user_id=?   ");


    Object[] params = new Object[]{

            user.getUserName(),
            user.getGender(),
            user.getBirthday(),
            user.getStatus(),
            user.getUserId()
    };
    //update -- delete 、insert、update
    int count = jdbcTemplate.update(sql.toString(),params);

    return count>0;
}
```

#### [5.2.10.2. 单元测试类](https://jshand.gitee.io/#/course/12_spring/spring?id=_52102-单元测试类)

```java
@Test
public  void testUpdate(){

    String id = "9";
    User user = userDao.selectById(id);
    user.setUserName("吕布");

    boolean success = userDao.update(user);

    System.out.println("success:"+success);
}
```

### [5.2.11. 完整的UserDao代码](https://jshand.gitee.io/#/course/12_spring/spring?id=_5211-完整的userdao代码)

```java
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.jdbc.core.RowMapper;

import java.sql.ResultSet;
import java.sql.SQLException;
import java.util.ArrayList;
import java.util.List;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/24  13:40 24
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : 用户的Dao层操作
 */
public class UserDao {


    //让Dao层持有一个 jt
    private JdbcTemplate jdbcTemplate;


    public JdbcTemplate getJdbcTemplate() {
        return jdbcTemplate;
    }

    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;



    }

    //写增删改查的代码

    /**
     * 插入用户信息
     * @param user
     * @return
     */
    public boolean insert(User user){
        String sql = "INSERT INTO t_user (  user_id,  user_name, gender,  birthday , status )  VALUES  (  ?,?,?,?,? ) ";

        Object[] params = new Object[]{
                user.getUserId(),
                user.getUserName(),
                user.getGender(),
                user.getBirthday(),
                user.getStatus(),
        };
        int count = jdbcTemplate.update(sql,params);

        return count>0;
    }

    /**
     * 删除
     * @param id
     * @return
     */
    public boolean delete(String id){
        String sql = "delete from  t_user where  user_id = ? ";

        int count = jdbcTemplate.update(sql,id);

        return count>0;
    }


    /**
     * 根据主键查询一条
     * @param id
     * @return
     */
    public User selectById(String id){

        String sql = "select * from t_user where user_id = ?";

        //聚合函数的时候使用并且是单行、单列的
//        User user = jdbcTemplate.queryForObject(sql,User.class,id);
        User user = jdbcTemplate.queryForObject(sql, new RowMapper<User>() {
            @Override
            public User mapRow(ResultSet resultSet, int i) throws SQLException {
                String userId = resultSet.getString("user_id");
                String userName = resultSet.getString("user_name");
                //其他属性照抄 String userName = resultSet.getString("user_name");


                User user = new User();
                user.setUserId(userId);
                user.setUserName(userName);
                return user;
            }
        },id);

        return user;

    }

    /**
     * 查询多条
     * @param
     * @return
     */
    public List selectList(){

        String sql = "select * from t_user ";

        List<User> userList= jdbcTemplate.queryForObject(sql, new RowMapper<List>() {
            @Override
            public List<User> mapRow(ResultSet resultSet, int i) throws SQLException {

                List<User> list = new ArrayList();
                do {
                    String userId = resultSet.getString("user_id");
                    String userName = resultSet.getString("user_name");
                    //其他属性照抄 String userName = resultSet.getString("user_name");

                    User user = new User();
                    user.setUserId(userId);
                    user.setUserName(userName);
                    list.add(user);
                }while(resultSet.next());

                return list;

            }
        });
        return userList;
    }


    /**
     * 根据主键更新
     * @param user
     * @return
     */
    public boolean update(User user){
        StringBuffer sql = new StringBuffer();

        sql.append(" UPDATE          ");
        sql.append("  t_user            ");
        sql.append(" SET                ");
        sql.append("   user_name=?,       ");
        sql.append("   gender=?,      ");
        sql.append("   birthday=?,    ");
        sql.append("   status=?           ");
        sql.append(" WHERE user_id=?   ");


        Object[] params = new Object[]{

                user.getUserName(),
                user.getGender(),
                user.getBirthday(),
                user.getStatus(),
                user.getUserId()
        };
        //update -- delete 、insert、update
        int count = jdbcTemplate.update(sql.toString(),params);

        return count>0;
    }
}
```

### [5.2.12. 完整的spring-jdbc.xml文件](https://jshand.gitee.io/#/course/12_spring/spring?id=_5212-完整的spring-jdbcxml文件)

```java
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">


    <!-- 声明数据源-->
    <!--<bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource" >
        <property name="username" value="root"/>
        <property name="password" value="root"/>
        <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
        <property name="url" value="jdbc:mysql://localhost:3306/mybatis"/>

    </bean>-->


    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource"
        p:username="root"
        p:password="root"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mybatis?useUnicode=true&unic" >

    </bean>



<!--    <bean id="jt" class="org.springframework.jdbc.core.JdbcTemplate">-->
<!--        <property name="dataSource" ref="dataSource"/>-->
<!--    </bean>-->


    <bean id="jt" class="org.springframework.jdbc.core.JdbcTemplate"
        p:dataSource-ref="dataSource" >
    </bean>


    <bean id="userDao" class="com.neuedu.UserDao">
        <property name="jdbcTemplate" ref="jt"/>
    </bean>


</beans>
```

### [5.2.13. 完整的单元测试类](https://jshand.gitee.io/#/course/12_spring/spring?id=_5213-完整的单元测试类)

```java
package com.neuedu;

import static org.junit.Assert.assertTrue;

import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.jdbc.core.JdbcTemplate;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import javax.sql.DataSource;
import java.sql.SQLException;
import java.util.List;
import java.util.Map;
import java.util.Set;

/**
 * Unit test for simple App.
 */
@RunWith(SpringJUnit4ClassRunner.class) // junit 4.12+
@ContextConfiguration(locations = "classpath:spring-jdbc.xml")
public class AppTest
{

    @Autowired
    DataSource dataSource;


    /**
     * 测试获取连接对象
     * @throws SQLException
     */
    @Test
    public void test1() throws SQLException {

        System.out.println(dataSource.getConnection());
    }

    @Test
    public void test2() throws SQLException {

        JdbcTemplate jt = new JdbcTemplate(dataSource);

        List<Map<String,Object>> userList = jt.queryForList("select * from t_user");
        for (Map<String, Object> userMap : userList) {
            Set<String> columns = userMap.keySet();

            for (String column : columns) {
                System.out.printf("column:%s,value:%s ",column,userMap.get(column));
            }
            System.out.println();
        }

    }



    @Autowired
    JdbcTemplate jt ;
    @Test
    public void test3() throws SQLException {

        List<Map<String,Object>> userList = jt.queryForList("select * from t_user");
        for (Map<String, Object> userMap : userList) {
            Set<String> columns = userMap.keySet();

            for (String column : columns) {
                System.out.printf("column:%s,value:%s ",column,userMap.get(column));
            }
            System.out.println();
        }
    }




    //使用jdbcTemplate测试CRUD
    @Autowired
    UserDao userDao;

    @Test
    public  void testUserDao(){
        System.out.println(userDao.getJdbcTemplate());
    }

    @Test
    public  void testInsert(){

        User user = new User();
        user.setUserId("20");
        user.setUserName("使用JT添加的");
        boolean success = userDao.insert(user);
        System.out.println(success);

    }


    @Test
    public  void testDelete(){

       String id = "16";
       boolean success = userDao.delete(id);
       System.out.println(success);

    }

    @Test
    public  void testSelectById(){

        String id = "1";
        User user =  userDao.selectById(id);
        System.out.println(user);

    }


    @Test
    public  void testSelectList(){
        List<User> list =  userDao.selectList();

        //应该在页面列表展示
        System.out.println(list.size());

        for (User user : list) {
            System.out.println(user);
        }
    }

    @Test
    public  void testUpdate(){

        String id = "9";
        User user = userDao.selectById(id);
        user.setUserName("吕布");

        boolean success = userDao.update(user);

        System.out.println("success:"+success);
    }

}
```

# [6. Spring事务](https://jshand.gitee.io/#/course/12_spring/spring?id=_6-spring事务)

## [6.1. 事务基础知识](https://jshand.gitee.io/#/course/12_spring/spring?id=_61-事务基础知识)

事务要么整体生效，要么整体失效。在数据库上即多条SQL语句要么全执行成功，要么全执行失败数据库事务必须同时满足4个特性： 原子性（Atomic），一致性（Consistency），隔离性(Isolation)和持久性(Durabiliy)

一致性是最终目标，其他特性都是为了达到这个目标而采取的措施。

数据库管理系统一般采用重执行日志来确保原子性，一致性和持久性。 重执行日志记录了数据库变化的每一个动作，数据库在一个事务中执行一部分操作后发生错误退出，数据库即可根据重执行日志撤销已经执行的操作。对于已经提交的事务，即使数据库奔溃，在重启数据库时也能根据日志对尚未持久化的数据进行相应的重执行操作数据库管理系统采用数据库锁机制保证事务的隔离性。当多个事务视图对相同的数据进行操作时，只有持有锁的事务才能操作数据，直到前一个事务完成后，后面的事务才有机会对数据进行操作。

Oracle数据库使用了数据版本的机制，在回滚段为数据的每个变化都保存一个版本，使数据的更改不影响数据的读取。

## [6.2. 事务并发问题](https://jshand.gitee.io/#/course/12_spring/spring?id=_62-事务并发问题)

在实际开发中数据库操作一般都是并发执行的，即有多个事务并发执行，并发执行就可能遇到问题，目前常见的问题如下：

丢失更新：

两个事务同时更新一行数据，最后一个事务的更新会覆盖掉第一个事务的更新，从而导致第一个事务更新的数据丢失，这是由于没有加锁造成的

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B065621.png)

脏读：

一个事务看到了另一个事务未提交的更新数据

不可重复读：

在同一事务中，多次读取同一数据却返回不同的结果；也就是有其他事务更改了这些数据

幻读：

一个事务在执行过程中读取到了另一个事务已提交的插入数据；即在第一个事务开始时读取到一批数据，但此后另一个事务又插入了新数据并提交，此时第一个事务又读取这批数据但发现多了一条，即好像发生幻觉一样

## [6.3. 数据的隔离级别（标准SQL）](https://jshand.gitee.io/#/course/12_spring/spring?id=_63-数据的隔离级别（标准sql）)

数据库默认隔离级别: mysql ---repeatable,oracle,sql server ---read commited

| 隔离级别        | 说明                                                         |
| --------------- | ------------------------------------------------------------ |
| DEFAULT         | 使用后端数据库默认的隔离级别(spring中的的选择项)             |
| READ_UNCOMMITED | 允许你读取还未提交的改变了的数据。可能导致脏、幻、不可重复读 |
| READ_COMMITTED  | 允许在并发事务已经提交后读取。可防止脏读，但幻读和 不可重复读仍可发生 |
| REPEATABLE_READ | 对相同字段的多次读取是一致的，除非数据被事务本身改变。可防止脏、不可重复读，但幻读仍可能发生。 |
| SERIALIZABLE    | 完全服从ACID的隔离级别，确保不发生脏、幻、不可重复读。这在所有的隔离级别中是最慢的，它是典型的通过完全锁定在事务中涉及的数据表来完成的。 |

## [6.4. Spring的事务传播策略](https://jshand.gitee.io/#/course/12_spring/spring?id=_64-spring的事务传播策略)

| 传播行为      | 说明                                                         |
| ------------- | ------------------------------------------------------------ |
| REQUIRED      | 业务方法需要在一个事务中运行。如果方法运行时，已经处在一个事务中，那么加入到该事务，否则为自己创建一个新的事务 |
| NOT_SUPPORTED | 声明方法不需要事务。如果方法没有关联到一个事务，容器不会为它开启事务。如果方法在一个事务中被调用，该事务会被挂起，在方法调用结束后，原先的事务便会恢复执行 |
| REQUIRESNEW   | 属性表明不管是否存在事务，业务方法总会为自己发起一个新的事务。如果方法已经运行在一个事务中，则原有事务会被挂起，新的事务会被创建，直到方法执行结束，新事务才算结束，原先的事务才会恢复执行 |
| MANDATORY     | 该属性指定业务方法只能在一个已经存在的事务中执行，业务方法不能发起自己的事务。如果业务方法在没有事务的环境下调用，容器就会抛出例外 |
| SUPPORTS      | 这一事务属性表明，如果业务方法在某个事务范围内被调用，则方法成为该事务的一部分。如果业务方法在事务范围外被调用，则方法在没有事务的环境下执行 |
| NEVER         | 指定业务方法绝对不能在事务范围内执行。如果业务方法在某个事务中执行，容器会抛出例外，只有业务方法没有关联到任何事务，才能正常执行 |
| NESTED        | 如果一个活动的事务存在，则运行在一个嵌套的事务中.，如果没有活动事务, 则按REQUIRED属性执行.它使用了一个单独的事务， 这个事务拥有多个可以回滚的保存点。内部事务的回滚不会对外部事务造成影响。它只对DataSourceTransactionManager事务管理器起效 |

## [6.5. 编程式事务的操作](https://jshand.gitee.io/#/course/12_spring/spring?id=_65-编程式事务的操作)

自己写代码通过TransactionDefinitionAPI控制事务，commit、rollback

## [6.6. 声明式的事务](https://jshand.gitee.io/#/course/12_spring/spring?id=_66-声明式的事务)

### [6.6.1. 需求：](https://jshand.gitee.io/#/course/12_spring/spring?id=_661-需求：)

在两个账户之前进行转账，A+20 B-20

| A    | 15.15 |
| ---- | ----- |
| B    | 80.15 |

成功的

```java
伪代码

{

   Update  A  +  20   执行sql就默认提交

  Update  B  -  20

}
```

| A    | 30.15 |
| ---- | ----- |
| B    | 60.15 |

失败的

```java
伪代码

{

Update  A  +  20   执行sql就默认提交

//此处有可能报错

Update  B  -  20

}
```

| A    | 30.15 |
| ---- | ----- |
| B    | 80.15 |

### [6.6.2. 搭建spring-jdbc-transcation工程，使用spring-jdbc连接数据库](https://jshand.gitee.io/#/course/12_spring/spring?id=_662-搭建spring-jdbc-transcation工程，使用spring-jdbc连接数据库)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B067175.png)

### [6.6.3. 添加依赖](https://jshand.gitee.io/#/course/12_spring/spring?id=_663-添加依赖)

添加tx模块，如果已经依赖过jdbc模块，可以不用单独引入。

```xml
<dependency>
    <groupId>junit</groupId>
    <artifactId>junit</artifactId>
    <version>4.12</version>
    <scope>test</scope>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-core</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-beans</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-context</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>

<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-expression</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>
<!-- https://mvnrepository.com/artifact/org.springframework/spring-core -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-test</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>


<!--Spring对JDBC封装-->
<!-- https://mvnrepository.com/artifact/org.springframework/spring-jdbc -->
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-jdbc</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>
<dependency>
    <groupId>org.springframework</groupId>
    <artifactId>spring-tx</artifactId>
    <version>5.2.4.RELEASE</version>
</dependency>

<!-- 数据源： https://mvnrepository.com/artifact/org.apache.commons/commons-dbcp2 -->
<dependency>
    <groupId>org.apache.commons</groupId>
    <artifactId>commons-dbcp2</artifactId>
    <version>2.7.0</version>
</dependency>



<!--msyql数据库的驱动-->
<dependency>
    <groupId>mysql</groupId>
    <artifactId>mysql-connector-java</artifactId>
    <version>5.1.46</version>
</dependency>



<!-- https://mvnrepository.com/artifact/org.aspectj/aspectjweaver -->
<dependency>
    <groupId>org.aspectj</groupId>
    <artifactId>aspectjweaver</artifactId>
    <version>1.9.5</version>
</dependency>
```

### [6.6.4. 声明AccountService接口](https://jshand.gitee.io/#/course/12_spring/spring?id=_664-声明accountservice接口)

定义转账的方法

```java
import java.sql.SQLException;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/25  9:12 25
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : 账户的接口类
 */
public interface AccountService {

    //转账
    public boolean transfer() throws SQLException;

} 
```

### [6.6.5. 声明AccountServiceImpl实现类](https://jshand.gitee.io/#/course/12_spring/spring?id=_665-声明accountserviceimpl实现类)

定义转账的方法

```java
import org.springframework.jdbc.core.JdbcTemplate;

import java.sql.SQLException;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/25  9:13 25
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     : 账户service层的操作的实现类
 */
public class AccountServiceImpl implements AccountService {

    private JdbcTemplate jdbcTemplate;

    /**
     * 转账的功能
     * @return
     */
    @Override
    public boolean transfer() throws SQLException {
        //给张三   +100
        //给李四  -100
//        System.out.println(jdbcTemplate);
//        System.out.println(jdbcTemplate.getDataSource());
//        System.out.println(jdbcTemplate.getDataSource().getConnection());

        String sql1 = "update t_stu_account set amount = amount + 20 where account_id = 1";
        jdbcTemplate.update(sql1);

//逻辑操作，中间可能出BUG，导致后面的代码无法执,如果没有事务造成上述sql执行成功，而下面的sql不执行。

        String sql2 = "update t_stu_account set amount = amount - 20 where account_id = 2";
        jdbcTemplate.update(sql2);

        return true;
    }


    public JdbcTemplate getJdbcTemplate() {
        return jdbcTemplate;
    }

    public void setJdbcTemplate(JdbcTemplate jdbcTemplate) {
        this.jdbcTemplate = jdbcTemplate;
    }
}
```

### [6.6.6. 使用容器管理service的Bean](https://jshand.gitee.io/#/course/12_spring/spring?id=_666-使用容器管理service的bean)

声明bean 并注入jdbctemple（注入datasouece）

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource"
        p:username="root"
        p:password="root"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mybatis?useUnicode=true&unic" >

    </bean>

    <bean id="jt" class="org.springframework.jdbc.core.JdbcTemplate"
        p:dataSource-ref="dataSource" >
    </bean>

    <bean id="accountService" class="com.neuedu.service.AccountServiceImpl">
        <property name="jdbcTemplate" ref="jt"/>
    </bean>

</beans>
```

### [6.6.7. 使用service操作数据库(单元测试)](https://jshand.gitee.io/#/course/12_spring/spring?id=_667-使用service操作数据库单元测试)

```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.sql.SQLException;

import static org.junit.Assert.*;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/25  9:17 25
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     :
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:spring-transcation.xml")
public class AccountServiceImplTest {

    @Autowired
    AccountService accountService;

    @Test
    public void transfer() throws SQLException {
        accountService.transfer();
    }
}
```

### [6.6.8. Xml声明事务](https://jshand.gitee.io/#/course/12_spring/spring?id=_668-xml声明事务)

#### [6.6.8.1. 在xml中声明tx标签的命名空间](https://jshand.gitee.io/#/course/12_spring/spring?id=_6681-在xml中声明tx标签的命名空间)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B072779.png)

```xml
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd">
```

#### [6.6.8.2. 在IOC容器中添加事务管理器](https://jshand.gitee.io/#/course/12_spring/spring?id=_6682-在ioc容器中添加事务管理器)

```xml
<!--    声明事务管理器(通知的实现代码)
    DataSourceTransactionManager :纯jdbc、jdbctem、Mybatis都是使用
-->
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

#### [6.6.8.3. 在切面中应用事务的通知（配置事务切面）](https://jshand.gitee.io/#/course/12_spring/spring?id=_6683-在切面中应用事务的通知（配置事务切面）)

事务通知的方法标签属性

- name*：匹配方法名*
- *propagation*：事*务*的*传*播特性（*Spring*独*有）*
  - REQUIRED 必*须*包含在一*个*事*务*中，如果有*则*正常*执*行，如果*没*有需要容器*帮*助*开启*事*务*
  - REQUIRED_New 必*须单独*再*开启*事*务*
- *no-rollback-for="java.lang.ArithmeticException" 指定特定的*类*型 不*会滚*（依然提交）*
- *rollback-for="" 指定特殊*异*常*进*行回*滚
- *isolation 设*置事*务*的隔*离级别 默*认*是*DEFAULT 根据*数*据*库决*定*
- *read-only 指定在*查询*的方法上只用，不*开启*事*务

```xml
<!--配置事务的通知-->
<tx:advice id="txAdvice" transaction-manager="txManager">
    <!--配置不同的方法如何应用事务的传播特性-->
    <tx:attributes>
        <!--REQUIRED 必须包含在一个事务中，如果有则正常执行，如果没有需要容器帮助开启事务
            REQUIRED_New  必须单独再开启事务
        -->
        <tx:method name="transfer" propagation="REQUIRED"/>
        <tx:method name="insert*" propagation="REQUIRED"/>
        <!--对于select、query事务并不是必须的，已经包含在事务中可以正常执行，如果没有事务也不用开启可以正常执行-->
        <tx:method name="select*" propagation="SUPPORTS"/>
        <tx:method name="query*" propagation="SUPPORTS"/>
    </tx:attributes>

</tx:advice>



<!--配置切面 应用事务的通知-->
<aop:config>
    <!--切点 想在哪个方法上添加事务 -->
    <aop:pointcut id="serviceMethod" expression="execution(* com.neuedu.service.*.*(..))"/>
    <aop:advisor advice-ref="txAdvice" pointcut-ref="serviceMethod"/>
</aop:config>
```

#### [6.6.8.4. 测试](https://jshand.gitee.io/#/course/12_spring/spring?id=_6684-测试)

1）中间产生异常，数据不提交，两条数据都不发生变化，

2）没有异常，数据正常的更新（2条数据都更新）

```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.sql.SQLException;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/25  9:17 25
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     :
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:spring-transcation.xml")
public class AccountServiceImplTest {

    @Autowired
    AccountService accountService;


    @Test
    public void transfer() throws SQLException {
        accountService.transfer();
    }
}
```

### [6.6.9. @Transcational注解的形式声明事务](https://jshand.gitee.io/#/course/12_spring/spring?id=_669-transcational注解的形式声明事务)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B075459.png)

#### [6.6.9.1. 声明service层代码，入住jdbctemplate（需要数据源）](https://jshand.gitee.io/#/course/12_spring/spring?id=_6691-声明service层代码，入住jdbctemplate（需要数据源）)

#### [6.6.9.2. 声明事务管理器(txManager)](https://jshand.gitee.io/#/course/12_spring/spring?id=_6692-声明事务管理器txmanager)

```xml
<bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
    <property name="dataSource" ref="dataSource"/>
</bean>
```

#### [6.6.9.3. annotation-driven](https://jshand.gitee.io/#/course/12_spring/spring?id=_6693-annotation-driven)

将tx:advice标签和aop：config标签删除了，使用annotation-driven标签替换

```
<tx:annotation-driven transaction-manager="txManager"/>
```

#### [6.6.9.4. 单元测试](https://jshand.gitee.io/#/course/12_spring/spring?id=_6694-单元测试)

有异常的，都不提交，

没有异常，同时成功提交

```java
import org.junit.Test;
import org.junit.runner.RunWith;
import org.springframework.beans.factory.annotation.Autowired;
import org.springframework.test.context.ContextConfiguration;
import org.springframework.test.context.junit4.SpringJUnit4ClassRunner;

import java.sql.SQLException;

/**
 * 项目    ： spring-java1
 * 创建时间 ：2020/3/25  9:17 25
 * author  ：张金山
 * site    :   https://jshand.gitee.io
 * 描述     :
 */
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration(locations = "classpath:spring-transcation-annotation.xml")
public class AccountServiceImplTest {

    @Autowired
    AccountService accountService;


    @Test
    public void transfer() throws SQLException {
        accountService.transfer();
    }
}
```

#### [6.6.9.5. 使用](https://jshand.gitee.io/#/course/12_spring/spring?id=_6695-使用)

在需要添加事物的方法上使用，@Transactional(propagation = Propagation.REQUIRED)

![img](https://jshand.gitee.io/imgs/spring/Spring%E7%AC%94%E8%AE%B076799.png)

#### [6.6.9.6. 完成的xml](https://jshand.gitee.io/#/course/12_spring/spring?id=_6696-完成的xml)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:tx="http://www.springframework.org/schema/tx"
       xmlns:aop="http://www.springframework.org/schema/aop"

       xsi:schemaLocation="http://www.springframework.org/schema/beans
        https://www.springframework.org/schema/beans/spring-beans.xsd
        http://www.springframework.org/schema/tx
        http://www.springframework.org/schema/tx/spring-tx.xsd
        http://www.springframework.org/schema/aop
        https://www.springframework.org/schema/aop/spring-aop.xsd">


    <bean id="dataSource" class="org.apache.commons.dbcp2.BasicDataSource"
          p:username="root"
          p:password="root"
          p:driverClassName="com.mysql.jdbc.Driver"
          p:url="jdbc:mysql://localhost:3306/mybatis?useUnicode=true&unic">

    </bean>

    <bean id="jt" class="org.springframework.jdbc.core.JdbcTemplate"
          p:dataSource-ref="dataSource">
    </bean>

    <bean id="accountService" class="com.neuedu.service.AccountServiceImpl">
        <property name="jdbcTemplate" ref="jt"/>
    </bean>

    <!--    声明事务管理器(通知的实现代码)
        DataSourceTransactionManager :纯jdbc、jdbctem、Mybatis都是使用
    -->
    <bean id="txManager" class="org.springframework.jdbc.datasource.DataSourceTransactionManager">
        <property name="dataSource" ref="dataSource"/>
    </bean>

<!-- 让注解生效，注解驱动 -->
    <tx:annotation-driven transaction-manager="txManager"/>

</beans>
```