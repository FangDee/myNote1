# Spring第17篇：@Configration和@Bean解密

路人甲Java [程序员路人](javascript:void(0);) *2022-04-15 20:20*

**大家好，我是路人~~~**

**点击下方卡片，关注我，java干货及时送达**

[**招收永久门徒，亲手带！**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485896&idx=2&sn=72f05d43ae9df22248994037f699e99c&chksm=c0fb57d7f78cdec182e0c5abefea5613f7cb547550ceaf79439476e6164e17f2b840c6d16670&scene=21#wechat_redirect)

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号



**这是 Spring 第 17 篇，点击上方卡片，即可翻阅前面章节。**

**上次去头条面试：**

**面试官**：spring中，类上加不加@Configuration注解，有什么区别？

**我**：当某个类上有@Configuration注解的时候，可以在这个类中使用@Bean注解向spring容器中注册bean；如果不加@Configuration注解，不能通过@Bean注解注册bean。

**面试官**：你确定不用@Configuration注解，不能通过@Bean注解来注册bean？

**我**：看着面试官，犹豫了10秒钟，说：不加@Configuration通过@Bean注解也可以注册bean

**面试官**：你确定可以注册？

**我**：嗯。。。。嗯。。。。嗯，我确定可以注册

**面试官**：那加不加到底有什么区别呢？

**我**：好像没有什么区别啊

**面试官**：好像没区别。。。。先回去等通知吧！

结果可想而知，没戏了！

回去之后立即看spring的源码，终于搞清了这个问题，原来加不加@Configuration还是有相当大的区别的。

下面我们就来看看@Configuration注解到底是干什么的，加不加到底有什么区别，下次你们去面试的时候就可以给面试官吹吹了。

之前我们都是通过xml的方式定义bean，里面会写很多bean元素，然后spring启动的时候，就会读取bean xml配置文件，然后解析这些配置，然后会将这些bean注册到spring容器中，供使用者使用。

jdk1.5里面有了注解的功能，spring也没闲着，觉得注解挺好用的，就将注解加了进来，让我们通过注解的方式来定义bean，用起来能达到xml中定义bean一样的效果，并且更简洁一些，这里面需要用到的注解就有`@Configuration`注解和`@Bean`注解。

## @Configuration注解

### 用法

@Configuration这个注解可以加在类上，让这个类的功能等同于一个bean xml配置文件，如下：

```
@Configuration
public class ConfigBean {

}
```

上面代码类似于下面的xml：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

</beans>
```

通过`AnnotationConfigApplicationContext`来加载`@Configuration`修饰的类，如下：

```
AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ConfigBean.class);
```

此时ConfigBean类中没有任何内容，相当于一个空的xml配置文件，此时我们要在ConfigBean类中注册bean，那么我们就要用到@Bean注解了。

### 总结一下

`@Configuration`使用步骤：

1. 1. 在类上使用`@Configuration`注解
   2. 通过`AnnotationConfigApplicationContext`容器来加`@Configuration`注解修饰的类

## @Bean注解

### 用法

这个注解类似于bean xml配置文件中的bean元素，用来在spring容器中注册一个bean。

@Bean注解用在方法上，表示通过方法来定义一个bean，默认将方法名称作为bean名称，将方法返回值作为bean对象，注册到spring容器中。

如：

```
@Bean
public User user1() {
    return new User();
}
```

@Bean注解还有很多属性，我们来看一下其源码：

```
@Target({ElementType.METHOD, ElementType.ANNOTATION_TYPE}) //@1
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Bean {

    @AliasFor("name")
    String[] value() default {};

    @AliasFor("value")
    String[] name() default {};

    @Deprecated
    Autowire autowire() default Autowire.NO;

    boolean autowireCandidate() default true;

    String initMethod() default "";

    String destroyMethod() default AbstractBeanDefinition.INFER_METHOD;
}
```

> @1：说明这个注解可以用在方法和注解类型上面。
>
> 每个参数含义：
>
> 1. value和name是一样的，设置的时候，这2个参数只能选一个，原因是@AliasFor导致的
>
>    @AliasFor这个注解不清楚的可以去前期的文章
>
> 2. value：字符串数组，第一个值作为bean的名称，其他值作为bean的别名
>
> 3. autowire：这个参数上面标注了@Deprecated，表示已经过期了，不建议使用了
>
> 4. autowireCandidate：是否作为其他对象注入时候的候选bean，之前的文章中专门介绍过这个属性，不清楚的的可以去前期的文章
>
> 5. initMethod：bean初始化的方法，这个和生命周期有关，以后详解
>
> 6. destroyMethod：bean销毁的方法，也是和生命周期相关的，以后详解

### 案例

#### User类

```
package com.javacode2018.lesson001.demo20;

public class User {
}
```

#### Bean配置类：ConfigBean

```
package com.javacode2018.lesson001.demo20;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ConfigBean {

    //bean名称为方法默认值：user1
    @Bean
    public User user1() {
        return new User();
    }

    //bean名称通过value指定了：user2Bean
    @Bean("user2Bean")
    public User user2() {
        return new User();
    }

    //bean名称为：user3Bean，2个别名：[user3BeanAlias1,user3BeanAlias2]
    @Bean({"user3Bean", "user3BeanAlias1", "user3BeanAlias2"})
    public User user3() {
        return new User();
    }

}
```

> 上面通过@Bean注解定义了3个bean，比较简单

#### 来个测试类

```
package com.javacode2018.lesson001.demo20;

import org.junit.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

import java.util.Arrays;

public class ConfigurationTest {
    @Test
    public void test1() {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ConfigBean.class);//@1
        for (String beanName : context.getBeanDefinitionNames()) {
            //别名
            String[] aliases = context.getAliases(beanName);
            System.out.println(String.format("bean名称:%s,别名:%s,bean对象:%s",
                    beanName,
                    Arrays.asList(aliases),
                    context.getBean(beanName)));
        }
    }
}
```

> @1：通过`AnnotationConfigApplicationContext`来加载配置类`ConfigBean`，会将配置类中所有的bean注册到spring容器中
>
> for循环中输出了bean名称、别名、bean对象

#### 运行test1方法输出

```
bean名称:org.springframework.context.annotation.internalConfigurationAnnotationProcessor,别名:[],bean对象:org.springframework.context.annotation.ConfigurationClassPostProcessor@3bd82cf5
bean名称:org.springframework.context.annotation.internalAutowiredAnnotationProcessor,别名:[],bean对象:org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor@544fa968
bean名称:org.springframework.context.annotation.internalCommonAnnotationProcessor,别名:[],bean对象:org.springframework.context.annotation.CommonAnnotationBeanPostProcessor@247bddad
bean名称:org.springframework.context.event.internalEventListenerProcessor,别名:[],bean对象:org.springframework.context.event.EventListenerMethodProcessor@d35dea7
bean名称:org.springframework.context.event.internalEventListenerFactory,别名:[],bean对象:org.springframework.context.event.DefaultEventListenerFactory@7770f470
bean名称:configBean,别名:[],bean对象:com.javacode2018.lesson001.demo20.ConfigBean$$EnhancerBySpringCGLIB$$dde45976@5e5d171f
bean名称:user1,别名:[],bean对象:com.javacode2018.lesson001.demo20.User@24313fcc
bean名称:user2Bean,别名:[],bean对象:com.javacode2018.lesson001.demo20.User@7d20d0b
bean名称:user3Bean,别名:[user3BeanAlias2, user3BeanAlias1],bean对象:com.javacode2018.lesson001.demo20.User@77f1baf5
```

> 上面的输出，我们主要关注与最后4行，前面的可以先忽略。
>
> 从输出中可以看出，有个名称为`configBean`的bean，正是ConfigBean这个类型，可以得出，被@Configuration修饰的类，也被注册到spring容器中了
>
> 最后3行输出就是几个User的bean对象了。

**上面的用法应该很多人都比较熟悉，下面的属于重点了。**

**[\**招收永久门徒，亲手带！\**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485896&idx=2&sn=72f05d43ae9df22248994037f699e99c&chksm=c0fb57d7f78cdec182e0c5abefea5613f7cb547550ceaf79439476e6164e17f2b840c6d16670&scene=21#wechat_redirect)**

## 去掉@Configuration会怎样？

我们来看一下没有@Configuration的时候，什么效果。

#### 新建一个ConfigBean1类

内容和ConfigBean类一样，只是将@Configuration注解去掉了，如下：

```
public class ConfigBean1 {

    //bean名称为方法默认值：user1
    @Bean
    public User user1() {
        return new User();
    }

    //bean名称通过value指定了：user2Bean
    @Bean("user2Bean")
    public User user2() {
        return new User();
    }

    //bean名称为：user3Bean，2个别名：[user3BeanAlias1,user3BeanAlias2]
    @Bean({"user3Bean", "user3BeanAlias1", "user3BeanAlias2"})
    public User user3() {
        return new User();
    }

}
```

#### 来个测试用例test2

代码类似于test1，给spring容器传递`ConfigBean1`

```
@Test
public void test2() {
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ConfigBean1.class);
    for (String beanName : context.getBeanDefinitionNames()) {
        //别名
        String[] aliases = context.getAliases(beanName);
        System.out.println(String.format("bean名称:%s,别名:%s,bean对象:%s",
                beanName,
                Arrays.asList(aliases),
                context.getBean(beanName)));
    }
}
```

#### 运行输出

```
bean名称:org.springframework.context.annotation.internalConfigurationAnnotationProcessor,别名:[],bean对象:org.springframework.context.annotation.ConfigurationClassPostProcessor@333291e3
bean名称:org.springframework.context.annotation.internalAutowiredAnnotationProcessor,别名:[],bean对象:org.springframework.beans.factory.annotation.AutowiredAnnotationBeanPostProcessor@479d31f3
bean名称:org.springframework.context.annotation.internalCommonAnnotationProcessor,别名:[],bean对象:org.springframework.context.annotation.CommonAnnotationBeanPostProcessor@40ef3420
bean名称:org.springframework.context.event.internalEventListenerProcessor,别名:[],bean对象:org.springframework.context.event.EventListenerMethodProcessor@498d318c
bean名称:org.springframework.context.event.internalEventListenerFactory,别名:[],bean对象:org.springframework.context.event.DefaultEventListenerFactory@6e171cd7
bean名称:configBean1,别名:[],bean对象:com.javacode2018.lesson001.demo20.ConfigBean1@402bba4f
bean名称:user1,别名:[],bean对象:com.javacode2018.lesson001.demo20.User@795cd85e
bean名称:user2Bean,别名:[],bean对象:com.javacode2018.lesson001.demo20.User@59fd97a8
bean名称:user3Bean,别名:[user3BeanAlias2, user3BeanAlias1],bean对象:com.javacode2018.lesson001.demo20.User@f5ac9e4
```

#### 分析结果

我们将2个输出的最后4行拿来对比一下：

##### 有@Configuration注解的

```
bean名称:configBean,别名:[],bean对象:com.javacode2018.lesson001.demo20.ConfigBean$$EnhancerBySpringCGLIB$$dde45976@5e5d171f
bean名称:user1,别名:[],bean对象:com.javacode2018.lesson001.demo20.User@24313fcc
bean名称:user2Bean,别名:[],bean对象:com.javacode2018.lesson001.demo20.User@7d20d0b
bean名称:user3Bean,别名:[user3BeanAlias2, user3BeanAlias1],bean对象:com.javacode2018.lesson001.demo20.User@77f1baf5
```

##### 没有@Configuration注解的

```
bean名称:configBean1,别名:[],bean对象:com.javacode2018.lesson001.demo20.ConfigBean1@402bba4f
bean名称:user1,别名:[],bean对象:com.javacode2018.lesson001.demo20.User@795cd85e
bean名称:user2Bean,别名:[],bean对象:com.javacode2018.lesson001.demo20.User@59fd97a8
bean名称:user3Bean,别名:[user3BeanAlias2, user3BeanAlias1],bean对象:com.javacode2018.lesson001.demo20.User@f5ac9e4
```

##### 对比得出

\1. 对比最后3行，可以看出：**有没有@Configuration注解，@Bean都会起效，都会将@Bean修饰的方法作为bean注册到容器中**

\2. 两个内容的第一行有点不一样，被@Configuration修饰的bean最后输出的时候带有`EnhancerBySpringCGLIB`的字样，而没有@Configuration注解的bean没有Cglib的字样；有`EnhancerBySpringCGLIB`字样的说明这个bean被cglib处理过的，变成了一个代理对象。

\3. [**招收永久门徒，亲手带！**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485896&idx=2&sn=72f05d43ae9df22248994037f699e99c&chksm=c0fb57d7f78cdec182e0c5abefea5613f7cb547550ceaf79439476e6164e17f2b840c6d16670&scene=21#wechat_redirect)

**目前为止我们还是看不出二者本质上的区别，继续向下看。**

## @Configuration加不加到底区别在哪？

通常情况下，bean之间是有依赖关系的，我们来创建个有依赖关系的bean，通过这个案例你就可以看出根本的区别了。

### 再来一个加@Configuration的案例

定义2个类。

#### ServiceA

```
package com.javacode2018.lesson001.demo20;

public class ServiceA {
}
```

#### ServiceB

```
package com.javacode2018.lesson001.demo20;

public class ServiceB {
    private ServiceA serviceA;

    public ServiceB(ServiceA serviceA) {
        this.serviceA = serviceA;
    }

    @Override
    public String toString() {
        return "ServiceB{" +
                "serviceA=" + serviceA +
                '}';
    }
}
```

上面定义了2个类，ServiceB依赖于ServiceA，ServiceB通过构造器注入ServiceA。

来个@Configuration类管理上面对象。

#### ConfigBean2

```
package com.javacode2018.lesson001.demo20;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class ConfigBean2 {

    @Bean
    public ServiceA serviceA() {
        System.out.println("调用serviceA()方法"); //@0
        return new ServiceA();
    }

    @Bean
    ServiceB serviceB1() {
        System.out.println("调用serviceB1()方法");
        ServiceA serviceA = this.serviceA(); //@1
        return new ServiceB(serviceA);
    }

    @Bean
    ServiceB serviceB2() {
        System.out.println("调用serviceB2()方法");
        ServiceA serviceA = this.serviceA(); //@2
        return new ServiceB(serviceA);
    }

}
```

> 上面通过@Bean注解，向容器中注册了3个bean
>
> 注意@1和@2，通过this.serviceA()获取需要注入的ServiceA对象。
>
> 上面每个方法第一行都输出了一行日志。
>
> **重点关注一下@0这行日志会输出几次，大家先思考一下1次还是3次？**

#### 测试用例

```
@Test
public void test3() {
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ConfigBean2.class);
    for (String beanName : context.getBeanDefinitionNames()) {
        //别名
        String[] aliases = context.getAliases(beanName);
        System.out.println(String.format("bean名称:%s,别名:%s,bean对象:%s",
                beanName,
                Arrays.asList(aliases),
                context.getBean(beanName)));
    }
}
```

#### 运行输出

截取了几行输出如下：

```
调用serviceA()方法
调用serviceB1()方法
调用serviceB2()方法
bean名称:configBean2,别名:[],bean对象:com.javacode2018.lesson001.demo20.ConfigBean2$$EnhancerBySpringCGLIB$$ffa0178@77f1baf5
bean名称:serviceA,别名:[],bean对象:com.javacode2018.lesson001.demo20.ServiceA@41a2befb
bean名称:serviceB1,别名:[],bean对象:ServiceB{serviceA=com.javacode2018.lesson001.demo20.ServiceA@41a2befb}
bean名称:serviceB2,别名:[],bean对象:ServiceB{serviceA=com.javacode2018.lesson001.demo20.ServiceA@41a2befb}
```

#### 分析结果

从输出中可以看出

\1. 前3行可以看出，被@Bean修饰的方法都只被调用了一次，这个很关键

\2. 最后三行中可以看出都是同一个ServiceA对象，都是ServiceA@41a2befb 这个实例

#### 这是为什么？

**被@Configuration修饰的类，spring容器中会通过cglib给这个类创建一个代理，代理会拦截所有被@Bean修饰的方法，默认情况（bean为单例）下确保这些方法只被调用一次，从而确保这些bean是同一个bean，即单例的。**

至于底层是如何实现的，可以去看下前面已发过的代理相关的文章。

现在各位应该感受到了，我写的文章前后一般都是有依赖的，所以也建议大家按顺序看，这样知识就是贯通的。

[**招收永久门徒，亲手带！**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485896&idx=2&sn=72f05d43ae9df22248994037f699e99c&chksm=c0fb57d7f78cdec182e0c5abefea5613f7cb547550ceaf79439476e6164e17f2b840c6d16670&scene=21#wechat_redirect)

### 不加的案例

我们再来看看将ConfigBean2上的的@Configuration去掉，效果如何，代码就不写了，直接上输出结果：

```
调用serviceA()方法
调用serviceB1()方法
调用serviceA()方法
调用serviceB2()方法
调用serviceA()方法
bean名称:configBean2,别名:[],bean对象:com.javacode2018.lesson001.demo20.ConfigBean2@6e171cd7
bean名称:serviceA,别名:[],bean对象:com.javacode2018.lesson001.demo20.ServiceA@402bba4f
bean名称:serviceB1,别名:[],bean对象:ServiceB{serviceA=com.javacode2018.lesson001.demo20.ServiceA@795cd85e}
bean名称:serviceB2,别名:[],bean对象:ServiceB{serviceA=com.javacode2018.lesson001.demo20.ServiceA@59fd97a8}
```

#### 结果分析

1. 1. serviceA()方法被调用了3次
   2. configBean2这个bean没有代理效果了
   3. 最后3行可以看出，几个ServiceA对象都是不一样的

## spring这块的源码

spring中用下面这个类处理@Configuration这个注解：

```
org.springframework.context.annotation.ConfigurationClassPostProcessor
```

这里面重点关注这几个方法：

```
public void postProcessBeanDefinitionRegistry(BeanDefinitionRegistry registry) 
public void postProcessBeanFactory(ConfigurableListableBeanFactory beanFactory)
public void enhanceConfigurationClasses(ConfigurableListableBeanFactory beanFactory) 
```

最后一个方法会创建cglib代理，大家可以设置断点进去看看，有问题欢迎交流。

## 总结

1. 1. 到目前为止加不加@Configuration注解，有什么区别，大家估计比我都清楚了
   2. @Configuration注解修饰的类，会被spring通过cglib做增强处理，通过cglib会生成一个代理对象，代理会拦截所有被@Bean注解修饰的方法，可以确保一些bean是单例的
   3. 不管@Bean所在的类上是否有@Configuration注解，都可以将@Bean修饰的方法作为一个bean注册到spring容器中