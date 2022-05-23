# Spring第19篇：SpringBoot底层原理深度解密，让你明明白白

[程序员路人](javascript:void(0);) *2022-04-19 21:06*

**大家好，我是路人~~~
**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

[**继续收徒弟，月薪四万以下的可以来找我**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)



**这是 Spring 第 19 篇，点击上方卡片，即可翻阅前面章节。**

## [灵魂拷问](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

\1. @Import你用过么？是做什么的？

\2. @Import使用有几种方式？有何区别？

\3. DeferredImportSelector是做什么的？他和ImportSelector有什么区别？

\4. spring中哪些功能是通过@Import来实现的？

\5. spring中是如何解析@Import注解的么？

\6. 本文搞懂后，SpringBoot的原理基本上就懂了，SpringBoot就是靠这个注解实现的

## @Import出现的背景

目前为止，注解的方式批量注册bean，前面2篇文章中，我们介绍了2种方式：

到目前，我们知道的批量定义bean的方式有2种：

1. 1. @Configuration结合@Bean注解的方式
   2. @CompontentScan扫描包的方式

下面我们来看几个问题。

### 问题1

如果需要注册的类是在第三方的jar中，那么我们如果想注册这些bean有2种方式：

1. 1. 通过@Bean标注方法的方式，一个个来注册
   2. @CompontentScan的方式：默认的@CompontentScan是无能为力的，默认情况下只会注册@Compontent标注的类，此时只能自定义@CompontentScan中的过滤器来实现了

这2种方式都不是太好，每次有变化，调整的代码都比较多。

### 问题2

通常我们的项目中有很多子模块，可能每个模块都是独立开发的，最后通过jar的方式引进来，每个模块中都有各自的@Configuration、@Bean标注的类，或者使用@CompontentScan标注的类，**被@Configuration、@Bean、@CompontentScan标注的类，我们统称为bean配置类，配置类可以用来注册bean**，此时如果我们只想使用其中几个模块的配置类，怎么办？

@Import可以很好的解决这2个问题，下面我们来看@Import怎么玩的。

## [@Import使用](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

先看Spring对它的注释，总结下来作用就是和xml配置的 <import />标签作用一样，允许通过它引入@Configuration标注的类 ， 引入ImportSelector接口和ImportBeanDefinitionRegistrar接口的实现，也包括 @Component注解的普通类。

**总的来说：@Import可以用来批量导入需要注册的各种类，如普通的类、配置类，完后完成普通类和配置类中所有bean的注册。**

@Import的源码：

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
public @interface Import {

    /**
     * {@link Configuration @Configuration}, {@link ImportSelector},
     * {@link ImportBeanDefinitionRegistrar}, or regular component classes to import.
     */
    Class<?>[] value();

}
```

> @Import可以使用在任何类型上，通常情况下，类和注解上用的比较多。
>
> value：一个Class数组，设置需要导入的类，可以是@Configuration标注的列，可以是ImportSelector接口或者ImportBeanDefinitionRegistrar接口类型的，或者需要导入的普通组件类。

### 使用步骤

\1. 将@Import标注在类上，设置value参数

\2. 将@Import标注的类作为AnnotationConfigApplicationContext构造参数创建AnnotationConfigApplicationContext对象

\3. 使用AnnotationConfigApplicationContext对象

### @Import的value常见的有5种用法

\1. value为普通的类

\2. value为@Configuration标注的类

\3. value为@CompontentScan标注的类

\4. value为ImportBeanDefinitionRegistrar接口类型

\5. value为ImportSelector接口类型

\6. value为DeferredImportSelector接口类型

下面我们通过案例来一个个详细介绍。

## [value为普通的类](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

来2个类

### Service1

```
package com.javacode2018.lesson001.demo24.test1;

public class Service1 {
}
```

### Service2

```
package com.javacode2018.lesson001.demo24.test1;

public class Service2 {
}
```

### 总配置类：使用@Import标注

```
package com.javacode2018.lesson001.demo24.test1;

import org.springframework.context.annotation.Import;

@Import({Service1.class, Service2.class})
public class MainConfig1 {
}
```

> @Import中导入了2个普通的类：Service1、Service2，这两个类会被自动注册到容器中

### 测试用例

```
package com.javacode2018.lesson001.demo24;

import com.javacode2018.lesson001.demo24.test1.MainConfig1;
import org.junit.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class ImportTest {
    @Test
    public void test1() {
        //1.通过AnnotationConfigApplicationContext创建spring容器，参数为@Import标注的类
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MainConfig1.class);
        //2.输出容器中定义的所有bean信息
        for (String beanName : context.getBeanDefinitionNames()) {
            System.out.println(String.format("%s->%s", beanName, context.getBean(beanName)));
        }
    }
}
```

### 运行输出

部分输出如下：

```
com.javacode2018.lesson001.demo24.test1.Service1->com.javacode2018.lesson001.demo24.test1.Service1@7e0b85f9
com.javacode2018.lesson001.demo24.test1.Service2->com.javacode2018.lesson001.demo24.test1.Service2@63355449
```

### 结果分析

从输出中可以看出：

\1. Service1和Service2成功注册到容器了。

\2. 通过@Import导入的2个类，bean名称为完整的类名

我们也可以指定被导入类的bean名称，使用@Compontent注解就可以了，如下：

```
@Component("service1")
public class Service1 {
}
```

再次运行test1输出：

```
service1->com.javacode2018.lesson001.demo24.test1.Service1@45efd90f
```

### 总结一下

**按模块的方式进行导入，需要哪个导入哪个，不需要的时候，直接修改一下总的配置类，调整一下@Import就可以了，非常方便。**

## [value为@Configuration标注的配置类](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

项目比较大的情况下，会按照模块独立开发，每个模块在maven中就表现为一个个的构建，然后通过坐标的方式进行引入需要的模块。

假如项目中有2个模块，2个模块都有各自的配置类，如下

### 模块1配置类

```
package com.javacode2018.lesson001.demo24.test2;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 模块1配置类
 */
@Configuration
public class ConfigModule1 {
    @Bean
    public String module1() {
        return "我是模块1配置类！";
    }
}
```

### 模块2配置类

```
package com.javacode2018.lesson001.demo24.test2;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

/**
 * 模块2配置类
 */
@Configuration
public class ConfigModule2 {
    @Bean
    public String module2() {
        return "我是模块2配置类！";
    }
}
```

### 总配置类：通过@Import导入2个模块的配置类

```
package com.javacode2018.lesson001.demo24.test2;

import org.springframework.context.annotation.Import;

/**
 * 通过Import来汇总多个@Configuration标注的配置类
 */
@Import({ConfigModule1.class, ConfigModule2.class}) //@1
public class MainConfig2 {
}
```

> @1导入了2个模块中的模块配置类，可以按需导入。

### 测试用例

ImportTest中新增个方法

```
@Test
public void test2() {
    //1.通过AnnotationConfigApplicationContext创建spring容器，参数为@Import标注的类
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MainConfig2.class);
    //2.输出容器中定义的所有bean信息
    for (String beanName : context.getBeanDefinitionNames()) {
        System.out.println(String.format("%s->%s", beanName, context.getBean(beanName)));
    }
}
```

### 运行输出

```
mainConfig2->com.javacode2018.lesson001.demo24.test2.MainConfig2@ba2f4ec
com.javacode2018.lesson001.demo24.test2.ConfigModule1->com.javacode2018.lesson001.demo24.test2.ConfigModule1$$EnhancerBySpringCGLIB$$700e65cd@1c1bbc4e
module1->我是模块1配置类！
com.javacode2018.lesson001.demo24.test2.ConfigModule2->com.javacode2018.lesson001.demo24.test2.ConfigModule2$$EnhancerBySpringCGLIB$$a87108ee@55fe41ea
module2->我是模块2配置类！
```

## [value为@CompontentScan标注的类](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

项目中分多个模块，每个模块有各自独立的包，我们在每个模块所在的包中配置一个@CompontentScan类，然后通过@Import来导入需要启用的模块。

### 定义模块1

2个组件和一个组件扫描类，模块1所有类所在的包为：

```
com.javacode2018.lesson001.demo24.test3.module1
```

#### 组件1：Module1Service1

```
package com.javacode2018.lesson001.demo24.test3.module1;

import org.springframework.stereotype.Component;

@Component
public class Module1Service1 {
}
```

#### 组件2：Module1Service2

```
package com.javacode2018.lesson001.demo24.test3.module1;

import org.springframework.stereotype.Component;

@Component
public class Module1Service2 {
}
```

#### 组件扫描类：CompontentScanModule1

负责扫描当前模块中的组件

```
package com.javacode2018.lesson001.demo24.test3.module1;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.stereotype.Component;

/**
 * 模块1的主键扫描
 */
@ComponentScan
public class CompontentScanModule1 {
}
```

### 同样的方式定义模块2

2个组件和一个组件扫描类，模块1所有类所在的包为：

```
com.javacode2018.lesson001.demo24.test3.module2
```

#### 组件1：Module2Service1

```
package com.javacode2018.lesson001.demo24.test3.module2;

import org.springframework.stereotype.Component;

@Component
public class Module2Service1 {
}
```

#### 组件2：Module2Service2

```
package com.javacode2018.lesson001.demo24.test3.module2;

import org.springframework.stereotype.Component;

@Component
public class Module2Service2 {
}
```

#### 组件扫描类：CompontentScanModule1

负责扫描当前模块中的组件

```
package com.javacode2018.lesson001.demo24.test3.module2;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.stereotype.Component;

/**
 * 模块2的组件扫描
 */
@ComponentScan
public class CompontentScanModule2 {
}
```

### 总配置类：通过@Import导入每个模块中的组件扫描类

```
package com.javacode2018.lesson001.demo24.test3;

import com.javacode2018.lesson001.demo24.test3.module1.CompontentScanModule1;
import com.javacode2018.lesson001.demo24.test3.module2.CompontentScanModule2;
import org.springframework.context.annotation.Import;

/**
 * 通过@Import导入多个@CompontentScan标注的配置类
 */
@Import({CompontentScanModule1.class, CompontentScanModule2.class}) //@1
public class MainConfig3 {
}
```

> @1导入了2个模块中的组件扫描类，可以按需导入。

### 测试用例

ImportTest中新增个方法

```
@Test
public void test3() {
    //1.通过AnnotationConfigApplicationContext创建spring容器，参数为@Import标注的类
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MainConfig3.class);
    //2.输出容器中定义的所有bean信息
    for (String beanName : context.getBeanDefinitionNames()) {
        System.out.println(String.format("%s->%s", beanName, context.getBean(beanName)));
    }
}
```

### 运行输出

部分输出如下：

```
module1Service1->com.javacode2018.lesson001.demo24.test3.module1.Module1Service1@5b239d7d
module1Service2->com.javacode2018.lesson001.demo24.test3.module1.Module1Service2@6572421
module2Service1->com.javacode2018.lesson001.demo24.test3.module2.Module2Service1@6b81ce95
module2Service2->com.javacode2018.lesson001.demo24.test3.module2.Module2Service2@2a798d51
```

两个模块中通过@Compontent定义的4个bean都输出了。

如果只想注册模块1中的bean，只需要修改一下@Import，去掉CompontentScanModule2，如下：

```
@Import({CompontentScanModule1.class})
```

再次运行输出：

```
module1Service1->com.javacode2018.lesson001.demo24.test3.module1.Module1Service1@6379eb
module1Service2->com.javacode2018.lesson001.demo24.test3.module1.Module1Service2@294425a7
```

此时模块2的bean就没有了。

## [先来了解一下相关的几个接口](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

### ImportBeanDefinitionRegistrar接口

**这个接口提供了通过spring容器api的方式直接向容器中注册bean**。

接口的完整名称：

```
org.springframework.context.annotation.ImportBeanDefinitionRegistrar
```

源码：

```
public interface ImportBeanDefinitionRegistrar {

    default void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry,
            BeanNameGenerator importBeanNameGenerator) {

        registerBeanDefinitions(importingClassMetadata, registry);
    }

    default void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
    }

}
```

2个默认方法，都可以用来调用spring容器api来注册bean。

#### 2个方法中主要有3个参数

##### importingClassMetadata

AnnotationMetadata类型的，通过这个可以获取被@Import注解标注的类所有注解的信息。

##### registry

BeanDefinitionRegistry类型，是一个接口，内部提供了注册bean的各种方法。

##### importBeanNameGenerator

BeanNameGenerator类型，是一个接口，内部有一个方法，用来生成bean的名称。

关于BeanDefinitionRegistry和BeanNameGenerator接口在来细说一下。

### BeanDefinitionRegistry接口：bean定义注册器

bean定义注册器，提供了bean注册的各种方法，来看一下源码：

```
public interface BeanDefinitionRegistry extends AliasRegistry {

    /**
     * 注册一个新的bean定义
     * beanName：bean的名称
     * beanDefinition：bean定义信息
     */
    void registerBeanDefinition(String beanName, BeanDefinition beanDefinition)
            throws BeanDefinitionStoreException;

    /**
     * 通过bean名称移除已注册的bean
     * beanName：bean名称
     */
    void removeBeanDefinition(String beanName) throws NoSuchBeanDefinitionException;

    /**
     * 通过名称获取bean的定义信息
     * beanName：bean名称
     */
    BeanDefinition getBeanDefinition(String beanName) throws NoSuchBeanDefinitionException;

    /**
     * 查看beanName是否注册过
     */
    boolean containsBeanDefinition(String beanName);

    /**
     * 获取已经定义（注册）的bean名称列表
     */
    String[] getBeanDefinitionNames();

    /**
     * 返回注册器中已注册的bean数量
     */
    int getBeanDefinitionCount();

    /**
     * 确定给定的bean名称或者别名是否已在此注册表中使用
     * beanName：可以是bean名称或者bean的别名
     */
    boolean isBeanNameInUse(String beanName);

}
```

基本上所有bean工厂都实现了这个接口，让bean工厂拥有bean注册的各种能力。

上面我们用到的`AnnotationConfigApplicationContext`类也实现了这个接口。

### BeanNameGenerator接口：bean名称生成器

bean名称生成器，这个接口只有一个方法，用来生成bean的名称：

```
public interface BeanNameGenerator {
    String generateBeanName(BeanDefinition definition, BeanDefinitionRegistry registry);
}
```

spring内置了3个实现

#### DefaultBeanNameGenerator

默认bean名称生成器，xml中bean未指定名称的时候，默认就会使用这个生成器，默认为：完整的类名#bean编号

#### AnnotationBeanNameGenerator

注解方式的bean名称生成器，比如通过@Component(bean名称)的方式指定bean名称，如果没有通过注解方式指定名称，默认会将完整的类名作为bean名称。

#### FullyQualifiedAnnotationBeanNameGenerator

将完整的类名作为bean的名称

### BeanDefinition接口：bean定义信息

用来表示bean定义信息的接口，我们向容器中注册bean之前，会通过xml或者其他方式定义bean的各种配置信息，bean的所有配置信息都会被转换为一个BeanDefinition对象，然后通过容器中BeanDefinitionRegistry接口中的方法，将BeanDefinition注册到spring容器中，完成bean的注册操作。

这个接口有很多实现类，有兴趣的可以去看看源码，BeanDefinition的各种用法，以后会通过专题细说。

## [value为ImportBeanDefinitionRegistrar接口类型](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

### 用法（4个步骤）

```
1. 定义ImportBeanDefinitionRegistrar接口实现类，在registerBeanDefinitions方法中使用registry来注册bean
2. 使用@Import来导入步骤1中定义的类
3. 使用步骤2中@Import标注的类作为AnnotationConfigApplicationContext构造参数创建spring容器
4. 使用AnnotationConfigApplicationContext操作bean
```

### 案例

来2个普通的类。

#### Service1

```
package com.javacode2018.lesson001.demo24.test4;

public class Service1 {
}
```

#### Service2

这个类中需要注入Service1

```
package com.javacode2018.lesson001.demo24.test4;

public class Service2 {
    private Service1 service1;

    public Service1 getService1() {
        return service1;
    }

    public void setService1(Service1 service1) {
        this.service1 = service1;
    }

    @Override
    public String toString() {
        return "Service2{" +
                "service1=" + service1 +
                '}';
    }
}
```

来个类实现ImportBeanDefinitionRegistrar接口，然后在里面实现上面2个类的注册，如下：

#### MyImportBeanDefinitionRegistrar

```
package com.javacode2018.lesson001.demo24.test4;

import org.springframework.beans.factory.config.BeanDefinition;
import org.springframework.beans.factory.support.BeanDefinitionBuilder;
import org.springframework.beans.factory.support.BeanDefinitionRegistry;
import org.springframework.context.annotation.ImportBeanDefinitionRegistrar;
import org.springframework.core.type.AnnotationMetadata;


public class MyImportBeanDefinitionRegistrar implements ImportBeanDefinitionRegistrar {
    @Override
    public void registerBeanDefinitions(AnnotationMetadata importingClassMetadata, BeanDefinitionRegistry registry) {
        //定义一个bean：Service1
        BeanDefinition service1BeanDinition = BeanDefinitionBuilder.genericBeanDefinition(Service1.class).getBeanDefinition();
        //注册bean
        registry.registerBeanDefinition("service1", service1BeanDinition);

        //定义一个bean：Service2，通过addPropertyReference注入service1
        BeanDefinition service2BeanDinition = BeanDefinitionBuilder.genericBeanDefinition(Service2.class).
                addPropertyReference("service1", "service1").
                getBeanDefinition();
        //注册bean
        registry.registerBeanDefinition("service2", service2BeanDinition);
    }
}
```

> 注意上面的registerBeanDefinitions方法，内部注册了2个bean，Service1和Service2。
>
> 上面使用了BeanDefinitionBuilder这个类，这个是BeanDefinition的构造器，内部提供了很多静态方法方便构建BeanDefinition对象。

上面定义的2个bean，和下面xml方式效果一样：

```
<bean id="service1" class="com.javacode2018.lesson001.demo24.test4.Service1" />
<bean id="service2" class="com.javacode2018.lesson001.demo24.test4.Service2">
    <property name="service1" ref="service1"/>
</bean>
```

#### 来个测试用例

ImportTest中新增个方法

```
@Test
public void test4() {
    //1.通过AnnotationConfigApplicationContext创建spring容器，参数为@Import标注的类
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MainConfig4.class);
    //2.输出容器中定义的所有bean信息
    for (String beanName : context.getBeanDefinitionNames()) {
        System.out.println(String.format("%s->%s", beanName, context.getBean(beanName)));
    }
}
```

#### 运行输出

```
service1->com.javacode2018.lesson001.demo24.test4.Service1@62150f9e
service2->Service2{service1=com.javacode2018.lesson001.demo24.test4.Service1@62150f9e}
```

## [value为ImportSelector接口类型](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

先来看一下ImportSelector接口

### ImportSelector接口

导入选择器，看一下源码：

```
public interface ImportSelector {

    /**
     * 返回需要导入的类名的数组，可以是任何普通类，配置类（@Configuration、@Bean、@CompontentScan等标注的类）
     * @importingClassMetadata：用来获取被@Import标注的类上面所有的注解信息
     */
    String[] selectImports(AnnotationMetadata importingClassMetadata);

}
```

### 用法（4个步骤）

```
1. 定义ImportSelector接口实现类，在selectImports返回需要导入的类的名称数组
2. 使用@Import来导入步骤1中定义的类
3. 使用步骤2中@Import标注的类作为AnnotationConfigApplicationContext构造参数创建spring容器
4. 使用AnnotationConfigApplicationContext操作bean
```

### 案例

#### 来个普通类：Service1

```
package com.javacode2018.lesson001.demo24.test5;

public class Service1 {
}
```

#### 来个@Configuration标注的配置类：Module1Config

```
package com.javacode2018.lesson001.demo24.test5;

import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Module1Config {
    @Bean
    public String name() {
        return "公众号：程序员路人";
    }

    @Bean
    public String address() {
        return "上海市";
    }
}
```

> 上面定义了两个string类型的bean：name和address

#### 下面自定义一个ImportSelector，然后返回上面2个类的名称

```
package com.javacode2018.lesson001.demo24.test5;

import org.springframework.context.annotation.ImportSelector;
import org.springframework.core.type.AnnotationMetadata;

public class MyImportSelector implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        return new String[]{
                Service1.class.getName(),
                Module1Config.class.getName()
        };
    }
}
```

#### 来个@Import标注的类，导入MyImportSelector

```
package com.javacode2018.lesson001.demo24.test5;

import com.javacode2018.lesson001.demo24.test4.MyImportBeanDefinitionRegistrar;
import org.springframework.context.annotation.Import;

/**
 * 通过@Import导入MyImportSelector接口实现类
 */
@Import({MyImportSelector.class})
public class MainConfig5 {
}
```

#### 新增测试用例

ImportTest中新增个方法

```
@Test
public void test5() {
    //1.通过AnnotationConfigApplicationContext创建spring容器，参数为@Import标注的类
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MainConfig5.class);
    //2.输出容器中定义的所有bean信息
    for (String beanName : context.getBeanDefinitionNames()) {
        System.out.println(String.format("%s->%s", beanName, context.getBean(beanName)));
    }
}
```

#### 运行输出

部分输出如下：

```
com.javacode2018.lesson001.demo24.test5.Service1->com.javacode2018.lesson001.demo24.test5.Service1@45b4c3a9
name->公众号：程序员路人
address->上海市
```

输出中可以看到Service1以及Module1Config中定义的2个bean都有了。

## [来一个牛逼的案例](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

### 需求

凡是类名中包含service的，调用他们内部任何方法，我们希望调用之后能够输出这些方法的耗时。

### 实现分析

之前我们讲过代理， 此处我们就可以通过代理来实现，bean实例创建的过程中，我们可以给这些bean生成一个代理，在代理中统计方法的耗时，这里面有2点：

\1. 创建一个代理类，通过代理来间接访问需要统计耗时的bean对象

\2. 拦截bean的创建，给bean实例生成代理生成代理

### 具体实现

#### 先来两个Service类

##### Service1

```
package com.javacode2018.lesson001.demo24.test6;

import org.springframework.stereotype.Component;

@Component
public class Service1 {
    public void m1() {
        System.out.println(this.getClass() + ".m1()");
    }
}
```

##### Service2

```
package com.javacode2018.lesson001.demo24.test6;

import org.springframework.stereotype.Component;

@Component
public class Service2 {
    public void m1() {
        System.out.println(this.getClass() + ".m1()");
    }
}
```

#### 创建统计耗时的代理类

下面我们使用cglib来实现一个代理类，如下：

```
package com.javacode2018.lesson001.demo24.test6;


import org.springframework.cglib.proxy.Enhancer;
import org.springframework.cglib.proxy.MethodInterceptor;
import org.springframework.cglib.proxy.MethodProxy;

import java.lang.reflect.Method;

public class CostTimeProxy implements MethodInterceptor {
    //目标对象
    private Object target;

    public CostTimeProxy(Object target) {
        this.target = target;
    }

    @Override
    public Object intercept(Object o, Method method, Object[] objects, MethodProxy methodProxy) throws Throwable {
        long starTime = System.nanoTime();
        //调用被代理对象（即target）的方法，获取结果
        Object result = method.invoke(target, objects); //@1
        long endTime = System.nanoTime();
        System.out.println(method + "，耗时(纳秒)：" + (endTime - starTime));
        return result;
    }

    /**
     * 创建任意类的代理对象
     *
     * @param target
     * @param <T>
     * @return
     */
    public static <T> T createProxy(T target) {
        CostTimeProxy costTimeProxy = new CostTimeProxy(target);
        Enhancer enhancer = new Enhancer();
        enhancer.setCallback(costTimeProxy);
        enhancer.setSuperclass(target.getClass());
        return (T) enhancer.create();
    }
}
```

> createProxy方法可以用来给某个对象生成代理对象
>
> 需要了解cglib的可以看：[代理详解（Java动态代理&cglib代理）](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485893&idx=1&sn=240a4308c7c5134400d8e4b673e010e4&chksm=c0fb57daf78cdecc07c5d16165f08bb5c9a6e3065ac67b32620805af82607fd28c0524968579&scene=21#wechat_redirect)

#### 拦截bean实例的创建，返回代理对象

这里我们需要用到spring中的一个接口：

```
org.springframework.beans.factory.config.BeanPostProcessor

public interface BeanPostProcessor {

    /**
     * bean初始化之后会调用的方法
     */
    @Nullable
    default Object postProcessBeforeInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

    /**
     * bean初始化之后会调用的方法
     */
    @Nullable
    default Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        return bean;
    }

}
```

这个接口是bean处理器，内部有2个方法，分别在bean初始化前后会进行调用，以后讲声明周期的时候还会细说的，这里你只需要知道bean初始化之后会调用`postProcessAfterInitialization`方法就行，这个方法中我们会给bean创建一个代理对象。

下面我们创建一个BeanPostProcessor实现类：

```
package com.javacode2018.lesson001.demo24.test6;

import com.javacode2018.lesson001.demo23.test4.CostTimeProxy;
import org.springframework.beans.BeansException;
import org.springframework.beans.factory.config.BeanPostProcessor;
import org.springframework.lang.Nullable;

public class MethodCostTimeProxyBeanPostProcessor implements BeanPostProcessor {
    @Nullable
    @Override
    public Object postProcessAfterInitialization(Object bean, String beanName) throws BeansException {
        if (bean.getClass().getName().toLowerCase().contains("service")) {
            return CostTimeProxy.createProxy(bean); //@1
        } else {
            return bean;
        }
    }
}
```

> @1：使用上面创建代理类来给当前bean对象创建一个代理

需要将MethodCostTimeProxyBeanPostProcessor注册到容器中才会起作用，下面我们通过@Import结合ImportSelector的方式来导入这个类，将其注册到容器中。

#### MethodCostTimeImportSelector

```
package com.javacode2018.lesson001.demo24.test6;


import com.javacode2018.lesson001.demo23.test4.MethodCostTimeProxyBeanPostProcessor;
import org.springframework.context.annotation.ImportSelector;
import org.springframework.core.type.AnnotationMetadata;

public class MethodCostTimeImportSelector implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        return new String[]{MethodCostTimeProxyBeanPostProcessor.class.getName()};
    }
}
```

#### 来一个@Import来导入MethodCostTimeImportSelector

下面我们使用注解的方式，在注解上使用@Import，如下：

```
package com.javacode2018.lesson001.demo24.test6;


import com.javacode2018.lesson001.demo23.test4.MethodCostTimeImportSelector;
import org.springframework.context.annotation.Import;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Import(MethodCostTimeImportSelector.class)
public @interface EnableMethodCostTime {
}
```

#### 来一个总的配置类

```
package com.javacode2018.lesson001.demo24.test6;

import org.springframework.context.annotation.ComponentScan;

@ComponentScan
@EnableMethodCostTime //@1
public class MainConfig6 {
}
```

> 上面使用了@CompontentScan注解，此时会将Servce1和Service2这两个类注册到容器中。
>
> @1：此处使用了@EnableMethodCostTime注解，而@EnableMethodCostTime注解上使用了@Import(MethodCostTimeImportSelector.class)，此时MethodCostTimeImportSelector类中的MethodCostTimeProxyBeanPostProcessor会被注册到容器，会拦截bean的创建，创建耗时代理对象。

#### 来个测试用例

ImportTest中新增个方法

```
@Test
public void test6() {
    //1.通过AnnotationConfigApplicationContext创建spring容器，参数为@Import标注的类
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MainConfig6.class);
    Service1 service1 = context.getBean(Service1.class);
    Service2 service2 = context.getBean(Service2.class);
    service1.m1();
    service2.m1();
}
```

> 上面会调用service1和service2的方法

#### 运行输出

```
class com.javacode2018.lesson001.demo24.test6.Service1.m1()
public void com.javacode2018.lesson001.demo24.test6.Service1.m1()，耗时(纳秒)：74200
class com.javacode2018.lesson001.demo24.test6.Service2.m1()
public void com.javacode2018.lesson001.demo24.test6.Service2.m1()，耗时(纳秒)：33800
```

**太牛逼了，需求实现了。**

**如果我们不想开启方法耗时统计，只需要将MainConfig6上的@EnableMethodCostTime去掉就可以了，用起来是不是特别爽。**

**spring中有很多类似的注解，以@EnableXXX开头的注解，基本上都是通过上面这种方式实现的，如：**

```
@EnableAspectJAutoProxy
@EnableCaching
@EnableAsync
```

继续向下看，还有一个更牛逼的接口DeferredImportSelector。

## [DeferredImportSelector接口](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

**先给你透露一下，springboot中的核心功能@EnableAutoConfiguration就是靠DeferredImportSelector来实现的。**

DeferredImportSelector是ImportSelector的子接口，既然是ImportSelector的子接口，所以也可以通过@Import进行导入，这个接口和ImportSelector不同地方有两点：

\1. 延迟导入

\2. 指定导入的类的处理顺序

### 延迟导入

比如@Import的value包含了多个普通类、多个@Configuration标注的配置类、多个ImportSelector接口的实现类，多个ImportBeanDefinitionRegistrar接口的实现类，还有DeferredImportSelector接口实现类，此时spring处理这些被导入的类的时候，**会将DeferredImportSelector类型的放在最后处理，会先处理其他被导入的类，其他类会按照value所在的前后顺序进行处理**。

那么我们是可以做很多事情的，比如我们可以在DeferredImportSelector导入的类中判断一下容器中是否已经注册了某个bean，如果没有注册过，那么再来注册。

以后我们会讲到另外一个注解@Conditional，这个注解可以按条件来注册bean，比如可以判断某个bean不存在的时候才进行注册，某个类存在的时候才进行注册等等各种条件判断，通过@Conditional来结合DeferredImportSelector可以做很多事情。

### 来个延迟导入的案例

来3个配置类，每个配置类中都通过@Bean定一个string类型的bean，内部输出一句文字。

#### Configuration1

```
package com.javacode2018.lesson001.demo24.test7;


import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Configuration1 {
    @Bean
    public String name1() {
        System.out.println("name1");
        return "name1";
    }
}
```

#### Configuration2

```
package com.javacode2018.lesson001.demo24.test7;


import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Configuration2 {
    @Bean
    public String name2() {
        System.out.println("name2");
        return "name2";
    }
}
```

#### Configuration3

```
package com.javacode2018.lesson001.demo24.test7;


import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Configuration3 {
    @Bean
    public String name3() {
        System.out.println("name3");
        return "name3";
    }
}
```

#### 来一个ImportSelector实现类，导入Configuration1

```
package com.javacode2018.lesson001.demo24.test7;

import org.springframework.context.annotation.ImportSelector;
import org.springframework.core.type.AnnotationMetadata;

public class ImportSelector1 implements ImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        return new String[]{
                Configuration1.class.getName()
        };
    }
}
```

#### 来一个DeferredImportSelector实现类，导入Configuration2

```
package com.javacode2018.lesson001.demo24.test7;

import org.springframework.context.annotation.DeferredImportSelector;
import org.springframework.core.type.AnnotationMetadata;

public class DeferredImportSelector1 implements DeferredImportSelector {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        return new String[]{Configuration2.class.getName()};
    }
}
```

#### 来一个总的配置类

```
package com.javacode2018.lesson001.demo24.test7;

import org.springframework.context.annotation.Import;

@Import({
        DeferredImportSelector1.class,
        Configuration3.class,
        ImportSelector1.class,
})
public class MainConfig7 {
}
```

> 注意上面的@Import中被导入类的顺序：
>
> DeferredImportSelector1->Configuration3->ImportSelector1

下面来个测试用例，看一下3个配置文件中@Bean标注的方法被执行的先后顺序。

#### 测试用例

ImportTest中新增个方法

```
@Test
public void test7() {
    //1.通过AnnotationConfigApplicationContext创建spring容器，参数为@Import标注的类
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MainConfig7.class);
}
```

#### 运行输出

```
name3
name1
name2
```

**输出的结果结合一下@Import中被导入的3个类的顺序，可以看出DeferredImportSelector1是被最后处理的，其他2个是按照value中所在的先后顺序处理的。**

### 指定导入的类的处理顺序

当@Import中有多个DeferredImportSelector接口的实现类时候，可以指定他们的顺序，指定顺序常见2种方式

#### 实现Ordered接口的方式

```
org.springframework.core.Ordered

public interface Ordered {

    int HIGHEST_PRECEDENCE = Integer.MIN_VALUE;

    int LOWEST_PRECEDENCE = Integer.MAX_VALUE;

    int getOrder();

}
```

> value的值越小，优先级越高。

#### 实现Order注解的方式

```
org.springframework.core.annotation.Order

@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.METHOD, ElementType.FIELD})
@Documented
public @interface Order {

    int value() default Ordered.LOWEST_PRECEDENCE;

}
```

> value的值越小，优先级越高。

下面我们来个案例感受一下。

### 来个指定导入类处理顺序的案例

来2个配置类，内部都有一个@Bean标注的方法，用来注册一个bean，方法内部输出一行文字

#### Configuration1

```
package com.javacode2018.lesson001.demo24.test8;


import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Configuration1 {
    @Bean
    public String name1() {
        System.out.println("name1");
        return "name1";
    }
}
```

#### Configuration2

```
package com.javacode2018.lesson001.demo24.test8;


import org.springframework.context.annotation.Bean;
import org.springframework.context.annotation.Configuration;

@Configuration
public class Configuration2 {
    @Bean
    public String name2() {
        System.out.println("name2");
        return "name2";
    }
}
```

来2个DeferredImportSelector实现类，分别来导入上面2个配置文件，顺便通过Ordered接口指定一下顺序

#### DeferredImportSelector1

```
package com.javacode2018.lesson001.demo24.test8;

import org.springframework.context.annotation.DeferredImportSelector;
import org.springframework.core.Ordered;
import org.springframework.core.type.AnnotationMetadata;

public class DeferredImportSelector1 implements DeferredImportSelector, Ordered {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        return new String[]{Configuration1.class.getName()};
    }

    @Override
    public int getOrder() {
        return 2;
    }
}
```

#### DeferredImportSelector2

```
package com.javacode2018.lesson001.demo24.test8;

import com.javacode2018.lesson001.demo24.test7.Configuration2;
import org.springframework.context.annotation.DeferredImportSelector;
import org.springframework.core.Ordered;
import org.springframework.core.type.AnnotationMetadata;

public class DeferredImportSelector2 implements DeferredImportSelector, Ordered {
    @Override
    public String[] selectImports(AnnotationMetadata importingClassMetadata) {
        return new String[]{Configuration2.class.getName()};
    }

    @Override
    public int getOrder() {
        return 1;
    }
}
```

**DeferredImportSelector1的order为2，DeferredImportSelector2的order为1，order值越小优先级越高。**

来个总的配置类，引入上面两个ImportSelector

#### MainConfig8

```
package com.javacode2018.lesson001.demo24.test8;


import org.springframework.context.annotation.Import;

@Import({
        DeferredImportSelector1.class,
        DeferredImportSelector2.class,
})
public class MainConfig8 {
}
```

#### 测试用例

ImportTest中新增个方法

```
@Test
public void test8() {
    //1.通过AnnotationConfigApplicationContext创建spring容器，参数为@Import标注的类
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(MainConfig8.class);
}
```

#### 运行输出

```
name2
name1
```

**结果配合order的值，按照order从小到大来处理，可以看出DeferredImportSelector2先被处理的。**

## [Spring中这块的源码](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

**@Import注解是被下面这个类处理的**

```
org.springframework.context.annotation.ConfigurationClassPostProcessor
```

> 前面介绍的@Configuration、@Bean、@CompontentScan、@CompontentScans都是被这个类处理的，这个类是高手必经之路，建议花点时间研究研究。

## [案例源码](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

```
https://gitee.com/javacode2018/spring-series
```

**程序员路人所有案例代码以后都会放到这个上面，大家watch一下，可以持续关注动态。**

## [总结](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485908&idx=2&sn=b21a888ef0cf2fa89b816c4312760f7b&chksm=c0fb57cbf78cdedd004bf45fa56799496cc009347656326fc2e5d23bf374ac59293bbaf4ca77&scene=21#wechat_redirect)

\1. @Import可以用来批量导入任何普通的组件、配置类，将这些类中定义的所有bean注册到容器中

\2. @Import常见的5种用法需要掌握

3.掌握ImportSelector、ImportBeanDefinitionRegistrar、DeferredImportSelector的用法

\4. DeferredImportSelector接口可以实现延迟导入、按序导入的功能

\5. spring中很多以@Enable开头的都是使用@Import集合ImportSelector方式实现的

\6. BeanDefinitionRegistry接口：bean定义注册器，这个需要掌握常见的方法