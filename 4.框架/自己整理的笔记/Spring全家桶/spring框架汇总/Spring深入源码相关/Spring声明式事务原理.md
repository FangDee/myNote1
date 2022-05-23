## Spring声明式事务原理

 2020-06-14 |  Visit count

在[Spring-事务管理](https://mrbird.cc/Spring-事务管理.html)一节中，我们了解了在Spring中如何方便的管理数据库事务，并了解了一些和事务相关的专业术语。本节将通过一个简单的例子回顾Spring声明式事务的使用，并通过源码解读内部实现原理，最后通过列举一些常见事务不生效的场景来加深对Spring事务原理的理解。



## 事务例子回顾

新建SpringBoot项目，Boot版本2.4.0，然后引入如下依赖：

```xml
<dependencies>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter</artifactId>
    </dependency>
    <dependency>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-jdbc</artifactId>
    </dependency>
    <dependency>
        <groupId>mysql</groupId>
        <artifactId>mysql-connector-java</artifactId>
    </dependency>
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>2.1.2</version>
    </dependency>
</dependencies>
```



引入了JDBC、MySQL驱动和mybatis等依赖。

然后在Spring入口类上加上`@EnableTransactionManagement`注解，以开启事务：

```java
@EnableTransactionManagement
@SpringBootApplication
public class TransactionApplication {

    public static void main(String[] args) throws Exception {
        SpringApplication.run(TransactionApplication.class, args);
    }
}
```



接着新建名称为test的MySQL数据库，并创建USER表：

```sql
CREATE TABLE `USER` (
  `USER_ID` varchar(10) NOT NULL COMMENT '用户ID',
  `USERNAME` varchar(10) DEFAULT NULL COMMENT '用户名',
  `AGE` varchar(3) DEFAULT NULL COMMENT '用户年龄',
  PRIMARY KEY (`USER_ID`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```



其中USER_ID字段非空。

在application.properties配置中添加数据库相关配置：

```yaml
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.url=jdbc:mysql://127.0.0.1:3306/test?useUnicode=true&characterEncoding=UTF-8&useJDBCCompliantTimezoneShift=true&useLegacyDatetimeCode=false&serverTimezone=GMT%2b8
```



创建USER表对应实体类User：

```java
public class User implements Serializable {

    private String userId;
    private String username;
    private String age;

    public User(String userId, String username, String age) {
        this.userId = userId;
        this.username = username;
        this.age = age;
    }

    public User() {
    }

    public String getUserId() {
        return userId;
    }

    public void setUserId(String userId) {
        this.userId = userId;
    }

    public String getUsername() {
        return username;
    }

    public void setUsername(String username) {
        this.username = username;
    }

    public String getAge() {
        return age;
    }

    public void setAge(String age) {
        this.age = age;
    }
}
```



创建UserMapper：

```java
@Mapper
public interface UserMapper {

    @Insert("insert into user(user_id,username,age) values(#{userId},#{username},#{age})")
    void save(User user);
}
```



包含一个新增用户的方法save。

创建Service接口UserService：

```java
public interface UserService {

    void saveUser(User user);

}
```



其实现类UserServiceImpl：

```java
@Service
public class UserServiceImpl implements UserService {

    private final UserMapper userMapper;

    public UserServiceImpl(UserMapper userMapper) {
        this.userMapper = userMapper;
    }

    @Transactional
    @Override
    public void saveUser(User user) {
        userMapper.save(user);
        // 测试事务回滚
        if (!StringUtils.hasText(user.getUsername())) {
            throw new RuntimeException("username不能为空");
        }
    }
}
```



在SpringBoot的入口类中测试一波：

```java
@EnableTransactionManagement
@SpringBootApplication
public class TransactionApplication {

    public static void main(String[] args) {
        ConfigurableApplicationContext context = SpringApplication.run(TransactionApplication.class, args);
        UserService userService = context.getBean(UserService.class);
        User user = new User("1", null, "18");
        userService.saveUser(user);
    }
}
```



如果事务生效的话，这条数据将不会被插入到数据库中，运行程序后，查看库表：

![QQ20201221-154216@2x](https://mrbird.cc/img/QQ20201221-154216@2x.png)

可以看到数据并没有被插入，说明事务控制成功。

我们注释掉UserServiceImpl的saveUser方法上的`@Transactional`注解，重新运行程序，查看库表：

![QQ20201221-154521@2x](https://mrbird.cc/img/QQ20201221-154521@2x.png)

可以看到数据被插入到数据库中了，事务控制失效。

## 事务原理

### @EnableTransactionManagement

上面例子中，我们通过模块驱动注解`@EnableTransactionManagement`开启了事务管理功能，查看其源码：

![2020年12月21日15-53-04](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A515-53-04.png)

接着查看TransactionManagementConfigurationSelector的源码：

![2020年12月21日15-57-32](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A515-57-32.png)

对通过Selector往IOC容器中导入组件不熟悉的读者可以参考[深入学习Spring组件注册](https://mrbird.cc/Spring-Bean-Regist.html)。

所以接下来我们重点关注AutoProxyRegistrar和ProxyTransactionManagementConfiguration的逻辑即可。

### AutoProxyRegistrar

查看AutoProxyRegistrar的源码：

![2020年12月21日16-11-23](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A516-11-23.png)

查看`AopConfigUtils.registerAutoProxyCreatorIfNecessary(registry)`源码：

![2020年12月21日16-15-50](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A516-15-50.png)

查看`InfrastructureAdvisorAutoProxyCreator`的层级关系图：

![QQ20201221-162016@2x](https://mrbird.cc/img/QQ20201221-162016@2x.png)

这和[深入理解Spring-AOP原理](https://mrbird.cc/深入理解Spring-AOP原理.html)一文中的AnnotationAwareAspectJAutoProxyCreator的层级关系图一致，所以我们可以推断出InfrastructureAdvisorAutoProxyCreator的作用为：为目标Service创建代理对象，增强目标Service方法，用于事务控制。

### ProxyTransactionManagementConfiguration

查看ProxyTransactionManagementConfiguration源码：

![2020年12月21日16-57-15](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A516-57-15.png)

1. 注册BeanFactoryTransactionAttributeSourceAdvisor增强器，该增强器需要如下两个Bean：

   - TransactionAttributeSource
   - TransactionInterceptor

2. 注册TransactionAttributeSource：

   ![QQ20201221-170327@2x](https://mrbird.cc/img/QQ20201221-170327@2x.png)

   方法体内部创建了一个类型为AnnotationTransactionAttributeSource的Bean，查看其源码：

   ![2020年12月21日17-02-22](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A517-02-22.png)

   查看SpringTransactionAnnotationParser源码：

   ![2020年12月21日17-06-16](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A517-06-16.png)

3. 注册TransactionInterceptor事务拦截器：

   ![QQ20201221-170716@2x](https://mrbird.cc/img/QQ20201221-170716@2x.png)

   查看TransactionInterceptor源码，其实现了MethodInterceptor方法拦截器接口，在[深入理解Spring-AOP原理](https://mrbird.cc/深入理解Spring-AOP原理.html)一文中曾介绍过，MethodBeforeAdviceInterceptor、AspectJAfterAdvice、AfterReturningAdviceInterceptor和AspectJAfterThrowingAdvice等增强器都是MethodInterceptor的实现类，目标方法执行的时候，对应拦截器的invoke方法会被执行，所以重点关注TransactionInterceptor实现的invoke方法：

   ![2020年12月21日17-14-11](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A517-14-11.png)

   查看invokeWithinTransaction方法源码：

   ![2020年12月21日18-33-23](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A518-33-23.png)

completeTransactionAfterThrowing源码如下：

![2020年12月21日18-41-49](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A518-41-49.png)

这里，假如没有在@Transactional注解上指定回滚的异常类型的话，默认只对RunTimeExcetion和Error类型异常进行回滚：

![2020年12月21日18-43-45](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A518-43-45.png)

commitTransactionAfterReturning源码如下：

![2020年12月21日18-45-46](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A518-45-46.png)

### debug验证

重新打开UserServiceImpl的saveUser方法上的`@Transactional`注解，然后在如下所示位置打个断点：

![2020年12月21日18-50-14](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A518-50-14.png)

以debug的方式启动程序：

![QQ20201221-185241@2x](https://mrbird.cc/img/QQ20201221-185241@2x.png)

可以看到目标对象已经被JDK代理（目标对象实现了接口，默认走JDK动态代理。可以通过spring.aop.proxy-target-class=true配置来强制使用cglib代理，需要额外引入AOP自动装配依赖）。

在断点处执行Step Into，程序跳转到JdkDynamicAopProxy的invoke方法：

![2020年12月21日19-01-32](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A519-01-32.png)

![2020年12月21日19-05-09](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A519-05-09.png)

程序跳转到TransactionInterceptor的invoke方法：

![2020年12月21日19-06-43](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A519-06-43.png)

![2020年12月21日19-09-45](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8821%E6%97%A519-09-45.png)

可以看到整个过程和[深入理解Spring-AOP原理](https://mrbird.cc/深入理解Spring-AOP原理.html)一文介绍的一致。

## 事务不生效场景

对Spring事务机制不熟悉的coder经常会遇到事务不生效的场景，这里列举两个最为常见的场景，并给出对应的解决方案。

### 场景一

Service方法抛出的异常不是RuntimeException或者Error类型，并且@Transactional注解上没有指定回滚异常类型。

对应的代码例子为：

```java
@Service
public class UserServiceImpl implements UserService {

    private final UserMapper userMapper;

    public UserServiceImpl(UserMapper userMapper) {
        this.userMapper = userMapper;
    }

    @Transactional
    @Override
    public void saveUser(User user) throws Exception {
        userMapper.save(user);
        // 测试事务回滚
        if (!StringUtils.hasText(user.getUsername())) {
            throw new Exception("username不能为空");
        }
    }
}
```



这冲情况下，Spring并不会进行事务回滚操作。

正如@Transactional注解源码注释所述的那样：

![QQ20201221-191914@2x](https://mrbird.cc/img/QQ20201221-191914@2x.png)

**默认情况下，Spring事务只对RuntimeException或者Error类型异常（错误）进行回滚，检查异常（通常为业务类异常）不会导致事务回滚。**。

所以要解决上面这个事务不生效的问题，我们主要有以下两种方式：

1. 手动在@Transactional注解上声明回滚的异常类型（方法抛出该异常及其所有子类型异常都能触发事务回滚）：

   ```java
   @Service
   public class UserServiceImpl implements UserService {
   
       private final UserMapper userMapper;
   
       public UserServiceImpl(UserMapper userMapper) {
           this.userMapper = userMapper;
       }
   
       @Transactional(rollbackFor = Exception.class)
       @Override
       public void saveUser(User user) throws Exception {
           userMapper.save(user);
           // 测试事务回滚
           if (!StringUtils.hasText(user.getUsername())) {
               throw new Exception("username不能为空");
           }
       }
   }
   ```

2. 方法内手动抛出的检查异常类型改为RuntimeException子类型：

   定义一个自定义异常类型ParamInvalidException：

   ```java
   public class ParamInvalidException extends RuntimeException{
   
       public ParamInvalidException(String message) {
           super(message);
       }
   }
   ```

   修改UserServiceImpl的saveUser方法：

   ```java
   @Service
   public class UserServiceImpl implements UserService {
   
       private final UserMapper userMapper;
   
       public UserServiceImpl(UserMapper userMapper) {
           this.userMapper = userMapper;
       }
   
       @Transactional
       @Override
       public void saveUser(User user) {
           userMapper.save(user);
           // 测试事务回滚
           if (!StringUtils.hasText(user.getUsername())) {
               throw new ParamInvalidException("username不能为空");
           }
       }
   }
   ```

这两种方式都能让事务按照我们的预期生效。

### 场景二

非事务方法直接通过this调用本类事务方法。这种情况也是比较常见的，举个例子，修改UserServiceImpl：

```java
@Service
public class UserServiceImpl implements UserService {

    private final UserMapper userMapper;

    public UserServiceImpl(UserMapper userMapper) {
        this.userMapper = userMapper;
    }

    @Override
    public void saveUserTest(User user) {
        this.saveUser(user);
    }

    @Transactional
    @Override
    public void saveUser(User user) {
        userMapper.save(user);
        // 测试事务回滚
        if (!StringUtils.hasText(user.getUsername())) {
            throw new ParamInvalidException("username不能为空");
        }
    }
}
```



在UserServiceImpl中，我们新增了saveUserTest方法，该方法没有使用@Transactional注解标注，为非事务方法，内部直接调用了saveUser事务方法。

在入口类里测试该方法的调用：

```java
@EnableTransactionManagement
@SpringBootApplication
public class TransactionApplication {

    public static void main(String[] args) throws Exception {
        ConfigurableApplicationContext context = SpringApplication.run(TransactionApplication.class, args);
        UserService userService = context.getBean(UserService.class);
        User user = new User("2", null, "28");
        userService.saveUserTest(user);
    }
}
```

启动程序，观察数据库数据：

![QQ20201222-091609@2x](https://mrbird.cc/img/QQ20201222-091609@2x.png)

可以看到，事务并没有回滚，数据已经被插入到了数据库中。

**这种情况下事务失效的原因为：Spring事务控制使用AOP代理实现，通过对目标对象的代理来增强目标方法。而上面例子直接通过this调用本类的方法的时候，this的指向并非代理类，而是该类本身。**

使用debug来验证this是否为代理对象：

![2020年12月22日09-23-04](https://mrbird.cc/img/2020%E5%B9%B412%E6%9C%8822%E6%97%A509-23-04.png)

这种情况下要让事务生效主要有如下两种解决方式（原理都是使用代理对象来替代this）：

1. 从IOC容器中获取UserService Bean，然后调用它的saveUser方法：

```java
@Service
public class UserServiceImpl implements UserService, ApplicationContextAware {

    private final UserMapper userMapper;
    private ApplicationContext context;

    public UserServiceImpl(UserMapper userMapper) {
        this.userMapper = userMapper;
    }

    @Override
    public void saveUserTest(User user) {
        UserService userService = context.getBean(UserService.class);
        userService.saveUser(user);
    }

    @Transactional
    @Override
    public void saveUser(User user) {
        userMapper.save(user);
        // 测试事务回滚
        if (!StringUtils.hasText(user.getUsername())) {
            throw new ParamInvalidException("username不能为空");
        }
    }

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.context = applicationContext;
    }
}
```

上面代码我们通过实现ApplicationContextAware接口注入了应用上下文ApplicationContext，然后从中取出UserService Bean来代替this。

1. 从AOP上下文中取出当前代理对象：

   这种情况首先需要引入AOP Starter：

   ```xml
   <dependency>
       <groupId>org.springframework.boot</groupId>
       <artifactId>spring-boot-starter-aop</artifactId>
   </dependency>
   ```

   然后在SpringBoot入口类中通过注解@EnableAspectJAutoProxy(exposeProxy = true)将当前代理对象暴露到AOP上下文中（通过AopContext的ThreadLocal实现）。

   最后在UserServcieImpl的saveUserTest方法中通过AopContext获取UserServce的代理对象：

   ```java
   @Service
   public class UserServiceImpl implements UserService {
   
       private final UserMapper userMapper;
   
       public UserServiceImpl(UserMapper userMapper) {
           this.userMapper = userMapper;
       }
   
       @Override
       public void saveUserTest(User user) {
           UserService userService = (UserService) AopContext.currentProxy();
           userService.saveUser(user);
       }
   
       @Transactional
       @Override
       public void saveUser(User user) {
           userMapper.save(user);
           // 测试事务回滚
           if (!StringUtils.hasText(user.getUsername())) {
               throw new ParamInvalidException("username不能为空");
           }
       }
   
   }
   ```


 