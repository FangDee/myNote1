> **定义复杂对象**

```javascript
var myclass = {
    name: "开发四组",
    users: [{
        name: 'liankun',
        age: 18
    },{
        name: 'suyang',
        age: 20
    }],
    teacher: {
        name: 'zn',
        age:30    
    },
    study:function(){
        console.log("studying")
    }
}

myclass.name
myclass.users
(2) [{…}, {…}]0: {name: "liankun", age: 18}1: {name: "suyang", age: 20}

myclass.users[0]
{name: "liankun", age: 18}

myclass.users[0].name
"liankun"
myclass.users[0].age
18

myclass.teacher
{name: "zn", age: 30}

myclass.teacher.name
"zn"
myclass.study()
VM820:15 studying
```

## 第一章 DOM编程

### 一、概述

在 HTML DOM (Document Object Model) 即文档对象模型中, 每个东西都是 **节点** :

- 文档本身就是一个文档对象
- 所有 HTML 元素都是元素节点
- 所有 HTML 属性都是属性节点
- 元素内的文本是文本节点
- 注释是注释节点，就不用

```html
<div class='test1' id='a'>test</div>

div整体是一个元素节点
class=‘test1’ 是一个属性节点
test是文本节点，注意中间没有东西空字符也是一个文本节点
```

所有的节点都有一个nodeType属性，可以判断节点类型，常用的就是以下

| NodeType | Named Constant          |
| -------- | ----------------------- |
| 1        | ELEMENT_NODE 元素节点   |
| 2        | ATTRIBUTE_NODE 属性节点 |
| 3        | TEXT_NODE 文本节点      |

```javascript
//元素节点
var mydiv = document.getElementById("div1")
mydiv.nodeType
1
//属性节点
mydiv.attributes[0].nodeType
2
```

DOM操作其实就是对节点的增删查改

### 二、元素节点

#### 1、获取元素节点的方法

```javascript
//根据id获取一个元素节点
var div1 = document.getElementById("div1")
//根据标签名获取一堆节点的集合
var li1 = document.getElementsByTagName("li");
//根据class获取一堆元素节点
var div2 = document.getElementsByClassName("content");

//使用css选择器选择第一个匹配的元素节点
var d1 = document.querySelector(".content")
//根据css选择器选择一批能够被匹配到的所有的元素
var d1 = document.querySelectorAll(".content")
```

#### 2、修改元素节点的内容

```javascript
//不渲染html标签，标签会当做文本打印出来
mydiv.innerText = "jiasoushi"
//会将html标签渲染出来
mydiv.innerHTML = "<h1>123</h1>"
```

#### 3、删除一个元素节点

```javascript
//直接把自己干掉
var mydiv = document.getElementById("div1")
mydiv.remove();

//清除内容
mydiv.innerText = “”;

//删除某个子元素节点
//1、找到这个字元素节点
var myul = document.getElementsByTagName('ul')[0];
//2、调用方法干掉，注意这个方法参数一定要是个元素节点
mydiv.removeChild(myul)

var div1 = document.getElementById("div1")
undefined
var child = document.getElementsByTagName("ul")[0]
undefined
child
<ul>…</ul>
div1.removeChild(child );
```

#### 4、新建一个元素节点

```javascript
//创建一个div标签，内存中
var mydiv = document.createElement("div");
//添加进几个属性
mydiv.setAttribute("name","mydiv");
mydiv.setAttribute("class","test");
//获取到我的div
var div1 = document.getElementById("div1");
//将内存中新建的div实实在在的加入到我的div中
div1.append(mydiv)
```

#### 5、获取所有的子节点

- 获取了之后当然就能像操作节点一样操作他了。
- 子节点一般是个集合，其实就是个数组
- 循环遍历可以批量操作
- 不仅仅是子节点集合，任何节点集合都能批量操作

```java
//children属性能获取所有的子元素节点，不包括文本节点
mydiv.children
HTMLCollection [ul]

//children属性能获取所有的子元素节点，包括文本节点
mydiv.childNodes
NodeList(3) [text, ul, text]
    
//子节点也是元素节点，一样可以有子节点    
mydiv.children[0].children    
```

### 三、属性节点

#### 1、使用元素节点方法进行增删查改

```javascript
var mydiv = document.getElementById("div1")
//获取这个属性的值
mydiv.getAttribute("name")
//修改，同时可以添加一个属性的值
mydiv.setAttribute("name","cccc")
//删除一个属性
mydiv.removeAttribute("name")
```

#### 2、使用属性节点对象对属性本身进行操作

```javascript
//获取所有的属性节点的集合，是个可以当成数组
mydiv.attributes
//通过下标拿到第二个属性
mydiv.attributes[1]
//拿到属性的name
var attrName = mydiv.attributes[1].name
//拿到属性的值
var attrValue = mydiv.attributes[1].value
//修改这个属性的值
mydiv.attributes[1].value = "aaa"
```

## 第二章 BOM编程

### 一、概述

BOM是浏览器对象模型。

BOM提供了独立于内容 而与浏览器窗口进行交互的对象；

BOM的核心对象是window；

BOM由一系列相关的对象构成，并且每个对象都提供了很多方法与属性；

打开浏览器，F12打开调试窗口，console里输入window，就能看到这个对象。里边有很多的方法和属性，能够帮助我们查看和浏览器相关的一些内容，比如浏览器的版本啦（navigator）、浏览的历时记录啦（history）、网站的地址信息啦（location），和屏幕相关的内容啦（带screende）等等，自己可以浏览一下即可。

### 二、常用方法

> 回调函数

```javascript
//js函数非常灵活，定义了参数传什么都行
var callback = function(fun){
    console.log(fun)
}
callback(1)
callback("123")
callback( {name:'zhangnan'} )
callback( [1,2,3] )

//实际上传什么，就要把这个参数当成什么来用
//要是传个方法就要在方法里找个合适的地方调用一下
var callback = function(fun){
    console.log(fun)
}

var test = function(fun){
    console.log("before");
    fun();
    console.log("after");
}

//你知道需要传方法却传了一个数字，更定不能调用，就会报错
test(1)
VM1038:2 before
VM1038:3 Uncaught TypeError: fun is not a function
    at test (<anonymous>:3:5)
    at <anonymous>:1:1
test @ VM1038:3
(anonymous) @ VM1060:1


var callback = function(){
    console.log("I am callback!")
}
test(callback);

//结果
VM1038:2 before
VM1151:2 I am callback!
VM1038:4 after

//callback就是个方法的名字
var callback = function(data){
    console.log("I am callback!"+data)
}

var test = function(fun){
    console.log("before");
    fun();
    console.log("after");
}

var test = function(fun){
    console.log("before");
    var i = 10;
    fun(i);
    console.log("after");
}
//可以直接传名字
test(callback)
VM1296:2 before
VM1255:2 I am callback!10
VM1296:5 after

//也能直接传个方法体进去
test( function(data){
    console.log("I am callback!"+data)
} )

VM1296:2 before
VM1363:2 I am callback!10
VM1296:5 after

//直接调用方法体也行
(function(){
    console.log(123)
})()
VM1427:2 123

var a  = function(){
    console.log(123)
}
//拿名字调用也行
a()
VM1452:2 123
```

#### 1、setTimeout

```javascript
//一次性定时器，会在多少毫秒后执行这个函数
//里边的是匿名函数，也叫回调函数（就是等过了两秒后回过头来再调用这个函数）
//返回值是个定时器，这个方法是在未来去执行某个函数
var timer = setTimeout( function(){
    console.log(123)
},2000 )

//如果时间未到，不想让他执行了，就需要取消这个定时器
clearTimeout(timer)
```

#### 2、setInterval

```javascript
//周期性定时器，会每隔多少毫秒后执行这个函数
//里边的是匿名函数，也叫回调函数（就是等过了两秒后回过头来再调用这个函数）
//返回值是个定时器，这个方法是在未来去执行某个函数
var timer = setInterval( function(){
    console.log(123)
},2000 )

//如果时间未到，或者中途不想让他执行了，就需要取消这个定时器
clearInterval(timer)
```

#### 3、浏览器自带小型数据库

为每一个网址提供两个小型数据库

```javascript
//localStorage只要不人为删除，会浏览器被删除数据会一直在
//增加或修改一个
window.localStorage.setItem("name","lucy")
//获取
window.localStorage.getItem("name")
//删除一个
window.localStorage.removeItem("name")
//清空
window.localStorage.clear()

//sessionStorage网页被关闭就没有了
//增加或修改一个
window.sessionStorage.setItem("name","lucy")
//获取
window.sessionStorage.getItem("name")
//删除一个
window.sessionStorage.removeItem("name")
//清空
window.sessionStorage.clear()
```

#### 4、弹窗其实没求用

```javascript
//弹个提示窗口，调试也不要用，不优雅
alert(1)

//弹出确认框
//点击确定就是true 点击否就是false
var flag = confirm("您确定要退出吗?")
console.log(flag)

//弹出信息框
//输入信息后点击确定返回填的内容，点击取消返回none
var message = prompt("请填写您的手机号！");
console.log(message);
VM3797:2 1373838438

var message = window.prompt("请输入名字：")
undefined
message
"张楠"
var message = window.prompt("请输入名字：")
undefined
message
""
var message = window.prompt("请输入名字：")
undefined
message
null
var message = window.prompt("请输入名字：","liankun")
```

#### 5、history

```javascript
//回退
history.go(-1)
//向前
history.go(1)
//回退
history.back()
//向前
window.history.forward()
```

#### 6、navigator

这个属性提供了一写浏览器的属性，比如浏览器类型，版本之类的信息。

#### 5、一点注意

在浏览器模型中，调用的方法或属性其实是属于window对象的

你在最外层定义一个方法或者一个变量其实是赋给了window对象

在浏览器模型中，调用window的方法可以省略window. 也可以不省略，如下：

```text
window.localStorage.setItem("name","lucy")
localStorage.setItem("name","lucy")
```

浏览器编程中，全局的变量，就是直接在最外边定义变量的时候尽量避开name，应为window有name属性，你再定义就覆盖了人家的了，当然在方法里，对象中可以随便使用。