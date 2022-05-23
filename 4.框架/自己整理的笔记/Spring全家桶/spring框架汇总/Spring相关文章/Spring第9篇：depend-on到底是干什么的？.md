# Spring第9篇：depend-on到底是干什么的？

路人甲Java [程序员路人](javascript:void(0);) *2022-04-01 20:20*

**大家好，我是路人~~~**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

**这是 Spring 第 9 篇，点击上方卡片，即可翻阅前面章节。**

***\**\*\*\*\*\*强烈推荐阅读\*\*\*\*\*\*\*\*\*\*\*\*：\*\*\*\*\*\*\*\*\*\*\*\*[\*\*到底如何学好Java？\*\*](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485855&idx=1&sn=20f2d0cb1fb9cbe842000ea4ebbcdda4&chksm=c0fb5780f78cde960e25a5b48d833ad5f49330b8663cbd2f29112eb8166f4a9113bc94dd9b27&scene=21#wechat_redirect)\*\*\*\*\*\**\***

# 无依赖bean创建和销毁的顺序

我们先来看一下没有任何依赖的bean的创建和销毁的顺序。

下面的xml中定义了3个bean：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="bean3" class="com.javacode2018.lesson001.demo7.NormalBean$Bean3"/>
    <bean id="bean2" class="com.javacode2018.lesson001.demo7.NormalBean$Bean2"/>
    <bean id="bean1" class="com.javacode2018.lesson001.demo7.NormalBean$Bean1"/>
</beans>
```

注意上面xml中bean定义顺序是：bean3、bean2、bean1。

对应java代码如下：

```
package com.javacode2018.lesson001.demo7;

import org.springframework.beans.factory.DisposableBean;

/**
 * 无任何依赖的bean创建的顺序
 */
public class NormalBean {
    public static class Bean1 implements DisposableBean {

        public Bean1() {
            System.out.println(this.getClass() + " constructor!");
        }

        @Override
        public void destroy() throws Exception {
            System.out.println(this.getClass() + " destroy()");
        }
    }

    public static class Bean2 implements DisposableBean {

        public Bean2() {
            System.out.println(this.getClass() + " constructor!");
        }

        @Override
        public void destroy() throws Exception {
            System.out.println(this.getClass() + " destroy()");
        }
    }

    public static class Bean3 implements DisposableBean {

        public Bean3() {
            System.out.println(this.getClass() + " constructor!");
        }

        @Override
        public void destroy() throws Exception {
            System.out.println(this.getClass() + " destroy()");
        }
    }
}
```

> 上面代码中使用到了DisposableBean接口，这个是spring容器提供的一个接口，这个接口中有个destroy方法，我们的bean类可以实现这个接口，当我们调用容器的close方法关闭容器的时候，spring会调用容器中所有bean的destory方法，用来做一些清理的工作，这个以后还会细讲的。
>
> 上面几个类中构造方法和destory方法中都有输出。

下面我们来搞个测试用例看一下spring容器启动和关闭的过程中，定义的3个bean的创建和销毁的顺序。

```
package com.javacode2018.lesson001.demo7;

import com.javacode2018.lesson001.demo5.IocUtils;
import org.junit.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 公众号：程序员路人
 * dependon详解
 */
public class DependOnTest {

    /**
     * 无依赖的bean创建和销毁的顺序
     */
    @Test
    public void normalBean() {
        System.out.println("容器启动中!");
        String beanXml = "classpath:/com/javacode2018/lesson001/demo7/normalBean.xml";
        ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
        System.out.println("容器启动完毕，准备关闭spring容器!");
        //关闭容器
        context.close();
        System.out.println("spring容器已关闭!");
    }

}
```

运行上面的normalBean方法，输出：

```
容器启动中!
class com.javacode2018.lesson001.demo7.NormalBean$Bean3 constructor!
class com.javacode2018.lesson001.demo7.NormalBean$Bean2 constructor!
class com.javacode2018.lesson001.demo7.NormalBean$Bean1 constructor!
容器启动完毕，准备关闭spring容器!
class com.javacode2018.lesson001.demo7.NormalBean$Bean1 destroy()
class com.javacode2018.lesson001.demo7.NormalBean$Bean2 destroy()
class com.javacode2018.lesson001.demo7.NormalBean$Bean3 destroy()
spring容器已关闭!
```

bean的定义结合上面输出我们来对比一下：

| bean定义顺序 | 创建顺序 | 销毁顺序 |
| :----------- | :------- | :------- |
| bean3        | bean3    | bean1    |
| bean2        | bean2    | bean2    |
| bean1        | bean1    | bean3    |

从输出中可以得到2点结论：

**1. bean对象的创建顺序和bean xml中定义的顺序一致**

**2. bean销毁的顺序和bean xml中定义的顺序相反**

# 通过构造器强依赖bean创建和销毁顺序

我们将上面案例改造一下，通过构造函数注入的方式使bean之间产生强依赖。

```
package com.javacode2018.lesson001.demo7;

import org.springframework.beans.factory.DisposableBean;

/**
 * 强依赖的bean创建和销毁顺序
 */
public class StrongDependenceBean {
    public static class Bean1 implements DisposableBean {

        public Bean1() {
            System.out.println(this.getClass() + " constructor!");
        }

        @Override
        public void destroy() throws Exception {
            System.out.println(this.getClass() + " destroy()");
        }
    }

    public static class Bean2 implements DisposableBean {

        private Bean1 bean1;

        public Bean2(Bean1 bean1) { //@1
            this.bean1 = bean1;
            System.out.println(this.getClass() + " constructor!");
        }

        @Override
        public void destroy() throws Exception {
            System.out.println(this.getClass() + " destroy()");
        }
    }

    public static class Bean3 implements DisposableBean {

        private Bean2 bean2;

        public Bean3(Bean2 bean2) { //@2
            this.bean2 = bean2;
            System.out.println(this.getClass() + " constructor!");
        }

        @Override
        public void destroy() throws Exception {
            System.out.println(this.getClass() + " destroy()");
        }
    }
}
```

代码解释：

@1：创建Bean2的时候需要传入一个bean1对象，对bean1产生了强依赖

@2：创建Bean3的时候需要传入一个bean2对象，对bean2产生了强依赖

依赖关系是：

```
bean3->bean2->bean1
```

对应的配置(strongDependenceBean.xml)：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="bean3" class="com.javacode2018.lesson001.demo7.StrongDependenceBean$Bean3">
        <constructor-arg index="0" ref="bean2"/> //@1
    </bean>
    <bean id="bean2" class="com.javacode2018.lesson001.demo7.StrongDependenceBean$Bean2">
        <constructor-arg index="0" ref="bean1"/> //@2
    </bean>
    <bean id="bean1" class="com.javacode2018.lesson001.demo7.StrongDependenceBean$Bean1">
    </bean>
</beans>
```

注意上面xml中bean定义顺序是：bean3、bean2、bean1。

@1：bean3中通过构造器注入bean2

@2：bean2中通过构造器注入bean1

DependOnTest中创建一个测试用例，如下：

```
/**
 * 强依赖的bean的创建和销毁顺序测试
 */
@Test
public void strongDependenceBean() {
    System.out.println("容器启动中!");
    String beanXml = "classpath:/com/javacode2018/lesson001/demo7/strongDependenceBean.xml";
    ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
    System.out.println("容器启动完毕，准备关闭spring容器!");
    context.close();
    System.out.println("spring容器已关闭!");
}
```

运行strongDependenceBean方法输出：

```
容器启动中!
class com.javacode2018.lesson001.demo7.StrongDependenceBean$Bean1 constructor!
class com.javacode2018.lesson001.demo7.StrongDependenceBean$Bean2 constructor!
class com.javacode2018.lesson001.demo7.StrongDependenceBean$Bean3 constructor!
容器启动完毕，准备关闭spring容器!
class com.javacode2018.lesson001.demo7.StrongDependenceBean$Bean3 destroy()
class com.javacode2018.lesson001.demo7.StrongDependenceBean$Bean2 destroy()
class com.javacode2018.lesson001.demo7.StrongDependenceBean$Bean1 destroy()
spring容器已关闭!
```

bean的定义结合上面输出我们来对比一下：

| bean定义顺序 | 依赖顺序（下面依赖上面的） | 创建顺序 | 销毁顺序 |
| :----------- | :------------------------- | :------- | :------- |
| bean3        | bean1                      | bean1    | bean3    |
| bean2        | bean2                      | bean2    | bean2    |
| bean1        | bean3                      | bean3    | bean1    |

从输出中可以得到2点结论：

**1. bean对象的创建顺序和bean依赖的顺序一致**

**2. bean销毁的顺序和bean创建的顺序相反**

# 通过depend-on干预bean创建和销毁顺序

上面看到了对于无依赖的bean，通过定义的顺序确实可以干预bean的创建顺序，通过强依赖也可以干预bean的创建顺序。

那么如果xml中定义的bean特别多，而有些bean之间也没有强依赖关系，此时如果想去调整bean的创建和销毁的顺序，得去调整xml中bean的定义顺序，或者去加强依赖，这样是非常不好的，spring中可以通过depend-on来解决这些问题，在不调整bean的定义顺序和强加依赖的情况下，可以通过通过depend-on属性来设置当前bean的依赖于哪些bean，那么可以保证depend-on指定的bean在当前bean之前先创建好，销毁的时候在当前bean之后进行销毁。

**depend-on使用方式：**

```
<bean id="bean1" class="" depend-on="bean2,bean3; bean4" />
```

**depend-on：设置当前bean依赖的bean名称，可以指定多个，多个之间可以用”,;空格“进行分割**

**上面不管bean2,bean2,bean4在任何地方定义，都可以确保在bean1创建之前，会先将bean2,bean3,bean4创建好，表示bean1依赖于这3个bean，可能bean1需要用到bean2、bean3、bean4中生成的一些资源或者其他的功能等，但是又没有强制去在bean1类中通过属性定义强依赖的方式去依赖于bean2、bean3、bean4；当然销毁的时候也会先销毁当前bean，再去销毁被依赖的bean，即先销毁bean1，再去销毁depend-on指定的bean。**

下面我们来个案例看一下：

dependOnBean.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="bean3" class="com.javacode2018.lesson001.demo7.NormalBean$Bean3" depends-on="bean2,bean1"/>
    <bean id="bean2" class="com.javacode2018.lesson001.demo7.NormalBean$Bean2"/>
    <bean id="bean1" class="com.javacode2018.lesson001.demo7.NormalBean$Bean1"/>
</beans>
```

上面xml中先定义的bean3，然后定义了bean2和bean1，并且指定了bean3的depend-on=“bean2,bean1”，根据depend-on的规则，所以会先创建bean2和bean1,然后再创建bean3，销毁的时候，会按照和创建相反的顺序来，即：bean1、bean2、bean3，下面我们来看看效果是不是这样：

上面xml对应的java代码如下：

```
package com.javacode2018.lesson001.demo7;

import org.springframework.beans.factory.DisposableBean;

/**
 * 通过depend-on来干预bean创建和销毁顺序
 */
public class DependOnBean {
    public static class Bean1 implements DisposableBean {

        public Bean1() {
            System.out.println(this.getClass() + " constructor!");
        }

        @Override
        public void destroy() throws Exception {
            System.out.println(this.getClass() + " destroy()");
        }
    }

    public static class Bean2 implements DisposableBean {

        public Bean2() {
            System.out.println(this.getClass() + " constructor!");
        }

        @Override
        public void destroy() throws Exception {
            System.out.println(this.getClass() + " destroy()");
        }
    }

    public static class Bean3 implements DisposableBean {

        public Bean3() {
            System.out.println(this.getClass() + " constructor!");
        }

        @Override
        public void destroy() throws Exception {
            System.out.println(this.getClass() + " destroy()");
        }
    }
}
```

DependOnTest中创建测试用例：

```
/**
 * 通过depend-on来干预bean创建和销毁顺序
 */
@Test
public void dependOnBean() {
    System.out.println("容器启动中!");
    String beanXml = "classpath:/com/javacode2018/lesson001/demo7/dependOnBean.xml";
    ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
    System.out.println("容器启动完毕，准备关闭spring容器!");
    context.close();
    System.out.println("spring容器已关闭!");
}
```

运行dependOnBean方法输出：

```
容器启动中!
class com.javacode2018.lesson001.demo7.NormalBean$Bean2 constructor!
class com.javacode2018.lesson001.demo7.NormalBean$Bean1 constructor!
class com.javacode2018.lesson001.demo7.NormalBean$Bean3 constructor!
容器启动完毕，准备关闭spring容器!
class com.javacode2018.lesson001.demo7.NormalBean$Bean3 destroy()
class com.javacode2018.lesson001.demo7.NormalBean$Bean1 destroy()
class com.javacode2018.lesson001.demo7.NormalBean$Bean2 destroy()
spring容器已关闭!
```

# 总结

\1. 无依赖的bean创建顺序和定义的顺序一致，销毁顺序刚好相反

\2. 通过构造器强依赖的bean，会先创建构造器参数中对应的bean，然后才会创建当前bean，销毁顺序刚好相反

\3. depend-on可以指定档期bean依赖的bean，通过这个可以确保depend-on指定的bean在当前bean创建之前先创建好，销毁顺序刚好相反

\4. bean的销毁顺序和bean创建的顺序相反