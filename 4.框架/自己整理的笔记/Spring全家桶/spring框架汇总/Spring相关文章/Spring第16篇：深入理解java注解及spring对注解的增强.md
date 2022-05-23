# Spring第16篇：深入理解java注解及spring对注解的增强

路人甲Java [程序员路人](javascript:void(0);) *2022-04-14 17:19*

**大家好，我是路人~~~
**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

[**招收永久门徒！**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485893&idx=2&sn=18b07477518049b762c05b07ca0e5315&chksm=c0fb57daf78cdecc6a705a28c90b3a9a1d8a5bcfd63b8e3a3eb3e97f161c230cf7b1248a60a1&scene=21#wechat_redirect)

***\*这是 Spring 第 16 篇，点击上方卡片，即可翻阅前面章节。\****

**最近有个朋友去阿里面试，被面试官来了个灵魂拷问：**

1. [**注解是干什么的？**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485893&idx=2&sn=18b07477518049b762c05b07ca0e5315&chksm=c0fb57daf78cdecc6a705a28c90b3a9a1d8a5bcfd63b8e3a3eb3e97f161c230cf7b1248a60a1&scene=21#wechat_redirect)
2. [**一个注解可以使用多次么？如何使用？**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485893&idx=2&sn=18b07477518049b762c05b07ca0e5315&chksm=c0fb57daf78cdecc6a705a28c90b3a9a1d8a5bcfd63b8e3a3eb3e97f161c230cf7b1248a60a1&scene=21#wechat_redirect)
3. [**@Inherited是做什么的？**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485893&idx=2&sn=18b07477518049b762c05b07ca0e5315&chksm=c0fb57daf78cdecc6a705a28c90b3a9a1d8a5bcfd63b8e3a3eb3e97f161c230cf7b1248a60a1&scene=21#wechat_redirect)
4. [**@Target中的`TYPE_PARAMETER和TYPE_USER`用在什么地方？**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485893&idx=2&sn=18b07477518049b762c05b07ca0e5315&chksm=c0fb57daf78cdecc6a705a28c90b3a9a1d8a5bcfd63b8e3a3eb3e97f161c230cf7b1248a60a1&scene=21#wechat_redirect)
5. [**泛型中如何使用注解？**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485893&idx=2&sn=18b07477518049b762c05b07ca0e5315&chksm=c0fb57daf78cdecc6a705a28c90b3a9a1d8a5bcfd63b8e3a3eb3e97f161c230cf7b1248a60a1&scene=21#wechat_redirect)
6. [**注解定义可以实现继承么？**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485893&idx=2&sn=18b07477518049b762c05b07ca0e5315&chksm=c0fb57daf78cdecc6a705a28c90b3a9a1d8a5bcfd63b8e3a3eb3e97f161c230cf7b1248a60a1&scene=21#wechat_redirect)
7. [**spring中对注解有哪些增强？@Aliasfor注解是干什么的？**](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485893&idx=2&sn=18b07477518049b762c05b07ca0e5315&chksm=c0fb57daf78cdecc6a705a28c90b3a9a1d8a5bcfd63b8e3a3eb3e97f161c230cf7b1248a60a1&scene=21#wechat_redirect)

**第1个他回答上来了，后面的几个直接懵了，然后就没有然后了。**

**之后跑来问我，然后我让他看本文，准备下次去吊打面试官。**

## 本文内容

**带你玩转java注解，解决上面的所有问题。**

## 什么是注解？

代码中注释大家都熟悉吧，注释是给开发者看的，可以提升代码的可读性和可维护性，但是对于java编译器和虚拟机来说是没有意义的，编译之后的字节码文件中是没有注释信息的；而注解和注释有点类似，唯一的区别就是注释是给人看的，而注解是给编译器和虚拟机看的，编译器和虚拟机在运行的过程中可以获取注解信息，然后可以根据这些注解的信息做各种想做的事情。比如：大家对@Override应该比较熟悉，就是一个注解，加在方法上，标注当前方法重写了父类的方法，当编译器编译代码的时候，会对@Override标注的方法进行验证，验证其父类中是否也有同样签名的方法，否则报错，通过这个注解是不是增强了代码的安全性。

**总的来说：注解是对代码的一种增强，可以在代码编译或者程序运行期间获取注解的信息，然后根据这些信息做各种牛逼的事情。**

## 注解如何使用？

**3个步骤：**

1. **定义注解**
2. **使用注解**
3. **获取注解信息做各种牛逼的事情**

## 定义注解

关于注解的定义，先来几个问题：

1. 如何为注解定义参数？
2. 注解可以用在哪里？
3. 注解会被保留到什么时候？

### 定义注解语法

jdk中注解相关的类和接口都定义在`java.lang.annotation`包中。

注解的定义和我们常见的类、接口类似，只是注解使用`@interface`来定义，如下定义一个名称为`MyAnnotation`的注解：

```
public @interface MyAnnotation {
}
```

### 注解中定义参数

注解有没有参数都可以，定义参数如下：

```
public @interface 注解名称{
    [public] 参数类型 参数名称1() [default 参数默认值];
    [public] 参数类型 参数名称2() [default 参数默认值];
    [public] 参数类型 参数名称n() [default 参数默认值];
}
```

注解中可以定义多个参数，参数的定义有以下特点：

1. 访问修饰符必须为public，不写默认为public
2. 该元素的类型只能是基本数据类型、String、Class、枚举类型、注解类型（体现了注解的嵌套效果）以及上述类型的一位数组
3. 该元素的名称一般定义为名词，如果注解中只有一个元素，请把名字起为value（后面使用会带来便利操作）
4. 参数名称后面的`()`不是定义方法参数的地方，也不能在括号中定义任何参数，仅仅只是一个特殊的语法
5. `default`代表默认值，值必须和第2点定义的类型一致
6. 如果没有默认值，代表后续使用注解时必须给该类型元素赋值

### 指定注解的使用范围：@Target

使用@Target注解定义注解的使用范围，如下：

```
@Target(value = {ElementType.TYPE,ElementType.METHOD})
public @interface MyAnnotation {
}
```

上面指定了`MyAnnotation`注解可以用在类、接口、注解类型、枚举类型以及方法上面，**自定义注解上也可以不使用@Target注解，如果不使用，表示自定义注解可以用在任何地方**。

看一下`@Target`源码：

```
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Target {
    ElementType[] value();
}
```

有一个参数value，是ElementType类型的一个数组，再来看一下`ElementType`，是个枚举，源码如下：

```
package java.lang.annotation;
/*注解的使用范围*/
public enum ElementType {
       /*类、接口、枚举、注解上面*/
    TYPE,
    /*字段上*/
    FIELD,
    /*方法上*/
    METHOD,
    /*方法的参数上*/
    PARAMETER,
    /*构造函数上*/
    CONSTRUCTOR,
    /*本地变量上*/
    LOCAL_VARIABLE,
    /*注解上*/
    ANNOTATION_TYPE,
    /*包上*/
    PACKAGE,
    /*类型参数上*/
    TYPE_PARAMETER,
    /*类型名称上*/
    TYPE_USE
}
```

### 指定注解的保留策略：@Retention

我们先来看一下java程序的3个过程

1. 源码阶段
2. 源码被编译为字节码之后变成class文件
3. 字节码被虚拟机加载然后运行

那么自定义注解会保留在上面哪个阶段呢？可以通过`@Retention`注解来指定，如：

```
@Retention(RetentionPolicy.SOURCE)
public @interface MyAnnotation {
}
```

上面指定了`MyAnnotation`只存在于源码阶段，后面的2个阶段都会丢失。

来看一下@Retention

```
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Retention {
    RetentionPolicy value();
}
```

有一个value参数，类型为RetentionPolicy枚举，如下：

```
public enum RetentionPolicy {
    /*注解只保留在源码中，编译为字节码之后就丢失了，也就是class文件中就不存在了*/
    SOURCE,
    /*注解只保留在源码和字节码中，运行阶段会丢失*/
    CLASS,
    /*源码、字节码、运行期间都存在*/
    RUNTIME
}
```

## 使用注解

### 语法

> 将注解加载使用的目标上面，如下：

```
@注解名称(参数1=值1,参数2=值2,参数n=值n)
目标对象
```

直接来案例说明。

### 无参注解

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface Ann1 { //@1
}

@Ann1 //@2
public class UseAnnotation1 {
}
```

> @1：Ann1为无参注解
>
> @2：类上使用@Ann1注解，没有参数

### 一个参数的注解

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface Ann2 { //@1
    String name();
}

@Ann2(name = "我是路人") //@2
public class UseAnnotation2 {

}
```

### 一个参数为value的注解，可以省略参数名称

只有一个参数，名称为value的时候，使用时参数名称可以省略

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface Ann3 {
    String value();//@1
}

@Ann3("我是路人") //@2
public class UseAnnotation3 {

}
```

> @1：注解之后一个参数，名称为value
>
> @2：使用注解，参数名称value省略了

### 数组类型参数

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface Ann4 {
    String[] name();//@1
}

@Ann4(name = {"我是路人", "欢迎和我一起学spring"}) //@2
public class UseAnnotation4 {
    @Ann4(name = "如果只有一个值，{}可以省略") //@3
    public class T1 {
    }
}
```

> @1：name的类型是一个String类型的数组
>
> @2：name有多个值的时候，需要使用{}包含起来
>
> @3：如果name只有一个值，{}可以省略

### 为参数指定默认值

通过default为参数指定默认值，用的时候如果没有设置值，则取默认值，没有指定默认值的参数，使用的时候必须为参数设置值，如下：

```
@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface Ann5 {
    String[] name() default {"路人", "spring系列"};//@1
    int[] score() default 1; //@2
    int age() default 30; //@3
    String address(); //@4
}

@Ann5(age = 32,address = "上海") //@5
public class UseAnnotation5 {

}
```

> @1：数组类型通过{}指定默认值
>
> @2：数组类型参数，默认值只有一个省略了{}符号
>
> @3：默认值为30
>
> @4：未指定默认值
>
> @5：age=32对默认值进行了覆盖，并且为address指定了值

### 综合案例

```
@Target(value = {
        ElementType.TYPE,
        ElementType.METHOD,
        ElementType.FIELD,
        ElementType.PARAMETER,
        ElementType.CONSTRUCTOR,
        ElementType.LOCAL_VARIABLE
})
@Retention(RetentionPolicy.RUNTIME)
@interface Ann6 {
    String value();

    ElementType elementType();
}

@Ann6(value = "我用在类上", elementType = ElementType.TYPE)
public class UseAnnotation6 {
    @Ann6(value = "我用在字段上", elementType = ElementType.FIELD)
    private String a;

    @Ann6(value = "我用在构造方法上", elementType = ElementType.CONSTRUCTOR)
    public UseAnnotation6(@Ann6(value = "我用在方法参数上", elementType = ElementType.PARAMETER) String a) {
        this.a = a;
    }

    @Ann6(value = "我用在了普通方法上面", elementType = ElementType.METHOD)
    public void m1() {
        @Ann6(value = "我用在了本地变量上", elementType = ElementType.LOCAL_VARIABLE) String a;
    }
}
```

> 上面演示了自定义注解在在类、字段、构造器、方法参数、方法、本地变量上的使用，@Ann6注解有个`elementType`参数，我想通过这个参数的值来告诉大家对应@Target中的那个值来限制使用目标的，大家注意一下上面每个`elementType`的值。

### @Target(ElementType.TYPE_PARAMETER)

这个是1.8加上的，用来标注类型参数，类型参数一般在类后面声明或者方法上声明，这块需要先了解一下泛型(http://itsoku.com/course/14/209)不然理解起来比较吃力，来个案例感受一下：

```
@Target(value = {
        ElementType.TYPE_PARAMETER
})
@Retention(RetentionPolicy.RUNTIME)
@interface Ann7 {
    String value();
}

public class UseAnnotation7<@Ann7("T0是在类上声明的一个泛型类型变量") T0, @Ann7("T1是在类上声明的一个泛型类型变量") T1> {

    public <@Ann7("T2是在方法上声明的泛型类型变量") T2> void m1() {
    }

    public static void main(String[] args) throws NoSuchMethodException {
        for (TypeVariable typeVariable : UseAnnotation7.class.getTypeParameters()) {
            print(typeVariable);
        }

        for (TypeVariable typeVariable : UseAnnotation7.class.getDeclaredMethod("m1").getTypeParameters()) {
            print(typeVariable);
        }
    }

    private static void print(TypeVariable typeVariable) {
        System.out.println("类型变量名称:" + typeVariable.getName());
        Arrays.stream(typeVariable.getAnnotations()).forEach(System.out::println);
    }
}
```

类和方法上面可以声明泛型类型的变量，上面有3个泛型类型变量，我们运行一下看看效果：

```
类型变量名称:T0
@com.javacode2018.lesson001.demo18.Ann7(value=T0是在类上声明的一个泛型类型变量)
类型变量名称:T1
@com.javacode2018.lesson001.demo18.Ann7(value=T1是在类上声明的一个泛型类型变量)
类型变量名称:T2
@com.javacode2018.lesson001.demo18.Ann7(value=T2是在方法上声明的泛型类型变量)
```

### @Target(ElementType.TYPE_USE)

这个是1.8加上的，能用在任何类型名称上，来个案例感受一下：

```
@Target({ElementType.TYPE_USE})
@Retention(RetentionPolicy.RUNTIME)
@interface Ann10 {
    String value();
}

@Ann10("用在了类上")
public class UserAnnotation10<@Ann10("用在了类变量类型V1上") V1, @Ann10("用在了类变量类型V2上") V2> {

    private Map<@Ann10("用在了泛型类型上") String, Integer> map;

    public <@Ann10("用在了参数上") T> String m1(String name) {
        return null;
    }

}
```

> 类后面的V1、V2都是类型名称，Map后面的尖括号也是类型名称，m1方法前面也定义了一个类型变量，名称为T

## 注解信息的获取

为了运行时能准确获取到注解的相关信息，Java在`java.lang.reflect` 反射包下新增了`AnnotatedElement`接口，它主要用于表示目前正在虚拟机中运行的程序中已使用注解的元素，通过该接口提供的方法可以利用反射技术地读取注解的信息，看一下UML图:

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CqH2JKZG8m1FEX5B33SCom3McUZlMQgRkWicGFFhRjzakOzaJjk8BSP8B0JFlpbswuFibPYbTwTJQg/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)



- Package：用来表示包的信息
- Class：用来表示类的信息
- Constructor：用来表示构造方法信息
- Field：用来表示类中属性信息
- Method：用来表示方法信息
- Parameter：用来表示方法参数信息
- TypeVariable：用来表示类型变量信息，如：类上定义的泛型类型变量，方法上面定义的泛型类型变量

### AnnotatedElement常用方法

![图片](https://mmbiz.qpic.cn/sz_mmbiz_png/xicEJhWlK06CqH2JKZG8m1FEX5B33SComxe9wYaKoYv4HkZ5Fg6hcY2bSBQc4XGyoXEjNalKdkG1TVKT0DV0iaicA/640?wx_fmt=png&wxfrom=5&wx_lazy=1&wx_co=1)

### 案例

#### 要解析的列如下

```
package com.javacode2018.lesson001.demo18;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;
import java.util.Map;

@Target({ElementType.PACKAGE,
        ElementType.TYPE,
        ElementType.FIELD,
        ElementType.CONSTRUCTOR,
        ElementType.METHOD,
        ElementType.PARAMETER,
        ElementType.TYPE_PARAMETER,
        ElementType.TYPE_USE})
@Retention(RetentionPolicy.RUNTIME)
@interface Ann11 {
    String value();
}

@Target({ElementType.PACKAGE,
        ElementType.TYPE,
        ElementType.FIELD,
        ElementType.CONSTRUCTOR,
        ElementType.METHOD,
        ElementType.PARAMETER,
        ElementType.TYPE_PARAMETER,
        ElementType.TYPE_USE})
@Retention(RetentionPolicy.RUNTIME)
@interface Ann11_0 {
    int value();
}

@Ann11("用在了类上")
@Ann11_0(0)
public class UseAnnotation11<@Ann11("用在了类变量类型V1上") @Ann11_0(1) V1, @Ann11("用在了类变量类型V2上") @Ann11_0(2) V2> {
    @Ann11("用在了字段上")
    @Ann11_0(3)
    private String name;

    private Map<@Ann11("用在了泛型类型上,String") @Ann11_0(4) String, @Ann11("用在了泛型类型上,Integer") @Ann11_0(5) Integer> map;

    @Ann11("用在了构造方法上")
    @Ann11_0(6)
    public UseAnnotation11() {
        this.name = name;
    }

    @Ann11("用在了返回值上")
    @Ann11_0(7)
    public String m1(@Ann11("用在了参数上") @Ann11_0(8) String name) {
        return null;
    }

}
```

#### 解析类上的注解

##### 解析这部分

```
@Ann11("用在了类上")
```

##### 代码

```
@Test
public void m1() {
    for (Annotation annotation : UserAnnotation10.class.getAnnotations()) {
        System.out.println(annotation);
    }
}
```

##### 运行输出

```
@com.javacode2018.lesson001.demo18.Ann11(value=用在了类上)
@com.javacode2018.lesson001.demo18.Ann11_0(value=0)
```

#### 解析类上的类型变量

解析类名后面的尖括号的部分，即下面的部分：

```
UseAnnotation11<@Ann11("用在了类变量类型V1上") @Ann11_0(1) V1, @Ann11("用在了类变量类型V2上") @Ann11_0(2) V2>
```

##### 用例代码

```
@Test
public void m2() {
    TypeVariable<Class<UserAnnotation10>>[] typeParameters = UserAnnotation10.class.getTypeParameters();
    for (TypeVariable<Class<UserAnnotation10>> typeParameter : typeParameters) {
        System.out.println(typeParameter.getName() + "变量类型注解信息：");
        Annotation[] annotations = typeParameter.getAnnotations();
        for (Annotation annotation : annotations) {
            System.out.println(annotation);
        }
    }
}
```

##### 运行输出

```
V1变量类型注解信息：
@com.javacode2018.lesson001.demo18.Ann11(value=用在了类变量类型V1上)
@com.javacode2018.lesson001.demo18.Ann11_0(value=1)
V2变量类型注解信息：
@com.javacode2018.lesson001.demo18.Ann11(value=用在了类变量类型V2上)
@com.javacode2018.lesson001.demo18.Ann11_0(value=2)
```

#### 解析字段name上的注解

##### 用例代码

```
@Test
public void m3() throws NoSuchFieldException {
    Field nameField = UserAnnotation10.class.getDeclaredField("name");
    for (Annotation annotation : nameField.getAnnotations()) {
        System.out.println(annotation);
    }
}
```

##### 运行输出

```
@com.javacode2018.lesson001.demo18.Ann11(value=用在了字段上)
@com.javacode2018.lesson001.demo18.Ann11_0(value=3)
```

#### 解析泛型字段map上的注解

##### 用例代码

```
@Test
public void m4() throws NoSuchFieldException, ClassNotFoundException {
    Field field = UseAnnotation11.class.getDeclaredField("map");
    Type genericType = field.getGenericType();
    Type[] actualTypeArguments = ((ParameterizedType) genericType).getActualTypeArguments();

    AnnotatedType annotatedType = field.getAnnotatedType();
    AnnotatedType[] annotatedActualTypeArguments = ((AnnotatedParameterizedType) annotatedType).getAnnotatedActualTypeArguments();
    int i = 0;
    for (AnnotatedType actualTypeArgument : annotatedActualTypeArguments) {
        Type actualTypeArgument1 = actualTypeArguments[i++];
        System.out.println(actualTypeArgument1.getTypeName() + "类型上的注解如下：");
        for (Annotation annotation : actualTypeArgument.getAnnotations()) {
            System.out.println(annotation);
        }
    }
}
```

##### 运行输出

```
java.lang.String类型上的注解如下：
@com.javacode2018.lesson001.demo18.Ann11(value=用在了泛型类型上,String)
@com.javacode2018.lesson001.demo18.Ann11_0(value=4)
java.lang.Integer类型上的注解如下：
@com.javacode2018.lesson001.demo18.Ann11(value=用在了泛型类型上,Integer)
@com.javacode2018.lesson001.demo18.Ann11_0(value=5)
```

#### 解析构造函数上的注解

##### 用例代码

```
@Test
public void m5() {
    Constructor<?> constructor = UseAnnotation11.class.getConstructors()[0];
    for (Annotation annotation : constructor.getAnnotations()) {
        System.out.println(annotation);
    }
}
```

##### 运行输出

```
@com.javacode2018.lesson001.demo18.Ann11(value=用在了构造方法上)
@com.javacode2018.lesson001.demo18.Ann11_0(value=6)
```

#### 解析m1方法上的注解

##### 用例代码

```
@Test
public void m6() throws NoSuchMethodException {
    Method method = UseAnnotation11.class.getMethod("m1", String.class);
    for (Annotation annotation : method.getAnnotations()) {
        System.out.println(annotation);
    }
}
```

##### 运行输出

```
@com.javacode2018.lesson001.demo18.Ann11(value=用在了返回值上)
@com.javacode2018.lesson001.demo18.Ann11_0(value=7)
```

#### 解析m1方法参数注解

##### 用例代码

```
@Test
public void m7() throws NoSuchMethodException {
    Method method = UseAnnotation11.class.getMethod("m1", String.class);
    for (Parameter parameter : method.getParameters()) {
        System.out.println(String.format("参数%s上的注解如下:", parameter.getName()));
        for (Annotation annotation : parameter.getAnnotations()) {
            System.out.println(annotation);
        }
    }
}
```

##### 运行输出

```
参数arg0上的注解如下:
@com.javacode2018.lesson001.demo18.Ann11(value=用在了参数上)
@com.javacode2018.lesson001.demo18.Ann11_0(value=8)
```

##### 上面参数名称为arg0，如果想让参数名称和源码中真实名称一致，操作如下：

```
如果你编译这个class的时候没有添加参数–parameters，运行的时候你会得到这个结果：

Parameter: arg0

编译的时候添加了–parameters参数的话，运行结果会不一样：

Parameter: args

对于有经验的Maven使用者，–parameters参数可以添加到maven-compiler-plugin的配置部分：

<plugin>
    <groupId>org.apache.maven.plugins</groupId>
    <artifactId>maven-compiler-plugin</artifactId>
    <version>3.1</version>
    <configuration>
        <compilerArgument>-parameters</compilerArgument>
        <source>1.8</source>
        <target>1.8</target>
    </configuration>
</plugin>
```

## @Inherit：实现类之间的注解继承

### 用法

来看一下这个注解的源码

```
@Documented
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.ANNOTATION_TYPE)
public @interface Inherited {
}
```

我们通过@Target元注解的属性值可以看出，这个@Inherited 是专门修饰注解的。

**作用：让子类可以继承父类中被@Inherited修饰的注解，注意是继承父类中的，如果接口中的注解也使用@Inherited修饰了，那么接口的实现类是无法继承这个注解的**

### 案例

```
package com.javacode2018.lesson001.demo18;

import java.lang.annotation.*;

public class InheritAnnotationTest {
    @Target(ElementType.TYPE)
    @Retention(RetentionPolicy.RUNTIME)
    @Inherited
    @interface A1{ //@1
    }
    @Target(ElementType.TYPE)
    @Retention(RetentionPolicy.RUNTIME)
    @Inherited
    @interface A2{ //@2
    }

    @A1 //@3
    interface I1{}
    @A2 //@4
    static class C1{}

    static class C2 extends C1 implements I1{} //@5

    public static void main(String[] args) {
        for (Annotation annotation : C2.class.getAnnotations()) { //@6
            System.out.println(annotation);
        }
    }
}
```

> @1：定义了一个注解A1，上面使用了@Inherited，表示这个具有继承功能
>
> @2：定义了一个注解A2，上面使用了@Inherited，表示这个具有继承功能
>
> @3：定义接口I1，上面使用了@A1注解
>
> @4：定义了一个C1类，使用了A2注解
>
> @5：C2继承了C1并且实现了I1接口
>
> @6：获取C2上以及从父类继承过来的所有注解，然后输出

运行输出：

```
@com.javacode2018.lesson001.demo18.InheritAnnotationTest$A2()
```

**从输出中可以看出类可以继承父类上被@Inherited修饰的注解，而不能继承接口上被@Inherited修饰的注解，这个一定要注意**

## @Repeatable重复使用注解

来看一段代码：

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.TYPE)
@interface Ann12{}

@Ann12
@Ann12
public class UseAnnotation12 {
}
```

上面代码会报错，原因是：UseAnnotation12上面重复使用了@Ann12注解，默认情况下@Ann12注解是不允许重复使用的。

像上面这样，如果我们想重复使用注解的时候，需要用到`@Repeatable`注解

### 使用步骤

#### 先定义容器注解

```
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.FIELD})
@interface Ann12s {
    Ann12[] value(); //@1
}
```

> 容器注解中必须有个value类型的参数，参数类型为子注解类型的数组。

#### 为注解指定容器

要让一个注解可以重复使用，需要在注解上加上@Repeatable注解，@Repeatable中value的值为容器注解，如下代码中的@2

```
@Retention(RetentionPolicy.RUNTIME)
@Target({ElementType.TYPE, ElementType.FIELD})
@Repeatable(Ann12s.class)//@2
@interface Ann12 {
    String name();
}
```

#### 使用注解

重复使用相同的注解有2种方式，如下面代码

1. 重复使用注解，如下面的类上重复使用@Ann12注解
2. 通过容器注解来使用更多个注解，如下面的字段v1上使用@Ann12s容器注解

```
@Ann12(name = "路人")
@Ann12(name = "Spring系列")
public class UseAnnotation12 {
    @Ann12s(
            {@Ann12(name = "Java高并发系列，见公众号"),
                    @Ann12(name = "mysql高手系列，见公众号")}
    )
    private String v1;
}
```

#### 获取注解信息

```
com.javacode2018.lesson001.demo18.UseAnnotation12

@Test
public void test1() throws NoSuchFieldException {
    Annotation[] annotations = UseAnnotation12.class.getAnnotations();
    for (Annotation annotation : annotations) {
        System.out.println(annotation);
    }
    System.out.println("-------------");
    Field v1 = UseAnnotation12.class.getDeclaredField("v1");
    Annotation[] declaredAnnotations = v1.getDeclaredAnnotations();
    for (Annotation declaredAnnotation : declaredAnnotations) {
        System.out.println(declaredAnnotation);
    }
}
```

运行输出：

```
@com.javacode2018.lesson001.demo18.Ann12s(value=[@com.javacode2018.lesson001.demo18.Ann12(name=路人), @com.javacode2018.lesson001.demo18.Ann12(name=Spring系列)])
-------------
@com.javacode2018.lesson001.demo18.Ann12s(value=[@com.javacode2018.lesson001.demo18.Ann12(name=Java高并发系列，见公众号), @com.javacode2018.lesson001.demo18.Ann12(name=mysql高手系列，见公众号)])
```

上面就是java中注解的功能，下面我们来介绍spring对于注解方面的支持。

## 先来看一个问题

代码如下：

```
package com.javacode2018.lesson001.demo18;

import org.junit.Test;
import org.springframework.core.annotation.AnnotatedElementUtils;
import org.springframework.core.annotation.AnnotationUtils;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface A1 {
    String value() default "a";//@0
}

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@A1
@interface B1 { //@1
    String value() default "b";//@2
}

@B1("路人") //@3
public class UseAnnotation13 {
    @Test
    public void test1() {
        //AnnotatedElementUtils是spring提供的一个查找注解的工具类
        System.out.println(AnnotatedElementUtils.getMergedAnnotation(UseAnnotation13.class, B1.class));
        System.out.println(AnnotatedElementUtils.getMergedAnnotation(UseAnnotation13.class, A1.class));
    }
}
```

> @0：A1注解value参数值默认为a
>
> @1：B1注解上使用到了@A1注解
>
> @2：B1注解value参数值默认为b
>
> @2：UseAnnotation13上面使用了@B1注解，value参数的值为：路人
>
> test1方法中使用到了spring中的一个类`AnnotatedElementUtils`，通过这个工具类可以很方便的获取注解的各种信息，方法中的2行代码用于获取UseAnnotation13类上B1注解和A1注解的信息。

运行test1方法输出：

```
@com.javacode2018.lesson001.demo18.B1(value=路人)
@com.javacode2018.lesson001.demo18.A1(value=a)
```

上面用法很简单，没什么问题。

**此时有个问题：此时如果想在`UseAnnotation13`上给B1上的A1注解设置值是没有办法的，注解定义无法继承导致的，如果注解定义上面能够继承，那用起来会爽很多，spring通过@Aliasfor方法解决了这个问题。**

## Spring  @AliasFor：对注解进行增强

直接上案例，然后解释代码。

### 案例1：通过@AliasFor解决刚才难题

```
package com.javacode2018.lesson001.demo18;

import org.junit.Test;
import org.springframework.core.annotation.AliasFor;
import org.springframework.core.annotation.AnnotatedElementUtils;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface A14 {
    String value() default "a";//@0
}

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@A14 //@6
@interface B14 { //@1

    String value() default "b";//@2

    @AliasFor(annotation = A14.class, value = "value") //@5
    String a14Value();
}

@B14(value = "路人",a14Value = "通过B14给A14的value参数赋值") //@3
public class UseAnnotation14 {
    @Test
    public void test1() {
        //AnnotatedElementUtils是spring提供的一个查找注解的工具类
        System.out.println(AnnotatedElementUtils.getMergedAnnotation(UseAnnotation14.class, B14.class));
        System.out.println(AnnotatedElementUtils.getMergedAnnotation(UseAnnotation14.class, A14.class));
    }
}
```

运行输出：

```
@com.javacode2018.lesson001.demo18.B14(a14Value=通过B14给A14的value参数赋值, value=路人)
@com.javacode2018.lesson001.demo18.A14(value=通过B14给A14的value参数赋值)
```

注意上面diam的@3只使用了B14注解，大家认真看一下，上面输出汇总可以看出A14的value值和B14的a14Value参数值一样，说明通过B14给A14设置值成功了。

重点在于代码@5，这个地方使用到了`@AliasFor`注解：

```
@AliasFor(annotation = A14.class, value = "value")
```

**这个相当于给某个注解指定别名，即将B1注解中`a14Value`参数作为`A14`中`value`参数的别名，当给`B1的a14Value`设置值的时候，就相当于给`A14的value设置值`，有个前提是@AliasFor注解的`annotation`参数指定的注解需要加载当前注解上面，如：@6**

### 案例2：同一个注解中使用@AliasFor

```
package com.javacode2018.lesson001.demo18;

import org.junit.Test;
import org.springframework.core.annotation.AliasFor;
import org.springframework.core.annotation.AnnotatedElementUtils;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target({ElementType.TYPE, ElementType.FIELD})
@Retention(RetentionPolicy.RUNTIME)
@interface A15 {
    @AliasFor("v2")//@1
    String v1() default "";

    @AliasFor("v1")//@2
    String v2() default "";
}

@A15(v1 = "我是v1") //@3
public class UseAnnotation15 {

    @A15(v2 = "我是v2") //@4
    private String name;

    @Test
    public void test1() throws NoSuchFieldException {
        //AnnotatedElementUtils是spring提供的一个查找注解的工具类
        System.out.println(AnnotatedElementUtils.getMergedAnnotation(UseAnnotation15.class, A15.class));
        System.out.println(AnnotatedElementUtils.getMergedAnnotation(UseAnnotation15.class.getDeclaredField("name"), A15.class));
    }
}
```

> 注意上面代码，A15注解中（@1和@2）的2个参数都设置了@AliasFor，@AliasFor如果不指定`annotation`参数的值，那么`annotation`默认值就是当前注解，所以上面2个属性互为别名，当给v1设置值的时候也相当于给v2设置值，当给v2设置值的时候也相当于给v1设置值。

运行输出

```
@com.javacode2018.lesson001.demo18.A15(v1=我是v1, v2=我是v1)
@com.javacode2018.lesson001.demo18.A15(v1=我是v2, v2=我是v2)
```

> 从输出中可以看出v1和v2的值始终是相等的，上面如果同时给v1和v2设置值的时候运行代码会报错。

我们回头来看看@AliasFor的源码：

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
@Documented
public @interface AliasFor {

    @AliasFor("attribute")
    String value() default "";

    @AliasFor("value")
    String attribute() default "";

    Class<? extends Annotation> annotation() default Annotation.class;

}
```

AliasFor注解中`value`和`attribute`互为别名，随便设置一个，同时会给另外一个设置相同的值。

### 案例2：@AliasFor中不指定value和attribute

当@AliasFor中不指定value或者attribute的时候，自动将@AliasFor修饰的参数作为value和attribute的值，如下@AliasFor注解的value参数值为name

```
package com.javacode2018.lesson001.demo18;

import org.junit.Test;
import org.springframework.core.annotation.AliasFor;
import org.springframework.core.annotation.AnnotatedElementUtils;

import java.lang.annotation.ElementType;
import java.lang.annotation.Retention;
import java.lang.annotation.RetentionPolicy;
import java.lang.annotation.Target;

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@interface A16 {
    String name() default "a";//@0
}

@Target(ElementType.TYPE)
@Retention(RetentionPolicy.RUNTIME)
@A16
@interface B16 { //@1

    @AliasFor(annotation = A16.class) //@5
    String name() default "b";//@2
}

@B16(name="我是v1") //@3
public class UseAnnotation16 {


    @Test
    public void test1() throws NoSuchFieldException {
        //AnnotatedElementUtils是spring提供的一个查找注解的工具类
        System.out.println(AnnotatedElementUtils.getMergedAnnotation(UseAnnotation16.class, A16.class));
        System.out.println(AnnotatedElementUtils.getMergedAnnotation(UseAnnotation16.class, B16.class));
    }
}
```

运行输出：

```
@com.javacode2018.lesson001.demo18.A16(name=我是v1)
@com.javacode2018.lesson001.demo18.B16(name=我是v1)
```