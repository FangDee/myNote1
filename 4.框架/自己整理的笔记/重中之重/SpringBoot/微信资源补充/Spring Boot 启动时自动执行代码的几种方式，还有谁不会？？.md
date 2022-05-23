# Spring Boot 启动时自动执行代码的几种方式，还有谁不会？？

### 前言

目前开发的SpringBoot项目在启动的时候需要预加载一些资源。而如何实现启动过程中执行代码，或启动成功后执行，是有很多种方式可以选择，我们可以在static代码块中实现，也可以在构造方法里实现，也可以使用`@PostConstruct`注解实现。

当然也可以去实现Spring的`ApplicationRunner`与`CommandLineRunner`接口去实现启动后运行的功能。在这里整理一下，在这些位置执行的区别以及加载顺序。

### java自身的启动时加载方式

##### static代码块

static静态代码块，在类加载的时候即自动执行。

##### 构造方法

在对象初始化时执行。执行顺序在static静态代码块之后。

### Spring启动时加载方式

##### @PostConstruct注解

PostConstruct注解使用在方法上，这个方法在对象依赖注入初始化之后执行。

##### ApplicationRunner和CommandLineRunner

SpringBoot提供了两个接口来实现Spring容器启动完成后执行的功能，两个接口分别为`CommandLineRunner`和`ApplicationRunner`。

这两个接口需要实现一个run方法，将代码在run中实现即可。这两个接口功能基本一致，其区别在于run方法的入参。`ApplicationRunner`的run方法入参为`ApplicationArguments`，为`CommandLineRunner`的run方法入参为String数组。

##### 何为ApplicationArguments

官方文档解释为：

> Provides access to the arguments that were used to run a SpringApplication.

在Spring应用运行时使用的访问应用参数。即我们可以获取到`SpringApplication.run(…)`的应用参数。最新 Spring Boot 面试题整理好了，点击[Java面试库](https://mp.weixin.qq.com/s?__biz=MzUyNDc0NjM0Nw==&mid=2247498458&idx=1&sn=b1ff43161f69d4ecea13f15d624c23b8&chksm=fa2a11decd5d98c86ce8c5db7cf1ac805c16bffb99c17a05c9cacb7883d28646e1188d159d31&mpshare=1&scene=1&srcid=0325zlBc4qyKI3uFLeA6EYSR&sharer_sharetime=1648196351788&sharer_shareid=30f99e6584d9264f0597aaec71172505&key=3e96c54cb1a2950ee3c6bb55faf7337c1a6eeb247be295e4f59f9216a29a3ddd2a63a87838d0cee8b2af2045c074c779ddeebb24525e368fe1a6b90fa5490af06bc3380ce1397dddd4fbbe776cce0f531cf8b4ffb4080ddb3078cb59eae1a170f5ed9dcc6258fd4d3346fc56f33f507b87ef3b46aee2df6e60ff6f8da6c2ddcf&ascene=1&uin=MjIyMzM5NDIzMg%3D%3D&devicetype=Windows+10+x64&version=6305002e&lang=zh_CN&exportkey=A%2FMOib%2B4f9FSI2AwAC3hazA%3D&acctmode=0&pass_ticket=S3vQ7PBxrRDdlbqXhuXglsXvAYV9gvE19o11t1n1i%2Fe3suZl%2F5n4D4mFsgMJaEz%2F&wx_header=0&fontgear=2)小程序在线刷题。

##### Order注解

当有多个类实现了`CommandLineRunner`和`ApplicationRunner`接口时，可以通过在类上添加@Order注解来设定运行顺序。

### 代码测试

为了测试启动时运行的效果和顺序，编写几个测试代码来运行看看。

Spring Boot 基础就不介绍了，推荐下这个实战教程：https://github.com/javastacks/spring-boot-best-practice

[**TestPostConstruct**](http://mp.weixin.qq.com/s?__biz=MzUyNDc0NjM0Nw==&mid=2247492574&idx=2&sn=f27a39ad8bf4540785d08d7d4be889df&chksm=fa2a08dacd5d81cc3b043fcf01b6b0d9f12e0ed43f02a97c0941c5d325d989c6af5fb0276dc7&scene=21#wechat_redirect)

```java
@Component
public class TestPostConstruct {

    static {
        System.out.println("static");
    }
    public TestPostConstruct() {
        System.out.println("constructer");
    }

    @PostConstruct
    public void init() {
        System.out.println("PostConstruct");
    }
}
```

[TestApplicationRunner](http://mp.weixin.qq.com/s?__biz=MzUyNDc0NjM0Nw==&mid=2247492574&idx=2&sn=f27a39ad8bf4540785d08d7d4be889df&chksm=fa2a08dacd5d81cc3b043fcf01b6b0d9f12e0ed43f02a97c0941c5d325d989c6af5fb0276dc7&scene=21#wechat_redirect)

```
@Component
@Order(1)
public class TestApplicationRunner implements ApplicationRunner{
    @Override
    public void run(ApplicationArguments applicationArguments) throws Exception {
        System.out.println("order1:TestApplicationRunner");
    }
}
```

[TestCommandLineRunner](http://mp.weixin.qq.com/s?__biz=MzUyNDc0NjM0Nw==&mid=2247492574&idx=2&sn=f27a39ad8bf4540785d08d7d4be889df&chksm=fa2a08dacd5d81cc3b043fcf01b6b0d9f12e0ed43f02a97c0941c5d325d989c6af5fb0276dc7&scene=21#wechat_redirect)

```java
@Component
@Order(2)
public class TestCommandLineRunner implements CommandLineRunner {
    @Override
    public void run(String... strings) throws Exception {java
        System.out.println("order2:TestCommandLineRunner");
    }
}
```

[执行结果](http://mp.weixin.qq.com/s?__biz=MzUyNDc0NjM0Nw==&mid=2247492574&idx=2&sn=f27a39ad8bf4540785d08d7d4be889df&chksm=fa2a08dacd5d81cc3b043fcf01b6b0d9f12e0ed43f02a97c0941c5d325d989c6af5fb0276dc7&scene=21#wechat_redirect)

![图片](https://mmbiz.qpic.cn/mmbiz_png/mR4CwoLXicg39JnAewJBFS5nIfDvgYmu76nicgZMxH1vibficlHc3LQLFjxSuJNicS8Vl2rYsFibYT2RhMznTBrURJJw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 总结

Spring应用启动过程中，肯定是要自动扫描有`@Component`注解的类，加载类并初始化对象进行自动注入。加载类时首先要执行static静态代码块中的代码，之后再初始化对象时会执行构造方法。最新 Spring Boot 面试题整理好了，点击[Java面试库](https://mp.weixin.qq.com/s?__biz=MzUyNDc0NjM0Nw==&mid=2247498458&idx=1&sn=b1ff43161f69d4ecea13f15d624c23b8&chksm=fa2a11decd5d98c86ce8c5db7cf1ac805c16bffb99c17a05c9cacb7883d28646e1188d159d31&mpshare=1&scene=1&srcid=0325zlBc4qyKI3uFLeA6EYSR&sharer_sharetime=1648196351788&sharer_shareid=30f99e6584d9264f0597aaec71172505&key=3e96c54cb1a2950ee3c6bb55faf7337c1a6eeb247be295e4f59f9216a29a3ddd2a63a87838d0cee8b2af2045c074c779ddeebb24525e368fe1a6b90fa5490af06bc3380ce1397dddd4fbbe776cce0f531cf8b4ffb4080ddb3078cb59eae1a170f5ed9dcc6258fd4d3346fc56f33f507b87ef3b46aee2df6e60ff6f8da6c2ddcf&ascene=1&uin=MjIyMzM5NDIzMg%3D%3D&devicetype=Windows+10+x64&version=6305002e&lang=zh_CN&exportkey=A%2FMOib%2B4f9FSI2AwAC3hazA%3D&acctmode=0&pass_ticket=S3vQ7PBxrRDdlbqXhuXglsXvAYV9gvE19o11t1n1i%2Fe3suZl%2F5n4D4mFsgMJaEz%2F&wx_header=0&fontgear=2)小程序在线刷题。

在对象注入完成后，调用带有`@PostConstruct`注解的方法。当容器启动成功后，再根据@Order注解的顺序调用`CommandLineRunner`和`ApplicationRunner`接口类中的run方法。

因此，加载顺序为`static`>`constructer`>`@PostConstruct`>`CommandLineRunner`和`ApplicationRunner`.