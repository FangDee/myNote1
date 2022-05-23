# Spring Aop 常见注解和执行顺序

点击关注 👉 [编程技术圈](javascript:void(0);) *2022-03-13 00:00*

**👇推荐大家关注一个公众号👇**

![img](http://mmbiz.qpic.cn/mmbiz_png/O6eemzuFHZzX3ib6MsHh9VS01eaJghDmfgH1J5JuddibkmuzDEXBZicVkKILMic2ZRC7HWlqmwBWMCpvAFHhPtZYkA/0?wx_fmt=png)



What is adhere to? Is day, and one day, you tell yourself, insist again one day.

什么是坚持？就是一天，又一天，你告诉自己，再坚持一天。

**每日掏心话**

你勇敢，世界就会让步。如果有时它战胜你，你要不断地勇敢再勇敢，它就会屈服。

Spring 一开始最强大的就是 IOC / AOP 两大核心功能，我们今天一起来学习一下 Spring AOP 常见注解和执行顺序。Spring Aop 的常用注解

首先我们一起来回顾一下 Spring Aop 中常用的几个注解：

- `@Before` 前置通知：目标方法之前执行
- `@After` 后置通知：目标方法之后执行（始终执行）
- `@AfterReturning` 返回之后通知：执行方法结束之前执行（异常不执行）
- `@AfterThrowing` 异常通知：出香异常后执行
- `@Around` 环绕通知：环绕目标方法执行

## 常见问题

1、你肯定知道 Spring  , 那说说 Aop 的去全部通知顺序， Spring Boot 或者 Spring Boot 2 对 aop 的执行顺序影响？

2、说说你在 AOP 中遇到的那些坑？

## 示例代码

下面我们先快速构建一个 spring aop 的 demo 程序来一起讨论 spring aop 中的一些细节。

### 配置文件

为了方便我直接使用 spring-boot 进行快速的项目搭建，大家可以使用 idea 的spring-boot 项目快速创建功能，或者去 `start.spring.io` 上面去快速创建spring-boot 应用。

> 因为本人经常手动去网上贴一些依赖导致，依赖冲突服务启动失败等一些问题。

```
plugins {
    id 'org.springframework.boot' version '2.6.3'
    id 'io.spring.dependency-management' version '1.0.11.RELEASE'
    id 'java'
}

group 'io.zhengsh'
version '1.0-SNAPSHOT'

repositories {
    mavenCentral()
    maven { url 'https://repo.spring.io/milestone' }
    maven { url 'https://repo.spring.io/snapshot' }
}

dependencies {
    # 其实这里也可以不增加 web 配置，为了试验简单，大家请忽略 
    implementation 'org.springframework.boot:spring-boot-starter-web'
    implementation 'org.springframework.boot:spring-boot-starter-actuator'
    implementation 'org.springframework.boot:spring-boot-starter-aop'
    
    testImplementation 'org.springframework.boot:spring-boot-starter-test'
}

tasks.named('test') {
    useJUnitPlatform()
}
```

### 接口类

首先我们需要定义一个接口。我们这里可以再来回顾一下 JDK 的默认代理实现的选择：

- 如果目标对象实现了接口，则默认采用JDK动态代理
- 如果目标对象没有实现接口，则采用进行动态代理
- 如果目标对象实现了接口，且强制Cglib，则使用cglib代理

这块的逻辑在 `DefaultAopProxyFactory` 大家有兴趣可以去看看。

```
public interface CalcService {

    public int div(int x, int y);
}
```

### 实现类

这里我门就简单一点做一个除法操作，可以模拟正常也可以很容易的模拟错误。

```
@Service
public class CalcServiceImpl implements CalcService {

    @Override
    public int div(int x, int y) {
        int result = x / y;
        System.out.println("====> CalcServiceImpl 被调用了，我们的计算结果是：" + result);
        return result;
    }
}
```

### aop 拦截器

申明一个拦截器我们要为当前对象增加 @Aspect 和 @Component ，笔者之前也是才踩过这样的坑，只加了一个。

其实这块我刚开始也不是很理解，但是我看了 Aspect 注解的定义我就清楚了

![图片](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbuc0uXnf8CM85qryZ4sy87oKYHhOCu9KPIP87nsvWoZr7UveD0FXVXiamib3cfIWGksxmSV8S1XXtPCA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

这里面根本就没有 Bean 的定义。所以我们还是乖乖的加上两个注解。

还有就是如果当测试的时候需要开启Aop 的支持为配置类上增加`@EnableAspectJAutoProxy` 注解。另外，搜索公众号Linux中文社区后台回复“私房菜”，获取一份惊喜礼包。

其实 Aop 使用就三个步骤：

- 定义 Aspect 定义切面
- 定义 Pointcut 就是定义我们切入点
- 定义具体的通知，比如: @After, @Before 等。

```
@Aspect
@Component
public class MyAspect {

    @Pointcut("execution(* io.zhengsh.spring.service.impl..*.*(..))")
    public void divPointCut() {

    }

    @Before("divPointCut()")
    public void beforeNotify() {
        System.out.println("----===>> @Before 我是前置通知");
    }

    @After("divPointCut")
    public void afterNotify() {
        System.out.println("----===>> @After  我是后置通知");
    }

    @AfterReturning("divPointCut")
    public void afterReturningNotify() {
        System.out.println("----===>> @AfterReturning 我是前置通知");
    }

    @AfterThrowing("divPointCut")
    public void afterThrowingNotify() {
        System.out.println("----===>> @AfterThrowing 我是异常通知");
    }

    @Around("divPointCut")
    public Object around(ProceedingJoinPoint proceedingJoinPoint) throws Throwable {
        Object retVal;
        System.out.println("----===>> @Around 环绕通知之前 AAA");
        retVal = proceedingJoinPoint.proceed();
        System.out.println("----===>> @Around 环绕通知之后 BBB");
        return retVal;
    }
}
```

### 测试类

其实我这个测试类，虽然用了 @Test 注解，但是我这个类更加像一个 main 方法把：如下所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbuc0uXnf8CM85qryZ4sy87oK4UTWAvfDokZk44bxRBoHEekoPAFwgNPEjNDFK2HyUrRtJgGQzUkjiaw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

## 执行结论

结果记录：spring 4.x, spring-boot 1.5.9

无法现在依赖，所以无法试验

我直接说一下结论：Spring 4 中环绕通知是在最里面执行的

结果记录：spring 版本5.3.15 springboot 版本2.6.3

![图片](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbuc0uXnf8CM85qryZ4sy87oKyhGSdHLticib8dMZNAmjN9zcmpOBryve7DGSAEicHRMNkSf4P15aXc4bw/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 多切面的情况

多个切面的情况下，可以通过@Order指定先后顺序，数字越小，优先级越高。如下图所示：

![图片](https://mmbiz.qpic.cn/mmbiz_png/eQPyBffYbuc0uXnf8CM85qryZ4sy87oK91utoe6LUW4PzHibZgjib053ibTwibiaopnS9BLkfCV3Sgb3gbwuokDFuUg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 代理失效场景

下面一种场景会导致 aop 代理失效，因为我们在执行 a 方法的时候其实本质是执行 `AServer#a` 的方法拦截器（`MethodInterceptor`）链, 当我们在 a 方法内直接执行b(), 其实本质就相当于 this.b() , 这个时候由执行 a方法是调用到 a 的原始对象相当于是 this 调用，那么会导致 b() 方法的代理失效。这个问题也是我们开发者在开发过程中最常遇到的一个问题。

```
@Service
public class AService {
    
    public void a() {
        System.out.println("...... a");
        b();
    }
    
    public void b() {
        System.out.println("...... b");
    }
```