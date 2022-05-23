# Spring第18篇：@ComponentScan、@ComponentScans详解

路人甲Java [程序员路人](javascript:void(0);) *2022-04-18 20:20*

**大家好，我是路人~~~
**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

[**继续收徒弟，月薪四万以下的可以来找我，四万以上的靠你们自己的天赋了**](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)



**这是 Spring 第 18 篇，点击上方卡片，即可翻阅前面章节。**

## [灵魂拷问](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect) 

1. 1. @ComponentScan注解是做什么的？
   2. basePackages的方式和basePackageClasses的方式有什么区别？你建议用哪个？为什么？
   3. useDefaultFilters有什么用？
   4. 常见的过滤器有哪些类型？说说你知道的几个
   5. @ComponentScan是在哪个类中处理的？说一下大概的解析过程？

这些问题如果都ok，恭喜你，太优秀了，不知道没关系，一起来看看。

## [背景介绍](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

到目前为止，介绍了2种注册bean的方式：

1. 1. xml中bean元素的方式
   2. @Bean注解标注方法的方式

通常情况下，项目中大部分类都需要交给spring去管理，按照上面这2种方式，代码量还是挺大的。

为了更方便bean的注册，Spring提供了批量的方式注册bean，方便大量bean批量注册，spring中的@ComponentScan就是干这个事情的。

## [@ComponentScan](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

@ComponentScan用于批量注册bean。

这个注解会让spring去扫描某些包及其子包中所有的类，然后将满足一定条件的类作为bean注册到spring容器容器中。

具体需要扫描哪些包？以及这些包中的类满足什么条件时被注册到容器中，这些都可以通过这个注解中的参数动态配置。

先来看一下这个注解的定义：

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@Documented
@Repeatable(ComponentScans.class) //@1
public @interface ComponentScan {

    @AliasFor("basePackages")
    String[] value() default {};

    @AliasFor("value")
    String[] basePackages() default {};

    Class<?>[] basePackageClasses() default {};

    Class<? extends BeanNameGenerator> nameGenerator() default BeanNameGenerator.class;

    Class<? extends ScopeMetadataResolver> scopeResolver() default AnnotationScopeMetadataResolver.class;

    ScopedProxyMode scopedProxy() default ScopedProxyMode.DEFAULT;

    String resourcePattern() default "**/*.class";

    boolean useDefaultFilters() default true;

    Filter[] includeFilters() default {};

    Filter[] excludeFilters() default {};

    boolean lazyInit() default false;
}
```

> 定义上可以看出此注解可以用在任何类型上面，不过我们通常将其用在类上面。
>
> 常用参数：
>
> value：指定需要扫描的包，如：com.javacode2018
>
> basePackages：作用同value；value和basePackages不能同时存在设置，可二选一
>
> basePackageClasses：指定一些类，spring容器会扫描这些类所在的包及其子包中的类
>
> nameGenerator：自定义bean名称生成器
>
> resourcePattern：需要扫描包中的那些资源，默认是：**/*.class，即会扫描指定包中所有的class文件
>
> useDefaultFilters：对扫描的类是否启用默认过滤器，默认为true
>
> includeFilters：过滤器：用来配置被扫描出来的那些类会被作为组件注册到容器中
>
> excludeFilters：过滤器，和includeFilters作用刚好相反，用来对扫描的类进行排除的，被排除的类不会被注册到容器中
>
> lazyInit：是否延迟初始化被注册的bean
>
> @1：@Repeatable(ComponentScans.class)，这个注解可以同时使用多个。

**@ComponentScan工作的过程：**

**1. Spring会扫描指定的包，且会递归下面子包，得到一批类的数组**

**2. 然后这些类会经过上面的各种过滤器，最后剩下的类会被注册到容器中**

**所以玩这个注解，主要关注2个问题：**

**第一个：需要扫描哪些包？通过`value、backPackages、basePackageClasses`这3个参数来控制**

**第二：过滤器有哪些？通过`useDefaultFilters、includeFilters、excludeFilters`这3个参数来控制过滤器**

**这两个问题搞清楚了，就可以确定哪些类会被注册到容器中。**

**默认情况下，任何参数都不设置的情况下，此时，会将@ComponentScan修饰的类所在的包作为扫描包；默认情况下useDefaultFilters为true，这个为true的时候，spring容器内部会使用默认过滤器，规则是：凡是类上有`@Repository、@Service、@Controller、@Component`这几个注解中的任何一个的，那么这个类就会被作为bean注册到spring容器中，所以默认情况下，只需在类上加上这几个注解中的任何一个，这些类就会自动交给spring容器来管理了。**

## [@Component、@Repository、@Service、@Controller](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

这几个注解都是spring提供的。

先说一下`@Component`这个注解，看一下其定义：

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Indexed
public @interface Component {
    String value() default "";
}
```

> 从定义中可以看出，这个注解可以用在任何类型上面。
>
> 通常情况下将这个注解用在类上面，标注这个类为一个组件，默认情况下，被扫描的时候会被作为bean注册到容器中。
>
> value参数：被注册为bean的时候，用来指定bean的名称，如果不指定，默认为类名首字母小写。如：类UserService对应的beanname为userService

再来看看`@Repository`源码如下：

```
@Target({ElementType.TYPE})
@Retention(RetentionPolicy.RUNTIME)
@Documented
@Component
public @interface Repository {

    @AliasFor(annotation = Component.class)
    String value() default "";

}
```

> Repository上面有@Component注解。
>
> value参数上面有`@AliasFor(annotation = Component.class)`，设置value参数的时候，也相当于给`@Component`注解中的value设置值。

其他两个注解`@Service、@Controller`源码和`@Repository`源码类似。

这4个注解本质上是没有任何差别，都可以用在类上面，表示这个类被spring容器扫描的时候，可以作为一个bean组件注册到spring容器中。

spring容器中对这4个注解的解析并没有进行区分，统一采用`@Component`注解的方式进行解析，所以这几个注解之间可以相互替换。

spring提供这4个注解，是为了让系统更清晰，通常情况下，系统是分层结构的，多数系统一般分为controller层、service层、dao层。

@controller通常用来标注controller层组件，@service注解标注service层的组件，@Repository标注dao层的组件，这样可以让整个系统的结构更清晰，当看到这些注解的时候，会和清晰的知道属于哪个层，对于spring来说，将这3个注解替换成@Component注解，对系统没有任何影响，产生的效果是一样的。

下面通过案例来感受@ComponentScan各种用法。

## [案例1：任何参数未设置](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

#### UserController

```
package com.javacode2018.lesson001.demo22.test1.controller;

import org.springframework.stereotype.Controller;

@Controller
public class UserController {
}
```

#### UserService

```
package com.javacode2018.lesson001.demo22.test1.service;

import org.springframework.stereotype.Service;

@Service
public class UserService {
}
```

#### UserDao

```
package com.javacode2018.lesson001.demo22.test1.dao;

import org.springframework.stereotype.Repository;

@Repository
public class UserDao {
}
```

#### UserModel

```
package com.javacode2018.lesson001.demo22.test1;

import org.springframework.stereotype.Component;

@Component
public class UserModel {
}
```

上面几个类中，分别使用了4种注解。

#### @CompontentScan修饰的类

```
package com.javacode2018.lesson001.demo22.test1;

import org.springframework.context.annotation.ComponentScan;

@ComponentScan
public class ScanBean1 {
}
```

#### 上面几个类的结构图

[![图片](https://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr0Q8Eny8IWTibC6BpbW4L6P8iclhV7cjlJ6xkyQ7ia80Z8fhv9iaSOM7cibic1ddOuEz4V4Qor4uicXXBZOA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

#### 测试用例

```
package com.javacode2018.lesson001.demo22;

import com.javacode2018.lesson001.demo22.test1.ScanBean1;
import org.junit.Test;
import org.springframework.context.annotation.AnnotationConfigApplicationContext;

public class ComponentScanTest {

    @Test
    public void test1() {
        AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ScanBean1.class);
        for (String beanName : context.getBeanDefinitionNames()) {
            System.out.println(beanName + "->" + context.getBean(beanName));
        }
    }
}
```

> @1：使用AnnotationConfigApplicationContext作为ioc容器，将`ScanBean`作为参数传入。
>
> 默认会扫描`ScanBean`类所在的包中的所有类，类上有@Component、@Repository、@Service、@Controller任何一个注解的都会被注册到容器中

#### 运行输出

部分输出如下：

```
userModel->com.javacode2018.lesson001.demo22.test1.UserModel@595b007d
userController->com.javacode2018.lesson001.demo22.test1.controller.UserController@72d1ad2e
userDao->com.javacode2018.lesson001.demo22.test1.dao.UserDao@2d7275fc
userService->com.javacode2018.lesson001.demo22.test1.service.UserService@399f45b1
```

> 注意最后4行这几个bean，都被注册成功了。

## [案例2：指定需要扫描的包](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

指定需要扫毛哪些包，可以通过value或者basePackage来配置，二者选其一，都配置运行会报错，下面我们通过value来配置。

#### ScanBean2

```
package com.javacode2018.lesson001.demo22.test2;

import org.springframework.context.annotation.ComponentScan;

@ComponentScan({
        "com.javacode2018.lesson001.demo22.test1.controller",
        "com.javacode2018.lesson001.demo22.test1.service"
})
public class ScanBean2 {
}
```

> 上面指定了2需要扫描的包，这两个包中有2个类。

#### 测试用例

ComponentScanTest中新增个方法

```
@Test
public void test2() {
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ScanBean2.class);
    for (String beanName : context.getBeanDefinitionNames()) {
        System.out.println(beanName + "->" + context.getBean(beanName));
    }
}
```

#### 运行输出

截取了关键几行如下：

```
userController->com.javacode2018.lesson001.demo22.test1.controller.UserController@dd8ba08
userService->com.javacode2018.lesson001.demo22.test1.service.UserService@245b4bdc
```

可以看出只有controller包和service包中的2个类被注册为bean了。

#### 注意

**指定包名的方式扫描存在的一个隐患，若包被重名了，会导致扫描会失效，一般情况下面我们使用basePackageClasses的方式来指定需要扫描的包，这个参数可以指定一些类型，默认会扫描这些类所在的包及其子包中所有的类，这种方式可以有效避免这种问题。**

下面来看一下basePackageClasses的方式。

## [案例：basePackageClasses指定扫描范围](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

我们可以在需要扫描的包中定义一个标记的接口或者类，他们的唯一的作用是作为basePackageClasses的值，其他没有任何用途。

#### 下面我们定义这样一个接口

```
package com.javacode2018.lesson001.demo22.test6.beans;

public interface ScanClass {
}
```

#### 再来定义2个类，用@Component注解标记

```
package com.javacode2018.lesson001.demo22.test6.beans;

import org.springframework.stereotype.Component;

@Component
public class Service1 {
}
package com.javacode2018.lesson001.demo22.test6.beans;

import org.springframework.stereotype.Component;

@Component
public class Service2 {
}
```

#### 来一个@CompontentScan标记的类

```
package com.javacode2018.lesson001.demo22.test6;

import com.javacode2018.lesson001.demo22.test6.beans.ScanClass;
import org.springframework.context.annotation.ComponentScan;

@ComponentScan(basePackageClasses = ScanClass.class)
public class ScanBean6 {
}
```

#### 测试用例

ComponentScanTest中新增个方法

```
@Test
public void test6() {
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ScanBean6.class);
    for (String beanName : context.getBeanDefinitionNames()) {
        System.out.println(beanName + "->" + context.getBean(beanName));
    }
}
```

#### 运行输出

```
service1->com.javacode2018.lesson001.demo22.test6.beans.Service1@79924b
service2->com.javacode2018.lesson001.demo22.test6.beans.Service2@7b9a4292
```

## [includeFilters的使用](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

### 用法

再来看一下includeFilters这个参数的定义：

```
Filter[] includeFilters() default {};
```

是一个`Filter`类型的数组，**多个Filter之间为或者关系，即满足任意一个就可以了**，看一下`Filter`的代码：

```
@Retention(RetentionPolicy.RUNTIME)
@Target({})
@interface Filter {

    FilterType type() default FilterType.ANNOTATION;

    @AliasFor("classes")
    Class<?>[] value() default {};

    @AliasFor("value")
    Class<?>[] classes() default {};

    String[] pattern() default {};

}
```

> 可以看出Filter也是一个注解，参数：
>
> **type：过滤器的类型，是个枚举类型，5种类型**
>
> ANNOTATION：通过注解的方式来筛选候选者，即判断候选者是否有指定的注解
>
> ASSIGNABLE_TYPE：通过指定的类型来筛选候选者，即判断候选者是否是指定的类型
>
> ASPECTJ：ASPECTJ表达式方式，即判断候选者是否匹配ASPECTJ表达式
>
> REGEX：正则表达式方式，即判断候选者的完整名称是否和正则表达式匹配
>
> CUSTOM：用户自定义过滤器来筛选候选者，对候选者的筛选交给用户自己来判断
>
> **value：和参数classes效果一样，二选一**
>
> **classes：3种情况如下**
>
> 当type=FilterType.ANNOTATION时，通过classes参数可以指定一些注解，用来判断被扫描的类上是否有classes参数指定的注解
>
> 当type=FilterType.ASSIGNABLE_TYPE时，通过classes参数可以指定一些类型，用来判断被扫描的类是否是classes参数指定的类型
>
> 当type=FilterType.CUSTOM时，表示这个过滤器是用户自定义的，classes参数就是用来指定用户自定义的过滤器，自定义的过滤器需要实现org.springframework.core.type.filter.TypeFilter接口
>
> **pattern：2种情况如下**
>
> 当type=FilterType.ASPECTJ时，通过pattern来指定需要匹配的ASPECTJ表达式的值
>
> 当type=FilterType.REGEX时，通过pattern来自正则表达式的值

### 案例：扫描包含注解的类

#### 需求

我们自定义一个注解，让标注有这些注解的类自动注册到容器中

#### 代码实现

下面的代码都在`com.javacode2018.lesson001.demo22.test3`包中。

##### 定义一个注解

```
package com.javacode2018.lesson001.demo22.test3;

import java.lang.annotation.*;

@Documented
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
public @interface MyBean {
}
```

##### 创建一个类，使用这个注解标注

```
package com.javacode2018.lesson001.demo22.test3;

@MyBean
public class Service1 {
}
```

##### 再来一个类，使用spring中的`@Compontent`标注

```
package com.javacode2018.lesson001.demo22.test3;

import org.springframework.stereotype.Component;

@Component
public class Service2 {
}
```

##### 再来一个类，使用@CompontentScan标注

```
package com.javacode2018.lesson001.demo22.test3;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.FilterType;

@ComponentScan(includeFilters = {
        @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = MyBean.class)
})
public class ScanBean3 {
}
```

> 上面指定了Filter的type为注解的类型，只要类上面有`@MyBean`注解的，都会被作为bean注册到容器中。

##### 测试用例

ComponentScanTest中新增个测试用例

```
@Test
public void test3() {
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ScanBean3.class);
    for (String beanName : context.getBeanDefinitionNames()) {
        System.out.println(beanName + "->" + context.getBean(beanName));
    }
}
```

##### 运行输出，截取了主要的几行

```
service1->com.javacode2018.lesson001.demo22.test3.Service1@6b81ce95
service2->com.javacode2018.lesson001.demo22.test3.Service2@2a798d51
```

**Service1上标注了`@MyBean`注解，被注册到容器了，但是`Service2`上没有标注`@MyBean`啊，怎么也被注册到容器了？**

**原因：Service2上标注了`@Compontent`注解，而@CompontentScan注解中的`useDefaultFilters`默认是`true`，表示也会启用默认的过滤器，而默认的过滤器会将标注有`@Component、@Repository、@Service、@Controller`这几个注解的类也注册到容器中**

如果我们只想将标注有`@MyBean`注解的bean注册到容器，需要将默认过滤器关闭，即：useDefaultFilters=false，我们修改一下ScanBean3的代码如下：

```
@ComponentScan(
        useDefaultFilters = false, //不启用默认过滤器
        includeFilters = {
                @ComponentScan.Filter(type = FilterType.ANNOTATION, classes = MyBean.class)
        })
public class ScanBean3 {
}
```

再次运行`test3`输出：

```
service1->com.javacode2018.lesson001.demo22.test3.Service1@294425a7
```

#### 扩展：自定义注解支持定义bean名称

上面的自定义的@MyBean注解，是无法指定bean的名称的，可以对这个注解做一下改造，加个value参数来指定bean的名称，如下：

```
@Documented
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@Component //@1
public @interface MyBean {
    @AliasFor(annotation = Component.class) //@2
    String value() default ""; //@3
}
```

> 重点在于@1和@2这2个地方的代码，通过上面的参数可以间接给@Component注解中的value设置值。
>
> 这块用到了@AliasFor注解，对这块不了解的，可以去看一下：[java注解详解及spring对注解的增强](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485896&idx=1&sn=1a46196c00486d291e4dcfdb4c079c2c&chksm=c0fb57d7f78cdec179fd576c1fde9695192d1f0f15dcfe341437ae07681d209dd8df72eb7791&scene=21#wechat_redirect)

修改一下Service1的代码：

```
@MyBean("service1Bean")
public class Service1 {
}
```

运行test3用例输出：

```
service1Bean->com.javacode2018.lesson001.demo22.test3.Service1@222545dc
```

此时bean名称就变成了`service1Bean`。

### 案例：包含指定类型的类

下面的代码都位于`com.javacode2018.lesson001.demo22.test4`包中。

#### 来个接口

```
package com.javacode2018.lesson001.demo22.test4;

public interface IService {
}
```

让spring来进行扫描，类型满足IService的都将其注册到容器中。

#### 来2个实现类

```
package com.javacode2018.lesson001.demo22.test4;

public class Service1 implements IService {
}
package com.javacode2018.lesson001.demo22.test4;

public class Service2 implements IService {
}
```

#### 来一个@CompontentScan标注的类

```
package com.javacode2018.lesson001.demo22.test4;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.FilterType;

@ComponentScan(
        useDefaultFilters = false, //不启用默认过滤器
        includeFilters = {
                @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, classes = IService.class) //@1
        })
public class ScanBean4 {
}
```

> @1：被扫描的类满足`IService.class.isAssignableFrom(被扫描的类)`条件的都会被注册到spring容器中

#### 来个测试用例

ComponentScanTest中新增个测试用例

```
@Test
public void test4() {
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ScanBean4.class);
    for (String beanName : context.getBeanDefinitionNames()) {
        System.out.println(beanName + "->" + context.getBean(beanName));
    }
}
```

#### 运行输出

```
service1->com.javacode2018.lesson001.demo22.test4.Service1@6379eb
service2->com.javacode2018.lesson001.demo22.test4.Service2@294425a7
```

## [自定义Filter](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

### 用法

有时候我们需要用到自定义的过滤器，使用自定义过滤器的步骤：

```
1.设置@Filter中type的类型为：FilterType.CUSTOM
2.自定义过滤器类，需要实现接口：org.springframework.core.type.filter.TypeFilter
3.设置@Filter中的classses为自定义的过滤器类型
```

来看一下`TypeFilter`这个接口的定义：

```
@FunctionalInterface
public interface TypeFilter {
    boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory)
            throws IOException;
}
```

是一个函数式接口，包含一个match方法，方法返回boolean类型，有2个参数，都是接口类型的，下面介绍一下这2个接口。

#### MetadataReader接口

**类元数据读取器，可以读取一个类上的任意信息，如类上面的注解信息、类的磁盘路径信息、类的class对象的各种信息，spring进行了封装，提供了各种方便使用的方法。**

看一下这个接口的定义：

```
public interface MetadataReader {

    /**
     * 返回类文件的资源引用
     */
    Resource getResource();

    /**
     * 返回一个ClassMetadata对象，可以通过这个读想获取类的一些元数据信息，如类的class对象、是否是接口、是否有注解、是否是抽象类、父类名称、接口名称、内部包含的之类列表等等，可以去看一下源码
     */
    ClassMetadata getClassMetadata();

    /**
     * 获取类上所有的注解信息
     */
    AnnotationMetadata getAnnotationMetadata();

}
```

#### MetadataReaderFactory接口

**类元数据读取器工厂，可以通过这个类获取任意一个类的MetadataReader对象。**

源码：

```
public interface MetadataReaderFactory {

    /**
     * 返回给定类名的MetadataReader对象
     */
    MetadataReader getMetadataReader(String className) throws IOException;

    /**
     * 返回指定资源的MetadataReader对象
     */
    MetadataReader getMetadataReader(Resource resource) throws IOException;

}
```

### 自定义Filter案例

#### 需求

我们来个自定义的Filter，判断被扫描的类如果是`IService`接口类型的，就让其注册到容器中。

#### 代码实现

来个自定义的TypeFilter类：

```
package com.javacode2018.lesson001.demo22.test5;

import com.javacode2018.lesson001.demo22.test4.IService;
import org.springframework.core.type.ClassMetadata;
import org.springframework.core.type.classreading.MetadataReader;
import org.springframework.core.type.classreading.MetadataReaderFactory;
import org.springframework.core.type.filter.TypeFilter;

import java.io.IOException;

public class MyFilter implements TypeFilter {
    /**
     * @param metadataReader
     * @param metadataReaderFactory
     * @return
     * @throws IOException
     */
    @Override
    public boolean match(MetadataReader metadataReader, MetadataReaderFactory metadataReaderFactory) throws IOException {
        Class curClass = null;
        try {
            //当前被扫描的类
            curClass = Class.forName(metadataReader.getClassMetadata().getClassName());
        } catch (ClassNotFoundException e) {
            e.printStackTrace();
        }
        //判断curClass是否是IService类型
        boolean result = IService.class.isAssignableFrom(curClass);
        return result;
    }
}
```

#### 来一个@CompontentScan标注的类

```
package com.javacode2018.lesson001.demo22.test5;

import org.springframework.context.annotation.ComponentScan;
import org.springframework.context.annotation.FilterType;

@ComponentScan(
        basePackages = {"com.javacode2018.lesson001.demo22.test4"},
        useDefaultFilters = false, //不启用默认过滤器
        includeFilters = {
                @ComponentScan.Filter(type = FilterType.CUSTOM, classes = MyFilter.class) //@1
        })
public class ScanBean5 {
}
```

> @1：type为FilterType.CUSTOM，表示Filter是用户自定义的，classes为自定义的过滤器

#### 再来个测试用例

ComponentScanTest中新增个测试用例

```
@Test
public void test5() {
    AnnotationConfigApplicationContext context = new AnnotationConfigApplicationContext(ScanBean5.class);
    for (String beanName : context.getBeanDefinitionNames()) {
        System.out.println(beanName + "->" + context.getBean(beanName));
    }
}
```

#### 运行输出

```
service1->com.javacode2018.lesson001.demo22.test4.Service1@4cc451f2
service2->com.javacode2018.lesson001.demo22.test4.Service2@6379eb
```

## excludeFilters

**配置排除的过滤器，满足这些过滤器的类不会被注册到容器中，用法上面和includeFilters用一样，这个我就不演示了，可以自己玩玩**

## [@ComponentScan重复使用](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

从这个注解的定义上可以看出这个注解可以同时使用多个，如：

```
@ComponentScan(basePackageClasses = ScanClass.class)
@ComponentScan(
        useDefaultFilters = false, //不启用默认过滤器
        includeFilters = {
                @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, classes = IService.class)
        })
public class ScanBean7 {
}
```

还有一种写法，使用@ComponentScans的方式：

```
@ComponentScans({
        @ComponentScan(basePackageClasses = ScanClass.class),
        @ComponentScan(
                useDefaultFilters = false, //不启用默认过滤器
                includeFilters = {
                        @ComponentScan.Filter(type = FilterType.ASSIGNABLE_TYPE, classes = IService.class)
                })})
public class ScanBean7 {
}
```

## [Spring中这块的源码](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

**@CompontentScan注解是被下面这个类处理的**

```
org.springframework.context.annotation.ConfigurationClassPostProcessor
```

**这个类非常非常关键，主要用户bean的注册，前面我们介绍的@Configuration,@Bean注解也是被这个类处理的。**

还有下面这些注解：

```
@PropertySource
@Import
@ImportResource
@Compontent
```

**以上这些注解都是被ConfigurationClassPostProcessor这个类处理的，内部会递归处理这些注解，完成bean的注册。**

**以@CompontentScan来说一下过程，第一次扫描之后会得到一批需要注册的类，然后会对这些需要注册的类进行遍历，判断是否有上面任意一个注解，如果有，会将这个类交给ConfigurationClassPostProcessor继续处理，直到递归完成所有bean的注册。**

**想成为高手，这个类是必看的。**

## [总结](https://mp.weixin.qq.com/s?__biz=MzkzOTI3Nzc0Mg==&mid=2247486603&idx=1&sn=c3df2c03a5d4122e68020dd96848c498&scene=21#wechat_redirect)

1. 1. @ComponentScan用于批量注册bean，spring会按照这个注解的配置，递归扫描指定包中的所有类，将满足条件的类批量注册到spring容器中
   2. 可以通过value、basePackages、basePackageClasses 这几个参数来配置包的扫描范围
   3. 可以通过useDefaultFilters、includeFilters、excludeFilters这几个参数来配置类的过滤器，被过滤器处理之后剩下的类会被注册到容器中
   4. 指定包名的方式配置扫描范围存在隐患，包名被重命名之后，会导致扫描实现，所以一般我们在需要扫描的包中可以创建一个标记的接口或者类，作为basePackageClasses的值，通过这个来控制包的扫描范围
   5. @CompontScan注解会被ConfigurationClassPostProcessor类递归处理，最终得到所有需要注册的类。