# Spring data jpa

#### 描述

学习 spring data jpa

#### 版本

1. spring boot 2.1.5
2. springfox-swagger2 2.4.0

#### 介绍

1. spring data jpa 简单的增删改查
2. spring data jpa 分页、条件查询
3. spring data jpa 一对多，多对多

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200811203213824.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjkxOTQ1,size_16,color_FFFFFF,t_70)

## 配置文件

### maven 的配置

```
  <dependencies>
        <!-- 加密解密-->
        <dependency>
            <groupId>commons-codec</groupId>
            <artifactId>commons-codec</artifactId>
            <version>1.13</version>
        </dependency>

        <!--junit 插件-->
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>

        <!--swagger 插件-->
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger2</artifactId>
            <version>2.4.0</version>
        </dependency>
        <dependency>
            <groupId>io.springfox</groupId>
            <artifactId>springfox-swagger-ui</artifactId>
            <version>2.4.0</version>
        </dependency>
        <!--spring web -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>
        <!--spring data jpa -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-data-jpa</artifactId>
        </dependency>
        <!--mysql  -->
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <!-- lombok -->
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <!-- devtools -->
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-devtools</artifactId>
            <optional>true</optional>
        </dependency>
        <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-test</artifactId>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>org.junit.jupiter</groupId>
            <artifactId>junit-jupiter</artifactId>
            <version>RELEASE</version>
            <scope>compile</scope>
        </dependency>
    </dependencies>
```

### spring boot 的配置文件

```
server:
  port: 8001

spring:
  datasource:
    url: jdbc:mysql:// ip /jpa?useUnicode=true&characterEncoding=utf-8&zeroDateTimeBehavior=convertToNull
    username: root
    password: 123456
    driver-class-name: com.mysql.cj.jdbc.Driver
  jpa:
    # 打印sql
    show-sql: true
    properties:
      hibernate:
        # hibernate 的配置格式化 sql
        format_sql: true
        # 打印 sql 语句
        show_sql: true
        # 懒加载
        enable_lazy_load_no_trans: true
        # 时间设置
        jdbc:
          time-zone: GMT+8
    # 对数据库表的配置
    # create 启动时删数据库中的表，然后创建，退出时不删除数据表
    # create-drop 启动时删数据库中的表，然后创建，退出时删除数据表 如果表不存在报错
    # update 如果启动时表格式不一致则更新表，原有数据保留
    # validate 项目启动表结构进行校验 如果不一致则报错
    hibernate:
      ddl-auto: none
  # 允许重复的 bean 覆盖
  main:
    allow-bean-definition-overriding: true

  # jack son 配置
  jackson:
    date-format: yyyy-MM-dd HH:mm:ss
    time-zone: GMT+8
```

### swagger 的配置

```
@Configuration
@Component
public class SwaggerConfig {

    @Bean
    public Docket api() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(this.apiInfo())
                .select()
                .apis(RequestHandlerSelectors.basePackage("cn.dianhun.om.jpa"))
                .paths(PathSelectors.any())
                .build();
    }


    private ApiInfo apiInfo() {
        @SuppressWarnings("deprecation")
        ApiInfo info = new ApiInfo(
                "Test Spring data jpa  Swagger",
                "study spring data jpa",
                "1.0",
                "study spring data jpa",
                "study spring data jpa",
                "Test Spring data jpa  Swagger",
                "Test Spring data jpa  Swagger");
        return info;
    }
}
```

## 项目结构

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200811202521619.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjkxOTQ1,size_16,color_FFFFFF,t_70)

## 普通 crud

**spring data jpa 是基于hiberate 的一个 全自动的orm框架， 他能够实现不用编写sql就对数据库实现基本的增删改功能, 其根本的缘由是jpa 为我们提供了很多的 Repository 我们在实现具体的业务时候不需要考虑底层怎么实现，是需要编写我们自己的数据库服务类直接继承 Repository 即可**

> Repository

要想使用spring data jpa 做出我们的查询。 首先需要 编写我们的 PersonRepository 来继承 JpaRepository ， 注意 JpaRepository 需要添加两个泛型的参数， 一个是绑定的实体类， 一个是实体类对应的主键， 这里的实体类就是一个和数据库表对应的类， 它通过注解来和数据库表做到绑定！ JpaRepository 里面封装了很多常用的方法。

```
public interface PersonRepository extends JpaRepository<PersonEntity, Integer> {
    /**
     * @param specification
     * @param page
     * @return
     */
    Page<PersonEntity> findAll(Specification<PersonEntity> specification, Pageable page);
}
```

> PersonEntity

- @Entity 这个注解就代表的是与我们数据库表所绑定的实体类
- @Table(name = "person", schema = "op_upms", catalog = "") 这个注解代表了和数据库中的那个表做到的绑定，
- @Id 数据库中的主键
- @GeneratedValue(strategy = GenerationType.IDENTITY) 主键的生成策略
- @Column(name = "id") 属性和表字段的映射

```
@Entity
@Table(name = "person", schema = "op_upms", catalog = "")
public class PersonEntity {
    @Id
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Column(name = "id")
    private int id;
    private String name;
    private String address;

    @Override
    public boolean equals(Object o) {
        if (this == o) {
            return true;
        }
        if (o == null || getClass() != o.getClass()) {
            return false;
        }
        PersonEntity that = (PersonEntity) o;
        return id == that.id &&
                Objects.equals(name, that.name) &&
                Objects.equals(address, that.address);

    }
    @Override
    public int hashCode() {
        return Objects.hash(id, name, address);
    }
}
```

> CRUD

1插入数据库

```
    @Autowired
    private PersonRepository personRepository;

    @PostMapping("/insert")
    public void insert() {
        for (int i = 0; i < 30; i++) {
            PersonEntity person = new PersonEntity();
            person.setName("name" + UUID.randomUUID().toString().substring(0, 2)).setId(i).setAddress("address" + UUID.randomUUID().toString().substring(0, 2));
            personRepository.save(person);
        }
    }
```

2 查询数据库

```
    /**
     * 安装 id 查询
     * @param id
     * @return
     */
    @GetMapping("/getById/{id}")
    public ResultVO getById(@PathVariable("id") Integer id) {
        Optional<PersonEntity> byId = personRepository.findById(id);
        PersonEntity entity = byId.get();
        return ResultVOUtil.successWith(entity);
    }
```

3删除

```
    /**
     * 删除
     *
     * @param id
     * @return
     */
    @DeleteMapping("/delete/{id}")
    public ResultVO delete(@PathVariable("id") Integer id) {
        personRepository.deleteById(id);
        return ResultVOUtil.success();
    }
```

4更新

```
    /**
     * 更新
     * @param personEntity
     * @return
     */
    @PostMapping("/update")
    public ResultVO update(@RequestBody PersonEntity personEntity) {
        Optional<PersonEntity> byId = personRepository.findById(personEntity.getId());
        BeanUtils.copyProperties(personEntity, byId.get());
        personRepository.save(byId.get());
        return ResultVOUtil.success();
    }
```

## 条件查询、分页查询

> 分页查询

```
    /**
     * 分页查询
     * @param page
     * @param size
     * @return
     */
    @GetMapping("/query/{page}/{size}")
    public ResultVO query(@PathVariable("page") Integer page, @PathVariable("size") Integer size) {
        Page<PersonEntity> repositoryAll = personRepository.findAll(getPage(page, size));
        return ResultVOUtil.successWith(repositoryAll);
    }
    static Pageable getPage(Integer page, Integer size) {
        Pageable of = PageRequest.of(
                page - 1, size,
                Sort.by(Sort.Direction.ASC, "id")
        );
        return of;
    }
```

> 带有条件的分页

```
    @PostMapping("/queryVo/{page}/{size}")
    public ResultVO queryVo(@PathVariable("page") Integer page, @PathVariable("size") Integer size, @RequestBody PersonEntity person) {
        log.info("search info:  " + person.toString());

        Specification<PersonEntity> specification = (Specification<PersonEntity>) (root, query, builder) -> {
            List<Predicate> predicateList = new ArrayList<>();
            if (!StringUtils.isEmpty(person.getName())) {
                predicateList.add(builder.like(root.get("name"),
                        "%" + StringUtils.trimWhitespace(person.getName()) + "%"));
            }
            if (!StringUtils.isEmpty(person.getAddress())) {
                predicateList.add(builder.like(root.get("address"),
                        "%" + StringUtils.trimWhitespace(person.getAddress()) + "%"));
            }
            return builder.and(predicateList.toArray(new Predicate[0]));
        };

        Page<PersonEntity> repositoryAll = personRepository.findAll(specification, getPage(page, size));
        return ResultVOUtil.successWith(repositoryAll);
    }
```

## Repository使用的规范

jpa 回根据我们的方法名来解析成相应的sql ,在实际的解析过程中会遵循一定的规范, 只用按照这样的规范才能够发货出jpa动态Sql的功能。

- findBy
- findAll
- And ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200811210108902.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjkxOTQ1,size_16,color_FFFFFF,t_70)

## 一对多

> 数据库中的外键设计

一对多的例子中，模拟的关系为： 一个老师可以拥有多个学生、一个学生只有一个老师。主外键关系： 学生表中包含外键 老师id， 老师id在老师表中是主键。 ![在这里插入图片描述](https://img-blog.csdnimg.cn/20200812194849937.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L3FxXzQxMjkxOTQ1,size_16,color_FFFFFF,t_70#pic_center)

> 实体类中配置主外键关系

学生的实体中应该是包含一个老师的， 并且添加上 **@ManyToOne** 表面一个多对一的关系， 然后在通过 **@JoinColumn(name = "teacher_id", referencedColumnName = "teacher_id")** 注解

- name 代表的是外键的名字
- referencedColumnName 代表的是参看栏目的名字

```
    @Id
    @Column(name = "stu_id")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int stuId;
    private String stuName;
    private String stuClass;

    @JoinColumn(name = "teacher_id", referencedColumnName = "teacher_id")
    @ManyToOne
    private TeacherEntity teacherEntity;
```

老师实体中, 一个老师拥有多个学生， 这就要在实体类中配置一个学生的集合， 并且通过添加注解的方式来实现标识

- @JoinColumn(name = "teacher_id", referencedColumnName = "teacher_id")
- @OneToMany(targetEntity = StuEntity.class)

targetEntity 代表的是一个目标实体。

```
    @Id
    @Column(name = "teacher_id")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private int teacherId;
    private String teacherName;
    private String teacherCourse;

    /**
     * 一个老师对应多个学生，
     * 一对多关系， 此时老师的主键是学生的外键
     * 1、配置一对多关系
     * 2、配置外键
     */
/*    @JoinColumn(name = "teacher_id", referencedColumnName = "teacher_id")
    @OneToMany(targetEntity = StuEntity.class)
    private Set<StuEntity> stuEntities = new HashSet<>();*/
```

> 测试

主要的测试就是新建实体类， 然后为实体类添加上关系， 最后开始保存。 可以看到我们在保存学生的时候会直接保存上老师的id。

```
    @ApiOperation("测试一对多")
    @PostMapping("/test")
    public R testOneToMany() {

        StuEntity stuEntity = new StuEntity();
        stuEntity.setStuName("小崔");
        stuEntity.setStuClass("一班");

        StuEntity stuEntity2 = new StuEntity();
        stuEntity2.setStuName("小崔");
        stuEntity2.setStuClass("二班");

        TeacherEntity teacherEntity = new TeacherEntity();
        teacherEntity.setTeacherName("王老师");
        teacherEntity.setTeacherCourse("高数");

        // 设置关系
        // teacherEntity.getStuEntities().add(stuEntity);

        // 设置关系
        stuEntity.setTeacherEntity(teacherEntity);
        stuEntity2.setTeacherEntity(teacherEntity);

        teacherRepository.save(teacherEntity);
        stuRepository.save(stuEntity);
        stuRepository.save(stuEntity2);
        return R.ok();
    }
```

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200812200631537.png#pic_center)

![在这里插入图片描述](https://img-blog.csdnimg.cn/20200812200619349.png#pic_center)

测试删除老师, 回自动删除下面的所有学生

```
    @ApiOperation("级联删除")
    @PostMapping("/test4")
    public R testOneToMany4() {
        // 1、查询数据
        Optional<TeacherEntity> byId = teacherRepository.findById(15);
        TeacherEntity teacherEntity = byId.get();
        //2 、删除数据
        teacherRepository.delete(teacherEntity);
        return R.ok();
    }
```

观察打印出来的sql

```
    select
        stuentitie0_.teacher_id as teacher_4_0_0_,
        stuentitie0_.stu_id as stu_id1_0_0_,
        stuentitie0_.stu_id as stu_id1_0_1_,
        stuentitie0_.stu_class as stu_clas2_0_1_,
        stuentitie0_.stu_name as stu_name3_0_1_,
        stuentitie0_.teacher_id as teacher_4_0_1_ 
    from
        stu stuentitie0_ 
    where
        stuentitie0_.teacher_id=?
```

> 补充

对于一对多， 多对一的操作， 在实体类关系的设置上:无论是在一的一放设置、还是在多的一方都不会妨碍最后的结果。 并且还可以设置级联操作，举例： 删除老师， 就会把老师下面的所有学生给全部删除。 更多的配置：

```
    /**
     * 一个老师对应多个学生，
     * 一对多关系， 此时老师的主键是学生的外键
     * 1、配置一对多关系
     * 2、配置外键
     */
/*    @JoinColumn(name = "teacher_id", referencedColumnName = "teacher_id")
    @OneToMany(targetEntity = StuEntity.class)
    private Set<StuEntity> stuEntities = new HashSet<>();*/
    /*
    放弃外键维护
    mappedBy： 对方的属性名称
     */
    //配置级联操作
    @OneToMany(mappedBy = "teacherEntity", cascade = CascadeType.ALL)
    // @OneToMany(mappedBy = "teacherEntity")
    private Set<StuEntity> stuEntities = new HashSet<>();
```

## 多对多

> 实体类设置

在多对多的测试中， 考虑的是一个用户拥有多个角色、一个角色拥有多个用户，并且做了一个用户

角色

```
    @Id
    @Column(name = "role_id")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long roleId;
    private String roleName;
    private String remark;
    private Long createUserId;
    private Timestamp createTime;


    /**
     * 中间表名
     * <p>
     * 当前对象在中间表的外键
     * 对方在中间表的外键
     */
     @JoinTable(name = "sys_user_role",
            joinColumns = {@JoinColumn(name = "role_id", referencedColumnName = "role_id")},
            inverseJoinColumns = {@JoinColumn(name = "user_id", referencedColumnName = "user_id")})
     // @ManyToMany(targetEntity = SysUserEntity.class, cascade = CascadeType.ALL)
     @ManyToMany(mappedBy = "roleEntities", cascade = CascadeType.ALL)
    private Set<SysUserEntity> userEntitySet = new HashSet<>();
```

用户

```
    @Id
    @Column(name = "user_id")
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    private long userId;
    private String username;
    private String password;
    private String salt;
    private String email;
    private String mobile;
    private Byte status;
    private Long createUserId;

    // 自动填充时间
    @CreatedDate
    private Timestamp createTime;

    /**
     * 中间表名
     * <p>
     * 当前对象在中间表的外键
     * 对方在中间表的外键
     */
    @JoinTable(name = "sys_user_role",
            joinColumns = {@JoinColumn(name = "user_id", referencedColumnName = "user_id")},
            inverseJoinColumns = {@JoinColumn(name = "role_id", referencedColumnName = "role_id")})
    @ManyToMany(targetEntity = SysRoleEntity.class, cascade = CascadeType.ALL)
    private Set<SysRoleEntity> roleEntities = new HashSet<>();
```

中间表

```
    @GeneratedValue(strategy = GenerationType.IDENTITY)
    @Id
    @Column(name = "id")
    private long id;
    private Long roleId;
    private Long userId;
```

> 测试

```
    @GetMapping("/manyTomany")
    public R manyTomany() {
        SysUserEntity userEntity = new SysUserEntity();
        userEntity.setUsername("测试2").setEmail("1721310909@qq.com");

        SysUserEntity userEntity2 = new SysUserEntity();
        userEntity2.setUsername("测试3").setEmail("1721310909@qq.com");


        SysRoleEntity roleEntity2 = new SysRoleEntity();
        roleEntity2.setRoleName("管理1").setRemark("测试");

        SysRoleEntity roleEntity = new SysRoleEntity();
        roleEntity.setRoleName("管理2").setRemark("测试");
        // 设置关系
        userEntity.getRoleEntities().add(roleEntity);
        userEntity.getRoleEntities().add(roleEntity2);

        roleEntity.getUserEntitySet().add(userEntity2);
        roleEntity.getUserEntitySet().add(userEntity);

        userRepository.save(userEntity);
        roleRepository.save(roleEntity);

        return R.ok();
    }
```

测试级联操作

```
    @ApiOperation("级联删除操作")
    @GetMapping("/manyTomany4")
    public R manyTomany4() {
        //1、 查出 角色
        SysRoleEntity one = roleRepository.getOne(9L);
        //2、 删除角色
        roleRepository.delete(one);
        return R.ok();
    }
```

## 常用的注解

### 标注在类上

> @DynamicInsert

再插入数据的时候动态生成的sql中只包含要插入字段的部分， 其他的内容不会一起产生。

> @DynamicUpdate

再更新数据的时候动态生成的sql中只包含要更新字段的部分， 其他的内容不会一起产生。

### 标注在字段上面

> Column

```
  /**
数据库表上的字段名字
     */
    String name() default "";

    /**
 标明这个是唯一标识
     */
    boolean unique() default false;

    /**
 标明这个是可以为空的
     */
    boolean nullable() default true;

    /**
 在动态的插入sql中会产生个片段
     */
    boolean insertable() default true;

    /**
 在动态的更新sql中会产生个片段
     */
    boolean updatable() default true;

    /**
     * (Optional) The SQL fragment that is used when 
     * generating the DDL for the column.
 创建表的时候会创建这个字段的sql
     */
    String columnDefinition() default "";

    /**
 
 包含列的表的名称。
     */
    String table() default "";

    /**
 这个字段的长度
     */
    int length() default 255;

    /**
 字段的精度
     */
    int precision() default 0;
 
```

### 标注在方法上面

> @PrePersist

再插入实体之前可以做的一个回调事件， 比如可以添加创建时间、指定字段的默认值等。

> @PreUpdate

再插入更新之前可以做的一个回调事件， 比如： 可以实现修改时间。

```
    @PrePersist
    public void prePersist() {
        this.createdAt = this.updatedAt = new Timestamp(System.currentTimeMillis());
    }

    @PreUpdate
    public void preUpdate() {
        this.updatedAt = new Timestamp(System.currentTimeMillis());
    }
```

**@PrePersist- 在新实体持久化之前（添加到EntityManager） @PostPersist- 在数据库中存储新实体（在commit或期间flush） @PostLoad - 从数据库中检索实体后。 @PreUpdate- 当一个实体被识别为被修改时EntityManager @PostUpdate- 更新数据库中的实体（在commit或期间flush） @PreRemove - 在EntityManager中标记要删除的实体时 @PostRemove- 从数据库中删除实体（在commit或期间flush）**