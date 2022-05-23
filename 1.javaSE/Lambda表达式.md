# Lambda表达式

## 简介

- Lambda 表达式，也可称为闭包，它是推动 Java 8 发布的最重要新特性。
- Lambda 允许把函数作为一个方法的参数（函数作为参数传递进方法中）。
- 使用 Lambda 表达式可以使代码变的更加简洁紧凑。

Lambda 表达式是 JDK8 的一个新特性，可以取代大部分的匿名内部类，写出更优雅的 Java 代码，尤其在集合的遍历和其他集合操作中，可以极大地优化代码结构。JDK 也提供了大量的内置函数式接口供我们使用，使得 Lambda 表达式的运用更加方便、高效。

对接口的要求#
Lambda 表达式可以隐式的转换函数式接口，函数式接口就是一个有且仅有一个抽象方法，但是可以有多个非抽象方法的接口。

## 语法

- 语法形式为 () -> {}，其中 () 用来描述参数列表，{} 用来描述方法体，-> 为 lambda运算符 ，读作(goes to)。

简单例子：

```java
public class Main {
    public static void main(String[] args) {
        ITest iTest1 = new ITest() {
            @Override
            public void test() {
                System.out.println("传统实现方法！");
            }
        };
        iTest1.test();

        ITest iTest2 = () -> System.out.println("lambda表达式实现方法！");
        iTest2.test();
    }
}

interface ITest{
    void test();
}
```

输出：
传统实现方法！
lambda表达式实现方法！

以上lambda表达式是单行时的写法，如果是多行语句应该用{}包裹：

```java
ITest iTest2 = () -> {
            System.out.println("lambda表达式实现方法！");
            // 其他语句
        };
```

## 有无参数和返回值的例子

下面列举一下有参数无返回值、有参数有返回值、无参数有返回值例子。

- 有参数无返回值

```java
public class Main {
    public static void main(String[] args) {
        ITest iTest = (String s) -> {
            System.out.println("lambda表达式实现方法！");
            System.out.println(s);
        };
        iTest.test("有参数无返回值！");
    }
}

interface ITest{
    void test(String s);
}
```

输出：
lambda表达式实现方法！
有参数无返回值！

其中如果参数只有一个的话，括号和里面的类型可以省略：
`ITest iTest = s -> {//代码};`

- 有参数有返回值

```java
public class Main {
    public static void main(String[] args) {
        ITest iTest = (String s,Integer i) -> {
            System.out.println("lambda表达式实现方法！");
            return s+i;
        };
        String s = iTest.test("有参数有返回值！值=",10);
        System.out.println(s);
    }
}

interface ITest{
    String test(String s,Integer i);
}
```

输出：
lambda表达式实现方法！
有参数有返回值！值=10

同样的多个参数存在时可以省略参数类型，但是顺序不能变括号不能少：
`ITest iTest = (s,i) -> {//代码}`

- 有返回值无参数

```java
public class Main {
    public static void main(String[] args) {
        ITest iTest = () -> {
            System.out.println("lambda表达式实现方法！");
            return "有返回值无参数！";
        };
        String s = iTest.test();
        System.out.println(s);
    }
}

interface ITest{
    String test();
}
```

输出：
lambda表达式实现方法！
有返回值无参数！

同样的如果方法体有且只有一句return语句，可以简写：
`ITest iTest = () -> "简写有且只有return语句";`

## ::引用方法

格式为： 归属者对象::归属者方法

- 静态方法的归属者为类名，普通方法的归属者为实体对象，类似于方法调用，静态方法用类名调用，普通方法用对象命调用。

例子：

```java
public class Main {
    public static void main(String[] args) {
        ITest iTest1 = Main::te1;
        iTest1.test();

        Main main = new Main();
        ITest iTest2 = main::te2;
        iTest2.test();

        // 等价于
        ITest iTest3 = () -> te1();
        iTest3.test();
        ITest iTest4 = () -> main.te2();
        iTest4.test();
    }
// 如果有参数和返回值的话，参数和返回值类型要和test()一致
    public static void te1(){
        System.out.println("静态方法");
    }

    public void te2(){
        System.out.println("普通方法");
    }
}

interface ITest{
    void test();
}
```

- 当函数式接口中传入的参数与归属者方法中需要的参数一致时可以简写。

```java
public class Main {
    public static void main(String[] args) {
        ITest iTest1 = System.out::print;
        // test传入的参数与print需要的参数是一样的，可以省略
        iTest1.test("参数");

        // 等价于
        ITest iTest = s -> System.out.println(s);
        iTest.test("等效");
    }
    public static String te(String s){
        return s;
    }
}

interface ITest{
    void test(String s);
}
```

- 通过 类名::new 的方式来实例化对象，然后调用方法返回对象。

```java
public class Main {
    public static void main(String[] args) {
        ITest iTest1 = Test::new;
        Test test1 = iTest1.test();

        ITest iTest2 = Main::getTest;
        Test test2 = iTest2.test();

        // 等价于
        ITest iTest3 = () -> new Test();
        Test test3 = iTest3.test();
    }
    public static Test getTest(){
        return new Test();
    }
}

interface ITest{
    Test test();
}
```

## lambda简单用例

**lambda表达式创建线程**
使用lambda表达式可以很好的简写匿名内部类的创建：

```java
    public static void main(String[] args) {
        Thread thread = new Thread(() -> {
            for (int i = 0; i < 3; i++){
                System.out.println(Thread.currentThread().getName()+i);
            }
        });
        thread.start();
    }
```

**遍历集合**
我们可以调用集合的 public void forEach(Consumer<? super E> action) 方法，通过 lambda 表达式的方式遍历集合中的元素。以下是 Consumer 接口的方法以及遍历集合的操作。Consumer 接口是 jdk 为我们提供的一个函数式接口。

```java
public class Main {
    public static void main(String[] args) {
        ArrayList<Integer> list = new ArrayList<>();
        list.add(1);
        list.add(2);
        list.add(3);

        list.forEach(System.out::println);// 用lambda遍历list
        list.forEach(element -> {
            if(element>1) System.out.println(element);// 用lambda输出list中大于1的值
        });

        // 遍历等效于
        list.forEach(new Consumer<Integer>() {
            @Override
            public void accept(Integer integer) {
                System.out.println(integer);
            }
        });
    }
}
```