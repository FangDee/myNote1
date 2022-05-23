# Spring第8篇：自动注入（autowire）详解

路人甲Java [程序员路人](javascript:void(0);) *2022-03-31 17:20*

**大家好，我是路人~~~
**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

**这是 Spring 第 8 篇，点击上方卡片，即可翻阅前面章节。**

***\**\*\*\*强烈推荐阅读\*\*\*\*\**\**\*\*\*\*：\*\*\*\*\**\**\*\*\*\*[\*\*到底如何学好Java？\*\*](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485855&idx=1&sn=20f2d0cb1fb9cbe842000ea4ebbcdda4&chksm=c0fb5780f78cde960e25a5b48d833ad5f49330b8663cbd2f29112eb8166f4a9113bc94dd9b27&scene=21#wechat_redirect)\*\*\*\*\****

### 本文内容 

- **手动注入的不足**
- **Class.isAssignableFrom方法介绍**
- **3种自动注入方式详解及案例**
- **按名称自动注入**
- **按类型自动注入**
- **按构造器进行自动注入**
- **按类型自动注入某种类型的所有bean给List和Map（重点）**
- **autowire=default介绍**
- **总结**
- **案例源码**

### 手动注入的不足

上篇文章中介绍了依赖注入中的手动注入，所谓手动注入是指在xml中采用硬编码的方式来配置注入的对象，比如通过构造器注入或者set方法注入，这些注入的方式都存在不足，比如下面代码：

```
public class A{
    private B b;
    private C c;
    private D d;
    private E e;
    ....
    private N n;

    //上面每个private属性的get和set方法
}
```

使用spring容器来管理，xml配置如下：

```
<bean class="b" class="B"/>
<bean class="c" class="C"/>
<bean class="d" class="D"/>
<bean class="e" class="E"/>
<bean class="a" class="A">
    <property name="b" ref="b"/>
    <property name="c" ref="c"/>
    <property name="d" ref="d"/>
    <property name="e" ref="e"/>
    ...
</bean>
```

上面的注入存在的问题：

- **如果需要注入的对象比较多，比如A类中有几十个属性，那么上面的property属性是不是需要写几十个，此时配置文件代码量暴增**
- **如果A类中新增或者删除了一些依赖，还需要手动去调整bean xml中的依赖配置信息，否则会报错**
- **总的来说就是不利于维护和扩展**

**为了解决上面这些问题，spring为我们提供了更强大的功能：自动注入**

在介绍自动注入之前需要先介绍一些基础知识。

### Class.isAssignableFrom方法

#### 用法

isAssignableFrom是Class类中的一个方法，看一下这个方法的定义：

```
public native boolean isAssignableFrom(Class<?> cls)
```

用法如下：

```
c1.isAssignableFrom(c2)
```

用来判断c2和c1是否相等，或者c2是否是c1的子类。

#### 案例

```
@Test
public void isAssignableFrom(){
    System.out.println(Object.class.isAssignableFrom(Integer.class)); //true
    System.out.println(Object.class.isAssignableFrom(int.class)); //false
    System.out.println(Object.class.isAssignableFrom(List.class)); //true
    System.out.println(Collection.class.isAssignableFrom(List.class)); //true
    System.out.println(List.class.isAssignableFrom(Collection.class)); //false
}
```

### 自动注入

自动注入是采用约定大约配置的方式来实现的，程序和spring容器之间约定好，遵守某一种都认同的规则，来实现自动注入。

xml中可以在bean元素中通过autowire属性来设置自动注入的方式：

```
<bean id="" class="" autowire="byType|byName|constructor|default" />
```

- **byteName：按照名称进行注入**
- **byType：按类型进行注入**
- **constructor：按照构造方法进行注入**
- **default：默认注入方式**

下面我们详解每种注入方式的用法。

### 按照名称进行注入（byName）

#### 用法

autowire设置为byName

```
<bean id="" class="X类" autowire="byName"/>
```

> spring容器会按照set属性的名称去容器中查找同名的bean对象，然后将查找到的对象通过set方法注入到对应的bean中，未找到对应名称的bean对象则set方法不进行注入
>
> 需要注入的set属性的名称和被注入的bean的名称必须一致。

来看看案例吧。

#### 案例

##### DiByName.java

```
package com.javacode2018.lesson001.demo6;

/**
 * 按照名称自动注入
 */
public class DiAutowireByName {
    public static class Service1 { //@1
        private String desc;

        public String getDesc() {
            return desc;
        }

        public void setDesc(String desc) {
            this.desc = desc;
        }

        @Override
        public String toString() {
            return "Service1{" +
                    "desc='" + desc + '\'' +
                    '}';
        }
    }

    public static class Service2 { //@1
        private String desc;

        public String getDesc() {
            return desc;
        }

        public void setDesc(String desc) {
            this.desc = desc;
        }

        @Override
        public String toString() {
            return "Service2{" +
                    "desc='" + desc + '\'' +
                    '}';
        }
    }

    private Service1 service1;//@3
    private Service2 service2;//@4

    public Service1 getService1() {
        return service1;
    }

    public void setService1(Service1 service1) {
        System.out.println("setService1->" + service1);
        this.service1 = service1;
    }

    public Service2 getService2() {
        return service2;
    }

    public void setService2(Service2 service2) {
        System.out.println("setService2->" + service2);
        this.service2 = service2;
    }

    @Override
    public String toString() {
        return "DiAutowireByName{" +
                "service1=" + service1 +
                ", service2=" + service2 +
                '}';
    }
}
```

这个类中有2个属性，名称为：

- service1
- service2

这两个属性都有对应的set方法。

下面我们在bean xml中定义2个和这2个属性同名的bean，然后使用按照名称进行自动注入。

##### diAutowireByName.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="service1" class="com.javacode2018.lesson001.demo6.DiAutowireByName$Service1">
        <property name="desc" value="service1"/>
    </bean>
    <bean id="service2" class="com.javacode2018.lesson001.demo6.DiAutowireByName$Service2">
        <property name="desc" value="service2"/>
    </bean>
    <bean id="service2-1" class="com.javacode2018.lesson001.demo6.DiAutowireByName$Service2">
        <property name="desc" value="service2-1"/>
    </bean>

    <!-- autowire：byName 配置按照name进行自动注入 -->
    <bean id="diAutowireByName1" class="com.javacode2018.lesson001.demo6.DiAutowireByName" autowire="byName"/>

    <!-- 当配置了自动注入，还可以使用手动的方式自动注入进行覆盖，手动的优先级更高一些 -->
    <bean id="diAutowireByName2" class="com.javacode2018.lesson001.demo6.DiAutowireByName" autowire="byName">
        <property name="service2" ref="service2-1"/>
    </bean>

</beans>
```

> 上面注释认真看一下。
>
> @1：定义了一个名称为service1的bean
>
> @2：定义了一个名称为service2的bean
>
> @3：定义diAutowireByName需要将autowire的值置为byName，表示按名称进行自动注入。
>
> spring容器创建diAutowireByName对应的bean时，会遍历DiAutowireByName类中的所有set方法，然后得到set对应的属性名称列表：{"service1","service2"}，然后遍历这属性列表，在容器中查找和属性同名的bean对象，然后调用属性对应的set方法，将bean对象注入进去

##### 测试用例

```
package com.javacode2018.lesson001.demo6;

import com.javacode2018.lesson001.demo5.IocUtils;
import org.junit.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.util.ArrayList;
import java.util.Collection;
import java.util.List;

/**
 * 公众号：程序员路人
 * xml中自动注入配置案例
 */
public class DiAutowireTest {

    /**
     * 按照名称进行注入
     */
    @Test
    public void diAutowireByName() {
        String beanXml = "classpath:/com/javacode2018/lesson001/demo6/diAutowireByName.xml";
        ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
        System.out.println(context.getBean("diAutowireByName"));
    }
}
```

##### 效果

运行diAutowireByName输出：

```
setService1->Service1{desc='service1'}
setService2->Service2{desc='service2'}
setService2->Service2{desc='service2-1'}
setService1->Service1{desc='service1'}
DiAutowireByName{service1=Service1{desc='service1'}, service2=Service2{desc='service2'}}
DiAutowireByName{service1=Service1{desc='service1'}, service2=Service2{desc='service2-1'}}
```

#### 优缺点

按名称进行注入的时候，要求名称和set属性的名称必须同名，相对于硬编码的方式注入，确实节省了不少代码。

### 按照类型进行自动注入

#### 用法

autowire设置为byType

```
<bean id="" class="X类" autowire="byType"/>
```

> spring容器会遍历x类中所有的set方法，会在容器中查找和set参数类型相同的bean对象，将其通过set方法进行注入，未找到对应类型的bean对象则set方法不进行注入。
>
> **需要注入的set属性的类型和被注入的bean的类型需要满足isAssignableFrom关系。**
>
> 按照类型自动装配的时候，如果按照类型找到了多个符合条件的bean，系统会报错。
>
> **set方法的参数如果是下面的类型或者下面类型的数组的时候，这个set方法会被跳过注入：**
>
> **Object,Boolean,boolean,Byte,byte,Character,char,Double,double,Float,float,Integer,int,Long,Short,shot,Enum,CharSequence,Number,Date,java.time.temporal.Temporal,java.net.URI,java.net.URI,java.util.Locale,java.lang.Class**

来看看案例吧。

#### 案例

##### DiByType.java

```
package com.javacode2018.lesson001.demo6;

/**
 * 按照类型自动注入
 */
public class DiAutowireByType {
    public static class Service1 {
        private String desc;

        public String getDesc() {
            return desc;
        }

        public void setDesc(String desc) {
            this.desc = desc;
        }

        @Override
        public String toString() {
            return "Service1{" +
                    "desc='" + desc + '\'' +
                    '}';
        }
    }

    public static class Service2 {
        private String desc;

        public String getDesc() {
            return desc;
        }

        public void setDesc(String desc) {
            this.desc = desc;
        }

        @Override
        public String toString() {
            return "Service2{" +
                    "desc='" + desc + '\'' +
                    '}';
        }
    }

    private Service1 service1;
    private Service2 service2;

    public Service1 getService1() {
        return service1;
    }

    public void setService1(Service1 service1) {
        System.out.println("setService1->" + service1); //@1
        this.service1 = service1;
    }

    public Service2 getService2() {
        return service2;
    }

    public void setService2(Service2 service2) {
        System.out.println("setService2->" + service2); //@2
        this.service2 = service2;
    }

    @Override
    public String toString() {
        return "DiByType{" +
                "service1=" + service1 +
                ", service2=" + service2 +
                '}';
    }
}
```

> DiAutowireByType类中有2个set方法分别来注入Service1和Service2，两个set方法中都输出了一行文字，一会执行的时候可以通过这个输出可以看出set方法是否被调用了。

##### diAutowireByName.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="service1" class="com.javacode2018.lesson001.demo6.DiAutowireByName$Service1">
        <property name="desc" value="service1"/>
    </bean>
    <bean id="service2" class="com.javacode2018.lesson001.demo6.DiAutowireByName$Service2">
        <property name="desc" value="service2"/>
    </bean>
    <bean id="service2-1" class="com.javacode2018.lesson001.demo6.DiAutowireByName$Service2">
        <property name="desc" value="service2-1"/>
    </bean>

    <!-- autowire：byName 配置按照name进行自动注入 -->
    <bean id="diAutowireByName1" class="com.javacode2018.lesson001.demo6.DiAutowireByName" autowire="byName"/>

    <!-- 当配置了自动注入，还可以使用手动的方式自动注入进行覆盖，手动的优先级更高一些 -->
    <bean id="diAutowireByName2" class="com.javacode2018.lesson001.demo6.DiAutowireByName" autowire="byName">
        <property name="service2" ref="service2-1"/>
    </bean>

</beans>
```

> 上面注释认真看一下。
>
> @1：定义了一个名称为service1的bean
>
> @2：定义了一个名称为service2的bean
>
> @3：定义diAutowireByName需要将autowire的值置为byName，表示按名称进行自动注入。
>
> spring容器创建diAutowireByName对应的bean时，会遍历DiAutowireByName类中的所有set方法，然后得到set对应的属性名称列表：{"service1","service2"}，然后遍历这属性列表，在容器中查找和属性同名的bean对象，然后调用属性对应的set方法，将bean对象注入进去

##### 测试用例

DiAutowireTest类中添加一个方法

```
/**
 * 按照set方法参数类型进行注入
 */
@Test
public void diAutowireByType() {
    String beanXml = "classpath:/com/javacode2018/lesson001/demo6/diAutowireByType.xml";
    ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
    System.out.println(context.getBean("diAutowireByType1"));
}
```

##### 效果

运行diAutowireByType输出：

```
setService1->Service1{desc='service1'}
setService2->Service2{desc='service2'}
DiByType{service1=Service1{desc='service1'}, service2=Service2{desc='service2'}}
```

#### 优缺点

相对于手动注入，节省了不少代码，新增或者删除属性，只需要增减对应的set方法就可以了，更容易扩展了。

#### 注入类型匹配的所有bean(重点)

**按照类型注入还有2中比较牛逼的用法：**

- **一个容器中满足某种类型的bean可以有很多个，将容器中某种类型中的所有bean，通过set方法注入给一个java.util.List<需要注入的Bean的类型或者其父类型或者其接口>对象**
- **将容器中某种类型中的所有bean，通过set方法注入给一个java.util.Map<String,需要注入的Bean的类型或者其父类型或者其接口>对象**

来看个案例就懂了。

##### DiAutowireByTypeExtend.java

```
package com.javacode2018.lesson001.demo6;

import java.util.List;
import java.util.Map;

/**
 * 满足条件的bean注入到集合中(重点)
 */
public class DiAutowireByTypeExtend {

    //定义了一个接口
    public interface IService1 {
    }

    public static class BaseServie {
        private String desc;

        public String getDesc() {
            return desc;
        }

        public void setDesc(String desc) {
            this.desc = desc;
        }

        @Override
        public String toString() {
            return "BaseServie{" +
                    "desc='" + desc + '\'' +
                    '}';
        }
    }

    //Service1实现了IService1接口
    public static class Service1 extends BaseServie implements IService1 {

    }

    //Service1实现了IService1接口
    public static class Service2 extends BaseServie implements IService1 {
    }

    private List<IService1> serviceList;//@1
    private List<BaseServie> baseServieList;//@2
    private Map<String, IService1> service1Map;//@3
    private Map<String, BaseServie> baseServieMap;//@4

    public List<IService1> getServiceList() {
        return serviceList;
    }

    public void setServiceList(List<IService1> serviceList) {//@5
        this.serviceList = serviceList;
    }

    public List<BaseServie> getBaseServieList() {
        return baseServieList;
    }

    public void setBaseServieList(List<BaseServie> baseServieList) {//@6
        this.baseServieList = baseServieList;
    }

    public Map<String, IService1> getService1Map() {
        return service1Map;
    }

    public void setService1Map(Map<String, IService1> service1Map) {//@7
        this.service1Map = service1Map;
    }

    public Map<String, BaseServie> getBaseServieMap() {
        return baseServieMap;
    }

    public void setBaseServieMap(Map<String, BaseServie> baseServieMap) {//@8
        this.baseServieMap = baseServieMap;
    }

    @Override
    public String toString() { //9
        return "DiAutowireByTypeExtend{" +
                "serviceList=" + serviceList +
                ", baseServieList=" + baseServieList +
                ", service1Map=" + service1Map +
                ", baseServieMap=" + baseServieMap +
                '}';
    }
}
```

> @1,@2,@3,@4：定义了4个属性，都是泛型类型的，都有对应的set方法。
>
> @5：参数类型是List<BaseServie>，这个集合集合中元素的类型是BaseServie，spring会找到容器中所有满足BaseServie.isAssignableFrom(bean的类型)的bean列表，将其通过@5的set方法进行注入。
>
> @6：同@5的代码
>
> @7：这个参数类型是一个map了，map的key是string类型，value是IService1类型，spring容器会将所有满足IService1类型的bean找到，按照name->bean对象这种方式丢到一个map中，然后调用@7的set方法进行注入，最后注入的这个map就是bean的名称和bean对象进行映射的一个map对象。
>
> @8：同@7的代码
>
> @9：重写了toString方法，输出的时候好看一些

##### 测试用例

DiAutowireTest新增一个方法：

```
/**
 * 按照类型注入集合
 */
@Test
public void diAutowireByTypeExtend() {
    String beanXml = "classpath:/com/javacode2018/lesson001/demo6/diAutowireByTypeExtend.xml";
    ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
    //从容器中获取DiAutowireByTypeExtend
    DiAutowireByTypeExtend diAutowireByTypeExtend = context.getBean(DiAutowireByTypeExtend.class);
    //输出diAutowireByTypeExtend中的属性看一下
    System.out.println("serviceList：" + diAutowireByTypeExtend.getServiceList());
    System.out.println("baseServieList：" + diAutowireByTypeExtend.getBaseServieList());
    System.out.println("service1Map：" + diAutowireByTypeExtend.getService1Map());
    System.out.println("baseServieMap：" + diAutowireByTypeExtend.getBaseServieMap());
}
```

##### 效果

运行diAutowireByTypeExtend输出：

```
serviceList：[BaseServie{desc='service1-1'}, BaseServie{desc='service1-2'}, BaseServie{desc='service2-1'}]
baseServieList：[BaseServie{desc='service1-1'}, BaseServie{desc='service1-2'}, BaseServie{desc='service2-1'}]
service1Map：{service1-1=BaseServie{desc='service1-1'}, service1-2=BaseServie{desc='service1-2'}, service2-1=BaseServie{desc='service2-1'}}
baseServieMap：{service1-1=BaseServie{desc='service1-1'}, service1-2=BaseServie{desc='service1-2'}, service2-1=BaseServie{desc='service2-1'}}
```

下面我们来介绍另外一种自动注入方式。

### 按照构造函数进行自动注入

#### 用法

autowire设置为constructor

```
<bean id="" class="X类" autowire="constructor"/>
```

> **spring会找到x类中所有的构造方法（一个类可能有多个构造方法），然后将这些构造方法进行排序（先按修饰符进行排序，public的在前面，其他的在后面，如果修饰符一样的，会按照构造函数参数数量倒叙，也就是采用贪婪的模式进行匹配，spring容器会尽量多注入一些需要的对象）得到一个构造函数列表，会轮询这个构造器列表，判断当前构造器所有参数是否在容器中都可以找到匹配的bean对象，如果可以找到就使用这个构造器进行注入，如果不能找到，那么就会跳过这个构造器，继续采用同样的方式匹配下一个构造器，直到找到一个合适的为止。**

来看看案例吧。

#### 案例

##### DiAutowireByConstructor.java

```
package com.javacode2018.lesson001.demo6;

/**
 * 构造函数的方式进行自动注入
 */
public class DiAutowireByConstructor {

    public static class BaseServie {
        private String desc;

        public String getDesc() {
            return desc;
        }

        public void setDesc(String desc) {
            this.desc = desc;
        }

        @Override
        public String toString() {
            return "BaseServie{" +
                    "desc='" + desc + '\'' +
                    '}';
        }
    }

    //Service1实现了IService1接口
    public static class Service1 extends BaseServie {
    }

    //Service1实现了IService1接口
    public static class Service2 extends BaseServie {
    }

    private Service1 service1;
    private Service2 service2;

    public DiAutowireByConstructor() { //@0
    }

    public DiAutowireByConstructor(Service1 service1) { //@1
        System.out.println("DiAutowireByConstructor(Service1 service1)");
        this.service1 = service1;
    }

    public DiAutowireByConstructor(Service1 service1, Service2 service2) { //@2
        System.out.println("DiAutowireByConstructor(Service1 service1, Service2 service2)");
        this.service1 = service1;
        this.service2 = service2;
    }

    public Service1 getService1() {
        return service1;
    }

    public void setService1(Service1 service1) {
        this.service1 = service1;
    }

    public Service2 getService2() {
        return service2;
    }

    public void setService2(Service2 service2) {
        this.service2 = service2;
    }

    @Override
    public String toString() {
        return "DiAutowireByConstructor{" +
                "service1=" + service1 +
                ", service2=" + service2 +
                '}';
    }
}
```

> @1：1个参数的构造函数
>
> @2：2个参数的构造函数
>
> 2个有参构造函数第一行都打印了一段文字，一会在输出中可以看到代码是调用了那个构造函数创建对象。

##### diAutowireByConstructor.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean class="com.javacode2018.lesson001.demo6.DiAutowireByConstructor$Service1">
        <property name="desc" value="service1"/>
    </bean>

    <bean id="diAutowireByConstructor" class="com.javacode2018.lesson001.demo6.DiAutowireByConstructor"
          autowire="constructor"/>

</beans>
```

##### 测试用例

DiAutowireTest新增一个方法

```
/**
 * 构造函数的方式进行自动注入
 */
@Test
public void diAutowireByConstructor() {
    String beanXml = "classpath:/com/javacode2018/lesson001/demo6/diAutowireByConstructor.xml";
    ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
    System.out.println(context.getBean("diAutowireByConstructor"));
}
```

##### 效果

运行diAutowireByConstructor输出：

```
DiAutowireByConstructor(Service1 service1)
DiAutowireByConstructor{service1=BaseServie{desc='service1'}, service2=null}
```

> 从输出中可以看到调用的是DiAutowireByConstructor类中的第一个构造函数注入了service1 bean。
>
> 构造函数匹配采用贪婪匹配，多个构造函数结合容器找到一个合适的构造函数，最匹配的就是第一个有参构造函数，而第二个有参构造函数的第二个参数在spring容器中找不到匹配的bean对象，所以被跳过了。

我们在diAutowireByConstructor.xml加入Service2的配置：

```
<bean class="com.javacode2018.lesson001.demo6.DiAutowireByConstructor$Service2">
    <property name="desc" value="service2"/>
</bean>
```

再来运行一下diAutowireByConstructor输出：

```
DiAutowireByConstructor(Service1 service1, Service2 service2)
DiAutowireByConstructor{service1=BaseServie{desc='service1'}, service2=BaseServie{desc='service2'}}
```

此时可以看到第二个有参构造函数被调用了，满足了贪婪方式的注入原则，最大限度的注入所有依赖的对象。

### autowire=default

#### 用法

bean xml的根元素为beans，注意根元素有个`default-autowire`属性，这个属性可选值有(no|byName|byType|constructor|default)，这个属性可以批量设置当前文件中所有bean的自动注入的方式，bean元素中如果省略了autowire属性，那么会取`default-autowire`的值作为其`autowire`的值，而每个bean元素还可以单独设置自己的`autowire`覆盖`default-autowire`的配置，如下：

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd"
       default-autowire="byName">

</beans>
```

#### 案例

##### diAutowireByDefault.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd"
       default-autowire="byName"> //@1

    <bean id="service1" class="com.javacode2018.lesson001.demo6.DiAutowireByName$Service1">
        <property name="desc" value="service1"/>
    </bean>
    <bean id="service2" class="com.javacode2018.lesson001.demo6.DiAutowireByName$Service2">
        <property name="desc" value="service2"/>
    </bean>
    <bean id="service2-1" class="com.javacode2018.lesson001.demo6.DiAutowireByName$Service2">
        <property name="desc" value="service2-1"/>
    </bean>

    <!-- autowire：default，会采用beans中的default-autowire指定的配置 -->
    <bean id="diAutowireByDefault1" class="com.javacode2018.lesson001.demo6.DiAutowireByName" autowire="default"/> //@2

    <!-- autowire：default，会采用beans中的default-autowire指定的配置，还可以使用手动的方式自动注入进行覆盖，手动的优先级更高一些 -->
    <bean id="diAutowireByDefault2" class="com.javacode2018.lesson001.demo6.DiAutowireByName" autowire="default"> //@3
        <property name="service2" ref="service2-1"/>
    </bean>

</beans>
```

注意上面的@1配置的default-autowire="byName"，表示全局默认的自动注入方式是：按名称注入

@2和@3的`autowire=default`，那么注入方式会取`default-autowire`的值。

##### 测试用例

DiAutowireTest中新增一个方法

```
/**
 * autowire=default
 */
@Test
public void diAutowireByDefault() {
    String beanXml = "classpath:/com/javacode2018/lesson001/demo6/diAutowireByDefault.xml";
    ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
    System.out.println(context.getBean("diAutowireByDefault1"));
    System.out.println(context.getBean("diAutowireByDefault2"));
}
```

##### 效果

运行diAutowireByDefault输出

```
setService1->Service1{desc='service1'}
setService2->Service2{desc='service2'}
setService2->Service2{desc='service2-1'}
setService1->Service1{desc='service1'}
DiAutowireByName{service1=Service1{desc='service1'}, service2=Service2{desc='service2'}}
DiAutowireByName{service1=Service1{desc='service1'}, service2=Service2{desc='service2-1'}}
```

### 总结

- **xml中手动注入存在的不足之处，可以通过自动注入的方式来解决，本文介绍了3中自动注入：通过名称自动注入、通过类型自动注入、通过构造器自动注入**
- **按类型注入中有个比较重要的是注入匹配类型所有的bean，可以将某种类型所有的bean注入给一个List对象，可以将某种类型的所有bean按照`bean名称->bean对象`的映射方式注入给一个Map对象，这种用法比较重要，用途比较大，要掌握**
- **spring中还有其他自动注入的方式，用起来会更爽，后面的文章中我们会详细介绍。**