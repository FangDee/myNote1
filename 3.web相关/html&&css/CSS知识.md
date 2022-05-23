# CSS知识

## 1.css入门

### 1.1 CSS概念

css(cascading Style Sheets)层叠样式表(样式可以叠加)，是一种用来表示HTML语言的文件样式的计算机语言。

最新版本为CSS3（能做到网页和内容分离）。**CSS是用来美化网页用的，没有网页则CSS毫无用处，css需要依赖HTML展示其功能。**

>1.虽然HTML标签的某些属性可以用来设置样式，但是细节处理不够好1
>
>2.HTML实现样式效果会出现大量重复代码，维护成本高！

### 1.2 css基本语法

css样式是由选择器和一条或者多条的以**分号隔开**的样式声明组成，每条声明的样式包含一个CSS属性和属性值。

![](CSS知识.assets\Snipaste_2021-02-22_22-32-10.png)

~~~html
选择器名{
    属性：属性值;
    ......
}
~~~

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style>
        body {
           background-color: antiquewhite;/*背景颜色*/
           color: red;/*字体颜色*/
           font-family: "arial black";
        }
    </style>
</head>
<body>
    <!-- 
        基础语法
            1.格式
            选择器名 {
                属性：属性值;
                ......
            }
            2.样式需要设置在style标签中
            3.注意：
               ** CSS声明要以分号结尾
               ** 声明要用大括号括起来
               ** 建议一行声明写一个属性
               ** 如果属性值由多个单词组成，要给值加上引号
    -->
    HELLO
</body>
</html>
~~~

### 1.3 CSS的使用

#### 1.3.1 行内样式

行内样式将样式定义在具体的html元素的style属性中。以行内样式写的CSS耦合性高，只适合当前元素。

~~~html
<p  style = "color:red;font-size:50px">
    这是一段文字
</p>
~~~

在当前元素使用style属性的声明方式

​        style是行内样式属性；

​        color是颜色属性，red是颜色属性值

​        font-size是字体大小属性：50px是字体大小属性值

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
</head>
<body>
   <!-- 
       将样式定义在html标签上的style属性中，
       以行内样式写的CSS耦合度较高
   -->
   <p style="color: red;font-family: 楷体;">这是一段文本</p>
</body>
</html>
~~~

#### 1.3.2 嵌入式

嵌入式通过在html页面内容开辟一段属于css的代码区域，通产做法是在<head>标签中嵌套<style>标签。在<style>中通过选择器的方式调用指定的元素并且设置相关的CSS。

~~~html
<style>
    p {
       color:red;
       font-size:50px;
    }
</style>
~~~

#### 1.3.3 引入外联样式文件

在实际开发中，许多时候都使用引入外部样式文件，这种形式可以使得html页面更加清晰，而且代码复用性高！

我们需要先定义一个css文件，里面写css样式语法代码

然后在html页面引入即可！

~~~html
<link rel="stylesheet" type="text/css" href="css/style.css">
rel:当前文件与引入的文件之间的关系
type:类型， css类型
href:引入的资源的路径
~~~

**样式的作用遵循就近原则，行内样式优先级最高！**

## 2.CSS选择器

选择器是一种模式，用于选择需要添加样式的元素

### 2.1 基本选择器

#### 2.1.1 通用选择器

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
     /*
         通用选择器：
             选择所有元素：*
             格式：
                 *{
                     属性名：属性值
                 }    
     
     
     */
     *{
         color:blue;
     }
    </style>
</head>
<body>
    <div>div1</div>
    <div>div2</div>
    <div>div3</div>
    <p>p</p>
</body>
</html>
~~~

#### 2.1.2 元素选择器

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
     /*
         元素选择器/标签选择器：
             选择指定元素：*
             格式：
                 标签名{
                     属性名：属性值
                 }    
     
     
     */
     div {
         color:blue;
         width:200px;
         height:200px;
         background-color: crimson;
     }
    </style>
</head>
<body>
    <div>div1</div>
    <div>div2</div>
    <div>div3</div>
    <p>p</p>
</body>
</html>
~~~

#### 2.1.3 ID选择器

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
     /*
         ID选择器：
             选择指定ID属性值的元素：*
             格式：
                 #id属性值{
                     属性名：属性值
                 }    
            注意：
                id值最好保持唯一
                id以字母数字下划线中划线组成，不要以数字开头
     
     */
     #div1 {
         color:blue;
         width:200px;
         height:200px;
         background-color: crimson;
     }
     #div2 {
         color:blue;
         width:200px;
         height:200px;
         background-color: cyan;
     }
    </style>
</head>
<body>
    <div id="div1">div1</div>
    <div id="div2">div2</div>
    <div>div3</div>
    <p>p</p>
</body>
</html>
~~~

#### 2.1.4 类选择器

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
     /*
         Class类选择器
               选择设置指定class属性值的元素
               。class属性值 {
                   属性名：属性值；
               }

     
     */
    .cls1 {
        font-weight: bold;/* 字体粗细*/
    }
    </style>
</head>
<body>
    <div class="cls1">div1</div>
    <div id="div2">div2</div>
    <div class="cls1">div3</div>
    <p>p</p>
</body>
</html>
~~~



#### 2.1.5  分组选择器

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
     /*
         分组选择器
               选择指定选择器选中的元素
               选择器1，选择器2，选择器3，... {
                   属性名：属性值；
               }

     
     */
    #div2,.cls1 ,p{
        width: 200px;
        height: 200px;
        font-weight: bold;/* 字体粗细*/
        /* 边框： 边框的粗细 边框的风格 边框的颜色*/
        border: 1px solid #0000FF;
    }
    </style>
</head>
<body>
    <div class="cls1">div1</div>
    <div id="div2">div2</div>
    <div class="cls1">div3</div>
    <p>p</p>
</body>
</html>
~~~

#### 2.1.6 优先级问题

>选择器的优先级（权重值）
>
>ID选择器 100  --> 类选择器 10 -->元素选择器 1 -->通过选择器
>
>行内样式style属性中 权重是1000

### 2.2 组合选择器

组合选择器说明了两个选择器直接的关系，css中包含四种：

后代选择器：以空格分隔

子选择器：以大于号分隔

相邻兄弟选择器：以加号分隔 （只找下面的）

普通兄弟选择器：以波浪号分隔 （只找下面的）

#### 2.2.1 后代选择器

用于选择指定元素标签下的后辈元素，以空格分隔

选择器1 选择器2 {

​        属性名：属性值；

}

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
    </style>
</head>
<!--
    后代选择器：‘
        选择指定元素的所有后代元素
        指定元素  指定自带元素 {
            属性名：属性值；
        }
-->   
    <style>
      .food  li {
         background-color:darkorange;
      
      
      }
    </style>
      
<body>
     <h1>食物</h1>
     <ul class="food">
         <li>水果
             <ul>
                 <li>香蕉</li>
                 <li>苹果</li>
                 <li>梨子</li>
             </ul>
         </li>
         <li>蔬菜
            <ul>
                <li>白菜</li>
                <li>油菜</li>
                <li>卷心菜</li>
            </ul>
        </li>



     </ul>
</body>
</html>
~~~

#### 2.2.2 子代选择器

选择指定元素的子代选择器，只找一级，以大于号分隔

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
    </style>
</head>
<!--
    后代选择器：‘
        选择指定元素的所有后代元素
        指定元素  指定自带元素 {
            属性名：属性值；
        }
-->   
    <style>
      .food > li {
         border: 1px solid red
      }
    </style>
      
<body>
     <h1>食物</h1>
     <ul class="food">
         <li>水果
             <ul>
                 <li>香蕉</li>
                 <li>苹果</li>
                 <li>梨子</li>
             </ul>
         </li>
         <li>蔬菜
            <ul>
                <li>白菜</li>
                <li>油菜</li>
                <li>卷心菜</li>
            </ul>
        </li>



     </ul>
</body>
</html>
~~~

#### 2.2.3 相邻兄弟选择器

选择指定元素的相邻的下一个指定元素（<font style="color:red">**只会向下找一个**</font>）以加号分隔 

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
    </style>
</head>
<!--
    相邻兄弟选择器：‘
        选择指定元素的相邻的下一个指定元素 只会向下找一个
        如果下一个不是指定的元素，则失效
-->   
    <style>
      #d + div {
          color:red;
      }
    </style>
      
<body>
     <div id="d">
     相邻兄弟选择器
          <ul>
              <li>开心麻花</li>
              <li>贾玲</li>
              <li>松下小宝</li>
          </ul>
     </div>
     <div>相邻兄弟选择器2</div>
     <div>相邻兄弟选择器3</div>
</body>
</html>
~~~

#### 2.2.4 普通兄弟选择器

选择指定元素后的指定同级元素（只会向下找）以波浪号分隔 

~~~java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
    </style>
</head>
<!--
    普通兄弟选择器：‘
        选择指定元素的后面的指定兄弟元素 
-->   
    <style>
      #tt ~ li {
          color:red;
      }
    </style>
      
<body>
     <div id="d">
     相邻兄弟选择器
          <ul>
              <li>张三</li>
              <li id="tt">开心麻花</li>
              <li>贾玲</li>
              <li>松下小宝</li>
          </ul>
     </div>
</body>
</html>
~~~

## 3.CSS常用属性

### 3.1 背景

~~~java
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
    </style>
</head>
<!--
    普通兄弟选择器：‘
        选择指定元素的后面的指定兄弟元素 
-->   
    <style>
      /*

      */
      body {
          /*
          设置元素的背景颜色
          */
          background-color: darkred;
           /*
          设置元素的背景图片，默认图片重复显示
          */
          background-image:url(../img/man03.jpg) ;
          /*
          设置背景图片是否重复
          no-repeat:不重复
          repeat-x:横向重复
          repeat-y:纵向重复
          repeat：纵向横向都重复
          */
           /*
          设置元素的背景图片大小
          */
          background-size: 200px 200px;
      }
    </style>
      
<body>

</body>
</html>
~~~

### 3.2 文本

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
    </style>
</head> 
    <style>
      /*
       文本：
      */
      #div1 {
          /* color:字体颜色
            取值：
                1.颜色单词
                2.#0000FF
                3.rgb(255,0,0)
          */
          color: blue;
          /*text-align:对齐方式：
             取值：
                 1.left:左对齐
                 2.right:右对齐
                 3.center:居中
          */
          text-align: center;
          /*text-decoration:文本修饰：上划线，下划线，中划线等效果
             取值：
                1.overline:上划线
                2.underline:下划线
                3.line-through:中划线
                4.none:没有效果
          
          */
          /* 同时设置元素的上中下划线*/
          text-decoration: overline underline line-through;
      }

      p {
          /*text-indent:设置首行缩进
          em：一个文字的宽度
          */
          text-indent: 2em;
      }
    </style>
      
<body>
     <div id="div1">文本1</div>
     <div>文本2</div>
     <div>文本3</div>
     <p>这是一段话哈哈哈哈</p>
</body>
</html>
~~~

### 3.3 字体

包括字体效果,大小,风格,粗细

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
    </style>
</head> 
    <style>
        p {
            /*
            设置字体：
                font-family（一般针对部分文字）：设置字体效果
                    1.当font-family的属性包含空格或者特殊字符时，需要将属性值用引号引起来
                    2.font-family的后备机制：可以为元素设置多种字体：，当浏览器不识别第一种字体时，会尝试找下一种字体
                    3.当font-family的属性值有多个时，使用逗号隔开
                font-size:设置字体大小
                font-style:设置字体风格
                     1.normal:正常体
                     2.italic:斜体
                     3.oblique:强制倾斜
                font-weight:设置字体的粗细
                     1.bold:粗体
                     2.100-900 值越大，字体越粗
                       400 正常字体
                       700 粗体
           */
            font-family: 楷体;
            font-family: 楷体,宋体;
            font-size: 30px;
            font-style:italic;
            font-weight:700;
        }
    </style>
      
<body>
     <p>这是一段话哈哈哈哈</p>
</body>
</html>
~~~

### 3.4 对齐方式

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
    </style>
</head> 
<!-- 
    对齐方式（一般针对一段话）：
        设置元素中字体的水平方式的对齐方式
        text-align:
             left
             right
             center
             justify:左右对齐
-->
    <style type="text/css">
       p {
           text-align: center;
       }
        
    </style>
      
<body>
     <p>全国人大代表李长青谈到如何拉动内需、促消费时表示：建议延长一些节假日或增设法定节假日，恢复小长假</p>
</body>
</html>
~~~

### 3.5 display

可以设置一个元素的生成效果！

**规定元素生成框的类型,可以让元素的显示性值发生改变：如行内元素可以变为块级元素**

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Document</title>
    <style   type="text/css">
    </style>
</head> 
<!-- 
    块级元素：
        可以设置元素的宽高和边距，元素会自动换行。前后有换行符
    行内元素：
        不可以设置元素的宽高和边距，行内元素不会自动换行，前后没有换行符
    行内块级元素
         可以设置宽高，元素不会自动换行
    
    display属性：
         规定元素生成框的类型
         block:        元素会被显示，且元素会被显示为块级元素，元素前后会有换行符
         none:         元素会被隐藏
         inline:       元素会被显示为行内元素
         inline-block   行内块级元素
-->
    <style type="text/css">
       #span2 {
           /*隐藏元素*/
           display: none;
       }
       #span2 {
           /*隐设置元素显示且变成块级元素*/
           display: block;
       }
        
    </style>
      
<body>
    <span id="span1">
        这是一个span1
    </span>
    <span id="span2">
        这是一个span2
    </span>
    <span id="span3">
        这是一个span3
    </span>
<body
</html>
~~~

### 3.6 浮动

**只在水平方向浮动**

~~~html
<!-- 
    浮动
        flost的属性值有none,left,right,
    注意：
        1，只有横向浮动，没有纵行浮动
        2.会将元素的display属性变为block
        3.浮动元素的后一个元素会围绕着浮动元素（典型应用是文字围绕图片）
        4.浮动元素的前一个元素不会受到影响
 
-->
~~~

### 3.7盒子模型

#### 3.7.1border

>也就是框：边框有宽度，样式（是否是实心线），颜色

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>边框</title>
    <!--
         边框border:
            1. 可以设置元素边框的宽度，颜色，类型
            2. 单独设置边框的颜色，宽度，类型
                border-color
                border-width
                border-style:
                      dotted:点状线
                      dashed：虚线
      3.设置属性值的顺序是：上右下左
        设置一个属性：表示边框四边属性一致
        设置两个属性：表示上下一致，左右一致
        设置三个属性：表示上，右，下不一致，左右一致
      4.border-collapse:
        设置是否将表格边框折叠为单一边框
        属性值：
             separate(默认。单元格边框独立)
            collapse(单元格边框合并)
    -->   
    <style>
       #div1 {
           border: 1px #0000FF dotted;
       }
       #div2 {
        border-color:antiquewhite;
        border-width:2px;
        border-style:dashed;
       }

       #div3 {
        border-color:antiquewhite;
        border-width:2px;
        border-style:solid dashed dotted none;
       }
       
    </style>
</head> 

<body>
     <div id="div1">
         这是一段文本
     </div>
     <hr>
     <div id="div2">
        这是一段文本
    </div>
    <hr>
     <div id="div3">
        这是一段文本
    </div>
<body
</html>
~~~

![](CSS知识.assets/Snipaste_2021-03-07_20-07-54.png)

#### 3.7.2padding

>内容距离边框的距离，简称内边距
>
>可以设置元素所有内边框的宽度，或者设置各边上内边距的宽度

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>边框</title>
    <!--
        padding：设置内边距
        设置一个值：表示四个方向相同
        设置两个值：表示上下一致，左右一致
        设置三个值：表示上，右，下不一致，左右一致
        设置四个值：表示上，右，下，左都不一致
        还可以单独设置某个方向的内边距：
            padding-top
            padding-left
            padding-right
            padding-bottom  
    -->
    <style>
        td {
            padding: 10px 20px 30px;
        }
    </style>
</head> 
     <table border="1" width="500px" align="center" style="border-collapse: collapse;">
         <tr>
           <th>班级</th>
           <th>姓名</th>
           <th>性别</th>
           <th>年龄</th>
         </tr>
         <tr align="center" valign="top" bgcolor="antiquewhite">
            <td>101班</td>
            <td>张三</td>
            <td>男</td>
            <td>18</td>
         </tr>
     </table>
<body>

<body
</html>
~~~

#### 3.7.3 margin

>边框距离最外边的距离，简称外边距

~~~html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>边框</title>
    <!--
         margin:设置外边距
        设置一个值：表示四个方向相同
        设置两个值：表示上下一致，左右一致
        设置三个值：表示上，右，下不一致，左右一致
        设置四个值：表示上，右，下，左都不一致
        还可以单独设置某个方向的内边距：
            margin-top
            margin-left
            margin-right
            margin-bottom  
    -->
    <style>
       p {
           background-color: aqua;
       }
       #p1{
           margin: 100px;
       }
    </style>
</head> 
     <p>这是文本</p>
     <p id="p1">这是文本</p>
<body>

<body
</html>
~~~







