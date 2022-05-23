# Spring第14篇：lookup-method和replaced-method比较陌生，怎么玩的？

路人甲Java [程序员路人](javascript:void(0);) *2022-04-12 20:20*

**大家好，我是路人~~~
**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

***\**\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*[\*\*路人手把手带你学java\*\*](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485855&idx=1&sn=20f2d0cb1fb9cbe842000ea4ebbcdda4&chksm=c0fb5780f78cde960e25a5b48d833ad5f49330b8663cbd2f29112eb8166f4a9113bc94dd9b27&scene=21#wechat_redirect)\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\****

***\**\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*
\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\*\****

**这是 Spring 第 14 篇，点击上方卡片，即可翻阅前面章节。**

## lookup-method：方法查找 

**通常情况下，我们使用的bean都是单例的，如果一个bean需要依赖于另一个bean的时候，可以在当前bean中声明另外一个bean引用，然后注入依赖的bean，此时被依赖的bean在当前bean中自始至终都是同一个实例。**

### 先来个案例回顾一下

```
package com.javacode2018.lesson001.demo13.normal;

public class ServiceA {
}
package com.javacode2018.lesson001.demo13.normal;

public class ServiceB {

    private ServiceA serviceA;

    public ServiceA getServiceA() {
        return serviceA;
    }

    public void setServiceA(ServiceA serviceA) {
        this.serviceA = serviceA;
    }
}
```

上面2个类，ServiceA和ServiceB，而ServiceB中需要用到ServiceA，可以通过setServiceA将serviceA注入到ServiceB中，spring配置如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="serviceA" class="com.javacode2018.lesson001.demo13.normal.ServiceA" scope="prototype"/>

    <bean id="serviceB" class="com.javacode2018.lesson001.demo13.normal.ServiceB">
        <property name="serviceA" ref="serviceA"/>
    </bean>

</beans>
```

> 上面serviceA的scope是prototype，表示serviceA是多例的，每次从容器中获取serviceA都会返回一个新的对象。
>
> 而serviceB的scope没有配置，默认是单例的，通过property元素将serviceA注入。

来个测试案例，如下：

```
package com.javacode2018.lesson001.demo13;


import com.javacode2018.lesson001.demo13.normal.ServiceA;
import com.javacode2018.lesson001.demo13.normal.ServiceB;
import org.junit.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 公众号：程序员路人
 * lookupMethod的使用
 */
public class LookupMethodTest {

    @Test
    public void normalBean() {
        String beanXml = "classpath:/com/javacode2018/lesson001/demo13/normalBean.xml";
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(beanXml);

        System.out.println(context.getBean(ServiceA.class)); //@1
        System.out.println(context.getBean(ServiceA.class)); //@2

        System.out.println("serviceB中的serviceA");
        ServiceB serviceB = context.getBean(ServiceB.class); //@3
        System.out.println(serviceB.getServiceA()); //@4
        System.out.println(serviceB.getServiceA()); //@5
    }
}
```

> @1和@2从容器中按照类型查找ServiceA对应的bean。
>
> @3：从容器中获取ServiceB
>
> @4和@5：获取serviceB中的serviceA对象

运行`normalBean()`看一下效果：

```
com.javacode2018.lesson001.demo13.normal.ServiceA@5bfa9431
com.javacode2018.lesson001.demo13.normal.ServiceA@5db250b4
serviceB中的serviceA
com.javacode2018.lesson001.demo13.normal.ServiceA@223f3642
com.javacode2018.lesson001.demo13.normal.ServiceA@223f3642
```

从输出中可以看出，@1和@2输出了不同的ServiceA，而@4和@5输出的是同一个serviceA，这是因为serviceB是单例的，serviceB中的serviceA会在容器创建serviceB的时候，从容器中获取一个serviceA将其注入到serviceB中，所以自始至终serviceB中的serviceA都是同一个对象。

如果我们希望beanB中每次使用beanA的时候beanA都是一个新的实例，我们怎么实现呢？

我们可以在serviceB中加个方法去获取serviceA，这个方法中我们主动去容器中获取serviceA，那么每次获取到的都是不同的serviceA实例。

那么问题来了，我们如何在serviceB中获取到spring容器呢？

spring中有个接口`ApplicationContextAware`：

```
org.springframework.context.ApplicationContextAware

public interface ApplicationContextAware extends Aware {
    void setApplicationContext(ApplicationContext applicationContext) throws BeansException;
}
```

上面这个接口有一个方法`setApplicationContext`，这个接口给了自定义的bean中获取applicationContext的能力，当我们的类实现这个接口之后，spring容器创建bean对象的时候，如果bean实现了这个接口，那么容器会自动调用`setApplicationContext`方法，将容器对象`applicationContext`传入，此时在我们的bean对象中就可以使用容器的任何方法了。

下面我们就通过`ApplicationContextAware`接口来实现单例bean中使用多例bean的案例。

### 单例bean中使用多例bean：ApplicationContext接口的方式

#### ServiceA.java

```
package com.javacode2018.lesson001.demo13.applicationcontextaware;

public class ServiceA {
}
```

#### ServiceB.java

```
package com.javacode2018.lesson001.demo13.applicationcontextaware;

import org.springframework.beans.BeansException;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

public class ServiceB implements ApplicationContextAware { //@1

    public void say(){
        ServiceA serviceA = this.getServiceA();//@2
        System.out.println("this:"+this+",serviceA:"+ serviceA);
    }

    public ServiceA getServiceA() {
        return this.context.getBean(ServiceA.class);//@3
    }

    private ApplicationContext context;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.context = applicationContext;
    }
}
```

> 注意上面代码，ServiceB实现了ApplicationContextAware接口，然后实现了这个接口中的setApplicationContext方法，spring容器在创建ServiceB的时候会自动调用setApplicationContext方法。
>
> @3：从容器中主动去获取ServiceA，这样每次获取到的ServiceA都是一个新的实例。
>
> @2：say方法中调用getServiceA方法获取ServiceA对象，然后将其输出。

#### alicationcontextaware.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="serviceA" class="com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceA" scope="prototype"/>

    <bean id="serviceB" class="com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceB"/>

</beans>
```

> 上面定义了2个bean，第一个是多例的。

#### 测试用例

```
@Test
public void alicationcontextaware() {
    String beanXml = "classpath:/com/javacode2018/lesson001/demo13/alicationcontextaware.xml";
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(beanXml);

    System.out.println(context.getBean(com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceA.class)); //@1
    System.out.println(context.getBean(com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceA.class)); //@2

    System.out.println("serviceB中的serviceA");
    com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceB serviceB = context.getBean(com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceB.class); //@3
    serviceB.say();
    serviceB.say();
}
```

运行输出：

```
com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceA@78047b92
com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceA@8909f18
serviceB中的serviceA
this:com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceB@79ca92b9,serviceA:com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceA@1460a8c0
this:com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceB@79ca92b9,serviceA:com.javacode2018.lesson001.demo13.applicationcontextaware.ServiceA@4f638935
```

最后2行是是serviceB中的say方法输出的，可以看出serviceB是一个对象，而serviceA是不同的对象。

### 单例bean中使用多例bean：lookup-method方式实现

上面这种方式实现了单例bean中使用多例bean的需求，但是用到spring中的接口`ApplicationContextAware`，此时对spring的api有耦合的作用，我们一直推行高内聚低耦合，所以我们得寻求更好的办法。

能不能有这样的功能，当serviceB中调用getServiceA的时候，系统自动将这个方法拦截，然后去spring容器中查找对应的serviceA对象然后返回，spring中的lookup-method就可以实现这样的功能。

下面我们使用lookup-method来实现一下。

#### ServiceA.java

```
package com.javacode2018.lesson001.demo13.lookupmethod;

public class ServiceA {
}
```

#### ServiceB.java

```
package com.javacode2018.lesson001.demo13.lookupmethod;

public class ServiceB {

    public void say() {
        ServiceA serviceA = this.getServiceA();
        System.out.println("this:" + this + ",serviceA:" + serviceA);
    }

    public ServiceA getServiceA() { //@1
        return null;
    }

}
```

**注意上面的@1，这个方法中返回了一个null对象，下面我们通过spring来创建上面2个bean对象，然后让spring对上面的getServiceA方法进行拦截，返回指定的bean，如下：**

#### lookupmethod.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="serviceA" class="com.javacode2018.lesson001.demo13.lookupmethod.ServiceA" scope="prototype"/>

    <bean id="serviceB" class="com.javacode2018.lesson001.demo13.lookupmethod.ServiceB">
        <lookup-method name="getServiceA" bean="serviceA"/>
    </bean>

</beans>
```

注意上面的配置，重点在于这行配置：

```
<lookup-method name="getServiceA" bean="serviceA"/>
```

当我们调用`serviceB`中的`getServiceA`方法的时候，这个方法会拦截，然后会按照lookup-method元素中bean属性的值作为bean的名称去容器中查找对应bean，然后作为getServiceA的返回值返回，即调用getServiceA方法的时候，会从spring容器中查找`id为serviceA`的bean然后返回。

#### 测试用例

LookupMethodTest中加个方法，如下：

```
@Test
public void lookupmethod() {
    String beanXml = "classpath:/com/javacode2018/lesson001/demo13/lookupmethod.xml";
    ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(beanXml);

    System.out.println(context.getBean(com.javacode2018.lesson001.demo13.lookupmethod.ServiceA.class)); //@1
    System.out.println(context.getBean(com.javacode2018.lesson001.demo13.lookupmethod.ServiceA.class)); //@2

    System.out.println("serviceB中的serviceA");
    com.javacode2018.lesson001.demo13.lookupmethod.ServiceB serviceB = context.getBean(com.javacode2018.lesson001.demo13.lookupmethod.ServiceB.class); //@3
    serviceB.say();
    serviceB.say();
}
```

运行看看效果：

```
com.javacode2018.lesson001.demo13.lookupmethod.ServiceA@619713e5
com.javacode2018.lesson001.demo13.lookupmethod.ServiceA@708f5957
serviceB中的serviceA
this:com.javacode2018.lesson001.demo13.lookupmethod.ServiceB$$EnhancerBySpringCGLIB$$aca8be5a@68999068,serviceA:com.javacode2018.lesson001.demo13.lookupmethod.ServiceA@7722c3c3
this:com.javacode2018.lesson001.demo13.lookupmethod.ServiceB$$EnhancerBySpringCGLIB$$aca8be5a@68999068,serviceA:com.javacode2018.lesson001.demo13.lookupmethod.ServiceA@2ef3eef9
```

注意最后2行的输出，serviceA是调用this.getServiceA()方法获取 ，源码中这个方法返回的是null，但是spring内部对这个方法进行了拦截，每次调用这个方法的时候，都会去容器中查找serviceA，然后返回，所以上面最后2行的输出中serviceA是有值的，并且是不同的serviceA实例。

**lookup-method：看其名字，就知道意思：方法查找，调用name属性指定的方法的时候，spring会对这个方法进行拦截，然后去容器中查找lookup-method元素中bean属性指定的bean，然后将找到的bean作为方法的返回值返回。**

**这个地方底层是使用cglib代理实现的，后面有篇文章会详细介绍代理的2种实现，到时候大家注意下，spring中很多牛逼的功能都是靠代理实现的。**

spring提供的还有一个功能，同样可以可以解决上面单例bean中用到多例bean的问题，也就是下面我们要说的replaced-method。

## replaced-method：方法替换

replaced-method：方法替换，比如我们要调用serviceB中的getServiceA的时候，我们可以对serviceB这个bean中的getServiceA方法进行拦截，把这个调用请求转发到一个替换者处理。这就是replaced-method可以实现的功能，比lookup-method更强大更灵活。

#### replaced-method的使用3个步骤

##### 步骤一：定义替换者

自定义一个替换者，替换者需要实现spring中的MethodReplacer接口，看一下这个接口的定义：

```
package org.springframework.beans.factory.support;

import java.lang.reflect.Method;

public interface MethodReplacer {

    /**
     * @param obj 被替换方法的目标对象
     * @param method 目标对象的方法
     * @param args 方法的参数
     * @return return value for the method
     */
    Object reimplement(Object obj, Method method, Object[] args) throws Throwable;

}
```

> 当调用目标对象需要被替换的方法的时候，这个调用请求会被转发到上面的替换者的reimplement方法进行处理。

如：

```
package com.javacode2018.lesson001.demo14;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.support.MethodReplacer;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

import java.lang.reflect.Method;
import java.util.Map;

/**
 * servieB的方法替换者
 */
public class ServiceBMethodReplacer implements MethodReplacer, ApplicationContextAware {

    @Override
    public Object reimplement(Object obj, Method method, Object[] args) throws Throwable {
        return this.context.getBean(ServiceA.class);
    }

    private ApplicationContext context;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.context = applicationContext;
    }
}
```

##### 步骤二：定义替换者bean

```
<!-- 定义替换者bean -->
<bean id="serviceBMethodReplacer" class="com.javacode2018.lesson001.demo14.ServiceBMethodReplacer" />
```

##### 步骤二：通过replaced-method元素配置目标bean需要被替换的方法

```
<bean id="serviceB" class="com.javacode2018.lesson001.demo14.ServiceB">
    <replaced-method name="getServiceA" replacer="serviceAMethodReplacer"/>
</bean>
```

> 注意上面的`replaced-method`元素的2个属性：
>
> name：用于指定当前bean需要被替换的方法
>
> replacer：替换者，即实现了MethodReplacer接口的类对应的bean
>
> 上面配置中当调用`serviceB`的getServiceA的时候，会自动调用`serviceAMethodReplacer`这个bean中的`reimplement`方法进行处理。

#### 案例

##### ServiceA.java

```
package com.javacode2018.lesson001.demo14;

public class ServiceA {
}
```

##### ServiceB.java

```
package com.javacode2018.lesson001.demo14;

public class ServiceB {

    public void say() {
        ServiceA serviceA = this.getServiceA();
        System.out.println("this:" + this + ",serviceA:" + serviceA);
    }

    public ServiceA getServiceA() { //@1
        return null;
    }
}
```

> 上面getServiceA需要返回一个ServiceA对象，此处返回的是null，下面我们通过spring对这个方法进行替换，然后从容器中获取ServiceA然后返回，下面我们来看看替换者的代码。

##### 替换者ServiceBMethodReplacer.java

> 这个替换者会替换ServiceB中的getServiceA方法

```
package com.javacode2018.lesson001.demo14;

import org.springframework.beans.BeansException;
import org.springframework.beans.factory.support.MethodReplacer;
import org.springframework.context.ApplicationContext;
import org.springframework.context.ApplicationContextAware;

import java.lang.reflect.Method;
import java.util.Map;

/**
 * servieB的方法替换者
 */
public class ServiceBMethodReplacer implements MethodReplacer, ApplicationContextAware {

    @Override
    public Object reimplement(Object obj, Method method, Object[] args) throws Throwable {
        return this.context.getBean(ServiceA.class);
    }

    private ApplicationContext context;

    @Override
    public void setApplicationContext(ApplicationContext applicationContext) throws BeansException {
        this.context = applicationContext;
    }
}
```

##### spring中bean配置文件：replacedmethod.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">
    <!-- 定义替换者bean -->
    <bean id="serviceBMethodReplacer" class="com.javacode2018.lesson001.demo14.ServiceBMethodReplacer" />

    <bean id="serviceA" class="com.javacode2018.lesson001.demo14.ServiceA" scope="prototype"/>

    <bean id="serviceB" class="com.javacode2018.lesson001.demo14.ServiceB">
        <replaced-method name="getServiceA" replacer="serviceBMethodReplacer"/>
    </bean>

</beans>
```

##### 测试用例ReplacedMethodTest

```
package com.javacode2018.lesson001.demo14;


import org.junit.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 公众号：程序员路人
 * replaced-method：方法替换
 */
public class ReplacedMethodTest {

    @Test
    public void replacedmethod() {
        String beanXml = "classpath:/com/javacode2018/lesson001/demo14/replacedmethod.xml";
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(beanXml);

        System.out.println(context.getBean(ServiceA.class)); //@1
        System.out.println(context.getBean(ServiceA.class)); //@2

        System.out.println("serviceB中的serviceA");
        ServiceB serviceB = context.getBean(ServiceB.class); //@3
        serviceB.say();
        serviceB.say();
    }

}
```

##### 运行输出

```
com.javacode2018.lesson001.demo14.ServiceA@7722c3c3
com.javacode2018.lesson001.demo14.ServiceA@2ef3eef9
serviceB中的serviceA
this:com.javacode2018.lesson001.demo14.ServiceB$$EnhancerBySpringCGLIB$$21bb8912@243c4f91,serviceA:com.javacode2018.lesson001.demo14.ServiceA@291ae
this:com.javacode2018.lesson001.demo14.ServiceB$$EnhancerBySpringCGLIB$$21bb8912@243c4f91,serviceA:com.javacode2018.lesson001.demo14.ServiceA@61df66b6
```

从输出中可以看出结果和lookup-method案例效果差不多，实现了单例bean中使用多例bean的案例。

输出中都有CGLIB这样的字样，说明这玩意也是通过cglib实现的。

## 总结

1. 1. lookup-method：方法查找，可以对指定的bean的方法进行拦截，然后从容器中查找指定的bean作为被拦截方法的返回值
   2. replaced-method：方法替换，可以实现bean方法替换的效果，整体来说比lookup-method更灵活一些
   3. 单例bean中使用多例bean，本文中列出了3种方式，大家消化一下