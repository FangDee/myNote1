# Spring第7篇：依赖注入之手动注入

路人甲Java [程序员路人](javascript:void(0);) *2022-03-30 16:59*

**大家好，我是路人~~~
**

**点击下方卡片，关注我，java干货及时送达**

![img](http://mmbiz.qpic.cn/mmbiz_png/kvBL17F8Dr1MFuX9tnHDHzic6yBpLSiaTibZLKcia9tHwm7aFueNjgoAIibdqXibicAUEia87xvDN77VhfDKbo59BJCwlw/0?wx_fmt=png)

**程序员路人**

专注Java相关技术：SSM、Spring全家桶、微服务、MySQL、MyCat、集群、分布式、中间件、Linux、网络、多线程，偶尔讲点运维Jenkins、Nexus、Docker、ELK，偶尔分享些技术干货，致力于Java全栈开发！

1篇原创内容



公众号

**这是 Spring 第 7 篇，点击上方卡片，即可翻阅前面章节。**

***\**\*强烈推荐阅读\*\**\******\**\*：\*\**\******\**\*[\*\*到底如何学好Java？\*\*](http://mp.weixin.qq.com/s?__biz=MzkwNTI4NDQ2NA==&mid=2247485855&idx=1&sn=20f2d0cb1fb9cbe842000ea4ebbcdda4&chksm=c0fb5780f78cde960e25a5b48d833ad5f49330b8663cbd2f29112eb8166f4a9113bc94dd9b27&scene=21#wechat_redirect)\*\**\***

### 本文内容 

1. **主要介绍xml中依赖注入的配置**
2. **构造器注入的3种方式详解**
3. **set方法注入详解**
4. **注入容器中的其他bean的2种方式**
5. **其他常见类型注入详解**

### 依赖回顾

通常情况下，系统中类和类之间是有依赖关系的，如果一个类对外提供的功能需要通过调用其他类的方法来实现的时候，说明这两个类之间存在依赖关系，如：

```
public class UserService{
    public void insert(UserModel model){
        //插入用户信息
    }
}

public class UserController{

    private UserService userService;

    public void insert(UserModel model){
        this.userService.insert(model);
    }

}
```

UserController中的insert方法中需要调用userService的insert方法，说明UserController依赖于UserService，如果userService不存在，此时UserControler无法对外提供insert操作。

那么我们创建UserController对象的时候如何将给userService设置值呢？通常有2种方法。

#### 依赖对象的初始化方式

##### 通过构造器设置依赖对象

UserController中添加一个有参构造方法，如下：

```
public class UserController{

    private UserService userService;

    public UserController(UserService userService){
        this.userService = userService;
    }

    public void insert(UserModel model){
        this.userService.insert(model);
    }

}

//UserController使用
UserSerivce userService = new UserService();
UserController userController = new UserController(userService);
//然后就可以使用userController对象了
```

##### 通过set方法设置依赖对象

可以在UserController中给userService添加一个set方法，如：

```
public class UserController{

    private UserService userService;

    public setUserService(UserService userService){
        this.userService = userService;
    }

    public void insert(UserModel model){
        this.userService.insert(model);
    }

}

//UserController使用
UserSerivce userService = new UserService();
UserController userController = new UserController();
userController.setService(userService);
//然后就可以使用userController对象了
```

上面这些操作，将被依赖的对象设置到依赖的对象中，spring容器内部都提供了支持，这个在spirng中叫做依赖注入。

### spring依赖注入

spring中依赖注入主要分为手动注入和自动注入，本文我们主要说一下手动注入，手动注入需要我们明确配置需要注入的对象。

刚才上面我们回顾了，将被依赖方注入到依赖方，通常有2种方式：构造函数的方式和set属性的方式，spring中也是通过这两种方式实现注入的，下面详解2种方式。

### 通过构造器注入

构造器的参数就是被依赖的对象，构造器注入又分为3种注入方式：

- 根据构造器参数索引注入
- 根据构造器参数类型注入
- 根据构造器参数名称注入

### 根据构造器参数索引注入

#### 用法

```
<bean id="diByConstructorParamIndex" class="com.javacode2018.lesson001.demo5.UserModel">
    <constructor-arg index="0" value="程序员路人"/>
    <constructor-arg index="1" value="上海市"/>
</bean>
```

> constructor-arg用户指定构造器的参数
>
> index：构造器参数的位置，从0开始
>
> value：构造器参数的值，value只能用来给简单的类型设置值，value对应的属性类型只能为byte,int,long,float,double,boolean,Byte,Long,Float,Double,枚举，spring容器内部注入的时候会将value的值转换为对应的类型。

#### 案例

##### UserModel.java

```
package com.javacode2018.lesson001.demo5;

public class UserModel {
    private String name;
    private int age;
    //描述信息
    private String desc;

    public UserModel() {
    }

    public UserModel(String name, String desc) {
        this.name = name;
        this.desc = desc;
    }

    public UserModel(String name, int age, String desc) {
        this.name = name;
        this.age = age;
        this.desc = desc;
    }

    @Override
    public String toString() {
        return "UserModel{" +
                "name='" + name + '\'' +
                ", age=" + age +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```

> 注意上面的3个构造函数。

##### diByConstructorParamIndex.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <!-- 通过构造器参数的索引注入 -->
    <bean id="diByConstructorParamIndex" class="com.javacode2018.lesson001.demo5.UserModel">
        <constructor-arg index="0" value="程序员路人"/>
        <constructor-arg index="1" value="我是通过构造器参数位置注入的"/>
    </bean>

</beans>
```

上面创建UserModel实例代码相当于下面代码：

```
UserModel userModel = new UserModel("程序员路人","我是通过构造器参数类型注入的");
```

##### 工具类IoUtils.java

```
package com.javacode2018.lesson001.demo5;

import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 公众号：程序员路人
 */
public class IocUtil {
    public static ClassPathXmlApplicationContext context(String beanXml) {
        return new ClassPathXmlApplicationContext(beanXml);
    }
}
```

##### 测试用例DiTest.java

```
package com.javacode2018.lesson001.demo5;

import org.junit.Test;
import org.springframework.context.support.ClassPathXmlApplicationContext;

/**
 * 公众号：程序员路人
 */
public class DiTest {

    /**
     * 通过构造器的参数位置注入
     */
    @Test
    public void diByConstructorParamIndex() {
        String beanXml = "classpath:/com/javacode2018/lesson001/demo5/diByConstructorParamIndex.xml";
        ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
        System.out.println(context.getBean("diByConstructorParamIndex"));
    }

}
```

##### 效果

运行diByConstructorParamIndex输出

```
UserModel{name='程序员路人', age=0, desc='我是通过构造器参数位置注入的'}
```

#### 优缺点

**参数位置的注入对参数顺序有很强的依赖性，若构造函数参数位置被人调整过，会导致注入出错。**

**不过通常情况下，不建议去在代码中修改构造函数，如果需要新增参数的，可以新增一个构造函数来实现，这算是一种扩展，不会影响目前已有的功能。**

### 根据构造器参数类型注入

#### 用法

```
<bean id="diByConstructorParamType" class="com.javacode2018.lesson001.demo5.UserModel">
    <constructor-arg type="参数类型" value="参数值"/>
    <constructor-arg type="参数类型" value="参数值"/>
</bean>
```

> constructor-arg用户指定构造器的参数
>
> type：构造函数参数的完整类型，如：java.lang.String,int,double
>
> value：构造器参数的值，value只能用来给简单的类型设置值

#### 案例

##### diByConstructorParamType.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <!-- 通过构造器参数的类型注入 -->
    <bean id="diByConstructorParamType" class="com.javacode2018.lesson001.demo5.UserModel">
        <constructor-arg type="int" value="30"/>
        <constructor-arg type="java.lang.String" value="程序员路人"/>
        <constructor-arg type="java.lang.String" value="我是通过构造器参数类型注入的"/>
    </bean>

</beans>
```

上面创建UserModel实例代码相当于下面代码：

```
UserModel userModel = new UserModel("程序员路人",30,"我是通过构造器参数类型注入的");
```

##### 新增测试用例

DiTest类中新增一个测试用例

```
/**
 * 通过构造器的参数类型注入
 */
@Test
public void diByConstructorParamType() {
    String beanXml = "classpath:/com/javacode2018/lesson001/demo5/diByConstructorParamType.xml";
    ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
    System.out.println(context.getBean("diByConstructorParamType"));
}
```

##### 效果

运行diByConstructorParamType输出

```
UserModel{name='程序员路人', age=30, desc='我是通过构造器参数类型注入的'}
```

#### 优缺点

**实际上按照参数位置或者按照参数的类型注入，都有一个问题，很难通过bean的配置文件，知道这个参数是对应UserModel中的那个属性的，代码的可读性不好，比如我想知道这每个参数对应UserModel中的那个属性，必须要去看UserModel的源码，下面要介绍按照参数名称注入的方式比上面这2种更优秀一些。**

### 根据构造器参数名称注入

#### 用法

```
<bean id="diByConstructorParamName" class="com.javacode2018.lesson001.demo5.UserModel">
    <constructor-arg name="参数类型" value="参数值"/>
    <constructor-arg name="参数类型" value="参数值"/>
</bean>
```

> constructor-arg用户指定构造器的参数
>
> name：构造参数名称
>
> value：构造器参数的值，value只能用来给简单的类型设置值

#### 关于方法参数名称的问题

java通过反射的方式可以获取到方法的参数名称，不过源码中的参数通过编译之后会变成class对象，通常情况下源码变成class文件之后，参数的真实名称会丢失，参数的名称会变成arg0,arg1,arg2这样的，和实际参数名称不一样了，**如果需要将源码中的参数名称保留在编译之后的class文件中，编译的时候需要用下面的命令**：

```
javac -parameters java源码
```

但是我们难以保证编译代码的时候，操作人员一定会带上-parameters参数，所以方法的参数可能在class文件中会丢失，导致反射获取到的参数名称和实际参数名称不符，这个我们需要先了解一下。

**参数名称可能不稳定的问题，spring提供了解决方案，通过ConstructorProperties注解来定义参数的名称，将这个注解加在构造方法上面**，如下：

```
@ConstructorProperties({"第一个参数名称", "第二个参数的名称",..."第n个参数的名称"})
public 类名(String p1, String p2...,参数n) {
}
```

#### 案例

##### CarModel.java

```
package com.javacode2018.lesson001.demo5;

import java.beans.ConstructorProperties;

public class CarModel {
    private String name;
    //描述信息
    private String desc;

    public CarModel() {
    }

    @ConstructorProperties({"name", "desc"})
    public CarModel(String p1, String p2) {
        this.name = p1;
        this.desc = p2;
    }

    @Override
    public String toString() {
        return "CarModel{" +
                "name='" + name + '\'' +
                ", desc='" + desc + '\'' +
                '}';
    }
}
```

##### diByConstructorParamName.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <!-- 通过构造器参数的名称注入 -->
    <bean id="diByConstructorParamName" class="com.javacode2018.lesson001.demo5.CarModel">
        <constructor-arg name="desc" value="我是通过构造器参数类型注入的"/>
        <constructor-arg name="name" value="保时捷Macans"/>
    </bean>

</beans>
```

上面创建CarModel实例代码相当于下面代码：

```
CarModel carModel = new CarModel("保时捷Macans","我是通过构造器参数类型注入的");
```

##### 新增测试用例

DiTest类中新增一个测试用例

```
/**
 * 通过构造器的参数名称注入
 */
@Test
public void diByConstructorParamName() {
    String beanXml = "classpath:/com/javacode2018/lesson001/demo5/diByConstructorParamName.xml";
    ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
    System.out.println(context.getBean("diByConstructorParamName"));
}
```

##### 效果

运行diByConstructorParamName输出

```
CarModel{name='保时捷Macans', desc='我是通过构造器参数类型注入的'}
```

### setter注入

**通常情况下，我们的类都是标准的javabean，javabean类的特点：**

- **属性都是private访问级别的**
- **属性通常情况下通过一组setter（修改器）和getter（访问器）方法来访问**
- **setter方法，以set开头，后跟首字母大写的属性名，如：setUserName，简单属性一般只有一个方法参数，方法返回值通常为void;**
- **getter方法，一般属性以get开头，对于boolean类型一般以is开头，后跟首字母大写的属性名，如：getUserName，isOk；**

spring对符合javabean特点类，提供了setter方式的注入，会调用对应属性的setter方法将被依赖的对象注入进去。

#### 用法

```
<bean id="" class="">
    <property name="属性名称" value="属性值" />
    ...
    <property name="属性名称" value="属性值" />
</bean>
```

> property用于对属性的值进行配置，可以有多个
>
> name：属性的名称
>
> value：属性的值

#### 案例

##### MenuModel.java

```
package com.javacode2018.lesson001.demo5;

/**
 * 菜单类
 */
public class MenuModel {
    //菜单名称
    private String label;
    //同级别排序
    private Integer theSort;

    public String getLabel() {
        return label;
    }

    public void setLabel(String label) {
        this.label = label;
    }

    public Integer getTheSort() {
        return theSort;
    }

    public void setTheSort(Integer theSort) {
        this.theSort = theSort;
    }

    @Override
    public String toString() {
        return "MenuModel{" +
                "label='" + label + '\'' +
                ", theSort=" + theSort +
                '}';
    }
}
```

##### diBySetter.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="diBySetter" class="com.javacode2018.lesson001.demo5.MenuModel">
        <property name="label" value="spring系列"/>
    </bean>

</beans>
```

##### 新增测试用例

DiTest类中添加一个测试方法

```
/**
 * 通过setter方法注入
 */
@Test
public void diBySetter() {
    String beanXml = "classpath:/com/javacode2018/lesson001/demo5/diBySetter.xml";
    ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
    System.out.println(context.getBean("diBySetter"));
}
```

##### 效果

运行diBySetter输出

```
MenuModel{label='spring系列', theSort=null}
```

#### 优缺点

**setter注入相对于构造函数注入要灵活一些，构造函数需要指定对应构造函数中所有参数的值，而setter注入的方式没有这种限制，不需要对所有属性都进行注入，可以按需进行注入。**

上面介绍的都是注入普通类型的对象，都是通过value属性来设置需要注入的对象的值的，value属性的值是String类型的，spring容器内部自动会将value的值转换为对象的实际类型。

**若我们依赖的对象是容器中的其他bean对象的时候，需要用下面的方式进行注入。**

### 注入容器中的bean

**注入容器中的bean有两种写法：**

- **ref属性方式**
- **内置bean的方式**

#### ref属性方式

将上面介绍的constructor-arg或者property元素的value属性名称替换为ref，ref属性的值为容器中其他bean的名称，如：

构造器方式，将value替换为ref：

```
<constructor-arg ref="需要注入的bean的名称"/>
```

setter方式，将value替换为ref：

```
<property name="属性名称" ref="需要注入的bean的名称" />
```

#### 内置bean的方式

构造器的方式：

```
<constructor-arg>
    <bean class=""/>
</constructor-arg>
```

setter方式：

```
<property name="属性名称">
    <bean class=""/>
</property>
```

#### 案例

##### PersonModel.java

```
package com.javacode2018.lesson001.demo5;

public class PersonModel {
    private UserModel userModel;
    private CarModel carModel;

    public PersonModel() {
    }

    public PersonModel(UserModel userModel, CarModel carModel) {
        this.userModel = userModel;
        this.carModel = carModel;
    }

    public UserModel getUserModel() {
        return userModel;
    }

    public void setUserModel(UserModel userModel) {
        this.userModel = userModel;
    }

    public CarModel getCarModel() {
        return carModel;
    }

    public void setCarModel(CarModel carModel) {
        this.carModel = carModel;
    }

    @Override
    public String toString() {
        return "PersonModel{" +
                "userModel=" + userModel +
                ", carModel=" + carModel +
                '}';
    }
}
```

PersonModel中有依赖于2个对象UserModel、CarModel，下面我们通过spring将UserModel和CarModel创建好，然后注入到PersonModel中，下面创建bean配置文件

##### diBean.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="user" class="com.javacode2018.lesson001.demo5.UserModel"></bean>

    <!-- 通过构造器方式注入容器中的bean -->
    <bean id="diBeanByConstructor" class="com.javacode2018.lesson001.demo5.PersonModel">
        <!-- 通过ref引用容器中定义的其他bean，user对应上面定义的id="user"的bean -->
        <constructor-arg index="0" ref="user"/>
        <constructor-arg index="1">
            <bean class="com.javacode2018.lesson001.demo5.CarModel">
                <constructor-arg index="0" value="宾利"/>
                <constructor-arg index="1" value=""/>
            </bean>
        </constructor-arg>
    </bean>

    <!-- 通过setter方式注入容器中的bean -->
    <bean id="diBeanBySetter" class="com.javacode2018.lesson001.demo5.PersonModel">
        <!-- 通过ref引用容器中定义的其他bean，user对应上面定义的id="user"的bean -->
        <property name="userModel" ref="user"/>
        <property name="carModel">
            <bean class="com.javacode2018.lesson001.demo5.CarModel">
                <constructor-arg index="0" value="保时捷"/>
                <constructor-arg index="1" value=""/>
            </bean>
        </property>
    </bean>

</beans>
```

##### 新增测试用例

DiTest中新增一个测试方法

```
@Test
public void diBean(){
    String beanXml = "classpath:/com/javacode2018/lesson001/demo5/diBean.xml";
    ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
    System.out.println(context.getBean("diBeanByConstructor"));
    System.out.println(context.getBean("diBeanBySetter"));
}
```

##### 效果

运行diBean用例，输出：

```
PersonModel{userModel=UserModel{name='null', age=0, desc='null'}, carModel=CarModel{name='宾利', desc=''}}
PersonModel{userModel=UserModel{name='null', age=0, desc='null'}, carModel=CarModel{name='保时捷', desc=''}}
```

### 其他类型注入

#### 注入java.util.List（list元素）

```
<list>
    <value>Spring</value>
    或
    <ref bean="bean名称"/>
    或
    <list></list>
    或
    <bean></bean>
    或
    <array></array>
    或
    <map></map>
</list>
```

#### 注入java.util.Set（set元素）

```
<set>
    <value>Spring</value>
    或
    <ref bean="bean名称"/>
    或
    <list></list>
    或
    <bean></bean>
    或
    <array></array>
    或
    <map></map>
</set>
```

#### 注入java.util.Map（map元素）

```
<map>
    <entry key="程序员路人" value="30" key-ref="key引用的bean名称" value-ref="value引用的bean名称"/>
    或
    <entry>
        <key>
            value对应的值，可以为任意类型
        </key>
        <value>
            value对应的值，可以为任意类型
        </value>
    </entry>
</map>
```

#### 注入数组（array元素）

```
<array>
    数组中的元素
</array>
```

#### 注入java.util.Properties（props元素）

Properties类相当于键值都是String类型的Map对象，使用props进行注入，如下：

```
<props>
    <prop key="key1">java高并发系列</prop>
    <prop key="key2">mybatis系列</prop>
    <prop key="key3">mysql系列</prop>
</props>
```

#### 案例

对于上面这些类型来个综合案例。

##### DiOtherTypeModel.java

```
package com.javacode2018.lesson001.demo5;

import java.util.*;

public class DiOtherTypeModel {
    private List<String> list1;
    private Set<UserModel> set1;
    private Map<String, Integer> map1;
    private int[] array1;
    private Properties properties1;

    public List<String> getList1() {
        return list1;
    }

    public void setList1(List<String> list1) {
        this.list1 = list1;
    }

    public Set<UserModel> getSet1() {
        return set1;
    }

    public void setSet1(Set<UserModel> set1) {
        this.set1 = set1;
    }

    public Map<String, Integer> getMap1() {
        return map1;
    }

    public void setMap1(Map<String, Integer> map1) {
        this.map1 = map1;
    }

    public int[] getArray1() {
        return array1;
    }

    public void setArray1(int[] array1) {
        this.array1 = array1;
    }

    public Properties getProperties1() {
        return properties1;
    }

    public void setProperties1(Properties properties1) {
        this.properties1 = properties1;
    }

    @Override
    public String toString() {
        return "DiOtherTypeModel{" +
                "list1=" + list1 +
                ", set1=" + set1 +
                ", map1=" + map1 +
                ", array1=" + Arrays.toString(array1) +
                ", properties1=" + properties1 +
                '}';
    }
}
```

上面这个类中包含了刚才我们介绍的各种类型，下面来进行注入。

##### diOtherType.xml

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans-4.3.xsd">

    <bean id="user1" class="com.javacode2018.lesson001.demo5.UserModel"/>
    <bean id="user2" class="com.javacode2018.lesson001.demo5.UserModel"/>

    <bean id="diOtherType" class="com.javacode2018.lesson001.demo5.DiOtherTypeModel">
        <!-- 注入java.util.List对象 -->
        <property name="list1">
            <list>
                <value>Spring</value>
                <value>SpringBoot</value>
            </list>
        </property>

        <!-- 注入java.util.Set对象 -->
        <property name="set1">
            <set>
                <ref bean="user1"/>
                <ref bean="user2"/>
                <ref bean="user1"/>
            </set>
        </property>

        <!-- 注入java.util.Map对象 -->
        <property name="map1">
            <map>
                <entry key="程序员路人" value="30"/>
                <entry key="路人" value="28"/>
            </map>
        </property>

        <!-- 注入数组对象 -->
        <property name="array1">
            <array>
                <value>10</value>
                <value>9</value>
                <value>8</value>
            </array>
        </property>

        <!-- 注入java.util.Properties对象 -->
        <property name="properties1">
            <props>
                <prop key="key1">java高并发系列</prop>
                <prop key="key2">mybatis系列</prop>
                <prop key="key3">mysql系列</prop>
            </props>
        </property>

    </bean>
</beans>
```

##### 新增测试用例

DiTest类中新增一个方法

```
/**
 * 其他各种类型注入
 */
@Test
public void diOtherType() {
    String beanXml = "classpath:/com/javacode2018/lesson001/demo5/diOtherType.xml";
    ClassPathXmlApplicationContext context = IocUtils.context(beanXml);
    System.out.println(context.getBean("diOtherType"));
}
```

##### 效果

运行测试用例输出：

```
DiOtherTypeModel{list1=[Spring, SpringBoot], set1=[UserModel{name='null', age=0, desc='null'}, UserModel{name='null', age=0, desc='null'}], map1={程序员路人=30, 路人=28}, array1=[10, 9, 8], properties1={key3=mysql系列, key2=mybatis系列, key1=java高并发系列}}
```

### 总结

1. **本文主要讲解了xml中bean的依赖注入，都是采用硬编码的方式进行注入的，这种算是手动的方式**
2. **注入普通类型通过value属性或者value元素设置注入的值；注入对象如果是容器的其他bean的时候，需要使用ref属性或者ref元素或者内置bean元素的方式**
3. **还介绍了其他几种类型List、Set、Map、数组、Properties类型的注入，多看几遍加深理解**
4. **后面我们将介绍spring为我们提供的更牛逼的自动注入**

### 更多好文章