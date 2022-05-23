## 深入学习Spring Bean生命周期

 2018-08-05 |  Visit count

所谓Spring Bean的生命周期指的是Bean从创建到初始化再到销毁的过程，这个过程由IOC容器管理。一个完整的Bean生命周期可以参考[Spring Bean生命周期](http://localhost:4001/Spring-Bean生命周期.html)。这里我们主要记录一些和Bean生命周期相关的细节。

## Bean的初始化和销毁

在整个生命周期过程中，我们可以自定义Bean的初始化和销毁钩子函数，当Bean的生命周期到达相应的阶段的时候，Spring会调用我们自定义的Bean的初始化和销毁方法。自定义Bean初始化和销毁方法有多种方式，下面逐一介绍。



### @Bean

上一节中介绍了可以在配置类中通过`@Bean`注解来注册Bean，我们也可以通过它来指定Bean的初始化和方法。

为了演示，我们新建一个Spring Boot项目，然后创建一个`User`类：

```
public class User {
    public User() {
        System.out.println("调用无参构造器创建User");
    }
    public void init() {
        System.out.println("初始化User");
    }
    public void destory() {
        System.out.println("销毁User");
    }
}
```



然后在配置类里注册该组件，并指定初始化和销毁方法：

```
@Configuration
public class WebConfig {

    @Bean(initMethod = "init", destroyMethod = "destory")
    public User user() {
        return new User();
    }
}
```



其中`initMethod = "init"`和`destroyMethod = "destory"`与User类里的`init`，`destory`方法相对应。

在Spring Boot入口类中测试:

```
// 返回 IOC 容器，使用注解配置，传入配置类
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(WebConfig.class);
User user = context.getBean(User.class);
// 关闭 IOC 容器
context.close();
```



启动项目，观察控制台输出:

![QQ截图20190130142237.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20190130142237.png)

从上面的输出我们看出在容器启动之前，先调用对象的无参构造器创建对象，然后调用初始化方法，在容器关闭的时候调用销毁方法。

上面的情况是对于单例而言的，如果组件是多例模式又是什么情况呢？我们把上面的组件注册配置改为多例，然后再次启动项目，观察控制台输出:

![QQ截图20190130142740.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20190130142740.png)

控制台的输出和我们上节讨论的一致，即在多例模式下，IOC容器启动的时候并不会去创建对象，而是在每次获取的时候才会去调用方法创建对象，创建完对象后再调用初始化方法。但在容器关闭后，Spring并没有调用相应的销毁方法，这是因为在多例模式下，容器不会管理这个组件（只负责在你需要的时候创建这个组件），所以容器在关闭的时候并不会调用相应的销毁方法。

### InitializingBean&DisposableBean

除了上面这种方式指定初始化和销毁方法外，Spring还为我们提供了和初始化，销毁相对应的接口：

- `InitializingBean`接口包含一个`afterPropertiesSet`方法，我们可以通过实现该接口，然后在这个方法中编写初始化逻辑。
- `DisposableBean`接口包含一个`destory`方法，我们可以通过实现该接口，然后再这个方法中编写销毁逻辑。

新建一个类，名称为`Bird`，然后实现这两个接口：

```java
public class Bird implements InitializingBean, DisposableBean {
    public Bird() {
        System.out.println("调用无参构造器创建Bird");
    }

    @Override
    public void destroy() {
        System.out.println("销毁Bird");
    }

    @Override
    public void afterPropertiesSet() {
        System.out.println("初始化Bird");
    }
}
```

在配置类中注册这个组件:

```java
@Bean
public Bird bird() {
    return new Bird();
}
```



测试一波：

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(WebConfig.class);
System.out.println("容器创建完毕");
context.close();
```



启动项目，观察控制台输出:

![QQ截图20190130145406.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20190130145406.png)

### @PostConstruct&@PreDestroy

除了上面两种指定初始化和销毁方法的方式外，我们还可以使用`@PostConstruct`和`@PreDestroy`注解修饰方法来指定相应的初始化和销毁方法。

新建一个类，名称为Fish：

```java
public class Fish {
    public Fish() {
        System.out.println("调用无参构造器创建Fish");
    }

    @PostConstruct
    public void init() {
        System.out.println("初始化Fish");
    }

    @PreDestroy
    public void destory() {
        System.out.println("销毁Fish");
    }
}
```



在配置类中注册这个组件:

```java
@Bean
public Fish fish(){
    return new Fish();
}
```



测试一波：

```java
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(WebConfig.class);
System.out.println("容器创建完毕");
context.close();
```



启动项目，观察控制台输出:

![QQ截图20190130150534.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20190130150534.png)

效果和上面两种方式一致。

这两个注解并非Spring提供，而是JSR250规范提供。

## BeanPostProcessor

Spring提供了一个`BeanPostProcessor`接口，俗称**Bean后置通知处理器**，它提供了两个方法`postProcessBeforeInitialization`和`postProcessAfterInitialization`。其中`postProcessBeforeInitialization`在组件的初始化方法调用之前执行，`postProcessAfterInitialization`在组件的初始化方法调用之后执行。它们都包含两个入参：

1. bean：当前组件对象；
2. beanName：当前组件在容器中的名称。

两个方法都返回一个Object类型，我们可以直接返回当前组件对象，或者包装后返回。

我们来定义一个`BeanPostProcessor`接口的实现类`MyBeanPostProcessor`：

```java
public class MyBeanPostProcessor implements BeanPostProcessor {
    @Override
    public Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        System.out.println(beanName + " 初始化之前调用");
        return bean;
    }
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        System.out.println(beanName + " 初始化之后调用");
        return bean;
    }
}
```



在配置类中注册该组件:

```java
@Bean
public MyBeanPostProcessor myBeanPostProcessor () {
    return new MyBeanPostProcessor();
}
```



再次启动项目，观察控制台输出:

![QQ截图20190130153607.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20190130153607.png)

![QQ截图20190130153450.png](https://mrbird.cc/img/QQ%E6%88%AA%E5%9B%BE20190130153450.png)