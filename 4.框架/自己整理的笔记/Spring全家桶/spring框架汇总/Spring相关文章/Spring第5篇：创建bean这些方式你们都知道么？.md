# Spring第5篇：创建bean这些方式你们都知道么？

路人甲Java [程序员路人](javascript:void(0);) *2022-03-28 21:01*

**大家好，我是路人~~~
**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

**这是 Spring 第 5 篇，点击上方卡片，即可翻阅前面章节。**

***\*最近我创建了一个高质量Java社群，含有最全Java学习线路图+视频\**：[\**点击入群\**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485855&idx=1&sn=20f2d0cb1fb9cbe842000ea4ebbcdda4&chksm=c0fb5780f78cde960e25a5b48d833ad5f49330b8663cbd2f29112eb8166f4a9113bc94dd9b27&scene=21#wechat_redirect)**

### 本文内容 

1. 1.通过反射调用构造方法创建bean对象

2. 2.通过静态工厂方法创建bean对象

3. 3.通过实例工厂方法创建bean对象

4. 4.通过FactoryBean创建bean对象

5. 5.Spring容器内部创建bean实例对象常见的有4种方式。

### 通过反射调用构造方法创建bean对象

调用类的构造方法获取对应的bean实例，是使用最多的方式，这种方式只需要在xml bean元素中指定class属性，spring容器内部会自动调用该类型的构造方法来创建bean对象，将其放在容器中以供使用。

#### 语法

```
<bean id="bean名称" name="bean名称或者别名" class="bean的完整类型名称">
    <constructor-arg index="0" value="bean的值" ref="引用的bean名称" />
    <constructor-arg index="1" value="bean的值" ref="引用的bean名称" />
    <constructor-arg index="2" value="bean的值" ref="引用的bean名称" />
    ....
    <constructor-arg index="n" value="bean的值" ref="引用的bean名称" />
</bean>
```

> constructor-arg用于指定构造方法参数的值
>
> index：构造方法中参数的位置，从0开始，依次递增
>
> value：指定参数的值
>
> ref：当插入的值为容器内其他bean的时候，这个值为容器中对应bean的名称

#### 案例

##### UserModel类

```
@Getter
@Setter
@ToString
public class UserModel {

    private String name;
    private int age;

    public UserModel() {
        this.name = "我是通过UserModel的无参构造方法创建的!";
    }

    public UserModel(String name, int age) {
        this.name = name;
        this.age = age;
    }
}
```

##### beans.xml配置

```
<!-- 通过UserModel的默认构造方法创建UserModel对象 -->
<bean id="createBeanByConstructor1" class="com.javacode2018.lesson001.demo3.UserModel"/>

<!-- 通过UserModel有参构造方法创建UserModel对象 -->
<bean id="createBeanByConstructor2" class="com.javacode2018.lesson001.demo3.UserModel">
    <constructor-arg index="0" value="我是通过UserModel的有参方法构造的对象!"/>
    <constructor-arg index="1" value="30"/>
</bean>
```

上面这2种写法，spring容器创建这两个UserModel的时候，都会通过反射的方式去调用UserModel类中对应的构造函数来创建UserModel对象。

##### 测试用例

```
package com.javacode2018.lesson001.demo3;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.net.URL;
import java.net.URLClassLoader;
import java.util.Arrays;

/**
 * 公众号：程序员路人
 */
public class Client {

    public static void main(String[] args) {
        //1.bean配置文件位置
        String beanXml = "classpath:/com/javacode2018/lesson001/demo3/beans.xml";

        //2.创建ClassPathXmlApplicationContext容器，给容器指定需要加载的bean配置文件
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(beanXml);

        System.out.println("spring容器中所有bean如下：");

        //getBeanDefinitionNames用于获取容器中所有bean的名称
        for (String beanName : context.getBeanDefinitionNames()) {
            System.out.println(beanName + ":" + context.getBean(beanName));
        }

    }
}
```

代码中会输出spring容器中所有bean的名称和其对应的bean对象。

##### 运行输出

```
spring容器中所有bean如下：
createBeanByConstructor1:UserModel(name=我是通过UserModel的无参构造方法创建的!, age=0)
createBeanByConstructor2:UserModel(name=我是通过UserModel的有参方法构造的对象!, age=30)
```

### 通过静态工厂方法创建bean对象

我们可以创建静态工厂，内部提供一些静态方法来生成所需要的对象，将这些静态方法创建的对象交给spring以供使用。

#### 语法

```
<bean id="bean名称" name="" class="静态工厂完整类名" factory-method="静态工厂的方法">
    <constructor-arg index="0" value="bean的值" ref="引用的bean名称" />
    <constructor-arg index="1" value="bean的值" ref="引用的bean名称" />
    <constructor-arg index="2" value="bean的值" ref="引用的bean名称" />
    ....
    <constructor-arg index="n" value="bean的值" ref="引用的bean名称" />
</bean>
```

> class：指定静态工厂完整的类名
>
> factory-method：静态工厂中的静态方法，返回需要的对象。
>
> constructor-arg用于指定静态方法参数的值，用法和上面介绍的构造方法一样。

spring容器会自动调用静态工厂的静态方法获取指定的对象，将其放在容器中以供使用。

#### 案例

##### 定义静态工厂

创建一个静态工厂类，用于生成UserModel对象。

```
package com.javacode2018.lesson001.demo3;

/**
 * 公众号：程序员路人
 */
public class UserStaticFactory {

    /**
     * 静态无参方法创建UserModel
     *
     * @return
     */
    public static UserModel buildUser1() {

        System.out.println(UserStaticFactory.class + ".buildUser1()");

        UserModel userModel = new UserModel();
        userModel.setName("我是无参静态构造方法创建的!");
        return userModel;
    }

    /**
     * 静态有参方法创建UserModel
     *
     * @param name 名称
     * @param age  年龄
     * @return
     */
    public static UserModel buildUser2(String name, int age) {

        System.out.println(UserStaticFactory.class + ".buildUser2()");

        UserModel userModel = new UserModel();
        userModel.setName(name);
        userModel.setAge(age);
        return userModel;
    }
}
```

##### beans.xml配置

```
<!-- 通过工厂静态无参方法创建bean对象 -->
<bean id="createBeanByStaticFactoryMethod1" class="com.javacode2018.lesson001.demo3.UserStaticFactory"
      factory-method="buildUser1"/>

<!-- 通过工厂静态有参方法创建bean对象 -->
<bean id="createBeanByStaticFactoryMethod2" class="com.javacode2018.lesson001.demo3.UserStaticFactory"
      factory-method="buildUser2">
    <constructor-arg index="0" value="通过工厂静态有参方法创建UerModel实例对象"/>
    <constructor-arg index="1" value="30"/>
</bean>
```

上面配置中，spring容器启动的时候会自动调用UserStaticFactory中的buildUser1静态方法获取UserModel对象，将其作为createBeanByStaticFactoryMethod1名称对应的bean对象放在spring容器中。

会调用UserStaticFactory的buildUser2方法，并且会传入2个指定的参数，得到返回的UserModel对象，将其作为createBeanByStaticFactoryMethod2名称对应的bean对象放在spring容器中。

##### 运行Client

```
class com.javacode2018.lesson001.demo3.UserStaticFactory.buildUser1()
class com.javacode2018.lesson001.demo3.UserStaticFactory.buildUser2()
spring容器中所有bean如下：
createBeanByStaticFactoryMethod1:UserModel(name=我是无参静态构造方法创建的!, age=0)
createBeanByStaticFactoryMethod2:UserModel(name=通过工厂静态有参方法创建UerModel实例对象, age=30)
```

从输出中可以看出，两个静态方法都被调用了，createBeanByStaticFactoryMethod1对应的bean对象是通过buildUser1方法创建的；createBeanByStaticFactoryMethod2对应的bean对象是通过buildUser2方法创建的。

### 通过实例工厂方法创建bean对象

让spring容器去调用某些对象的某些实例方法来生成bean对象放在容器中以供使用。

#### 语法

```
<bean id="bean名称" factory-bean="需要调用的实例对象bean名称" factory-method="bean对象中的方法">
    <constructor-arg index="0" value="bean的值" ref="引用的bean名称" />
    <constructor-arg index="1" value="bean的值" ref="引用的bean名称" />
    <constructor-arg index="2" value="bean的值" ref="引用的bean名称" />
    ....
    <constructor-arg index="n" value="bean的值" ref="引用的bean名称" />
</bean>
```

spring容器以factory-bean的值为bean名称查找对应的bean对象，然后调用该对象中factory-method属性值指定的方法，将这个方法返回的对象作为当前bean对象放在容器中供使用。

#### 案例

##### 定义一个实例工厂

内部写2个方法用来创建UserModel对象。

```
package com.javacode2018.lesson001.demo3;

/**
 * 公众号：程序员路人
 */
public class UserFactory {
    public UserModel buildUser1() {
        System.out.println("----------------------1");
        UserModel userModel = new UserModel();
        userModel.setName("bean实例方法创建的对象!");
        return userModel;
    }

    public UserModel buildUser2(String name, int age) {
        System.out.println("----------------------2");
        UserModel userModel = new UserModel();
        userModel.setName(name);
        userModel.setAge(age);
        return userModel;
    }
}
```

##### beans.xml

```
<!-- 定义一个工厂实例 -->
<bean id="userFactory" class="com.javacode2018.lesson001.demo3.UserFactory"/>
<!-- 通过userFactory实例的无参user方法创建UserModel对象 -->
<bean id="createBeanByBeanMethod1" factory-bean="userFactory" factory-method="buildUser1"/>

<!-- 通过userFactory实例的有参user方法创建UserModel对象 -->
<bean id="createBeanByBeanMethod2" factory-bean="userFactory" factory-method="buildUser2">
    <constructor-arg index="0" value="通过bean实例有参方法创建UserModel实例对象"/>
    <constructor-arg index="1" value="30"/>
</bean>
```

createBeanByBeanMethod1对应的bean是通过userFactory的buildUser1方法生成的。

createBeanByBeanMethod2对应的bean是通过userFactory的buildUser2方法生成的。

##### 运行Client

```
spring容器中所有bean如下：
createBeanByBeanMethod1:UserModel(name=bean实例方法创建的对象!, age=0)
createBeanByBeanMethod2:UserModel(name=通过bean实例有参方法创建UserModel实例对象, age=30)
```

### 通过FactoryBean来创建bean对象

前面我们学过了BeanFactory接口，BeanFactory是spring容器的顶层接口，而这里要说的是FactoryBean，也是一个接口，这两个接口很容易搞混淆，FactoryBean可以让spring容器通过这个接口的实现来创建我们需要的bean对象。

FactoryBean接口源码：

```
public interface FactoryBean<T> {

    /**
     * 返回创建好的对象
     */
    @Nullable
    T getObject() throws Exception;

    /**
     * 返回需要创建的对象的类型
     */
    @Nullable
    Class<?> getObjectType();

    /**
    * bean是否是单例的
    **/
    default boolean isSingleton() {
        return true;
    }

}
```

接口中有3个方法，前面2个方法需要我们去实现，getObject方法内部由开发者自己去实现对象的创建，然后将创建好的对象返回给Spring容器，getObjectType需要指定我们创建的bean的类型；最后一个方法isSingleton表示通过这个接口创建的对象是否是单例的，如果返回false，那么每次从容器中获取对象的时候都会调用这个接口的getObject() 去生成bean对象。

#### 语法

```
<bean id="bean名称" class="FactoryBean接口实现类" />
```

#### 案例

##### 创建一个FactoryBean实现类

```
package com.javacode2018.lesson001.demo3;

import org.springframework.beans.factory.FactoryBean;
import org.springframework.lang.Nullable;

/**
 * 公众号：程序员路人
 */
public class UserFactoryBean implements FactoryBean<UserModel> {
    int count = 1;
    @Nullable
    @Override
    public UserModel getObject() throws Exception { //@1
        UserModel userModel = new UserModel();
        userModel.setName("我是通过FactoryBean创建的第"+count+++ "对象");//@4
        return userModel;
    }

    @Nullable
    @Override
    public Class<?> getObjectType() {
        return UserModel.class; //@2
    }

    @Override
    public boolean isSingleton() { 
        return true; //@3
    }
}
```

@1：返回了一个创建好的UserModel对象

@2：返回对象的Class对象

@3：返回true，表示创建的对象是单例的，那么我们每次从容器中获取这个对象的时候都是同一个对象

@4：此处用到了一个count，通过这个一会可以看出isSingleton不同返回值的时候从容器获取的bean是否是同一个

##### bean xml配置

```
<!-- 通过FactoryBean 创建UserModel对象 -->
<bean id="createByFactoryBean" class="com.javacode2018.lesson001.demo3.UserFactoryBean"/>
```

##### Client代码

```
package com.javacode2018.lesson001.demo3;

import org.springframework.context.support.ClassPathXmlApplicationContext;

import java.net.URL;
import java.net.URLClassLoader;
import java.util.Arrays;

/**
 * 公众号：程序员路人
 */
public class Client {

    public static void main(String[] args) {
        //1.bean配置文件位置
        String beanXml = "classpath:/com/javacode2018/lesson001/demo3/beans.xml";

        //2.创建ClassPathXmlApplicationContext容器，给容器指定需要加载的bean配置文件
        ClassPathXmlApplicationContext context = new ClassPathXmlApplicationContext(beanXml);

        System.out.println("spring容器中所有bean如下：");

        //getBeanDefinitionNames用于获取容器中所有bean的名称
        for (String beanName : context.getBeanDefinitionNames()) {
            System.out.println(beanName + ":" + context.getBean(beanName));
        }

        System.out.println("--------------------------");
        //多次获取createByFactoryBean看看是否是同一个对象
        System.out.println("createByFactoryBean:" + context.getBean("createByFactoryBean"));
        System.out.println("createByFactoryBean:" + context.getBean("createByFactoryBean"));
    }
}
```

运行输出

```
class com.javacode2018.lesson001.demo3.UserStaticFactory.buildUser1()
class com.javacode2018.lesson001.demo3.UserStaticFactory.buildUser2()
----------------------1
----------------------2
spring容器中所有bean如下：
createBeanByConstructor1:UserModel(name=我是通过UserModel的无参构造方法创建的!, age=0)
createBeanByConstructor2:UserModel(name=我是通过UserModel的有参方法构造的对象!, age=30)
createBeanByStaticFactoryMethod1:UserModel(name=我是无参静态构造方法创建的!, age=0)
createBeanByStaticFactoryMethod2:UserModel(name=通过工厂静态有参方法创建UerModel实例对象, age=30)
userFactory:com.javacode2018.lesson001.demo3.UserFactory@610694f1
createBeanByBeanMethod1:UserModel(name=bean实例方法创建的对象!, age=0)
createBeanByBeanMethod2:UserModel(name=通过bean实例有参方法创建UserModel实例对象, age=30)
createByFactoryBean:UserModel(name=我是通过FactoryBean创建的第1对象, age=0)
--------------------------
createByFactoryBean:UserModel(name=我是通过FactoryBean创建的第1对象, age=0)
createByFactoryBean:UserModel(name=我是通过FactoryBean创建的第1对象, age=0)
```

注意最后4行输出，有3行输出的都是同一个createByFactoryBean，程序中通过getBean从spring容器中查找createByFactoryBean了3次，3次结果都是一样的，说明返回的都是同一个UserModel对象。

下面我们将UserFactoryBean中的isSingleton调整一下，返回false

```
@Override
public boolean isSingleton() {
    return false;
}
```

当这个方法返回false的时候，表示由这个FactoryBean创建的对象是多例的，那么我们每次从容器中getBean的时候都会去重新调用FactoryBean中的getObject方法获取一个新的对象。

再运行一下Client，最后4行输出：

```
createByFactoryBean:UserModel(name=我是通过FactoryBean创建的第1对象, age=0)
--------------------------
createByFactoryBean:UserModel(name=我是通过FactoryBean创建的第2对象, age=0)
createByFactoryBean:UserModel(name=我是通过FactoryBean创建的第3对象, age=0)
```

这3次获取的对象不一样了。

### 总结

spring容器提供了4种创建bean实例的方式，除了构造函数的方式，其他几种方式可以让我们手动去控制对象的创建，这几种方式大家都掌握一下，能够灵活使用。