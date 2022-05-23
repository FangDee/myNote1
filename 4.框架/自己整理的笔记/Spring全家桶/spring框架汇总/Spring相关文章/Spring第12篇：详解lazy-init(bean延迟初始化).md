# Spring第12篇：详解lazy-init(bean延迟初始化)

路人甲Java [程序员路人](javascript:void(0);) *2022-04-08 19:26*

**大家好，我是路人~~~
**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

***\**\*\*\*\*\*\*\*\*\*\*\*强烈推荐阅读\*\*\*\*\*\*\*\*\*\*\*\*：\*\*\*\*\*\*\*\*\*\*\*\*[\*\*到底如何学好Java？\*\*](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485855&idx=1&sn=20f2d0cb1fb9cbe842000ea4ebbcdda4&chksm=c0fb5780f78cde960e25a5b48d833ad5f49330b8663cbd2f29112eb8166f4a9113bc94dd9b27&scene=21#wechat_redirect)\*\*\*\*\*\*\*\*\*\*\*\*\****

**
**

**这是 Spring 第 12 篇，点击上方卡片，即可翻阅前面章节。**

## bean初始化的方式2种方式 

1. 实时初始化
2. 延迟初始化

## bean实时初始化

在容器启动过程中被创建组装好的bean，称为实时初始化的bean，spring中默认定义的bean都是实时初始化的bean，这些bean默认都是单例的，在容器启动过程中会被创建好，然后放在spring容器中以供使用。

### 实时初始化bean的有一些优点

1. 更早发下bean定义的错误：实时初始化的bean如果定义有问题，会在容器启动过程中会抛出异常，让开发者快速发现问题
2. 查找bean更快：容器启动完毕之后，实时初始化的bean已经完全创建好了，此时被缓存在spring容器中，当我们需要使用的时候，容器直接返回就可以了，速度是非常快的

### 案例

#### ActualTimeBean类

```
package com.javacode2018.lesson001.demo11;

/**
 * 实时初始化的bean
 */
public class ActualTimeBean {
    public ActualTimeBean() {
        System.out.println("我是实时初始化的bean!");
    }
}
```

一会我们在spring中创建上面这个对象，构造函数中会输出一段话，这段话会在spring容器创建过程中输出。

#### actualTimeBean.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="actualTimeBean" class="com.javacode2018.lesson001.demo11.ActualTimeBean"/>

</beans>
```

#### 测试用例

```
package com.javacode2018.lesson001.demo11;


import org.junit.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 公众号：、程序员路人
 * bean默认是实时初始化的，可以通过bean元素的lazy-init="true"将bean定义为延迟初始化
 */
public class LazyBeanTest {

    @Test
    public void actualTimeBean() {
        System.out.println("spring容器启动中...");
        String beanXml = "classpath:/com/javacode2018/lesson001/demo11/actualTimeBean.xml";
        new ClassPathXmlApplicationContext(beanXml); //启动spring容器
        System.out.println("spring容器启动完毕...");
    }
}
```

注意上面代码，容器启动前后有输出，运行`actualTimeBean`输出：

```
spring容器启动中...
我是实时初始化的bean!
spring容器启动完毕...
```

可以看出`actualTimeBean`这个bean是在容器启动过程中被创建好的。

## 延迟初始化的bean

从上面我们可以看出，实时初始化的bean都会在容器启动过程中创建好，如果程序中定义的bean非常多，并且有些bean创建的过程中比较耗时的时候，会导致系统消耗的资源比较多，并且会让整个启动时间比较长，这个我估计大家都是有感受的，使用spring开发的系统比较大的时候，整个系统启动耗时是比较长的，基本上多数时间都是在创建和组装bean。

**spring对这些问题也提供了解决方案：bean延迟初始化。**

所谓延迟初始化，就是和实时初始化刚好相反，延迟初始化的bean在容器启动过程中不会创建，而是需要使用的时候才会去创建，先说一下bean什么时候会被使用：

1. 被其他bean作为依赖进行注入的时候，比如通过property元素的ref属性进行引用，通过构造器注入、通过set注入、通过自动注入，这些都会导致被依赖bean的创建
2. 开发者自己写代码向容器中查找bean的时候，如调用容器的getBean方法获取bean。

上面这2种情况会导致延迟初始化的bean被创建。

### 延迟bean的配置

在bean定义的时候通过`lazy-init`属性来配置bean是否是延迟加载，true：延迟初始化，false：实时初始化

```
<bean lazy-init="是否是延迟初始化" />
```

我们来2个案例看一下效果。

### 案例1

#### LazyInitBean类

```
package com.javacode2018.lesson001.demo11;

public class LazyInitBean {
    public LazyInitBean() {
        System.out.println("我是延迟初始化的bean!");
    }
}
```

#### lazyInitBean.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="lazyInitBean" class="com.javacode2018.lesson001.demo11.LazyInitBean" lazy-init="true"/>

</beans>
```

> 注意上面的`lazy-init="true"`表示定义的这个bean是延迟初始化的bean。

#### 测试用例

LazyBeanTest中加个方法

```
@Test
public void lazyInitBean() {
    System.out.println("spring容器启动中...");
    String beanXml = "classpath:/com/javacode2018/lesson001/demo11/lazyInitBean.xml";
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(beanXml); //启动spring容器
    System.out.println("spring容器启动完毕...");
    System.out.println("从容器中开始查找LazyInitBean");
    LazyInitBean lazyInitBean = context.getBean(LazyInitBean.class);
    System.out.println("LazyInitBean:" + lazyInitBean);
}
```

> 注意上面的输出，容器启动前后有输出，然后又从容器中查找LazyInitBean。

#### 运行输出

执行lazyInitBean方法，输出：

```
spring容器启动中...
spring容器启动完毕...
从容器中开始查找LazyInitBean
我是延迟初始化的bean!
LazyInitBean:com.javacode2018.lesson001.demo11.LazyInitBean@11dc3715
```

> 代码结合输出可以看出来，LazyInitBean在容器启动过程中并没有创建，当我们调用`context.getBean`方法的时候，LazyInitBean才被创建的。

### 案例2

上面这种方式是我们主动从容器中获取bean的时候，延迟初始化的bean才被容器创建的，下面我们再来看一下当延迟初始化的bean被其他实时初始化的bean依赖的时候，是什么时候创建的。

#### ActualTimeDependencyLazyBean类

```
package com.javacode2018.lesson001.demo11;

public class ActualTimeDependencyLazyBean {

    public ActualTimeDependencyLazyBean() {
        System.out.println("ActualTimeDependencyLazyBean实例化!");
    }

    private LazyInitBean lazyInitBean;

    public LazyInitBean getLazyInitBean() {
        return lazyInitBean;
    }

    public void setLazyInitBean(LazyInitBean lazyInitBean) {
        this.lazyInitBean = lazyInitBean;
        System.out.println("ActualTimeDependencyLazyBean.setLazyInitBean方法!");
    }
}
```

> ActualTimeDependencyLazyBean类中有个lazyInitBean属性，对应的有get和set方法，我们将通过set方法将lazyInitBean对象注入。

#### actualTimeDependencyLazyBean.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="lazyInitBean" class="com.javacode2018.lesson001.demo11.LazyInitBean" lazy-init="true"/>

    <bean id="actualTimeDependencyLazyBean" class="com.javacode2018.lesson001.demo11.ActualTimeDependencyLazyBean">
        <property name="lazyInitBean" ref="lazyInitBean"/>
    </bean>
</beans>
```

> 注意上面定义了2个bean：
>
> lazyInitBean：lazy-init为true，说明这个bean是延迟创建的
>
> actualTimeDependencyLazyBean：通过property元素来注入lazyInitBean，actualTimeDependencyLazyBean中没有指定lazy-init，默认为false，表示是实时创建的bean，会在容器创建过程中被初始化

#### 测试用例

LazyBeanTest中加个方法，如下：

```
@Test
public void actualTimeDependencyLazyBean() {
    System.out.println("spring容器启动中...");
    String beanXml = "classpath:/com/javacode2018/lesson001/demo11/actualTimeDependencyLazyBean.xml";
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(beanXml); //启动spring容器
    System.out.println("spring容器启动完毕...");
}
```

#### 运行输出

```
spring容器启动中...
ActualTimeDependencyLazyBean实例化!
我是延迟初始化的bean!
ActualTimeDependencyLazyBean.setLazyInitBean方法!
spring容器启动完毕...
```

从容器中可以看到，xml中定义的2个bean都在容器启动过程中被创建好了。

有些朋友比较迷茫，lazyInitBean的lazy-init为true，怎么也在容器启动过程中被创建呢？

由于`actualTimeDependencyLazyBean`为实时初始化的bean，而这个bean在创建过程中需要用到`lazyInitBean`，此时容器会去查找`lazyInitBean`这个bean，然后会进行初始化，所以这2个bean都在容器启动过程中被创建的。