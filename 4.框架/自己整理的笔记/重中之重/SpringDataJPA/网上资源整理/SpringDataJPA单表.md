# SpringDataJPA单表

## 1.SpringDataJPA基本配置

最近在看spring boot相关的内容，在看到spring data JPA的时候，发现通过程序猿DD以及泥瓦匠的博客来看，都是基础的引入JPA依赖，且版本也不是最新的。通过搜索引擎得到的结果也大多是copy by copy，没有什么实际有深度的文章，这里将通过阅读官方文档，学到的一些东西记录一下，方便别人来学习参考。
这一节讲你基础的，配置依赖：

### 1.1 在pom.xml里加入如下依赖：

```xml
<dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
</dependency>
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
</dependency>
```

### 1.2 当前springboot的版本信息如下：

```xml
<parent>
        <groupId>org.springframework.boot</groupId>
        <artifactId>spring-boot-starter-parent</artifactId>
        <version>1.5.4.RELEASE</version>
        <relativePath/> <!-- lookup parent from repository -->
</parent>
```

### 1.3 创建dto对象：

```Java
package com.example.demo.dto;
import javax.persistence.Entity;
import javax.persistence.GeneratedValue;
import javax.persistence.GenerationType;
import javax.persistence.Id;

@Entity
public class Customer {
    @Id
    @GeneratedValue(strategy=GenerationType.AUTO)
    private Long id;
    private String firstName;
    private String lastName;
    protected Customer() {}
    public Customer(String firstName, String lastName) {
        this.firstName = firstName;
        this.lastName = lastName;
    }
    @Override
    public String toString() {
        return String.format(
                "Customer[id=%d, firstName='%s', lastName='%s']",
                id, firstName, lastName);
    }
}
```

### 1.4 创建操作数据的Repository对象：

```java
package com.example.demo.repositories;
import com.example.demo.dto.Customer;
import org.springframework.data.jpa.repository.JpaRepository;
public interface CustomerRepository extends JpaRepository<Customer, Long> {
}
```

==对于JpaRepository需要知道：==

```java
public interface JpaRepository<T,ID extends Serializable>
extends PagingAndSortingRepository<T,ID>, QueryByExampleExecutor<T>

Methods inherited from interface org.springframework.data.repository.PagingAndSortingRepository
findAll
Methods inherited from interface org.springframework.data.repository.CrudRepository
count, delete, delete, delete, deleteAll, exists, findOne, save
Methods inherited from interface org.springframework.data.repository.query.QueryByExampleExecutor
count, exists, findAll, findOne
```

**从上面可以看出，CustomerRepository继承了JpaRepository之后就拥有了CrudRepository、QueryByExampleExecutor、PagingAndSortingRepository的基本能力了，包括基本的增删改查都有了。**

### 1.5 数据库配置

需要在application.properties加上如下参数：

```properties
spring.datasource.url=jdbc:mysql://localhost:3306/test
spring.datasource.username=root
spring.datasource.password=123456
spring.datasource.driver-class-name=com.mysql.jdbc.Driver
spring.jpa.properties.hibernate.hbm2ddl.auto=create
```

这里要注意到最后一行参数`spring.jpa.properties.hibernate.hbm2ddl.auto=create`，这个参数是为了方便处理数据库表，该属性的值一般有如下几个：

```Txt
validate               加载hibernate时，验证创建数据库表结构
create                  每次加载hibernate，重新创建数据库表结构，这就是导致数据库表数据丢失的原因。
create-drop        加载hibernate时创建，退出是删除表结构
update                 加载hibernate自动更新数据库结构
```

很多教程上写create-drop，就是每次启动都要重新创建表，如果表比较少还好，但是多了就是麻烦，还有初始化数据，我这里写的是create，只完成表的初始化创建即可，后面就不需要关注了。

## 2.SpringDataJPA继承的方法

上一节讲了[spring-data-jpa的基本配置](http://www.spring4all.com/article/459)，这一节主要是讲自定义Repository继承了JpaRepository相关的一些方法，包括基本的增删改查方法。

### 2.1、方法列表

1）、从PagingAndSortingRepository继承的findAll方法
2）、从CrudRepository继承的count, delete,deleteAll, exists, findOne, save等方法
3）、从QueryByExampleExecutor继承的count, exists, findAll, findOne**

### 2.2、demo示例

因为比较简单，这里只是简单写几个简单的方法进行测试，这里为了简单方便起见，我讲测试的方法放到Controller内——Customercontroller。

#### 1）save方法，初始化，保存Customer数据

```java
    @Controller
    @RequestMapping("/customer")
    public class CustomerController {
        @Autowired
        private CustomerRepository repository;

        /**
         * 初始化数据
         */
        @RequestMapping("/index")
        public void index() {
            // save a couple of customers
            repository.save(new Customer("Jack", "Bauer"));
            repository.save(new Customer("Chloe", "O'Brian"));
            repository.save(new Customer("Kim", "Bauer"));
            repository.save(new Customer("David", "Palmer"));
            repository.save(new Customer("Michelle", "Dessler"));
            repository.save(new Customer("Bauer", "Dessler"));
        }

```

这里调用了接口继承的save方法。

#### 2）findAll方法，查找出完成初始化的数据

```java
 /**
  * 查询所有
 */
@RequestMapping("/findAll")
public void findAll(){
    List&lt;Customer&gt; result = repository.findAll();
    for (Customer customer:result){
        System.out.println(customer.toString());
    }
    System.out.println("-------------------------------------------");
}
```

#### 3）findOne方法，根据long型Id为参数，返回对应的人

```java
/**
 * 查询ID为1的数据
 */
@RequestMapping("/findOne")
public void findOne(){
    Customer result = repository.findOne(1L);
    if(result!=null){
        System.out.println(result.toString());
    }
    System.out.println("-------------------------------------------");
}
```

#### 4）delete方法，根据Id删除指定数据，也可以用批量删除方法

```java
/**
 * 查询ID为1的数据
 */
@RequestMapping("/delete")
public void delete(){

    System.out.println("删除前数据：");
    List&lt;Customer&gt; customers = repository.findAll();
    for (Customer customer:customers){
        System.out.println(customer.toString());
    }

    System.out.println("删除ID=3数据：");
    repository.delete(3l);

    System.out.println("删除后数据：");
    customers = repository.findAll();
    for (Customer customer:customers){
        System.out.println(customer.toString());
    }
    System.out.println("-------------------------------------------");
}
```

#### 5)、其他还有很多，用法方法，大同小异，只是在功能上不完全重叠。

是不是感觉有JPA真心方便，不用谢mapper文件，不用各种配置，甚至不用定义基本的增删改查方法，写个类似DAO的类，继承一下JpaRepository或者CrudRepository，当程序运行的时候，会创建一个虚拟的实训类，然后执行相关逻辑。

这一节就到这里，下一节讲通过方法名来实现一些简单查询，有一定的局限性，但是可以学习一下这种操作，以备不时之需。

## 3.SpringDataJPA创建查询

通常，JPA的查询创建机制如查询方法所描述的那样工作。下面是一个关于JPA查询方法的简单示例:

### 3.1、在CustomerRepository接口中，有如下定义：

```
        /**
         * 根据lastName查询结果
         * @param lastName
         * @return
         */
        List<Customer> findByLastName(String lastName);
这是通过参数lastName去解析，并将查询结果返回，语义相当于`select * from customer where last_name=${lastName}`,在controller中就可以直接进行使用该方法了。
```

### 3.2、在CustomerController代码中使用CustomerRepository进行数据查询操作。如下：

```
        /**
         * 查询lastName为指定用户昵称
         */
        @RequestMapping("/findByLastName")
        public void findByLastName(){
            List&lt;Customer&gt; result = repository.findByLastName("Bauer");
            for (Customer customer:result){
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");
        }
```

### 3.3、创建查询的方法名命名指南

同时，查询创建还提供了一些基本的方法命名规则，如下：
![img](1.SpringDataJPA基本配置.assets/9a32bc484cda2b7d89371d909296b81d1515160.png)![img](1.SpringDataJPA基本配置.assets/a0af26b59b775a77cf5d5702ce0597a21515160.png)![img](1.SpringDataJPA基本配置.assets/99481fa01fabba86843fb2422b9e85771515160.png)

通过以上，可以通过简单的方法命名来实现查询，但是在使用的过程中，很多时候我们会不满足这个状态，因为可能好多的数据集按照这个来实现是极其费劲的。

接下来的篇幅将会讲JPA查询的重点内容，算是深入一点点，敬请期待吧。

## 4.预定义查询

前面讲了[Spring-data-JPA的基本配置](http://www.spring4all.com/article/459)、[继承的方法](http://www.spring4all.com/article/460)和[创建查询](http://www.spring4all.com/article/462)，都比较简单，本节讲预定义查询（NamedQueries）。

------

预定义查询有两种，一种是通过XML配置<named-query />或配置[@NamedQuery](https://github.com/NamedQuery)，另一种是通过XML配置<named-native-query />或配置[@NamedNativeQuery](https://github.com/NamedNativeQuery)，实现。对于配置XML的示例，这里就不做演示了，重点是通过Annotation实现的。

### 4.1、修改实体(Entity)

在[@Entity](https://github.com/Entity)下增加[@NamedQuery](https://github.com/NamedQuery)定义，需要注意，这里的sql表达式里的表名要和当前的Entity一致，否则会找不到，报错！！！查询参数也要和实体进行对应起来，是firstName而不是first_name,切记！！

当然，有人可能会问会不会有复杂的表查询，这里不做探究，因为实际场景中这种破坏Entity的侵入式很不美感，也不方便，有更优雅的方式可选择，后面会提到。

```
    package com.example.demo.dto;
    import org.hibernate.annotations.NamedQuery;

    import javax.persistence.Entity;
    import javax.persistence.GeneratedValue;
    import javax.persistence.GenerationType;
    import javax.persistence.Id;

    @Entity
    @NamedQuery(name="Customer.findByFirstName",query = "select c from Customer c where c.firstName = ?1")
    public class Customer {
        @Id
        @GeneratedValue(strategy=GenerationType.AUTO)
        private Long id;
        private String firstName;
        private String lastName;
        protected Customer() {}
        public Customer(String firstName, String lastName) {
            this.firstName = firstName;
            this.lastName = lastName;
        }
        @Override
        public String toString() {
            return String.format(
                    "Customer[id=%d, firstName='%s', lastName='%s']",
                    id, firstName, lastName);
        }
    }
```

### 4.2、修改CustomerRepository

```
增加方法：
Customer findByFirstName(String bauer);
```

### 4.3、修改CustomerController

```
增加方法：
        /**
         * 查询FirstName为指定用户昵称
         */
        @RequestMapping("/findByFirstName")
        public void findByFirstName(){
            Customer customer = repository.findByFirstName("Bauer");
            if(customer!=null){
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");
        }
```

**关于预定义查询（NamedQueries）的入门就到这里，如果想深入使用，需要深入的去摸索，挖坑填坑。下一节是使用[@Query](https://github.com/Query)注解，实现查询，相对而言会更灵活一些。**

## 5.使用[@Query](https://github.com/Query)注解

经过几天的折腾，终于到了学习一个重量级的查询方式上，使用[@Query](https://github.com/Query)注解，使用注解有两种方式，一种是JPQL的SQL语言方式，一种是原生SQL的语言，略有区别，后者我们更熟悉一些。话不多说，看代码。

1、在CustomerRepository里添加

```java
        /**
         * 模糊匹配
         * @param bauer
         * @return
         */
        @Query("select c from Customer c where c.firstName=?1")
        Customer findByFirstName2(String bauer);

        @Query("select c from Customer c where c.lastName=?1 order by c.id desc")
        List&lt;Customer&gt; findByLastName2(String lastName);

        /**
         * 一个参数，匹配两个字段
         * @param name2
         * @return
         * 这里Param的值和=:后面的参数匹配，但不需要和方法名对应的参数值对应
         */
        @Query("select c from Customer c where c.firstName=:name or c.lastName=:name  order by c.id desc")
        List&lt;Customer&gt; findByName(@Param("name") String name2);

        /**
         * 一个参数，匹配两个字段
         * @param name
         * @return
         * 这里的%只能放在占位的前面，后面不行
         */
        @Query("select c from Customer c where c.firstName like %?1")
        List&lt;Customer&gt; findByName2(@Param("name") String name);

        /**
         * 一个参数，匹配两个字段
         * @param name
         * @return
         * 开启nativeQuery=true，在value里可以用原生SQL语句完成查询
         */
        @Query(nativeQuery = true,value = "select * from Customer c where c.first_name like concat('%' ,?1,'%') ")
        List&lt;Customer&gt; findByName3(@Param("name") String name);
```

在CustomerController内添加

```java
        /**
         * @Query注解方式查询
         * 查询FirstName为指定字符串
         */
        @RequestMapping("/findByFirstName2")
        public void findByFirstName2(){
            Customer customer = repository.findByFirstName2("Bauer");
            if(customer!=null){
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");
        }

        /**
         * @Query注解方式查询
         * 查询LastName为指定字符串
         */
        @RequestMapping("/findByLastName2")
        public void findByLastName2(){
            List&lt;Customer&gt; result = repository.findByLastName2("Bauer");
            for (Customer customer:result){
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");
        }

        /**
         * @Query注解方式查询,
         * 用@Param指定参数，匹配firstName和lastName
         */
        @RequestMapping("/findByName")
        public void findByName(){
            List&lt;Customer&gt; result = repository.findByName("Bauer");
            for (Customer customer:result){
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");
        }

        /**
         * @Query注解方式查询,使用关键词like
         * 用@Param指定参数，firstName的结尾为e的字符串
         */
        @RequestMapping("/findByName2")
        public void findByName2(){
            List&lt;Customer&gt; result = repository.findByName2("e");
            for (Customer customer:result){
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");
        }

        /**
         * @Query注解方式查询，模糊匹配关键字e
         */
        @RequestMapping("/findByName3")
        public void findByName3(){
            List&lt;Customer&gt; result = repository.findByName3("e");
            for (Customer customer:result){
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");
        }
```

可能看了上面的代码有些疑惑，这里做一下解释：
`？加数字表示占位符，？1代表在方法参数里的第一个参数，区别于其他的index，这里从1开始`

```
=:加上变量名，这里是与方法参数中有@Param的值匹配的，而不是与实际参数匹配的
JPQL的语法中，表名的位置对应Entity的名称，字段对应Entity的属性,详细语法见相关文档
要使用原生SQL需要在@Query注解中设置nativeQuery=true，然后value变更为原生SQL即可
```

## 6.使用sort排序

通过[上一节](http://www.spring4all.com/article/464)的学习，我们知道了如何用[@Query](https://github.com/Query)注解来实现灵活的查询。在上一节的示例中，我也尝试给出简单的排序，通过JPQL语句以及原生SQL来实现的。这样的实现，虽然在一定程度上可以应用，但是灵活度不够，因此结合[@Query](https://github.com/Query)注解，我们可以使用Sort来对结果进行排序。**

6.1、在CustomerRepository内添加方法

```
        /**
         * 一个参数，匹配两个字段
         * @param name2
         * @param sort 指定排序的参数，可以根据需要进行调整
         * @return
         * 这里Param的值和=:后面的参数匹配，但不需要和方法名对应的参数值对应
         *
         */
        @Query("select c from Customer c where c.firstName=:name or c.lastName=:name")
        List&lt;Customer&gt; findByName4(@Param("name") String name2,Sort sort);
方法一如既往，是声明式的，只是在原有方法的基础上，加上Sort（`org.springframework.data.domain.Sort`）作为参数即可。
```

6.2、在CustomerController中测试

```
        /**
         * @Query注解方式查询,
         * 用@Param指定参数，匹配firstName和lastName
         */
        @RequestMapping("/findByName")
        public void findByName4(){
            //按照ID倒序排列
            System.out.println("直接创建sort对象，通过排序方法和属性名");
            Sort sort = new Sort(Sort.Direction.DESC,"id");
            List&lt;Customer&gt; result = repository.findByName4("Bauer",sort);
            for (Customer customer:result){
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");
            //按照ID倒序排列
            System.out.println("通过Sort.Order对象创建sort对象");
            Sort sortx = new Sort(new Sort.Order(Sort.Direction.DESC,"id"));
            List&lt;Customer&gt; resultx = repository.findByName4("Bauer",sort);
            for (Customer customer:result){
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");

            System.out.println("通过排序方法和属性List创建sort对象");
            List&lt;String&gt; sortProperties = new ArrayList&lt;&gt;();
            sortProperties.add("id");
            sortProperties.add("firstName");
            Sort sort2 = new Sort(Sort.Direction.DESC,sortProperties);
            List&lt;Customer&gt; result2 = repository.findByName4("Bauer",sort2);
            for (Customer customer:result2){
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");

            System.out.println("通过创建Sort.Order对象的集合创建sort对象");
            List&lt;Sort.Order&gt; orders = new ArrayList&lt;&gt;();
            orders.add(new Sort.Order(Sort.Direction.DESC,"id"));
            orders.add(new Sort.Order(Sort.Direction.ASC,"firstName"));
            List&lt;Customer&gt; result3 = repository.findByName4("Bauer",new Sort(orders));
            for (Customer customer:result3){
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");
        }
```

这里总共列举了四种排序方式：

1）直接创建Sort对象，适合对单一属性做排序

2）通过Sort.Order对象创建Sort对象，适合对单一属性做排序

3）通过属性的List集合创建Sort对象，适合对多个属性，采取同一种排序方式的排序

4）通过Sort.Order对象的List集合创建Sort对象，适合所有情况，比较容易设置排序方式

对应着我们的使用场景来进行选择创建Sort对象的方式。

**注意，这里并没有列举所有的Sort使用方式，还有忽略大小写，使用JpaSort.unsafe、聚合函数等进行排序，查询的属性值是Entity的属性名，不是数据库的字段，要注意到!!**

## 7.使用[@Modifying](https://github.com/Modifying)去做数据更新

通过之前的讲解和示例，我们掌握了基本的JPA使用方法，大多数是一些查询数据的方法，这一节我们学习通过[@Modifying](https://github.com/Modifying)去做数据更新的方法示例。

1、在CustomerRepository上增加新方法

```java
        /**
         * 根据lastName去更新firstName，返回结果是更改数据的行数
         * @param firstName
         * @param lastName
         * @return
         */
        @Modifying//更新查询
        @Transactional//开启事务
        @Query("update Customer c set c.firstName = ?1 where c.lastName = ?2")
        int setFixedFirstnameFor(String firstName, String lastName);
这里需要注意，在使用@Modifying注解的时候，一定要加上事务注解@Transactional，如果你忘了或者加错了，那很可能报如下错误：
    2017-07-01 01:03:24.844 ERROR 8072 --- [nio-8080-exec-1] o.a.c.c.C.[.[.[/].[dispatcherServlet]    : Servlet.service() for servlet [dispatcherServlet] in context with path [] threw exception [Request processing failed; nested exception is org.springframework.dao.InvalidDataAccessApiUsageException: Executing an update/delete query; nested exception is javax.persistence.TransactionRequiredException: Executing an update/delete query] with root cause
    javax.persistence.TransactionRequiredException: Executing an update/delete query at org.hibernate.jpa.spi.AbstractQueryImpl.executeUpdate(AbstractQueryImpl.java:54) ~[hibernate-entitymanager-5.0.12.Final.jar:5.0.12.Final]
如果你发现了上述错误就去加上事务吧。
```

2、在CustomerController中测试：

```java
        /**
         * 根据FirstName进行修改
         */
        @RequestMapping("/modifying")
        public void modifying(){
            Integer result = repository.setFixedFirstnameFor("Bauorx","Bauer");
            if(result!=null){
                System.out.println("modifying result:"+result);
            }
            System.out.println("-------------------------------------------");

        }
```

至此，关于[@Modifying](https://github.com/Modifying)的基本用法已经给出，希望对你有所帮助。

## 8.应用查询提示

这一节讲应用查询提示，学这一节比较波折，文档上的介绍太简单了，而且面对示例不知道如何下手，所以拖了一下，才有点头绪。

```
    public interface UserRepository extends Repository&lt;User, Long&gt; {

      @QueryHints(value = { @QueryHint(name = "name", value = "value")},
                  forCounting = false)
      Page&lt;User&gt; findByLastname(String lastname, Pageable pageable);
    }
对name和value的值我一直以为是自定义的，最后发现不完全是，是有一组定义好的值供我们选择。
```

1、QueryHints源码如下：

```java
    /*
     * Hibernate, Relational Persistence for Idiomatic Java
     *
     * License: GNU Lesser General Public License (LGPL), version 2.1 or later.
     * See the lgpl.txt file in the root directory or &lt;http://www.gnu.org/licenses/lgpl-2.1.html&gt;.
     */
    package org.hibernate.jpa;

    import java.util.HashSet;
    import java.util.Set;

    import static org.hibernate.annotations.QueryHints.CACHEABLE;
    import static org.hibernate.annotations.QueryHints.CACHE_MODE;
    import static org.hibernate.annotations.QueryHints.CACHE_REGION;
    import static org.hibernate.annotations.QueryHints.COMMENT;
    import static org.hibernate.annotations.QueryHints.FETCHGRAPH;
    import static org.hibernate.annotations.QueryHints.FETCH_SIZE;
    import static org.hibernate.annotations.QueryHints.FLUSH_MODE;
    import static org.hibernate.annotations.QueryHints.LOADGRAPH;
    import static org.hibernate.annotations.QueryHints.NATIVE_LOCKMODE;
    import static org.hibernate.annotations.QueryHints.READ_ONLY;
    import static org.hibernate.annotations.QueryHints.TIMEOUT_HIBERNATE;
    import static org.hibernate.annotations.QueryHints.TIMEOUT_JPA;

    /**
     * Defines the supported JPA query hints
     *
     * @author Steve Ebersole
     */
    public class QueryHints {
        /**
         * The hint key for specifying a query timeout per Hibernate O/RM, which defines the timeout in seconds.
         *
         * @deprecated use {@link #SPEC_HINT_TIMEOUT} instead
         */
        @Deprecated
        public static final String HINT_TIMEOUT = TIMEOUT_HIBERNATE;

        /**
         * The hint key for specifying a query timeout per JPA, which defines the timeout in milliseconds
         */
        public static final String SPEC_HINT_TIMEOUT = TIMEOUT_JPA;

        /**
         * The hint key for specifying a comment which is to be embedded into the SQL sent to the database.
         */
        public static final String HINT_COMMENT = COMMENT;

        /**
         * The hint key for specifying a JDBC fetch size, used when executing the resulting SQL.
         */
        public static final String HINT_FETCH_SIZE = FETCH_SIZE;

        /**
         * The hint key for specifying whether the query results should be cached for the next (cached) execution of the
         * "same query".
         */
        public static final String HINT_CACHEABLE = CACHEABLE;

        /**
         * The hint key for specifying the name of the cache region (within Hibernate's query result cache region)
         * to use for storing the query results.
         */
        public static final String HINT_CACHE_REGION = CACHE_REGION;

        /**
         * The hint key for specifying that objects loaded into the persistence context as a result of this query execution
         * should be associated with the persistence context as read-only.
         */
        public static final String HINT_READONLY = READ_ONLY;

        /**
         * The hint key for specifying the cache mode ({@link org.hibernate.CacheMode}) to be in effect for the
         * execution of the hinted query.
         */
        public static final String HINT_CACHE_MODE = CACHE_MODE;

        /**
         * The hint key for specifying the flush mode ({@link org.hibernate.FlushMode}) to be in effect for the
         * execution of the hinted query.
         */
        public static final String HINT_FLUSH_MODE = FLUSH_MODE;

        public static final String HINT_NATIVE_LOCKMODE = NATIVE_LOCKMODE;

        /**
         * Hint providing a "fetchgraph" EntityGraph.  Attributes explicitly specified as AttributeNodes are treated as
         * FetchType.EAGER (via join fetch or subsequent select).
         * 
         * Note: Currently, attributes that are not specified are treated as FetchType.LAZY or FetchType.EAGER depending
         * on the attribute's definition in metadata, rather than forcing FetchType.LAZY.
         */
        public static final String HINT_FETCHGRAPH = FETCHGRAPH;

        /**
         * Hint providing a "loadgraph" EntityGraph.  Attributes explicitly specified as AttributeNodes are treated as
         * FetchType.EAGER (via join fetch or subsequent select).  Attributes that are not specified are treated as
         * FetchType.LAZY or FetchType.EAGER depending on the attribute's definition in metadata
         */
        public static final String HINT_LOADGRAPH = LOADGRAPH;

        private static final Set&lt;String&gt; HINTS = buildHintsSet();

        private static Set&lt;String&gt; buildHintsSet() {
            HashSet&lt;String&gt; hints = new HashSet&lt;String&gt;();
            hints.add( HINT_TIMEOUT );
            hints.add( SPEC_HINT_TIMEOUT );
            hints.add( HINT_COMMENT );
            hints.add( HINT_FETCH_SIZE );
            hints.add( HINT_CACHE_REGION );
            hints.add( HINT_CACHEABLE );
            hints.add( HINT_READONLY );
            hints.add( HINT_CACHE_MODE );
            hints.add( HINT_FLUSH_MODE );
            hints.add( HINT_NATIVE_LOCKMODE );
            hints.add( HINT_FETCHGRAPH );
            hints.add( HINT_LOADGRAPH );
            return java.util.Collections.unmodifiableSet( hints );
        }

        public static Set&lt;String&gt; getDefinedHints() {
            return HINTS;
        }

        protected QueryHints() {
        }
    }
```

对应值的介绍，除了注释，我在网上也找到了一些资料，讲的很详细：
https://www.thoughts-on-java.org/11-jpa-hibernate-query-hints-every-developer-know
https://docs.jboss.org/hibernate/orm/4.3/javadocs/org/hibernate/jpa/QueryHints.html

2、在CustomerRepository中添加示例：

导入QueryHint的name的对应的值
`import static org.hibernate.jpa.QueryHints.HINT_COMMENT;`

```java
        /**
         * 一个参数，匹配两个字段
         * @param name2
         * @Param pageable 分页参数
         * @return
         * 这里Param的值和=:后面的参数匹配，但不需要和方法名对应的参数值对应
         * 这里增加了@QueryHints注解，是给查询添加一些额外的提示
         * 比如当前的name值为HINT_COMMENT是在查询的时候带上一些备注信息
         */
        @QueryHints(value = { @QueryHint(name = HINT_COMMENT, value = "a query for pageable")})
        @Query("select c from Customer c where c.firstName=:name or c.lastName=:name")
        Page&lt;Customer&gt; findByName(@Param("name") String name2,Pageable pageable);
```

3、在CustomerController中添加示例：

```java
        /**
         * 分页
         * 应用查询提示@QueryHints，这里是在查询的适合增加了一个comment
         * 查询结果是lastName和firstName都是bauer这个值的数据
         */
        @RequestMapping("/pageable")
        public void pageable(){
            //Pageable是接口，PageRequest是接口实现
            //PageRequest的对象构造函数有多个，page是页数，初始值是0，size是查询结果的条数，后两个参数参考Sort对象的构造方法
            Pageable pageable = new PageRequest(0,3, Sort.Direction.DESC,"id");
            Page&lt;Customer&gt; page = repository.findByName("bauer",pageable);
            //查询结果总行数
            System.out.println(page.getTotalElements());
            //按照当前分页大小，总页数
            System.out.println(page.getTotalPages());
            //按照当前页数、分页大小，查出的分页结果集合
            for (Customer customer: page.getContent()) {
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");
        }
```

注意到，这里除了方法调用了带有查询提示的方法以外，还对方法的Pageable参数进行了简单实现——PageRequest，这个类包含了多个构造函数，可以根据自己的需求自由定制，对于排序不分参考Sort那一篇。

## 9.Pageable分页功能

在JPA中提供了很方便的分页功能，那就是Pageable（org.springframework.data.domain.Pageable）以及它的实现类PageRequest（org.springframework.data.domain.PageRequest），详细的可以见示例代码：

**1、改变CustomerRepository方法**

```java
        /**
         * 一个参数，匹配两个字段
         * @param name2
         * @Param pageable 分页参数
         * @return
         * 这里Param的值和=:后面的参数匹配，但不需要和方法名对应的参数值对应
         * 这里增加了@QueryHints注解，是给查询添加一些额外的提示
         * 比如当前的name值为HINT_COMMENT是在查询的时候带上一些备注信息
         */
        @QueryHints(value = { @QueryHint(name = HINT_COMMENT, value = "a query for pageable")})
        @Query("select c from Customer c where c.firstName=:name or c.lastName=:name")
        Page&lt;Customer&gt; findByName(@Param("name") String name2,Pageable pageable);
```

**2、增加CustomerController方法pageable**

```java
        /**
         * 分页
         * 应用查询提示@QueryHints，这里是在查询的适合增加了一个comment
         * 查询结果是lastName和firstName都是bauer这个值的数据
         */
        @RequestMapping("/pageable")
        public void pageable(){
            //Pageable是接口，PageRequest是接口实现
            //PageRequest的对象构造函数有多个，page是页数，初始值是0，size是查询结果的条数，后两个参数参考Sort对象的构造方法
            Pageable pageable = new PageRequest(0,3, Sort.Direction.DESC,"id");
            Page&lt;Customer&gt; page = repository.findByName("bauer",pageable);
            //查询结果总行数
            System.out.println(page.getTotalElements());
            //按照当前分页大小，总页数
            System.out.println(page.getTotalPages());
            //按照当前页数、分页大小，查出的分页结果集合
            for (Customer customer: page.getContent()) {
                System.out.println(customer.toString());
            }
            System.out.println("-------------------------------------------");
        }
```

从示例代码的注释当中可以看到Page对象的相关参数及值的说明，更详细的用法，参考PageRequest源码。

## 10.投影（Projection的用法）

在JPA的查询中，有一个不方便的地方，[@Query](https://github.com/Query)注解，如果查询直接是`Select C from Customer c`,这时候，查询的返回对象就是Customer这个完整的对象，包含所有字段，对于我们的示例并没有什么问题，但是对于比较庞大的domain类，这个查询时就比较要命，并不是所有的字段都能用到，比较头疼。另外，如果定义`select c.firstName as firstName,c.lastName as lastName from Customer c`这个查询结果，返回的对象是Object类型，而且无法直接转换成Customer对象，这样用起来就不是很方便。
对于这种情况，JPA提供了一种声明方式来解决，即声明一个接口类，然后直接使用这个接口类接受返回的数据即可。下面奉上代码：

1、增加CustomerProjection接口类

```
    package com.example.demo.dto;
    import org.springframework.beans.factory.annotation.Value;
    /**
     * Created by Administrator on 2017/7/9 0009.
     */
    public interface CustomerProjection {
        @Value("#{target.firstName + ' ' + target.lastName}")
        String getFullName();

        String getFirstName();

        String getLastName();
    }
这里声明的方式是可以直接通过get+属性名，这是普通的，另外也可以通过@Value注解来实现指定字段，除了指定字段也可以做聚合展示，比如有些地方需要展示客户的全名，这里定义的getFullName()方法及注解@Value即完成这一操作。需要注意这里的@Value中的target表达式写法及拼接方法。
```

2、增加CustomerRepository方法

```
    @Query("SELECT c.firstName as firstName,c.lastName as lastName from Customer  c")
    Collection&lt;CustomerProjection&gt; findAllProjectedBy();
```

3、增加CustomerController方法

```
        /**
         * find by projections
         */
        @RequestMapping("/findAllProjections")
        public void findAllProjections(){
            Collection&lt;CustomerProjection&gt; projections = repository.findAllProjectedBy();
            System.out.println(projections);
            System.out.println(projections.size());
            for (CustomerProjection projection:projections){
                System.out.println("FullName:"+projection.getFullName());
                System.out.println("FirstName:"+projection.getFirstName());
                System.out.println("LastName:"+projection.getLastName());
            }
        }
```

这里只是做了简单示意，深入的内容需要自己去挖掘探索。不过关于Projection的资料比较少，我也是扒了不少资料才理解的差不多了，还需要多多实践。

另外spring-data-examples项目中有一些JPA的例子，可以用来学习，梳理思路。https://github.com/spring-projects/spring-data-examples/tree/master/jpa

## 11.数据更新

上次通过《Spring Data JPA系列：使用[@Modifying](https://github.com/Modifying)修改（Modifying queries）》介绍了数据更新的方式，这种更新方式会很不方便，写的时候也比较麻烦，可以为更新密码、更新用户名等一些特殊的更新单独定义，但是对大多数数据操作是不方便的，比如我要更新一条有一百个字段的数据，这时候如果要通过Modifying方式就非常的不方便，因此，我们需要一种新的方式来解救。
通过阅读Spring-Data-JPA相关的文档和博客，找到了对应的解决方案，就是使用`save()`方法，经过测试，可用。
我们平时对`save()`方法的理解，大多是等同于`insert()`，主要是指新增一条数据，而JPA的`save()`方法包含了`merge()`的概念，就是说，如果save的对象不存在primary key或者primary key值在database内不存在的时候会新添加一条数据，如果primary key 存在并且primary key已经在database中存在，那就会依据primary key对该条数据进行更新，这是我们乐意见到的。

参考的文章：https://stackoverflow.com/questions/11881479/how-do-i-update-an-entity-using-spring-data-jpa

相关描述如下：

Identity of entities is defined by their primary keys. Since firstname and lastname are not parts of the primary key, you cannot tell JPA to treat Users with the same firstnames and lastnames as equal if they have different userIds.

So, if you want to update a User identified by its firstname and lastname, you need to find that User by a query, and then change appropriate fields of the object your found. These changes will be flushed to the database automatically at the end of transaction, so that you don’t need to do anything to save these changes explicitly.

EDIT:

Perhaps I should elaborate on overall semantics of JPA. There are two main approaches to design of persistence APIs:

- insert/update approach. When you need to modify the database you should call methods of persistence API explicitly: you call insert to insert an object, or update to save new state of the object to the database.
- Unit of Work approach. In this case you have a set of objects managed by persistence library. All changes you make to these objects will be flushed to the database automatically at the end of Unit of Work (i.e. at the end of the current transaction in typical case). When you need to insert new record to the database, you make the corresponding object managed. Managed objects are identified by their primary keys, so that if you make an object with predefined primary key managed, it will be associated with the database record of the same id, and state of this object will be propagated to that record automatically.

JPA follows the later approach. save() in Spring Data JPA is backed by merge() in plain JPA, therefore it makes your entity managed as described above. It means that calling save() on an object with predefined id will update the corresponding database record rather than insert a new one, and also explains why save() is not called create().

## 12. 动态查询1

在JPA中，如果用[@Query](https://github.com/Query)查询有诸多不便，因此JPA提供了基于Criteria API的查询，比[@Query](https://github.com/Query)查询更灵活方便，这篇文章就简单说说Specification的简单使用。
这篇文章是参考:[Useing JPA Criteria API](https://jverhoelen.github.io/spring-data-queries-jpa-criteria-api) ，相关代码如下所示：

1、定义一个继承`JpaSpecificationExecutor`的接口：

```
    import com.example.demo.dto.Customer;
    import org.springframework.data.jpa.repository.JpaRepository;
    import org.springframework.data.jpa.repository.JpaSpecificationExecutor;

    /**
     * Created by Administrator on 2017/7/12 0012.
     */
    public interface CustomerSpecificationRepository extends JpaRepository&lt;Customer,Long&gt;,
        JpaSpecificationExecutor&lt;Customer&gt; {
    }
这里只是继承接口声明的方法，比如
    T findOne(Specification<T> spec);
    List<T> findAll(Specification<T> spec);
    Page<T> findAll(Specification<T> spec, Pageable pageable);
    List<T> findAll(Specification<T> spec, Sort sort);
    long count(Specification<T> spec);
包含了常用的查询单个对象，查询数据集合，查询分页数据集合，查询带排序参数的数据集合，查询数据的大小，这些都是常用的数据结果集，因此可以不用做其他定义即可直接使用。
```

2、创建SpecificationFactory工具类

```
    import org.springframework.data.jpa.domain.Specification;
    import javax.persistence.criteria.Path;
    /**
     * Created by Administrator on 2017/7/13 0013.
     */
    public final class SpecificationFactory {
        public static Specification containsLike(String attribute, String value) {
            return (root, query, cb) -&gt; cb.like(root.get(attribute), "%" + value + "%");
        }
        public static Specification isBetween(String attribute, int min, int max) {
            return (root, query, cb) -&gt; cb.between(root.get(attribute), min, max);
        }
        public static Specification isBetween(String attribute, double min, double max) {
            return (root, query, cb) -&gt; cb.between(root.get(attribute), min, max);
        }
        public static &lt;T extends Enum&lt;T&gt;&gt; Specification enumMatcher(String attribute, T queriedValue) {
            return (root, query, cb) -&gt; {
                Path&lt;T&gt; actualValue = root.get(attribute);
                if (queriedValue == null) {
                    return null;
                }
                return cb.equal(actualValue, queriedValue);
            };
        }
    }
这个工具类定义了一些基本的查询，包括模糊匹配（containsLike）,数值区间（isBetween）以及枚举类参数匹配（enumMatcher），这些也是在查询中常用的方法，在查询的时候，通过Specifications进行调用组织参数，即可获得接口Specification的实例，然后用于查询。
```

3、在CustomerController中添加测试方法

- 初始化

  ```
  @Autowired
  private CustomerSpecificationRepository csr;
  ```

- 单一条件查询

  ```
      @RequestMapping("/spec")
      public void specificationQuery(){
          Specification&lt;Customer&gt; spec = SpecificationFactory.containsLike("lastName","bau");
          Pageable pageable = new PageRequest(0,5, Sort.Direction.DESC,"id");
          Page&lt;Customer&gt; page = csr.findAll(spec,pageable);
          System.out.println(page);
          System.out.println(page.getTotalElements());
          System.out.println(page.getTotalPages());
          for (Customer c:page.getContent()){
              System.out.println(c.toString());
          }
      }
  ```

- 复合条件查询

  ```
      @RequestMapping("/spec2")
      public void specificationQuery2(){
          Specification&lt;Customer&gt; spec2 = Specifications
                  .where(SpecificationFactory.containsLike("firstName","bau"))
                  .or(SpecificationFactory.containsLike("lastName","bau"));
          Pageable pageable = new PageRequest(0,5, Sort.Direction.DESC,"id");
          Page&lt;Customer&gt; page = csr.findAll(spec2,pageable);
          System.out.println(page);
          System.out.println(page.getTotalElements());
          System.out.println(page.getTotalPages());
          for (Customer c:page.getContent()){
              System.out.println(c.toString());
          }
      }
  ```

  至此，使用Specification通过Criteria API进行查询，获得查询的结果集。相较于[@Query](https://github.com/Query)方式的查询定义，更人性化和方便操作，后面在对Criteria API的使用进一步深入，会继续给出相关实践，敬请期待。

## 13.动态查询2

写了一系列入门文章之后，博客也有了一些访问量，按照计划，对数据查询进行深入一些的探究，包括

- inner join查询
- 连接对象的属性值查询
- in条件查询
- left join查询
  还是入门级的示例，更深入的用法需要在实际场景中深化。

### 1、更改Customer类

增加[@OneToMany](https://github.com/OneToMany)注解的订单对象
需要注意的是，这次增加了Lombok依赖，一个简化对象类定义的插件，详见：https://projectlombok.org/

```
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;
    import org.hibernate.annotations.NamedQuery;
    import javax.persistence.*;
    import java.util.List;
    @Entity
    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    @NamedQuery(name="Customer.findByFirstName",query = "select c from Customer c where c.firstName = ?1")
    public class Customer {
        @Id
        @GeneratedValue(strategy=GenerationType.AUTO)
        private Long id;
        private String firstName;
        private String lastName;

        //一对多，一个客户对应多个订单，关联的字段是订单里的cId字段
        @OneToMany
        @JoinColumn(name = "cId")
        private List&lt;MyOrder&gt; myOrders;
        public Customer(String firstName, String lastName) {
            this.firstName = firstName;
            this.lastName = lastName;
        }
        @Override
        public String toString() {
            return String.format(
                    "Customer[id=%d, firstName='%s', lastName='%s']",
                    id, firstName, lastName);
        }
    }
```

### 2、增加MyOrder类

```
我的订单对象
    import lombok.AllArgsConstructor;
    import lombok.Data;
    import lombok.NoArgsConstructor;
    import javax.persistence.*;
    import java.math.BigDecimal;
    /**
     * Created by Administrator on 2017/7/17 0017.
     */
    @Entity
    @Data
    @AllArgsConstructor
    @NoArgsConstructor
    public class MyOrder {
        @Id
        @GeneratedValue(strategy = GenerationType.AUTO)
        private Long id;
        private String code;
        private Long cId;
        private BigDecimal total;
        //实体映射重复列必须设置：insertable = false,updatable = false
        @OneToOne
        @JoinColumn(name = "cId",insertable = false,updatable = false)
        private Customer customer;
        @Override
        public String toString() {
            return "MyOrder{" +
                    "id=" + id +
                    ", code='" + code + '\'' +
                    ", cId=" + cId +
                    ", total=" + total +
                    ", customer=" + customer +
                    '}';
        }
    }
```

### 3、新增MyOrderRepository类

```
    这里主要是继承JpaSpecificationExecutor接口，进行Specification查询

    import com.example.demo.dto.MyOrder;
    import org.springframework.data.jpa.repository.JpaSpecificationExecutor;
    import org.springframework.data.repository.CrudRepository;
    /**
     * Created by Administrator on 2017/7/17 0017.
     */
    public interface MyOrderRepository extends JpaSpecificationExecutor&lt;MyOrder&gt;,CrudRepository&lt;MyOrder,Long&gt; {
    }
```

### 4、新增ShoppingController类

```
    import com.example.demo.dto.Customer;
    import com.example.demo.dto.MyOrder;
    import com.example.demo.repositories.MyOrderRepository;
    import org.springframework.beans.factory.annotation.Autowired;
    import org.springframework.data.domain.Page;
    import org.springframework.data.domain.PageRequest;
    import org.springframework.data.domain.Pageable;
    import org.springframework.data.domain.Sort;
    import org.springframework.data.jpa.domain.Specification;
    import org.springframework.stereotype.Controller;
    import org.springframework.web.bind.annotation.RequestMapping;
    import javax.persistence.criteria.*;
    @Controller
    @RequestMapping("/shop")
    public class ShoppingController {
        @Autowired
        private MyOrderRepository myOrderRepository;
        /**
         * 内连接查询
         */
        @RequestMapping("/q1")
        public void specification1(){
            //根据查询结果，声明返回值对象，这里要查询用户的订单列表，所以声明返回对象为MyOrder
            Specification&lt;MyOrder&gt; spec = new Specification&lt;MyOrder&gt;() {
                //Root&lt;X&gt;  根查询，默认与声明相同
                @Override
                public Predicate toPredicate(Root&lt;MyOrder&gt; root, CriteriaQuery&lt;?&gt; query, CriteriaBuilder cb) {
                    //声明并创建MyOrder的CriteriaQuery对象
                    CriteriaQuery&lt;MyOrder&gt; q1 = cb.createQuery(MyOrder.class);
                    //连接的时候，要以声明的根查询对象（这里是root，也可以自己创建）进行join
                    //Join&lt;Z,X&gt;是Join生成的对象，这里的Z是被连接的对象，X是目标对象，
                    //  连接的属性字段是被连接的对象在目标对象的属性，这里是我们在MyOrder内声明的customer
                    //join的第二个参数是可选的，默认是JoinType.INNER(内连接 inner join)，也可以是JoinType.LEFT（左外连接 left join）
                    Join&lt;Customer,MyOrder&gt; myOrderJoin = root.join("customer",JoinType.INNER);
                    //用CriteriaQuery对象拼接查询条件，这里只增加了一个查询条件，cId=1
                    q1.select(myOrderJoin).where(cb.equal(root.get("cId"),1));
                    //通过getRestriction获得Predicate对象
                    Predicate p1= q1.getRestriction();
                    //返回对象
                    return p1;
                }
            };
            resultPrint(spec);
        }
        /**
         * 增加查询条件，关联的对象Customer的对象值
         */
        @RequestMapping("/q2")
        public void specification2(){
            Specification&lt;MyOrder&gt; spec = new Specification&lt;MyOrder&gt;() {
                @Override
                public Predicate toPredicate(Root&lt;MyOrder&gt; root, CriteriaQuery&lt;?&gt; query, CriteriaBuilder cb) {
                    CriteriaQuery&lt;MyOrder&gt; q1 = cb.createQuery(MyOrder.class);
                    Join&lt;Customer,MyOrder&gt; myOrderJoin = root.join("customer");
                    q1.select(myOrderJoin)
                            .where(
                                    cb.equal(root.get("cId"),1),//cId=1
                                    cb.equal(root.get("customer").get("firstName"),"Jack")//对象customer的firstName=Jack
                            );
                    Predicate p1= q1.getRestriction();
                    return p1;
                }
            };
            resultPrint(spec);
        }
        /**
         * in的条件查询
         * 需要将对应的结果集以root.get("attributeName").in(Object.. values)的方式传入
         * values支持多个参数，支持对象（Object），表达式Expression&lt;?&gt;，集合Collection以及Expression&lt;Collection&lt;?&gt;&gt;
         */
        @RequestMapping("/q3")
        public void specification3(){
            Specification&lt;MyOrder&gt; spec = new Specification&lt;MyOrder&gt;() {
                @Override
                public Predicate toPredicate(Root&lt;MyOrder&gt; root, CriteriaQuery&lt;?&gt; query, CriteriaBuilder cb) {
                    CriteriaQuery&lt;MyOrder&gt; q1 = cb.createQuery(MyOrder.class);
                    Join&lt;Customer,MyOrder&gt; myOrderJoin = root.join("customer");
                    q1.select(myOrderJoin)
                            .where(
                                    cb.equal(root.get("cId"),1)
                                    ,root.get("id").in(1,2,4)
                            );

                    Predicate p1= q1.getRestriction();
                    return p1;
                }
            };
            resultPrint(spec);
        }
        /**
         * 左外链接查询，对比inner join，
         * 这里只是改了一个参数，将JoinType.INNER改成JoinType.LEFT
         *
         * 注意，当前示例不支持JoinType.RIGHT，用的比较少，没有探究
         */
        @RequestMapping("/q4")
        public void specification4(){
            Specification&lt;MyOrder&gt; spec = new Specification&lt;MyOrder&gt;() {
                @Override
                public Predicate toPredicate(Root&lt;MyOrder&gt; root, CriteriaQuery&lt;?&gt; query, CriteriaBuilder cb) {
                    CriteriaQuery&lt;MyOrder&gt; q1 = cb.createQuery(MyOrder.class);
                    Join&lt;Customer,MyOrder&gt; myOrderJoin = root.join("customer",JoinType.LEFT);
                    q1.select(myOrderJoin).where(cb.equal(root.get("cId"),1));
                    Predicate p1= q1.getRestriction();
                    return p1;
                }
            };
            resultPrint(spec);
        }
        /***
        *输出分页信息
        **/
        private void resultPrint(Specification&lt;MyOrder&gt; spec) {
            //分页查询
            Pageable pageable = new PageRequest(0,10, Sort.Direction.DESC,"id");
            //查询的分页结果
            Page&lt;MyOrder&gt; page =myOrderRepository.findAll(spec,pageable);
            System.out.println(page);
            System.out.println(page.getTotalElements());
            System.out.println(page.getTotalPages());
            for (MyOrder c:page.getContent()){
                System.out.println(c.toString());
            }
        }

    }
内容已经写进注释了，请读源码，有问题请留言。
```

### 5、测试

#### 1）、内连接查询及结果：

- URL:http://localhost:8080/shop/q1

- 结果：

  ```java
  Hibernate: select myorder0_.id as id1_1_, myorder0_.c_id as c_id2_1_, myorder0_.code as code3_1_, myorder0_.total as total4_1_ from my_order myorder0_ inner join customer customer1_ on myorder0_.c_id=customer1_.id where myorder0_.c_id=1 order by myorder0_.id desc limit ?
  Hibernate: select customer0_.id as id1_0_0_, customer0_.first_name as first_na2_0_0_, customer0_.last_name as last_nam3_0_0_ from customer customer0_ where customer0_.id=?
  Page 1 of 1 containing com.example.demo.dto.MyOrder instances
  5
  1
  MyOrder{id=5, code='123455', cId=1, total=55.23, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=4, code='123459', cId=1, total=9.99, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=3, code='123458', cId=1, total=11.90, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=2, code='123457', cId=1, total=20.90, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=1, code='123456', cId=1, total=11.10, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  ```

#### 2）、关联对象条件查询及结果：

- URL:http://localhost:8080/shop/q2

- 结果：

  ```java
  Hibernate: select myorder0_.id as id1_1_, myorder0_.c_id as c_id2_1_, myorder0_.code as code3_1_, myorder0_.total as total4_1_ from my_order myorder0_ inner join customer customer1_ on myorder0_.c_id=customer1_.id where myorder0_.c_id=1 and customer1_.first_name=? order by myorder0_.id desc limit ?
  Hibernate: select customer0_.id as id1_0_0_, customer0_.first_name as first_na2_0_0_, customer0_.last_name as last_nam3_0_0_ from customer customer0_ where customer0_.id=?
  Page 1 of 1 containing com.example.demo.dto.MyOrder instances
  5
  1
  MyOrder{id=5, code='123455', cId=1, total=55.23, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=4, code='123459', cId=1, total=9.99, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=3, code='123458', cId=1, total=11.90, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=2, code='123457', cId=1, total=20.90, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=1, code='123456', cId=1, total=11.10, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  ```

  #### 3）、in条件查询测试及结果：

- URL:http://localhost:8080/shop/q3

- 结果：

  ```java
  Hibernate: select myorder0_.id as id1_1_, myorder0_.c_id as c_id2_1_, myorder0_.code as code3_1_, myorder0_.total as total4_1_ from my_order myorder0_ inner join customer customer1_ on myorder0_.c_id=customer1_.id where myorder0_.c_id=1 and (myorder0_.id in (1 , 2 , 4)) order by myorder0_.id desc limit ?
  Hibernate: select customer0_.id as id1_0_0_, customer0_.first_name as first_na2_0_0_, customer0_.last_name as last_nam3_0_0_ from customer customer0_ where customer0_.id=?
  Page 1 of 1 containing com.example.demo.dto.MyOrder instances
  3
  1
  MyOrder{id=4, code='123459', cId=1, total=9.99, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=2, code='123457', cId=1, total=20.90, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=1, code='123456', cId=1, total=11.10, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  ```

#### 4）、左外连接测试及结果：

- URL:http://localhost:8080/shop/q4

- 结果：

  ```java
  Hibernate: select myorder0_.id as id1_1_, myorder0_.c_id as c_id2_1_, myorder0_.code as code3_1_, myorder0_.total as total4_1_ from my_order myorder0_ left outer join customer customer1_ on myorder0_.c_id=customer1_.id where myorder0_.c_id=1 order by myorder0_.id desc limit ?
  Hibernate: select customer0_.id as id1_0_0_, customer0_.first_name as first_na2_0_0_, customer0_.last_name as last_nam3_0_0_ from customer customer0_ where customer0_.id=?
  Page 1 of 1 containing com.example.demo.dto.MyOrder instances
  5
  1
  MyOrder{id=5, code='123455', cId=1, total=55.23, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=4, code='123459', cId=1, total=9.99, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=3, code='123458', cId=1, total=11.90, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=2, code='123457', cId=1, total=20.90, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  MyOrder{id=1, code='123456', cId=1, total=11.10, customer=Customer[id=1, firstName='Jack', lastName='Bauer']}
  ```