# JavaScript

# [1. ECMAScript 的历史](https://jshand.gitee.io/#/course/front/javascript?id=_1-ecmascript-的历史)

ES6 是 ECMAScript 标准十余年来变动最大的一个版本，为其添加了许多新的语法特性。

- 1997 年 ECMAScript 1.0 诞生。
- 1998 年 6 月 ECMAScript 2.0 诞生，包含一些小的更改，用于同步独立的 ISO 国际标准。
- 1999 年 12 月 ECMAScript 3.0诞生，它是一个巨大的成功，在业界得到了广泛的支持，它奠定了 JS 的基本语法，被其后版本完全继承。直到今天，我们一开始学习 JS ，其实就是在学 3.0 版的语法。
- 2000 年的 ECMAScript 4.0 是当下 ES6 的前身，但由于这个版本太过激烈，对 ES 3 做了彻底升级，所以暂时被"和谐"了。
- 2009 年 12 月，ECMAScript 5.0 版正式发布。ECMA 专家组预计 ECMAScript 的第五个版本会在 2013 年中期到 2018 年作为主流的开发标准。2011年6月，ES 5.1 版发布，并且成为 ISO 国际标准。
- 2013 年，ES6 草案冻结，不再添加新的功能，新的功能将被放到 ES7 中；2015年6月， ES6 正式通过，成为国际标准。 现在的事实标准是 JavaScript

ES6===ES2015

# [2. JavaScript的用法](https://jshand.gitee.io/#/course/front/javascript?id=_2-javascript的用法)

> script 标签
>
> 如需在 HTML 页面中插入 JavaScript，请使用 之间的代码行包含了 JavaScript:

# [3. javaScript语法](https://jshand.gitee.io/#/course/front/javascript?id=_3-javascript语法)

可以定义变量、可以使用语句块，可以使用分支语句、有类的概念(js一些解释对象)、解释执行，不需要编译，浏览器逐行解释。

## [3.1 变量](https://jshand.gitee.io/#/course/front/javascript?id=_31-变量)

变量的声明，使用var关键字用于声明.js中的变量是弱类型的。

```
var 变量的名字 = 赋值
<script type="text/javascript">
    var name = '金山';
</script>
```

### [3.1.1 关键字保留字](https://jshand.gitee.io/#/course/front/javascript?id=_311-关键字保留字)

| abstract | arguments | boolean    | break     | byte         |
| -------- | --------- | ---------- | --------- | ------------ |
| case     | catch     | char       | class*    | const        |
| continue | debugger  | default    | delete    | do           |
| double   | else      | enum*      | eval      | export*      |
| extends* | false     | final      | finally   | float        |
| for      | function  | goto       | if        | implements   |
| import*  | in        | instanceof | int       | interface    |
| let      | long      | native     | new       | null         |
| package  | private   | protected  | public    | return       |
| short    | static    | super*     | switch    | synchronized |
| this     | throw     | throws     | transient | true         |
| try      | typeof    | var        | void      | volatile     |
| while    | with      | yield      |           |              |

## [3.2 js的输出](https://jshand.gitee.io/#/course/front/javascript?id=_32-js的输出)

> 向控制台输出

```js
console.log('age:'+age)
```

> 提示框

```js
window.alert("我是alert")
```

> **document.write()**

```js
document.write("我是document.write的内容")
```

> **innerHTML**

# [4. JavaScript 数据类型](https://jshand.gitee.io/#/course/front/javascript?id=_4-javascript-数据类型)

------

> **值类型(基本类型)**：字符串（String）、数字(Number)、布尔(Boolean)、对空（Null）、未定义（Undefined）。
>
> **引用数据类型**：对象(Object)、数组(Array)、函数(Function)。

## [4.1 字符串（String）](https://jshand.gitee.io/#/course/front/javascript?id=_41-字符串（string）)

字符串是存储字符（比如 "Bill Gates"）的变量。

字符串可以是引号中的任意文本。您可以使用单引号或双引号：

```
<script type="text/javascript">
    var name = '金山';
</script>
```

## [字符串属性和方法](https://jshand.gitee.io/#/course/front/javascript?id=字符串属性和方法)

原始值字符串，如 "John", 没有属性和方法(因为他们不是对象)。

原始值可以使用 JavaScript 的属性和方法，因为 JavaScript 在执行方法和属性时可以把原始值当作对象。

------

## [4.1.1字符串属性](https://jshand.gitee.io/#/course/front/javascript?id=_411字符串属性)

| 属性        | 描述                       |
| ----------- | -------------------------- |
| constructor | 返回创建字符串属性的函数   |
| length      | 返回字符串的长度           |
| prototype   | 允许您向对象添加属性和方法 |

------

## [4.1.2 字符串方法](https://jshand.gitee.io/#/course/front/javascript?id=_412-字符串方法)

更多方法实例可以参见：[JavaScript String 对象](https://www.runoob.com/jsref/jsref-obj-string.html)。

| 方法                | 描述                                                         |
| ------------------- | ------------------------------------------------------------ |
| charAt()            | 返回指定索引位置的字符                                       |
| charCodeAt()        | 返回指定索引位置字符的 Unicode 值                            |
| concat()            | 连接两个或多个字符串，返回连接后的字符串,**产生新字符串**    |
| fromCharCode()      | 将 Unicode 转换为字符串                                      |
| indexOf()           | 返回字符串中检索指定字符第一次出现的位置                     |
| lastIndexOf()       | 返回字符串中检索指定字符最后一次出现的位置                   |
| localeCompare()     | 用本地特定的顺序来比较两个字符串                             |
| match()             | 找到一个或多个正则表达式的匹配                               |
| replace()           | 替换与正则表达式匹配的子串                                   |
| search()            | 检索与正则表达式相匹配的值                                   |
| slice()             | 提取字符串的片断，并在新的字符串中返回被提取的部分           |
| split()             | 把字符串分割为子字符串数组                                   |
| substr()            | 从起始索引号提取字符串中指定数目的字符                       |
| substring()         | 提取字符串中两个指定的索引号之间的字符                       |
| toLocaleLowerCase() | 根据主机的语言环境把字符串转换为小写，只有几种语言（如土耳其语）具有地方特有的大小写映射 |
| toLocaleUpperCase() | 根据主机的语言环境把字符串转换为大写，只有几种语言（如土耳其语）具有地方特有的大小写映射 |
| toLowerCase()       | 把字符串转换为小写                                           |
| toString()          | 返回字符串对象值                                             |
| toUpperCase()       | 把字符串转换为大写                                           |
| trim()              | 移除字符串首尾空白                                           |
| valueOf()           | 返回某个字符串对象的原始值                                   |

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
            //msg ="he"
            var msg = 'hello javascript';
            //length 属性，表示的是字符串的长度
            //charAt 返回索引位置的字符

            //需求：查询字符串的最后一个 字符是什么
            console.log('字符串最后一个 字符：'+msg.charAt(msg.length-1))

            //concat 
            var str1  = "hello ";
            var str2 = "world!"
            console.log("str1.concat(str2):"+str1.concat(str2))
            console.log("str1: "+str1);
            console.log("str2: "+str2);

            //indexOf lastIndexOf
            var str3 = "hello world!"
            console.log("str3.indexOf('l'): "+str3.indexOf('l'))
            console.log("str3.lastIndexOf('l'): "+str3.lastIndexOf('l'))



            //slice 切片
            /**
             * 17
             * j s h a n d
               1 一个参数，表示，从索引位置开始（包含），到字符串末尾结束
               2 两个参数，start 从索引位置开始（包含），end :结束位置 (不包含)
             */
            var str4 ="Hello My Name is Jshand,Age is 18" 

            console.log('str4.slice(6): '+str4.slice(6))
            console.log('str4.slice(17): '+str4.slice(17,23))
            console.log('str4.slice(-2,str4.length): '+str4.slice(-2,str4.length))

            // substr\substring
            console.log("str4.substr(17,6): "+str4.substr(17,6))
            console.log("str4.substring(17,6): "+str4.substring(17,23))

            //toLowerCase  \  toUpperCase
            console.log("str4.toUpperCase(): "+(str4.toUpperCase()))
            console.log("str4.toLowerCase(): "+(str4.toLowerCase()))
            console.log("str4: "+str4);//原始字符串不会改变

            //trim 
            var str5 ="  Hello  " 
            console.log("str5.length: "+str5.length);
            console.log("str5.trim().length: "+str5.trim().length);

        </script>
    </head>
    <body>
    </body>
</html>
```

## [4.2. JavaScript 数字](https://jshand.gitee.io/#/course/front/javascript?id=_42-javascript-数字)

JavaScript 只有一种数字类型。数字可以带小数点，也可以不带：

```html
<script type="text/javascript">
    var age = 18;
    //输出
    console.log('age:'+age)
</script>
```

### [4.2.1 算术运算符](https://jshand.gitee.io/#/course/front/javascript?id=_421-算术运算符)

y=5，下面的表格解释了这些算术运算符：

| 运算符 | 描述         | 例子  | x 运算结果 | y 运算结果 | 在线实例                                                     |
| ------ | ------------ | ----- | ---------- | ---------- | ------------------------------------------------------------ |
| +      | 加法         | x=y+2 | 7          | 5          | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_add) |
| -      | 减法         | x=y-2 | 3          | 5          | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_sub) |
| *      | 乘法         | x=y*2 | 10         | 5          | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_mult) |
| /      | 除法         | x=y/2 | 2.5        | 5          | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_div) |
| %      | 取模（余数） | x=y%2 | 1          | 5          | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_mod) |
| ++     | 自增         | x=++y | 6          | 6          | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_incr) |
| x=y++  | 5            | 6     |            |            | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_incr2) |
| --     | 自减         | x=--y | 4          | 4          | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_decr) |
| x=y--  | 5            | 4     |            |            |                                                              |

> 自增和自减：放到变量的前面有限参与运算，然后再跟表达式计算
>
> - 如果+参与运算的类型是字符串,则会将结果以字符串的形式返回
> - 数值类型和bool类型运算时，bool类型会转换成数字， true：1 false：0

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
            //算数运算
            var numa = 5;
            var numb = 6;
            var numc = 10;

            console.log("numa+numb: "+(numa+numb));
            console.log("numa-numb: "+(numa-numb));
            console.log("numa*numb: "+(numa*numb));
            console.log("numa/numb: "+(numa/numb));
            console.log("numc%numb: "+(numc%numb));

            // -- ++ 

            var numd = 2;
            // console.log(numd++);
            // console.log(numd);


            console.log(++numd);
            console.log(numd);
            //字符串 参与算数运算
            console.log('1'+'1')
            //boolean 参与算数运算
            console.log("true+1 : "+(true+1));
            console.log("false+1 : "+(false+1));

        </script>
    </head>
    <body>
    </body>
</html>
```

### [4.2.2赋值运算符](https://jshand.gitee.io/#/course/front/javascript?id=_422赋值运算符)

赋值运算符用于给 JavaScript 变量赋值。

给定 **x=10** 和 **y=5**，下面的表格解释了赋值运算符：

| 运算符 | 例子 | 等同于 | 运算结果 | 在线实例                                                     |
| ------ | ---- | ------ | -------- | ------------------------------------------------------------ |
| =      | x=y  |        | x=5      | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_equal) |
| +=     | x+=y | x=x+y  | x=15     | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_plusequal) |
| -=     | x-=y | x=x-y  | x=5      | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_minequal) |
| *=     | x*=y | x=x*y  | x=50     | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_multequal) |
| /=     | x/=y | x=x/y  | x=2      | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_oper_divequal) |
| %=     | x%=y | x=x%y  | x=0      | [实例»](https://www.runoob.com/try/try.php?filename=tryjs_oper_modequal) |

> 代码实现

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>

            //赋值运算

            var numa = 100;
            var numb = numa;

            console.log("numa: "+numa); //100
            console.log("numb: "+numb); // 100


            numa += 10; //numa  = numa +10
            console.log("numa: "+numa); //110

            numa -= 10; //numa  = numa - 10
            console.log("numa: "+numa); //100

            numa *= 2; //numa  = numa * 2
            console.log("numa: "+numa); //200

            numa /= 5; //numa  = numa /5 
            console.log("numa: "+numa); // 40

            numa %= 38; //numa  = numa % 38
            console.log("numa: "+numa); // 2

        </script>
    </head>
    <body>
    </body>
</html>
```

### [4.2.3比较运算符](https://jshand.gitee.io/#/course/front/javascript?id=_423比较运算符)

 比较运算符在逻辑语句中使用，以测定变量或值是否相等。

x=5，下面的表格解释了比较运算符：

| 运算符 | 描述                                               | 比较                                                         | 返回值  | 实例                                                         |
| ------ | -------------------------------------------------- | ------------------------------------------------------------ | ------- | ------------------------------------------------------------ |
| ==     | 等于                                               | x==8                                                         | *false* | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_comparison1) |
| x==5   | *true*                                             | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_comparison2) |         |                                                              |
| ===    | 绝对等于（值和类型均相等）                         | x==="5"                                                      | *false* | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_comparison3) |
| x===5  | *true*                                             | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_comparison4) |         |                                                              |
| !=     | 不等于                                             | x!=8                                                         | *true*  | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_comparison5) |
| !==    | 不绝对等于（值和类型有一个不相等，或两个都不相等） | x!=="5"                                                      | *true*  | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_comparison6) |
| x!==5  | *false*                                            | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_comparison7) |         |                                                              |
| >      | 大于                                               | x>8                                                          | *false* | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_comparison8) |
| <      | 小于                                               | x<8                                                          | *true*  | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_comparison9) |
| >=     | 大于或等于                                         | x>=8                                                         | *false* | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_comparison10) |
| <=     | 小于或等于                                         | x<=8                                                         | *true*  | [实例 »](https://www.runoob.com/try/try.php?filename=tryjs_comparison11) |

> 代码实现、

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
            //比较运算符， 返回的是  true  或者是 false
            var numa = 100;
            var numb = 150;
            var numc = 100;

            console.log("100 == 150: " + (numa == numb));
            console.log("100 != 150: " + (numa != numb));
            console.log("100 >  150: " + (numa > numb));
            console.log("100 <  150: " + (numa < numb));


            console.log("100 >=  150: " + (numa >= numb));
            console.log("100 <  150: " + (numa <= numb));

            //== ===
            var v1 = 100;
            var v2 = '100';
            var v3 = 100;

            //两个等于会有类型转换（自动）
            //== 判断字面量
            console.log("v1 == v2: " +(v1 == v2));   //  fasle

            //即想判断字面量 还想判断类型 相等与否    ===
            console.log("v1 === v2: " +(v1 === v2));
            console.log("v1 === v3: " +(v1 === v3));
        </script>
    </head>
    <body>
        <h3>请打开浏览器控制台，观察结果</h3>

    </body>
</html>
```

#### [4.2.3.1 类型判断](https://jshand.gitee.io/#/course/front/javascript?id=_4231-类型判断)

 typeof 运算符，返回的是变量的类型名字（字符串）

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
            let name = 'jshand';//string
            let age = 30; //number

            let type1 = typeof name;  //  'string'
            let type2 = typeof age;   //  'number'
            console.log(type1 === 'string');
            console.log(type2 === 'number');




            var t1 = 'jshand';
            var t2 = 3.14;
            var t3 = false;
            var t4 = null;
            var t5 = undefined;

            var t6 = [];
            var t7 = {};
            var t8 = function(){};

            console.log("typeof(字符串): "+typeof(t1))
            console.log("typeof(数字): "+typeof(t2))
            console.log("typeof(null): "+typeof(null))
            console.log("typeof(undefined): "+typeof(undefined))
            console.log("typeof([]): "+typeof(t6))
            console.log("typeof({}): "+typeof(t7))
            console.log("typeof(function): "+typeof(t8))

            if(typeof(t1) === 'string'){
                alert('是字符串')
            }

            //想要判断t8是不是函数
            if(typeof(t8) === 'function'){
                console.log("typeof(t8) === 'function' = "+(typeof(t8) === 'function'))
            }


            let  msg = 'my name is "jshand" '
            console.log(msg);

        </script>
    </head>
    <body> 
        <h3>请打开浏览器控制台，观察结果</h3>
    </body>
</html>
```

### [4.2.4逻辑运算符](https://jshand.gitee.io/#/course/front/javascript?id=_424逻辑运算符)

逻辑运算符用于测定变量或值之间的逻辑。

给定 x=6 以及 y=3，下表解释了逻辑运算符：

| 运算符 | 描述 | 例子                      |
| ------ | ---- | ------------------------- |
| &&     | and  | (x < 10 && y > 1) 为 true |
| \|\|   | or   | (x==5 \|\| y==5) 为 false |
| !      | not  | !(x==y) 为 true           |

> 代码实现

```html
<!DOCTYPE html>
<htm<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
            // var b1 = true;
            // var b2 = false;
            // var b3 = true;
            // var b4 = false;


            //两个都是true，则表达式的结果为true，否则为false
            console.log( "true && true : "+(true && true)); //true
            console.log( "true && false : "+(true && false)) ;//false
            console.log( "false && false : "+(false && false)); //false

            //只要有一个为true，则表达式的结果为true
            console.log( "true || true : "+(true ||  true)) ;// true
            console.log( "true || false : "+(true ||  false)) ;// true
            console.log( "false || false : "+(false ||  false)) ;// true

            //
            console.log( "!true : "+(!true))
            console.log( "!false : "+(!false))


            // var result =  逻辑表达式 true/false  ? '表达式位真的结果'   : '表达式为false'            
            var  age = 70;
            //三目运算
            var info =  age>=60  ?  '已退休，另退休金'  : '不退休';

            console.log(info)

        </script>
    </head>
    <body>
    </body>
</html>
```

### [4.2.5 位运算](https://jshand.gitee.io/#/course/front/javascript?id=_425-位运算)

[参考](https://www.w3school.com.cn/js/pro_js_operators_bitwise.asp)

```html
<!DOCTYPE html>    
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
                /**
                 *   10进制            0    1    2        3        4        5            6        7
                 *   二进制          0    1    10        011        100        101            110        111
                 * 
                 */

                let numa = 3;  // 0 1 1
                let numb = 4;  // 1 0 0

                let numc = numa & numb;
                console.log("numc: "+numc)

                let numd = numa | numb;
                console.log("numd: "+numd)


                console.log("numa << 1 "+ (numa << 1)); // 011  <<1   110
                console.log("numa >> 1 "+ (numa >> 1));// 011 >>1    
                //无符号的位移
                let nume = -3;
                console.log("nume >>> 1 "+ (nume >>> 1));    


        </script>
    </head>
    <body>
        <h3>请打开浏览器控制台，观察结果</h3>
    </body>
</html>
```

## [4.3. bool 类型](https://jshand.gitee.io/#/course/front/javascript?id=_43-bool-类型)

表示真或者假.常用于判断,

取值

> true ，取值，true 、非0的数字、非空字符串(字符长度为0)
>
> false：取值 false、0、 空字符串、null、undefined

## [4.4. null](https://jshand.gitee.io/#/course/front/javascript?id=_44-null)

> 表示空对象的。

## [4.5. Undefined](https://jshand.gitee.io/#/course/front/javascript?id=_45-undefined)

> 变量声明，未初始化

## [4.6 Object](https://jshand.gitee.io/#/course/front/javascript?id=_46-object)

对象由花括号分隔。在括号内部，对象的属性以名称和值对的形式 (name : value) 来定义。也叫json属性由逗号分隔：

空格和折行无关紧要。声明可横跨多行：

> 定义一个对象, 姓名：张飞, 年龄：18 ，地址：长坂坡，isEdu:false

```js
var zs = {
    'name': '张飞',
    'age': 18,
    address: '长坂坡',
    isEdu:false
}
console.log(zs)
```

### [4.6.1 对象的创建](https://jshand.gitee.io/#/course/front/javascript?id=_461-对象的创建)

> 使用{}创建，

```js
var person = {
    name : '张飞',
    sayHi:function(){
        console.log('hi, my name is :'+this.name)
    }
};


// console.log(person)
person.sayHi()
```

> 使用Object创建

```js
//2 使用Object
var p2 = new Object();
p2.name = '张飞';
p2.age = 18;

console.log(p2);
```

> 使用Function创建

```js
function Student(){
    this.name = '';
    this.age = 0;
}

var stu1 = new Student();
stu1.name = "诸葛亮";
stu1.age = 18;
stu1.address = '五丈原';

console.log(stu1);

var stu2 = new Student();
console.log(stu2);
```

> 使用class关键字

```js
class Human{
    constructor(name) {
    this.name = name;
    }

    sayHi(){
        console.log('我是： '+this.name);
    }

}

var  h1 = new Human('习大大');
h1.sayHi()
```

### [4.6.2 对象的原型模型](https://jshand.gitee.io/#/course/front/javascript?id=_462-对象的原型模型)

通过对象可以给对象扩展字段（属性、方法）;如果想同一个类型，都添加属性，则需要用到原型 prototype

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
            function Student(){
                this.name = '';
                this.age = 0;
            }

            var  s1 = new Student();

            //给对象扩展方法
            // s1.sayHi =function(){
            //     console.log('打招呼')
            // }

            //给原型扩展
            Student.prototype.sayHi = function(){
                console.log('打招呼')
            }

            s1.sayHi();


            var  s2 = new Student();
            s2.sayHi();
        </script>
    </head>
    <body>
    </body>
</html>
```

## [4.7 数组](https://jshand.gitee.io/#/course/front/javascript?id=_47-数组)

用于存储多个数据集合的类型.

```js
var names = ['张飞','刘备','关羽'];
console.log(names);
```

数据元素的访问, 通过下标的形式 数组名[下标], names[0];

```js
var names = ['张飞','刘备','关羽'];
console.log(names[1]);
```

### [4.7.1数组的定义](https://jshand.gitee.io/#/course/front/javascript?id=_471数组的定义)

1 使用[] 创建数组，可以直接初始化,推荐使用

```js
var arr1 = [10,20,30];
console.log(arr1);
```

2 使用Array

```javascript
var arr2 = new Array();

console.log(arr2);
```

3 数组的访问

length 属性代表数字的长度

通过元素[下标]访问数组,赋值、获取元素内容,数组是可变长度。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>

            var arr1 = [10,20,30];
            console.log("arr1.length: "+arr1.length);

            var arr3 = [];
            arr3[0] = 100;
            arr3[1] = 101

            arr3[100] = 999;

            console.log("arr3.length: "+arr3.length);
            console.log("arr3[2]: "+arr3[2]);

        </script>
    </head>
    <body>
    </body>
</html>
```

4 .数组的方法

> push 向数组中添加元素

```js
var arr = [];
arr.push(10);
arr.push(20);
arr.push(30);
arr.push('马上下课了');

console.log("arr[0]:"+arr[0]);
console.log("arr[1]:"+arr[1]);
console.log("arr[2]:"+arr[2]);
console.log("arr[3]:"+arr[3]);
```

> pop 从数组的末尾获取元素，并将元素从数组中删除，相当于是 出栈

```js
var arr = [];

arr.push(10);
arr.push(20);
arr.push(30);
arr.push('马上下课了');

console.log(arr.pop()); //马
console.log(arr.pop()); // 30
console.log(arr.pop()); //20
console.log(arr.pop()); //10
console.log(arr)  //[]
```

> shift 从数组首个下标获取元素，并将元素从数组中删除， 类似出队

```js
var arr4 = [10,20,30,40];
console.log(arr4.shift()) // 10
console.log(arr4)   // 20,30,40
```

>  slice 切片

```js
var arr5 = [10,20,30,40];
console.log(arr5)
console.log("arr5.slice(1) : "+arr5.slice(1))
console.log("arr5.slice(1,2) : "+arr5.slice(1,2))
```

> join 和 split

```js
//将数组拼接以字符串的形式返回 join    10-20-30,40
var arr6 = [10,20,30,40];
console.log("arr6.join(\"-\"): "+ (arr6.join('-'))) ;// 返回的是字符串 , 10-20-30-40

//将字符串拆分成数组
var arrStr = '10-20-30-40';
console.log(arrStr.split('-')) ;// 数组元素，为字符串类型
```

## [4.8 函数](https://jshand.gitee.io/#/course/front/javascript?id=_48-函数)

函数是由事件驱动的或者当它被调用时执行的可重复使用的代码块。

//方法的声明

```js
function 名字(参数列表...){
    //语句块
}
//调用
方法名字()
```

> 实例

```js
function sayHi(){
    console.log('hello JavaScript')
}

sayHi();        
```

> 在js中一切皆是对象,方法只要加括号就可以执行

```js
function sayHi(){
    console.log('hello JavaScript')
}

var myfun = sayHi;
myfun()
```

### [4.8.1 无参函数](https://jshand.gitee.io/#/course/front/javascript?id=_481-无参函数)

```
function sayHi(){
    console.log('hello JavaScript')
}

sayHi();
```

### [4.8.2 有参函数](https://jshand.gitee.io/#/course/front/javascript?id=_482-有参函数)

![img](https://jshand.gitee.io/imgs/html-css-js/2020-11-03_084903.png)

> arguments,代表参数列表,js中的参数列白可以跟实际参数不一致。

变量的作用域： 变量只能在作用域中使用，不严格的js，如果生命变量的时候没有写var关键字，则会发生变量提权

> 4.7.3. this关键字

通过call、apply调用方法,非严格模式下

outer.call('绑定this对象',参数列表....)

outer.apply('绑定this对象',[参数内容])

非严格模式 'use strict'

返回值

闭包

# [5. 流程控制语句](https://jshand.gitee.io/#/course/front/javascript?id=_5-流程控制语句)

## [5.1 条件语句](https://jshand.gitee.io/#/course/front/javascript?id=_51-条件语句)

 只有当指定条件为 true 时，该语句才会执行代码。

### [if的使用](https://jshand.gitee.io/#/course/front/javascript?id=if的使用)

```js
var age = 70;
if(age>=60){
    console.log("退休了....")
}
```

### [else if](https://jshand.gitee.io/#/course/front/javascript?id=else-if)

```js
var age = 56;
if(age>=60){
    console.log("退休了....")
}else if(age>=55){
    console.log("可以协议退休了....")
}
```

### [else 当前面的判断都不满足的时候执行](https://jshand.gitee.io/#/course/front/javascript?id=else-当前面的判断都不满足的时候执行)

```js
var age = 18;
if(age>=60){
    console.log("退休了....")
}else if(age>=55){
    console.log("可以协议退休了....")
}else{
    console.log("上班中....")
}
```

### [switch分支语句](https://jshand.gitee.io/#/course/front/javascript?id=switch分支语句)

switch语句块中当满足一个分支，则默认后面的分支也会顺序执行，可以使用break中断。

```js
var caseNum = 3;
switch(caseNum){
    case 1:
        console.log('第一种条件');
        break;
    case 2:
        console.log('第二种条件');
        break;
    default:
        console.log('默认执行的代码块');

}
```

## [5.2循环语句](https://jshand.gitee.io/#/course/front/javascript?id=_52循环语句)

### [5.2.1 for循环](https://jshand.gitee.io/#/course/front/javascript?id=_521-for循环)

![img](https://jshand.gitee.io/imgs/html-css-js/2020-11-03_100806.png)

执行的顺序就是 ，1 、2、3、4 （2、3、4）重复，直到条件表达式位false，终止循环换

```html
<!DOCTYPE html>    
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>

        let sum = 0;
        for (let i = 0; i < 10; i++) {
            sum +=i;        
        }
        console.log("0+...9:  "+sum);

        //循环数组
        let arr = [10,20,30,40,50,60,70,80,90,100];
        sum = 0;
        for (let i = 0; i < arr.length; i++) {
            sum +=arr[i];        
        }
        console.log("10+...100:  "+sum);


        //数组--可迭代对象
        sum = 0;
        for (let i in arr) {
            sum +=arr[i];    
        }
        console.log("let i in arr:  10+...100:  "+sum);



        //迭代对象 JSON
        let person = {
            "name":"jshand",
            "key1":"value1",
            "key2":"value2",
            "key3":"value3",
            "key4":"value4",
            "key5":"value5"
        }
        console.log("***************根据key获取value********************\r\n\r\n")
        console.log("person.name:"+person.name)
        console.log("person.key1:"+person.key1)
        console.log("************************************\r\n\r\n")

        console.log("person[\"name\"]:"+person["name"])
        console.log("person[\"key1\"]:"+person["key1"])

        console.log("*****************循环迭代JSON*******************\r\n\r\n")
        for (let key in person) {
            console.log("key="+key+", value="+person[key]);    
        }

        </script>
    </head>
    <body>
        <h3>请打开浏览器控制台，观察结果---流程控制语句-循环-for-示例</h3>
    </body>
</html>
```

### [5.2.2 while循环](https://jshand.gitee.io/#/course/front/javascript?id=_522-while循环)

 当while表达式中的结果为false时终止循环

```
var i = 0;
while(i<10){
    console.log('执行'+i)
    i++
}
```

### [5.2.3 do while循环](https://jshand.gitee.io/#/course/front/javascript?id=_523-do-while循环)

```js
i = 0;
sum = 0;
do {
    sum += i;
    i++;
} while (i < 10);

console.log(" do sum  " + sum);
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
            let sum = 0;
            for (let i = 0; i < 10; i++) {
                sum += i;
            }
            console.log("0+...9:  " + sum); //



            let i = 0;
            sum = 0;
            while (i < 10) {
                sum += i;
                i++;
            }
            console.log(" sum  " + sum);


            i = 0;
            sum = 0;
            do {
                sum += i;
                i++;
            } while (i < 10);

            console.log(" do sum  " + sum);
        </script>
    </head>
    <body>
        <h3>请打开浏览器控制台，观察结果---流程控制语句-循环-for-示例</h3>
    </body>
</html>
```

### [5.2.4 循环嵌套](https://jshand.gitee.io/#/course/front/javascript?id=_524-循环嵌套)

```js
//9*9 乘法口诀

var arr = [
   // ['1*1=1'],
   // ['1*2=2' ,'2*2=4'],
   // ['1*3=3', '2*3=6' ,'3*3=9' ]
 ]

for(var hang=1 ; hang<=9 ; hang++ ){
    var hangArr = [];
    for(var col =1; col<= hang  ;col++){
        hangArr.push(col+'*'+hang+"="+(col * hang))
    }
    arr.push(hangArr.join('    '));
}

for(var i=0; i<arr.length;i++){
    var content = arr[i];
    console.log(content)
}
```

### [5.2.5 break、continue关键字](https://jshand.gitee.io/#/course/front/javascript?id=_525-break、continue关键字)

当for、while没有终止条件，则语句块一只执行，直到内存溢出。

```js
var i =0;
for( ; ; ){
    i++;
    console.log("i="+i);
}
```

> break: 有时需要比较复杂的运算，才能知道是否终止循环 运算，通过break，语句终止循环

```js
var i=0;
while(true){
    console.log("i:"+i)

        i++;
    if(i>9){
        break;
    }
}
```

 continue: 终止本次循环，继续下一次循环;

需求：计算从0-100，偶数和

```js
var sum = 0;
for(var i=0; i<=100 ;i++){

    //当i 为奇数的时候，跳出
    if( i %2 != 0){
        continue;
    }
    console.log("i:"+i);
    sum += i;
}
```

嵌套循环、break、continue关键字示例

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
            //想要输出一个乘法口诀表
            // h： 行 
            // l： 列   
            for(let h=1 ;h<=9 ;h++ ){
                let str = '';
                for(let l =1 ; l<=9 ;l++){
                    if(l>h){
                        break; //终止 当前循环
                    }
                    str += l+" *"+h+" ="+(l * h)+"\t\t";
                }

                console.log(str);    
            }



            //计算  1--100  等被三整除的数加和
            let sum =0;
            for(let i =1; i<=13 ;i++){
                if(i%3 != 0){
                    continue;//终止本次循环，进行下一次循环 
                }

                sum += i;
            }
            //3 6 9 12 
            console.log("sum: ",sum);     //30


        </script>
    </head>
    <body>
        <h3>请打开浏览器控制台，观察结果---流程控制语句-循环-嵌套-示例</h3>
    </body>
</html>
```

# [6 js参与用户交互](https://jshand.gitee.io/#/course/front/javascript?id=_6-js参与用户交互)

常用的交互函数 alert、confirm、prompt

## [6.1 alert : 弹出框](https://jshand.gitee.io/#/course/front/javascript?id=_61-alert-弹出框)

阻塞的函数

```js
window.alert("提示文本.....");
```

![img](https://jshand.gitee.io/imgs/html-css-js/2020-11-03_114343.png) ![img](https://jshand.gitee.io/imgs/html-css-js/2020-11-03_114454.png)

## [6.2 prompt : 输入框 ,](https://jshand.gitee.io/#/course/front/javascript?id=_62-prompt-输入框-)

返回类型是字符串string，

![img](https://jshand.gitee.io/imgs/html-css-js/2020-11-03_115400.png)

### [6.2.1类型转换](https://jshand.gitee.io/#/course/front/javascript?id=_621类型转换)

字符串转数字，使用parseInt、parseFloat

```js
var numa = window.prompt("请输入一个数字");

//返回类型是字符串的
//数据类型转换，将字符串，转换成数字
//parstInt  parseFloat
numa = parseInt(numa)


console.log('输入的数字为：'+(numa +10));
```

### [6.2.2 confirm:确认对话框](https://jshand.gitee.io/#/course/front/javascript?id=_622-confirm确认对话框)

```js
var isdel = confirm("是否删除？");
if(isdel){
    //执行删除的逻辑
    alert("执行删除的逻辑")
}else{
    alert("不删,点错了")
}
```

## [6.3 事件交互](https://jshand.gitee.io/#/course/front/javascript?id=_63-事件交互)

alert、prompt，这些交互，都是立即执行，需要让用户决定，什么时机执行。需要绑定事件使用on+事件的名字绑定事件

```
<button onclick="sayHi()">按钮1</button>
```

| 事件               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| onactivate         | [当对象设置为活动元素时触发。](https://jshand.gitee.io/#/../properties/activeElement.html) |
| onbeforeactivate   | 对象要被设置为当前元素前立即触发。                           |
| onbeforecut        | 当选中区从文档中删除之前在源对象触发。                       |
| onbeforedeactivate | [在 activeElement 从当前对象变为父文档其它对象之前立即触发。](https://jshand.gitee.io/#/../properties/activeElement.html) |
| onbeforeeditfocus  | 在包含于可编辑元素内的对象进入用户界面激活状态前或可编辑容器变成控件选中区前触发。 |
| onbeforepaste      | 在选中区从系统剪贴板粘贴到文档前在目标对象上触发。           |
| **onblur**         | 在对象失去输入焦点时触发。                                   |
| **onclick**        | 在用户用鼠标左键单击对象时触发。                             |
| oncontextmenu      | 在用户使用鼠标右键单击客户区打开上下文菜单时触发。           |
| oncontrolselect    | 当用户将要对该对象制作一个控件选中区时触发。                 |
| oncut              | 当对象或选中区从文档中删除并添加到系统剪贴板上时在源元素上触发。 |
| **ondblclick**     | 当用户双击对象时触发。                                       |
| ondeactivate       | 当 activeElement 从当前对象变为父文档其它对象时触发。        |
| ondrag             | 当进行拖曳操作时在源对象上持续触发。                         |
| ondragend          | 当用户在拖曳操作结束后释放鼠标时在源对象上触发。             |
| ondragenter        | 当用户拖曳对象到一个合法拖曳目标时在目标元素上触发。         |
| ondragleave        | 当用户在拖曳操作过程中将鼠标移出合法拖曳目标时在目标对象上触发。 |
| ondragover         | 当用户拖曳对象划过合法拖曳目标时持续在目标元素上触发。       |
| ondragstart        | 当用户开始拖曳文本选中区或选中对象时在源对象上触发。         |
| ondrop             | 当鼠标按钮在拖曳操作过程中释放时在目标对象上触发。           |
| onfilterchange     | 当可视滤镜更改状态或完成转换时触发。                         |
| **onfocus**        | 当对象获得焦点时触发。                                       |
| onfocusin          | 当元素将要被设置为焦点之前触发。                             |
| onfocusout         | 在移动焦点到其它元素之后立即触发于当前拥有焦点的元素上触发。 |
| onhelp             | 当用户在浏览器为当前窗口时按 F1 键时触发。                   |
| onkeydown          | 当用户按下键盘按键时触发。                                   |
| onkeypress         | 当用户按下字面键时触发。                                     |
| onkeyup            | 当用户释放键盘按键时触发。                                   |
| onlosecapture      | 当对象失去鼠标捕捉时触发。                                   |
| **onmousedown**    | 当用户用任何鼠标按钮单击对象时触发。                         |
| **onmouseenter**   | 当用户将鼠标指针移动到对象内时触发。                         |
| **onmouseleave**   | 当用户将鼠标指针移出对象边界时触发。                         |
| **onmousemove**    | 当用户将鼠标划过对象时触发。                                 |
| onmouseout         | 当用户将鼠标指针移出对象边界时触发。                         |
| onmouseover        | 当用户将鼠标指针移动到对象内时触发。                         |
| onmouseup          | 当用户在鼠标位于对象之上时释放鼠标按钮时触发。               |
| onmousewheel       | 当鼠标滚轮按钮旋转时触发。                                   |
| onmove             | 当对象移动时触发。                                           |
| onmoveend          | 当对象停止移动时触发。                                       |
| onmovestart        | 当对象开始移动时触发。                                       |
| onpaste            | 当用户粘贴数据以便从系统剪贴板向文档传送数据时在目标对象上触发。 |
| onpropertychange   | 当在对象上发生对象上发生属性更改时触发。                     |
| onreadystatechange | 当对象状态变更时触发。                                       |
| onresize           | 当对象的大小将要改变时触发。                                 |
| onresizeend        | 当用户更改完控件选中区中对象的尺寸时触发。                   |
| onresizestart      | 当用户开始更改控件选中区中对象的尺寸时触发。                 |
| onselectstart      | 对象将要被选中时触发。                                       |
| ontimeerror        | 当特定时间错误发生时无条件触发，通常由将属性设置为无效值导致。 |

### [6.3.1 控件的常用事件](https://jshand.gitee.io/#/course/front/javascript?id=_631-控件的常用事件)

> onblur 失去焦点 validate(this) this: 代表控件本身input

```html
用户名: <input type="text" value=""   onblur="validate(this)" /> <br/>
<script>
//用户名不能大于8个
function validate(inp){
    console.log(inp);

    //获取元素 值 inp 的value  js中通过属性，可以访问页面的元素
    console.log(inp.value);

    if(inp.value.length >8){
        alert('用户名不能超过8个字符')
    }

}
</script>
```

> onclick 点击事件

```
<!--点击事件: onclick  -->
    用户名2: <input type="text"     onclick="alert('点击')"  /> <br/>
```

> onfocus 获取焦点
>
> onblur 失去焦点

```html
<!-- onfocus 获取焦点
    操作元素的样式: 待添加的样式 style="border: solid 2px aqua;"
onblur 失去焦点  
    //通过className操作样式
    inp.className = "";

    需求 :获取焦点时候显示高亮，失去焦点的时候还原样式
-->


    <style>
            .hightlight{
                border: solid 2px aqua;
            }

        </style>

        <script>
        function myfocus(inp){
                //通过className操作样式
                inp.className = "hightlight";
            }

            function myblur(inp){
                //通过className操作样式
                inp.className = "";
            }
        </script>    

        用户名3: <input type="text"    onfocus ="myfocus(this)"  onblur="myblur(this)" /> <br/>
<!--ondblclick 双击事件，不是点两下，是要连续点击两下 -->` `<input type="button" value="按钮" ondblclick="alert('按钮被双击....')"  /> 
<script>
    function selectGender(rad){
    alert(rad.value == 1 ? '男':'女')
}
</script>
<input type="radio"  name="gender"  value="1" onclick="selectGender(this)"/> 男
<input type="radio"  name="gender"  value="2" onclick="selectGender(this)"  /> 女<br/>
```

onchange事件

```html
<script>
    function showSelectValue(sel){
        //获取当前选中的值
        alert(sel.value);
    }

    //获取选中的option
    function showSeelctedOption(){
        //获取页面上的元素
        var sel = document.getElementById('sel');

        //获取下拉选选中的option 集合属性
        sel.options
    }

</script>
<select onchange="showSelectValue(this)" id="sel" >
    <option value="1" >黑龙江</option>
    <option value="2" selected>江苏</option>
    <option value="3">河南</option>
</select><br/>

<input type="button"  value="获取 select选中的值"  onclick="showSeelctedOption()" />
```

### [6.3.2 事件冒泡](https://jshand.gitee.io/#/course/front/javascript?id=_632-事件冒泡)

事件冒泡 ：当一个元素接收到事件的时候 会把他接收到的事件传给自己的父级，一直到window 。（注意这里传递的仅仅是事件 并不传递所绑定的事件函数。所以如果父级没有绑定事件函数，就算传递了事件 也不会有什么表现 但事件确实传递了。）

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
            function clickinner(){
                console.log("小的div被点击了");

                //阻止事件冒泡
                //获取事件对象
                let event = window.event;
                console.log(event);
                // event.stopPropagation();
                //，使用event.stopPropagation()起到阻止捕获和冒泡阶段中当前事件的进一步传播。
                // 使用event.preventDefault()可以取消默认事件

            }
            function clickouter(){
                console.log("外层的div被点击了");
            }
        </script>
    </head>


    <body>


        <div onclick="clickouter()"  style="width: 300px;height: 300px; border:1px solid red ;"  >

            <div onclick="clickinner()"  style="width: 300px;height: 150px; background-color: aliceblue ;"  >    

            </div>


        </div>
    </body>
</html>
```

# [7. JS操作网页](https://jshand.gitee.io/#/course/front/javascript?id=_7-js操作网页)

## [7.1 获取页面元素](https://jshand.gitee.io/#/course/front/javascript?id=_71-获取页面元素)

> 浏览器BOM模型

![img](https://jshand.gitee.io/imgs/html-css-js/2021-08-04_091201.png)

>  文档对象模型 DOM

![img](https://jshand.gitee.io/imgs/html-css-js/2020-11-04_091231.png)

#### [DOM是什么](https://jshand.gitee.io/#/course/front/javascript?id=dom是什么)

DOM全称为The Document Object Model，应该理解为是一个规范，定义了HTML和XML文档的逻辑结构和文档操作的编程接口。

#### [文档逻辑结构](https://jshand.gitee.io/#/course/front/javascript?id=文档逻辑结构)

DOM基础概念、操作 DOM是什么 DOM全称为The Document Object Model，应该理解为是一个规范，定义了HTML和XML文档的逻辑结构和文档操作的编程接口。

文档逻辑结构 DOM实际上是以面向对象方式描述的对象模型，它将文档建模为一个个对象，以树状的结构组织（本文称之为“文档树”，树中的对象称为“节点”）。 每个文档包含1个document节点，0个或1个doctype节点以及0个或多个元素节点等。document节点是文档树的根节点。 如对于HTML文档，DOM 是这样规定的：

- 整个文档是一个文档节点
- 每个 HTML 标签是一个元素节点
- 包含在 HTML 元素中的文本是文本节点
- 每一个 HTML 属性是一个属性节点
- 注释属于注释节点

节点与文档内容是一一对应的关系，节点之间有层次关系。

例如下面的html文档：

通过document对象,

```html
        <div id="blo1" class="bc"> block 1</div>
        <div id="blo1" class="bc"> block 2</div>
        <div class="bc2"> block 3</div>
        <div> block 4</div>
        <div> block 5</div>
        <div> block 6</div>
```

> getElementById 通过id查找元素，只能找到第一个,定义id的时候不能重复

```js
var blo1 = document.getElementById("blo1");
 console.log(blo1); 
```

需求：给元素添加点击事件，当点击完成后改变样式，实现如下

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <style>
            .inner {
                width: 300px;
                height: 150px;
                background-color: aliceblue;
            }

            .changeClass {
                width: 300px;
                height: 150px;
                background-color: cornflowerblue;
            }
        </style>
        <script>
            function changeStyle(){
                //1 获取内部div元素
                //getElementById 根据id查找元素
                let innerDiv = document.getElementById("innerDiv");
                innerDiv.className = 'changeClass';
            }    

        </script>
    </head>


    <body>


        <div style="width: 300px;height: 300px; border:1px solid red ;">

            <div id="innerDiv" class="inner" onclick="changeStyle()" >

            </div>


        </div>
    </body>
</html>
```

>  getElementsByTagName

 通过标签访问页面元素 HTMLCollection, 可以按照数组小标的形式访问元素,及时找到一个元素，也得通过下标的形式访问

```js
 var div = document.getElementsByTagName('div');
 console.log(div)
```

> getElementsByClassName 通过class获取

```js
// var bc = document.getElementsByClassName('bc2') ;// HTMLCollection 
// console.log(bc[0]);// 及时一个元素，也得用下标形式访问
// console.log(bc.innerText);//
```

获取节点属性

//获取元素子元素

//获取nodeType.

## [7.2 NodeType](https://jshand.gitee.io/#/course/front/javascript?id=_72-nodetype)

元素.nodeType：只读 属性 当前元素的节点类型

DOM节点的类型nodeType一种有12种，可以查看这里。

常用的节点类型包括：元素节点、文本节点、属性节点：

元素节点 ELEMENT_NODE：1

属性节点 ATTRIBUTE_NODE ：2

文本节点 TEXT_NODE：3

## [7.3 对Dom节点的操作](https://jshand.gitee.io/#/course/front/javascript?id=_73-对dom节点的操作)

### [7.3.1 创建添加元素](https://jshand.gitee.io/#/course/front/javascript?id=_731-创建添加元素)

 createElement 只能使用标签名字，通过innerText属性设置或获取文本内容，通过innerHTML设置或获取html内容

### [7.3.2. 添加元素](https://jshand.gitee.io/#/course/front/javascript?id=_732-添加元素)

 insertBefore: 向元素开始位置插入

 appendChild :向元素内部追加子元素

### [7.3.3 移除元素](https://jshand.gitee.io/#/course/front/javascript?id=_733-移除元素)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>


        <script>
            function appChild(){
                //在body中添加一个div


                //1 创建div元素
                var div = document.createElement('div');


                //给div添加内容
                //innerHTML 添加html代码
                //innerText 添加普通文本的
                div.innerText = "通过js 动态添加的内容";

                //2 找到body 并将div追加到 body中 ,通过append方法给元素追加
                document.body.appendChild(div);

            }


            function rmNode(){
                var div1 = document.getElementById('div1');
                // removeChild 通过父元素，删除自己节点
                document.body.removeChild(div1);
                //remove 移除自身
                // div1.remove();
            }
        </script>
        <div id="div1">
            待删除的div,可以点击按钮删除
        </div>

        <button onclick="appChild()">创建添加元素</button>
        <button onclick="rmNode()">移除节点</button>


    </body>
</html>
```

> parentNode

找到上级元素

> firstchild

元素.firstChild：只读 属性 第一个子节点

标准下：firstChild会包含文本类型的节点

非标准下： 只包含元素节点

> firstElementChild

元素.firstElementChild：只读 属性

标准下获取第一个元素类型节点的子节点

> .lastChild

元素.lastElementChild 最后一个子节点 原理与firstChild firstElementChild相似，不再赘述 nextSibling、previousSibling

元素.nextSibling 元素.nextElementSibling 下一个兄弟节点

元素.previousSibling 元素.previousElementSibling 上一个兄弟节点 父节点

> 示例

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <style>
            .workItem {
                display: inline-block;
                position: relative;
                width: 190px;
                height: 500px;
                border: 1px solid green;
            }

            .close {
                position: absolute;
                top: 0;
                right: 0;
                cursor: pointer;
                border: solid 1px black;
                width: 30px;
                height: 30px;
                display: flex;
                flex: 1;
                align-content: center;
                text-align: center;
            }

            .close:active{
                background-color: #FF0000;
            }
        </style>
        <script>
            function closeDiv(btn){
                // console.log(btn)
                let workItem = btn.parentNode;//获取父元素

                //移除元素（从文档对象中 移除自身）
                workItem.remove();
            }
        </script>
    </head>


    <body>

        <div id="mydiv" style="width: 1000px; height:500px;border:2px solid red ;">

            <div class="workItem">111 <div class="close" onclick="closeDiv(this)">X</div>
            </div>
            <div class="workItem">222 <div class="close" onclick="closeDiv(this)">X</div>
            </div>
            <div class="workItem"> 333 <div class="close" onclick="closeDiv(this)">X</div>
            </div>
            <div class="workItem">444 <div class="close" onclick="closeDiv(this)">X</div>
            </div>
            <div class="workItem"> 555 <div class="close" onclick="closeDiv(this)">X</div>
            </div>

        </div>

    </body>
</html>
```

## [7.4 元素属性读写](https://jshand.gitee.io/#/course/front/javascript?id=_74-元素属性读写)

### [7.4.1 操作文档](https://jshand.gitee.io/#/course/front/javascript?id=_741-操作文档)

innerHTML ;获取标签内容

innerText ： 将所有文本返回

```html
<div id="b1">

            block ...

            <p>我是段落</p>


        </div>

        <script>
            function getElText(){
                var b1 = document.getElementById('b1');
                alert(b1.innerText);
            }

            function getElHtml(){
                var b1 = document.getElementById('b1');
                alert(b1.innerHTML); 
            }
        </script>
        <button onclick="getElText()">获取文本</button>
        <button onclick="getElHtml()">获取html</button>
```

### [7.4.2 操作样式](https://jshand.gitee.io/#/course/front/javascript?id=_742-操作样式)

 class、style分别操作元素样式

> class

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <style>

            .d1{
                border: solid 1px red;
                background-color: #996699;
            }

        </style>
    </head>
    <body>

        <div id="div1">
            文本内容
        </div>


        <script>
            function setCssClass(){
                //设置class
                var div1 = document.getElementById('div1');
                div1.className = 'd1';
            }

            function rmCssClass(){
                var div1 = document.getElementById('div1');
                div1.className = '';
            }
        </script>
        <button onclick="setCssClass()">设置样式</button>
        <button onclick="rmCssClass()">移除样式</button>


    </body>
</html>
```

>  style

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <style>


        </style>
    </head>
    <body>

        <!-- style="border: 1px solid red;
        background-color: #00FFFF;
        opacity: 0.5;

        " -->
        <div id="div1"  >
            文本内容
        </div>


        <script>
            function setCssStyleProp(){
                //设置class
                var div1 = document.getElementById('div1');
                div1.style.backgroundColor  = '#00FFFF';
            }

            function rmCssStyleProp(){
                var div1 = document.getElementById('div1');
                div1.style.backgroundColor = '';
                div1.style.opacity = '1';
            }

            function setOpacity(){
                var div1 = document.getElementById('div1');
                div1.style.opacity = '0.5';
            }

            function setOtherCss(){
                var div1 = document.getElementById('div1');
                div1.style.border = 'solid 1px grey';
            }
        </script>




        <button onclick="setCssStyleProp()">设置样式</button>
        <button onclick="rmCssStyleProp()">移除样式</button>
        <button onclick="setOpacity()">设置透明度</button>
        <button onclick="setOtherCss()">设置其他样式</button>


        <hr/>
        <script>
            function hide(){
                var div1 = document.getElementById('div1');
                div1.style.display = 'none';
            }

            function show(){
                var div1 = document.getElementById('div1');
                div1.style.display = '';
            }
        </script>
        <button onclick="show()">显示元素</button>
        <button onclick="hide()">隐藏元素</button>

    </body>
</html>
```

> cssText

```html
<div id="div2"> blocknnnn</div>

        <script>

            function getElId(id){
                var el = document.getElementById(id);
                return el;
            }


            function setCssText(){
                var div = getElId('div2');
                div.style.cssText ="border: 1px solid red; background-color: #00FFFF; opacity: 0.5" ;
            }


        </script>
        <button onclick="setCssText()">通过cssText设置样式</button>
```

### [7.4.3操作属性](https://jshand.gitee.io/#/course/front/javascript?id=_743操作属性)

以img 的src为例

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <style>

        </style>
        <script>
            let imgs = [
                "https://img1.youlu.net/pic/flash/20210506160329329.jpg",
                "https://img1.youlu.net/pic/flash/202103011728392839.jpg",
                "https://img1.youlu.net/pic/flash/202103240936263626.jpg",
                "https://img1.youlu.net/pic/flash/202008271438183818.jpg",
                "https://img1.youlu.net/pic/flash/202103040857425742.jpg"
            ]

            let current = 0;

            function swipe(img) {
                img.src = imgs[++current]; //访问图片的src属性
                if (current >= imgs.length - 1) {
                    current = 0;
                }
            }
        </script>
    </head>

    <body>

        <img onclick="swipe(this)" width="750px" height="340px" src="https://img1.youlu.net/pic/flash/20210506160329329.jpg" />
    </body>
</html>
```

### [7.4.4 操作自定义属性：](https://jshand.gitee.io/#/course/front/javascript?id=_744-操作自定义属性：)

在元素上可以直接声明自定义属性

> getAttribute： 获取自定义属性

> setAttribute 设置属性（属性名、属性值）;

```html
<img id="img" src="./imgs/1.jpg">

        <script>

            // var current = 0;

            function swiperImg(btn){
                // //准备一个图片列表



                //获取自定义属性， getAttribute('自定义属性名')

                var current = btn.getAttribute('current');
                //console.log(current);



                var srcs = [
                    './imgs/1.jpg',
                    './imgs/2.jpg',
                    './imgs/3.jpg'
                ]
                // var img = document.getElementById('img');
                // //通过src 切换图片
                // // img.src  = srcs[1];

                current++;
                btn.setAttribute('current',current);
                //console.log("img src-->:"+srcs[current%3])
                img.src  = srcs[current%3]

            }

        </script>
        <button onclick="swiperImg(this)" current="0">切换图片</button>
```

> removeAttribute() 移除属性

```html
<script>
        function unchecked(){
            var inp = document.getElementsByTagName('input')[0];
            inp.removeAttribute('checked');
        }
</script>
<input type="checkbox"  checked  /> 爱好
<button onclick="unchecked()" current="0">移除选中属性</button>
```

## [7.5 定时器](https://jshand.gitee.io/#/course/front/javascript?id=_75-定时器)

> setTimeOut ： 一次定时器
>
> setInterval 间隔定时器

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
    </head>
    <body>
        <script>
            var timeid =  null;
            function changeOpti(){
                //定时器
                // //一次定时器
                // timeid = setTimeout(function(){
                //     var img = document.getElementById('img');
                //     var opti = document.getElementById('opti').value;

                //     img.style.opacity = opti;

                // },500);    

                function fadeInOut(intervalId){

                    var img = document.getElementById('img');
                    var opt = img.getAttribute('opt');
                    var dur = img.getAttribute('dur');


                    opt = parseFloat(opt) +  parseFloat(dur);

                    img.setAttribute('opt',opt);
                    img.style.opacity = opt;

                    console.log("opt:"+opt);

                    if(opt<=0 || opt>=1){
                        dur = -dur;
                        img.setAttribute('dur',dur);

                        clearInterval(intervalId)
                        var srcs = [
                            './imgs/1.jpg',
                            './imgs/2.jpg',
                            './imgs/3.jpg'
                        ]

                        var current = img.getAttribute('current');
                        current++;
                        img.setAttribute('current',current);
                        img.src  = srcs[current%3]

                        var intervalId = setInterval(function(){
                            fadeInOut(intervalId);
                        },200);
                    }    
                }

                //1 
                var intervalId = setInterval(function(){
                    fadeInOut(intervalId);
                },200);

            }

            function rmChangeOpti(){
                //清除定时器
                clearTimeout(timeid)
            }
        </script>
        <img id="img" src="./imgs/1.jpg" opt="1" current="0" dur="-0.1"> <br/>

        <button onclick="changeOpti()">改变透明度</button>
        <button onclick="rmChangeOpti()">取消改变透明度</button>

    </body>
</html>
```

onload:网页加载完成(dom节点、流媒体图片)

# [8. 系统常用函数](https://jshand.gitee.io/#/course/front/javascript?id=_8-系统常用函数)

## [8.1 Date](https://jshand.gitee.io/#/course/front/javascript?id=_81-date)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>

            window.onload = function(){

                setInterval(function(){
                    console.log('time refresh');

                    //获取当前时间
                    var now = new Date();
                    //获取年月日、时分秒
                    // console.log("now.getYear():"+now.getYear())
                    // console.log("now.getFullYear():"+now.getFullYear())
                    //2020-11-4 15:33:34 
                    var  year = now.getFullYear();
                    var  month = now.getMonth();
                    var  date = now.getDate();

                    var  hour = now.getHours();
                    var  min = now.getMinutes();
                    var  secs = now.getSeconds();

                    var time = year +"-"+(month+1) +"-"+date+" "+hour +":"+min+":"+secs;

                    // 设置到div中
                    // console.log( ='time');
                    document.getElementById('time').innerText =time

                },1000)
            }


        </script>
    </head>
    <body>

        <div id="time">2020-11-4 15:33:34</div>

    </body>
</html>
```

## [8.2 Math](https://jshand.gitee.io/#/course/front/javascript?id=_82-math)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>


        <script>
            // console.log(Math.E)
            // console.log(Math.PI)

            console.log("Math.max(100,50):"+Math.max(100,50))
            console.log("Math.min(100,50):"+Math.min(100,50))
            console.log("Math.abs(-10):"+Math.abs(-10))


            console.log("Math.random()："+Math.random() * 100)
            console.log("Math.random()："+Math.random() * 100)
            console.log("Math.random()："+Math.random() * 100)
            console.log("Math.random()："+Math.random() * 100)
            console.log("Math.random()："+Math.random() * 100)
            console.log("Math.random()："+Math.random())
            console.log("Math.random()："+Math.random())
            console.log("Math.random()："+Math.random())

            console.log("Math.round(15.6):"+Math.round(15.6))


            console.log("Math.ceil(10.01): "+Math.ceil(10.01))
            console.log("Math.ceil(10.1): "+Math.ceil(10.1))
            console.log("Math.ceil(10.2): "+Math.ceil(10.2))
            console.log("Math.ceil(10.6): "+Math.ceil(10.8))
            console.log("Math.ceil(10.7): "+Math.ceil(10.9))



            console.log("Math.floor(10.01): "+Math.floor(10.01))
            console.log("Math.floor(10.1): "+Math.floor(10.1))
            console.log("Math.floor(10.2): "+Math.floor(10.2))
            console.log("Math.floor(10.6): "+Math.floor(10.8))
            console.log("Math.floor(10.7): "+Math.floor(10.9))

        </script>



    </head>
    <body>
    </body>
</html>
```

## [8.3.综合实战](https://jshand.gitee.io/#/course/front/javascript?id=_83综合实战)

使用定时器和Date类型实现数字时钟

![img](https://jshand.gitee.io/imgs/html-css-js/2021-08-04_173417.png)

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title>定时器</title>

        <style>
            body{
                padding:0;
                margin:0;
                height: 100vh;
                width: 100vw;
                overflow: hidden;

            }
            .container{
                width: 100%;
                height: 100%;
                display: flex;
                align-items:center;
                justify-content:center;
                background-color: #122629;

            }
            .time{
                flex:1;
                font-size: 5em;
                /* border:solid 1px red; */
                color: #00DEFF;
                align-items: center;
                text-align: center;
            }
            /**工具条*/
            .tools{
                position: fixed;
                left:30px;
                top:30px

            }

        </style>

<script>
    Date.prototype.formart = function(){

        let month = (this.getMonth(+1))<=9?  "0"+(this.getMonth()+1) : this.getMonth()+1;
        let date = this.getDate()<=9 ? "0"+this.getDate():this.getDate();
        let hour = this.getHours()<=9 ? "0"+this.getHours():this.getHours();
        let minutes = this.getMinutes()<=9 ? "0"+this.getMinutes():this.getMinutes();
        let seconds = this.getSeconds()<=9 ? "0"+this.getSeconds():this.getSeconds();    

        let time = this.getFullYear() +"-"+month+"-"+date+" "+hour+":"+minutes+":"+seconds;

        return time;
    }



    function showtime(){
        let now = new Date();
        document.getElementById("time").innerText = now.formart();

        //定时器

    }


    //延迟多少 毫秒之后 执行某一个方法
    // let timeoutId = setTimeout(function(){
    //     console.log("setTimeout")
    // },2000);

    // clearTimeout(timeoutId);


    let timer ;  //1 2  3 4 5 

    function starttime(btn){
        timer = setInterval(function(){
            let now = new Date();
            document.getElementById("time").innerText = now.formart()
        },1000);

        btn.disabled="disabled"
    }

    //停止刷新时间
    function stopTime (){
        if(timer){
            document.getElementById("btnStart").removeAttribute("disabled")
            clearInterval(timer);//5
        }else{
            alert("没有开始时钟")
        }

    }

</script>

    </head>
    <body>
        <div class="tools" >
            <input type="button" value="刷新"  onclick="showtime()"/>  
            <input type="button" value="开始" id="btnStart"   onclick="starttime(this)"/>  
            <input type="button" value="停止"  onclick="stopTime()"/>  
        </div>

        <div class="container" >

            <div class="time" id="time">2021-08-04 14:16:01</div>

        </div>


    </body>
</html>
```

# [9.综合应用实战](https://jshand.gitee.io/#/course/front/javascript?id=_9综合应用实战)

## [9.1 购物车](https://jshand.gitee.io/#/course/front/javascript?id=_91-购物车)

```html
<!DOCTYPE html>
<html>
<head>
    <title>购物车</title>
    <meta charset="utf-8" />
    <style type="text/css">
        h1 {
            text-align:center;
        }
        table {
            margin:0 auto;
            width:60%;
            border:2px solid #aaa;
            border-collapse:collapse;
        }
        table th, table td {
            border:2px solid #aaa;
            padding:5px;
        }
        th {
            background-color:#eee;
        }
    </style>

    <script type="text/javascript">
        function add_shoppingcart(btn){
            var tr = btn.parentNode.parentNode;
            var goodsName = tr.children[0].innerText;
            var price = tr.children[1].innerText;

            var exists = false; //是否存在形同商品
            var goodsTd = document.getElementsByClassName('cartGoods');
            var index = -1;
            for(var i=0 ; i<goodsTd.length ; i++){
                if(goodsTd[i].innerText == goodsName){
                    exists = true;
                    index = i;
                    break;
                }
            }

            if(!exists){
                var tr = document.createElement("tr");
                var tb = document.getElementById('goods');
                tr.innerHTML = '<tr>                                                                            '+
                                '    <td class="cartGoods" >'+ goodsName     +'</td>                                                       '+
                                '    <td>'+price+'</td>                                                                 '+
                                '    <td align="center">                                                         '+
                                    '    <input type="button" value="-" onclick="decrease(this)"/>                   '+
                                    '    <input type="text" size="3" readonly value="1" class="num"/>                            '+
                                    '    <input type="button" value="+" onclick="increase(this)"/>                   '+
                                    '</td>                                                                          '+
                                    '<td>'+price+'</td>                                                                    '+
                                    '<td align="center"><input type="button" value="x" onclick="this.parentNode.parentNode.remove()"/></td>   '+
                                '</tr>      ';

                // console.log(tr.innerHTML);
                tb.appendChild(tr)
            }else{

                var inpPrice = document.getElementsByClassName('num')[index];


                inpPrice.value = parseInt(inpPrice.value) +1;

                sumLine(inpPrice) ; //计算单行的合计
            }


            sum();
        }

        //btnInCre 加号按钮
        function increase(btnInCre){

            var inpNum = btnInCre.parentNode.children[1];
            inpNum.value = parseInt(inpNum.value )+ 1;

            sumLine(inpNum);

        }

        //btnDeCre 减号按钮
        function decrease(btnDeCre){

            var inpNum = btnDeCre.parentNode.children[1];
            if(inpNum.value <=1){
                //如果数量为1 则删除行
                btnDeCre.parentNode.parentNode.remove();
            }else{        
                inpNum.value = parseInt(inpNum.value )- 1;
            }
            sumLine(inpNum);

        }




        //inpNum
        function sumLine(inpNum){
            var tr = inpNum.parentNode.parentNode;
            var num = parseFloat(inpNum.value);
            var price = parseFloat(tr.children[1].innerText);
            var oneSum  = num * price; //行的合计

            console.log(oneSum);
            tr.children[3].innerText = oneSum;

            sum();
        }



        function sum(){
            var inpNum = document.getElementsByClassName('num')

            var summ = 0;
            if(inpNum && inpNum.length>0){
                for(var i =0; i<inpNum.length ; i++){
                    summ +=  parseInt(inpNum[i].value) *  parseFloat(inpNum[i].parentNode.parentNode.children[1].innerText);
                }
            }

            // console.log("summ: "+summ);

            document.getElementById('total').innerText = summ;
        }


    </script>
</head>
<body>
<h1>真划算</h1>
<table>
    <tr>
        <th>商品</th>
        <th>单价(元)</th>
        <th>颜色</th>
        <th>库存</th>
        <th>好评率</th>
        <th>操作</th>
    </tr>
    <tr>
        <td>罗技M185鼠标</td>
        <td>80</td>
        <td>黑色</td>
        <td>893</td>
        <td>98%</td>
        <td align="center">
            <input type="button" value="加入购物车" onclick="add_shoppingcart(this)"/>
        </td>
    </tr>
    <tr>
        <td>微软X470键盘</td>
        <td>150</td>
        <td>黑色</td>
        <td>9028</td>
        <td>96%</td>
        <td align="center">
            <input type="button" value="加入购物车" onclick="add_shoppingcart(this);"/>
        </td>
    </tr>
    <tr>
        <td>洛克iphone6手机壳</td>
        <td>60</td>
        <td>透明</td>
        <td>672</td>
        <td>99%</td>
        <td align="center">
            <input type="button" value="加入购物车" onclick="add_shoppingcart(this);"/>
        </td>
    </tr>
    <tr>
        <td>蓝牙耳机</td>
        <td>100</td>
        <td>蓝色</td>
        <td>8937</td>
        <td>95%</td>
        <td align="center">
            <input type="button" value="加入购物车" onclick="add_shoppingcart(this);"/>
        </td>
    </tr>
    <tr>
        <td>金士顿U盘</td>
        <td>70</td>
        <td>红色</td>
        <td>482</td>
        <td>100%</td>
        <td align="center">
            <input type="button" value="加入购物车" onclick="add_shoppingcart(this);"/>
        </td>
    </tr>
</table>



<h1>购物车</h1>
<table>
    <thead>
    <tr>
        <th>商品</th>
        <th>单价(元)</th>
        <th>数量</th>
        <th>金额(元)</th>
        <th>删除</th>
    </tr>
    </thead>
    <tbody id="goods">

 </tbody>
    <tfoot>
    <tr>
        <td colspan="3" align="right">总计</td>
        <td id="total"></td>
        <td></td>
    </tr>
    </tfoot>
</table>
</body>
</html>
```

## [9.2. 选项卡实现](https://jshand.gitee.io/#/course/front/javascript?id=_92-选项卡实现)

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <style>
            .container{
                position :relative;
                display: inline-block;
                width: 800px;
                height: 500px;
                /* border: solid 1px #aaaaff; */
                box-sizing: border-box ;
            }

            .tabcontainer{
                width: 100%;
                height: 40px;
                border: solid 1px #00FFFF;
            }

            .contentcontainter{
                position: relative;
                border: solid 3px red;
                width: calc(100% - 3px);
                height: calc(100% - 46px);
            }


            .content{
                width: 100%;
                height: 100%;
                position: absolute;
            }


            .hide{
                display: none;
            }
            .active{
                display: block ;
            }

            .tabcontainer span{
                display: inline-block;
                padding: 5px 12px ;
                height: 100%;
                margin: 0;
                line-height: 40px;
                border: solid 1px #EEEEEE;
                cursor: pointer;
            }

            .selectTab{
                background-color: #AAAAAA;
            }


        </style>

        <script>

            function switchTab(current){
                // alert("current:"+current);

                var contents = document.getElementsByClassName('content');
                for(var i=0 ;i<contents.length ;i++ ){
                    var content =  contents[i];
                    var index = content.getAttribute("index");
                    if(current == index){
                        content.className ='content  active';
                    }else{
                        content.className ='content  hide';
                    }
                }

                var span = document.getElementsByTagName('span');
                for(var i=0 ;i<span.length ;i++){
                    if( (i+1) == current){
                        span[i].className = 'selectTab';
                    }else{
                        span[i].className = '';
                    }
                }

            }
        </script>
    </head>
    <body>


    <div class="container">
        <div class="tabcontainer">


            <span onclick="switchTab(1)" class="selectTab"> 首页 </span>
            <span onclick="switchTab(2)"> 购物车 </span>
            <span onclick="switchTab(3)"> 会员中心 </span>



        </div>


        <div class="contentcontainter">
            <div class="content  active  " index="1" style="background-color: #00FFFF;">
                111
            </div>

            <div class="content  hide " index="2" style="background-color: #6b6bff;">
                2222


            </div>


            <div class="content hide" index="3" style="background-color: #00aa7f;">
                3333


            </div>
        </div>


    </div>






    </body>
</html>
```

7 浏览器bom对象

JavaScript Window JavaScript Window Screen JavaScript Window Location JavaScript Window History JavaScript Navigator