# [1. jQuery是什么](https://jshand.gitee.io/#/course/front/jquery?id=_1-jquery是什么)

jQuery是一个JavaScript函数库。

jQuery是一个轻量级的"写的少，做的多"的JavaScript库。

jQuery库包含以下功能：

- HTML 元素选取
- HTML 元素操作
- CSS 操作
- HTML 事件函数
- JavaScript 特效和动画
- HTML DOM 遍历和修改
- AJAX
- Utilities

> **提示：** 除此之外，Jquery还提供了大量的插件。

# [2. jQuery的安装](https://jshand.gitee.io/#/course/front/jquery?id=_2-jquery的安装)

使用script标记引入jquery类库即可:

 1 使用cdn

```html
<script src="http://code.jquery.com/jquery-2.1.1.min.js"></script>
```

 2.使用本地化的文档

```html
<script src="js/jquery.min.js"> </script>
```

# [3.语法](https://jshand.gitee.io/#/course/front/jquery?id=_3语法)

通过 jQuery，您可以选取（查询，query） HTML 元素，并对它们执行"操作"（actions）。

## [3.1 jQuery 语法](https://jshand.gitee.io/#/course/front/jquery?id=_31-jquery-语法)

jQuery 语法是通过选取 HTML 元素，并对选取的元素执行某些操作。

基础语法： **$(selector).\*action\*()**

- 美元符号定义 jQuery
- 选择符（selector）"查询"和"查找" HTML 元素
- jQuery 的 action() 执行对元素的操作

实例:

- $(this).hide() - 隐藏当前元素

- $("p").hide() - 隐藏所有 元素

- $("p.test").hide() - 隐藏所有 class="test" 的 元素

- $("#test").hide() - 隐藏 id="test" 的元素

  ```html
  <!DOCTYPE html>
  <html>
      <head>
          <meta charset="utf-8" />
          <title></title>
  
          <script src="js/jquery.min.js"></script>
          <!-- <script src="http://libs.baidu.com/jquery/1.11.1/jquery.min.js"></script> -->
          <style>
              #d1 span{
                  color: red;
              }    
          </style>
      </head>
      <body>
          <div id="d1">block1
              <span>span1111</span>    
          </div>
  
          <script>
              function getEl(){
                  // document.getElementById('d1').innerText
                  // document.getElementById('d1').children[0].innerText
                  //$('#d1').text()
                  alert($('#d1 span').text())
              }
          </script>
          <button onclick="getEl()">根据id获取元素的文本</button>
      </body>
  </html>
  ```

  通过$选择的元素已经是 jQuey对象，只能调用jQuery的方法或属性。

> jquery对象转换成 原生的dom对象 1 通过get方法

```js
console.log($('#d1 span').get(0).innerText)
```

> 2 通过下标的形式访问原生的dom对象

```js
console.log($('#d1 span')[0].innerText)
```

原生Dom 对象转 jQuery对象,使用$()包裹下，即可转换成jQuery对象

```js
alert($(d1).text())
```

------

## [3.2 文档就绪事件](https://jshand.gitee.io/#/course/front/jquery?id=_32-文档就绪事件)

您也许已经注意到在我们的实例中的所有 jQuery 函数位于一个 document ready 函数中：

$(document).ready(function(){ // 开始写 jQuery 代码... });

这是为了防止文档在完全加载（就绪）之前运行 jQuery 代码，即在 DOM 加载完成后才可以对 DOM 进行操作。

如果在文档没有完全加载之前就运行函数，操作可能失败。下面是两个具体的例子：

- 试图隐藏一个不存在的元素
- 获得未完全加载的图像的大小

**提示：**简洁写法（与以上写法效果相同）:

$(function(){ // 开始写 jQuery 代码... });

以上两种方式你可以选择你喜欢的方式实现文档就绪后执行 jQuery 方法。

```js
$(document).ready(function(){
     alert('文档加载成功')
})

// 或 简便写法
$(function(){
    alert('文档加载成功2')
})
```

![img](https://jshand.gitee.io/imgs/jquery/20171231003829544.jpeg)

# [4.选择器](https://jshand.gitee.io/#/course/front/jquery?id=_4选择器)

jquery通过选择器查找页面上的元素

## [4.1 基本](https://jshand.gitee.io/#/course/front/jquery?id=_41-基本)

*

element

id

class

[selector1,selector2,selectorN]

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <!--  引入jQuery文档-->
        <script src="../js/jquery.min.js"></script>
        <script>

            $(function(){
                //1 * 选择所有元素
                //console.log($('*'));

                //2 标签选贼气
                // console.log('div: '+($('div').length))
                // console.log('div: '+($('div').size()))

                //3 类选择器
                //$('.c1').css("color","red");
                //console.log('class: '+($('.c1').size()))

                //4 并集选择器
                $('.c1,#d1').css('color','green')

            })

        </script>
    </head>
    <body>

        <div>block11</div>
        <div>block12</div>

        <div class="c1" >block33</div>
        <div class="c1" >block44</div>

        <div id="d1" >block555</div>

    </body>
</html>
```

## [4.2 层级](https://jshand.gitee.io/#/course/front/jquery?id=_42-层级)

> ancestor descendant 查找所有子元素 parent > child 直接子元素 ,不包括间接子元素

```html
<script>
    $(function(){
        //ancestor descendant 所有子元素，包括间接的子元素
        console.log($('.container div').get())
        //parent > child 直接子元素 ,不包括间接子元素
        console.log($('.container>div').get())
})
</script>
<div class="container">

    <div class="head" >
        head
    </div>

    <div class="content" >
        <div > content1</div>
        <div > content2</div>
        <div > content3</div>
    </div>
</div>
```

> prev + next 查找后面兄弟(紧挨着的)节点 prev ~ siblings 查找后面的所有兄弟节点

```html
<script>
    $(function(){
        //prev + next 查找后面兄弟(紧挨着的)节点
        console.log($('span+a').get())
        // prev ~ siblings  查找后面的所有兄弟节点
        console.log($('span~a').get())
    })

</script>
<span> span1</span> <a>link1</a>  <a>link11</a> 
<span> span2</span> <a>link2</a>
<span> span3</span> <a>link3</a>
<span> span4</span> <a>link4</a>
```

## [4.3 基本筛选器](https://jshand.gitee.io/#/course/front/jquery?id=_43-基本筛选器)

### [4.3.1 each方法](https://jshand.gitee.io/#/course/front/jquery?id=_431-each方法)

```html
<ul>
            <li>鱼香肉丝0</li>
            <li>鱼香肉丝1</li>
            <li>鱼香肉丝2</li>
            <li>鱼香肉丝3</li>
            <li>鱼香肉丝4</li>
            <li>鱼香肉丝5</li>
            <li>鱼香肉丝6</li>
            <li>鱼香肉丝7</li>
            <li>鱼香肉丝8</li>
            <li>鱼香肉丝9</li>
        </ul>html

<script>
//each方法用于迭代
$('li').each(function(i,el){
    //i 代表遍历的 序号，
    //el 代表上下文对象(每一个匹配的元素)
    //this 此处this 是DOM原生对象
    //console.log(i,el,$(this).text())

})
</script>
```

### [4.3.2 基本筛选器的使用](https://jshand.gitee.io/#/course/front/jquery?id=_432-基本筛选器的使用)

> :first 获取第一个元素
>
> :not(selector) 去除所有与给定选择器匹配的元素
>
> :even 匹配所有索引值为偶数的元素，从 0 开始计数
>
> :odd 匹配所有索引值为奇数的元素，从 0 开始计数
>
> :eq(index) 匹配一个给定索引值的元素
>
> :gt(index) 匹配所有大于给定索引值的元素
>
> :lang1.9+ 选择指定语言的所有元素。
>
> :last 获取最后个元素
>
> :lt(index) 匹配所有小于给定索引值的元素
>
> :header 匹配如 h1, h2, h3之类的标题元素
>
> :animated 匹配所有正在执行动画效果的元素
>
> :focus 匹配当前获取焦点的元素
>
> :root1.9+ 选择该文档的根元素。 在HTML中，文档的根元素，和$(":root")选择的元素一样， 永远是元素。
>
> :target1.9+ 选择由文档URI的格式化识别码表示的目标元素。 如果文档的URI包含一个格式化的标识符，或hash（哈希）， 然后:target选择器将匹配ID和标识符相匹配的元素。 例如，给定的URI https://example.com/#foo， $( "p:target" )，将选择
>
> 元素。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <!--  引入jQuery文档-->
        <script src="../js/jquery.min.js"></script>
        <script>
            $(function(){


                //each方法用于迭代
                $('li').each(function(i,el){
                    //i 代表遍历的 序号，
                    //el 代表上下文对象(每一个匹配的元素)
                    //this 此处this 是DOM原生对象
                    //console.log(i,el,$(this).text())

                })


                //li:first 第一个元素
                console.log('li:first',$('li:first').text())

                //li:last 最后一个
                console.log('li:last',$('li:last').text())

                //li:eq(5) eq 获取索引所在的元素
                console.log('li:eq(5)',$('li:eq(5)').text())
                console.log("-------")
                //:even :odd 代表奇数和偶数
                console.log('li:even',$('li:even').text())
                console.log('li:odd',$('li:odd').text())


                //:gt :lt 
                console.log('li:gt(4)',$('li:gt(4)').text())
                console.log('lt(4)',$('li:lt(4)').text())

            })

        </script>
    </head>
    <body>

        <ul>
            <li>鱼香肉丝0</li>
            <li>鱼香肉丝1</li>
            <li>鱼香肉丝2</li>
            <li>鱼香肉丝3</li>
            <li>鱼香肉丝4</li>
            <li>鱼香肉丝5</li>
            <li>鱼香肉丝6</li>
            <li>鱼香肉丝7</li>
            <li>鱼香肉丝8</li>
            <li>鱼香肉丝9</li>
        </ul>

    </body>
</html>
```

## [4.4 内容](https://jshand.gitee.io/#/course/front/jquery?id=_44-内容)

> :contains(text) 匹配包含给定文本的元素
>
> :empty 匹配所有不包含子元素或者文本的空元素
>
> :has(selector) 匹配含有选择器所匹配的元素的元素
>
> :parent 匹配含有子元素或者文本的元素

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <!--  引入jQuery文档-->
        <script src="../js/jquery.min.js"></script>
        <script>

            $(function(){
                //contains 包含文字
                console.log($('div:contains("block12")')[0].outerHTML)
                console.log($('div:contains("block12")')[0].innerHTML)

                //:empty
                $('p:empty').each(function(i,el){
                    console.log(el.outerHTML)
                })

                //匹配    含有选择器所匹配的元素（子元素） 的元素
                console.log($('div:has(.mysapn)')[0].outerHTML)


                //:parent 查找包含子元素的 元素
                console.log('size',$("td:parent").length);
                console.log('html',$("td:parent").html());
                console.log('text',$("td:parent").text());

            })

        </script>
    </head>
    <body>

        <div>block1123333</div>
        <div>block12</div>
        <div >block333
            <span class="mysapn"></span>
        </div>

        <p></p>


        <table>
          <tr><td>Value 1</td><td></td></tr>
          <tr><td>Value 2</td><td></td></tr>
        </table>

    </body>
</html>
```

## [4.5 可见性](https://jshand.gitee.io/#/course/front/jquery?id=_45-可见性)

> :hidden 匹配所有不可见元素，或者type为hidden的元素
>
> :visible 匹配所有的可见元素

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <style>
            div{
                background-color: aliceblue;
                margin: 5px;
            }
        </style>
        <script src="../js/jquery.min.js"></script>
        <script>
            $(function(){
                //不可见的元素
                console.log($(":hidden").length);
                console.log($("input:hidden")[0].outerHTML);
                console.log($("div:hidden")[0].outerHTML);

                //visible 可见的元素
                console.log($("div:visible").text());

            })
        </script>
    </head>
    <body>
        <input type="hidden" />
        <div style="display: none;">d1</div>
        <div>d2</div>
        <div>d3</div>

    </body>
</html>
```

## [4.6 属性](https://jshand.gitee.io/#/course/front/jquery?id=_46-属性)

> [attribute] 匹配包含给定属性的元素
>
> [attribute=value] 匹配给定的属性是某个特定值的元素
>
> [attribute!=value] 匹配所有不含有指定的属性，或者属性不等于特定值的元素。
>
> [attribute^=value] 匹配给定的属性是以某些值开始的元素
>
> [attribute$=value] 匹配给定的属性是以某些值结尾的元素
>
> [attribute*=value] 匹配给定的属性是以包含某些值的元素
>
> [attrSel1] [attrSel2] [attrSelN] 复合属性选择器，需要同时满足多个条件时使用。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <!--  引入jQuery文档-->
        <script src="../js/jquery.min.js"></script>
        <script>

            $(function(){
                console.log($('[type=password]').val());        

                //查找是否包含属性为class的元素
                $('[class]').css({
                    'color':'red'
                })

                // $('input[type!=password]').css({
                //     'color':'red'
                // })

                //查找name属性以a开头
                // $('input[name^=a]').css({
                //     'color':'red'
                // })    
                //查找name属性以a结束
                // $('input[name$=name]').css({
                //     'color':'red'
                // })

                //查找name属性包含a
                // $('input[name*=a]').css({
                //     'color':'red'
                // })

            })

        </script>
    </head>
    <body>

        用户名: <input type="text"  name="username" /> <br/>
        昵称: <input type="text" name="nickname"/><br/>
        密码: <input type="password" name="password" value="123456" /><br/>
        地址:<input type="text" name="address"/><br/>
        <input type="text" name="gender"/><br/>

        <input type="text" class="aaa" /><br/>
        <input type="text" class="aaa" /><br/>
        <input type="text" class="aaa" /><br/>

    </body>
</html>
```

## [4.7 子元素](https://jshand.gitee.io/#/course/front/jquery?id=_47-子元素)

> :first-child 匹配第一个子元素 ':first' 只匹配一个元素，而此选择符将为每个父元素匹配一个子元素
>
> :first-of-type1.9+ 结构化伪类，匹配E的父元素的第一个E类型的孩子。等价于[:nth-of-type(1)](https://jquery.cuishifeng.cn/nthOfType.html) 选择器。
>
> :last-child 匹配最后一个子元素
>
> :last-of-type1.9+
>
> :nth-child 匹配其父元素下的第N个子或奇偶元素 要匹配元素的序号，从1开始
>
> :nth-last-child()1.9+ 选择所有他们父元素的第n个子元素。计数从最后一个元素开始到第一个。
>
> :nth-last-of-type()1.9+
>
> :nth-of-type()1.9+
>
> :only-child
>
> :only-of-type1.9+

## [4.8 表单](https://jshand.gitee.io/#/course/front/jquery?id=_48-表单)

> :input 匹配所有表单控件
>
> :text 匹配所有的单行文本框
>
> :password 匹配所有密码框
>
> :radio 匹配所有单选按钮
>
> :checkbox 匹配所有复选框
>
> :submit 匹配所有提交按钮，理论上只匹配 type="submit" 的input或者button，但是现在的很多浏览器，button元素默认的type即为submit，所以很多情况下，不设置type的button也会成为筛选结果。为了防止歧义或者误操作，建议所有的button在使用时都添加type属性。
>
> :image 匹配所有图像域
>
> :reset 匹配所有重置按钮
>
> :button 匹配所有按钮
>
> :file 匹配所有文件域

## [4.9 表单对象属性](https://jshand.gitee.io/#/course/front/jquery?id=_49-表单对象属性)

> :enabled 匹配所有可用元素
>
> :disabled 匹配所有不可用元素
>
> :checked 匹配所有选中的被选中元素(复选框、单选框等，select中的option)，对于select元素来说，获取选中推荐使用
>
> :selected 匹配所有选中的option元素

# [5.筛选](https://jshand.gitee.io/#/course/front/jquery?id=_5筛选)

## [5.1 过滤](https://jshand.gitee.io/#/course/front/jquery?id=_51-过滤)

eq(index|-index)

first()

last()

hasClass(class)

filter(expr|obj|ele|fn)

is(expr|obj|ele|fn)

map(callback)

has(expr|ele)

not(expr|ele|fn)

slice(start,[end])

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <!--  引入jQuery文档-->
        <script src="../js/jquery.min.js"></script>
        <script>

            $(function(){

                // console.log($("p").eq(0))


                // .first()
                // console.log($('li').first().text());

                // console.log($('li').last().text());

                // :filter
                var len = $('li').filter(function(i){
                    //根据自定义的规则
                    console.log("i:",i)

                    return i%2 == 0;

                    // return true;
                }).length;


                console.log('len',len)

            })
        </script>
    </head>
    <body>

        <p> This is just a test.</p> <p> So is this</p>


        <ul>
            <li>list item 0</li>
            <li>list item 1</li>
            <li id="d1">list item 2</li>
            <li>list item 3</li>
            <li>list item 4</li>
            <li>list item 5</li>
        </ul>
    </body>
</html>
```

## [5.2 查找](https://jshand.gitee.io/#/course/front/jquery?id=_52-查找)

children([expr])

closest(e|o|e)1.7*

find(e|o|e)

next([expr])

nextAll([expr])

nextUntil([e|e][,f])

offsetParent()

parent([expr])

parents([expr])

parentsUntil([e|e][,f])

prev([expr])

prevAll([expr])

prevUntil([e|e][,f])

siblings([expr])

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <!--  引入jQuery文档-->
        <script src="../js/jquery.min.js"></script>
        <script>
            $(function() {
                // 查找div 下的所有直接子元素
                //console.log($("div").children())


                // $('h3').next().css('color','red');
                // $('h3').nextAll().css('color','red');

                // $('h3').prev().css('color','red');

                $('h3').prevAll().css('color', 'red');



            })
        </script>
    </head>
    <body>

        <p>Hello</p>
        <div><span>Hello Again1</span></div>
        <div>
            <span>Hello Again2</span>
            <p>And Again2
                <span>innerSpan</span>
            </p>
        </div>
        <p>And Again</p>

        <ul>
            <li>
                <a href="#">时长营销</a>
                <a href="#">经济学理论</a>
                <h3>经济管理</h3>
                <a href="#">时长营销</a>
                <a href="#">经济学理论</a>
            </li>
            <li>
                <a href="#">时长营销</a>
                <h3>经济管理</h3>
                <a href="#">时长营销</a>
            </li>
            <li>
                <a href="#">时长营销</a>
                <h3>经济管理</h3>
                <a href="#">时长营销</a>
            </li>

        </ul>





        <script>
            $(function() {
                console.log($(".div").siblings())
            })
        </script>

        <p>Hello</p>
        <div class="div"><span>Hello Again</span></div>
        <p>And Again</p>


    </body>
</html>
```

## [5.3 串联](https://jshand.gitee.io/#/course/front/jquery?id=_53-串联)

add(e|e|h|o[,c])1.9*

andSelf()1.8-

addBack()1.9+ 加入先前所选的加入当前元素中

contents()

end()

```html
<!DOCTYPE html>
<html>
  <head>
      <meta charset="utf-8">
      <title></title>
      <!--  引入jQuery文档-->
      <script src="../js/jquery.min.js"></script>
      <script>
          $(function() {
              //addBack 1.9+ 加入先前所选的加入当前元素中  
              $('div').find('p').addBack().css({
                  'border':'solid 2px #784512'
              })
          })
      </script>
  </head>
  <body>

      <div>
          <p>pra111</p>
          <p>pra2222</p>
      </div>

  </body>
</html>
```

# [6.事件](https://jshand.gitee.io/#/course/front/jquery?id=_6事件)

## [6.1页面载入](https://jshand.gitee.io/#/course/front/jquery?id=_61页面载入)

## [6.2 事件处理](https://jshand.gitee.io/#/course/front/jquery?id=_62-事件处理)

### [6.2.1 页面加载](https://jshand.gitee.io/#/course/front/jquery?id=_621-页面加载)

```
$(document).ready(function(){

})

//或
$(function(){


})
```

### [6.2.2 事件处理](https://jshand.gitee.io/#/course/front/jquery?id=_622-事件处理)

on(eve,[sel],[data],fn)1.7+ off(eve,[sel],[fn])1.7+

one(type,[data],fn) trigger(type,[data]) triggerHandler(type, [data])

> 绑定事件

```html
 <script>
 $(function(){
     //绑定事件
     $('#btn1').on('click',function(){
         alert('btn click invoke ...')
     })
 })
 </script>
 <button id="btn1" >普通按钮</button>
```

> 事件委派

```js
$(document).on('click','span',function(){
    alert('span click invoke ....')
})
```

> 完整的demo

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <!--  引入jQuery文档-->
        <script src="../js/jquery.min.js"></script>
        <script>

            // $(document).ready(function(){

            // })

            //或
            $(function(){


                 $('#btn1').on('click',function(){
                     alert('btn click invoke ...')
                 })


                $('#btn1').click(function(){
                    //alert('hello  click invoke ...');

                    var div  = document.createElement('div');

                    div.innerHTML="<span style='background-color: cadetblue;'>动态添加的块</span>"

                    document.body.appendChild(div)

                });


                //click 单独写只能给现有的元素添加事件

                // $('span').click(function(){
                //     alert('span click invoke ....')
                // });

                //解决动态添加元素的事件  ,事件的委派
                $(document).on('click','span',function(){
                    alert('span click invoke ....')
                })

            });



        </script>
    </head>
    <body>

    <!--  Vue  -->
        <button id="btn1" >普通按钮</button>
    <!-- <span style='background-color: cadetblue;cursor: pointer;'>动态添加的块</span> -->
    </body>
</html>
```

> 绑定一次事件
>
> 事件的触发、事件的移除

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script src="../js/jquery.min.js"></script>
        <script>

            $(function(){


                // $('button').click(function(){

                //     $('body').append("<span style='background-color: cadetblue;'>动态添加的块</span>");
                //     //点击只有 按钮的需要移除
                //     //off 解除事件绑定
                //     $(this).off('click')

                // })

                //one 绑定一次事件 ,trigger 触发
                $('button').one('click',function(){

                    $('body').append("<span style='background-color: cadetblue;'>动态添加的块</span>");
                }).trigger('click');

            })

        </script>
    </head>
    <body>

        <button> invoke </button>
    </body>
</html>
```

> jquery 默认支持的事件

blur([[data],fn]) change([[data],fn]) click([[data],fn]) dblclick([[data],fn]) error([[data],fn])1.8- focus([[data],fn]) focusin([data],fn) focusout([data],fn) keydown([[data],fn]) keypress([[data],fn]) keyup([[data],fn]) mousedown([[data],fn]) mouseenter([[data],fn]) mouseleave([[data],fn]) mousemove([[data],fn]) mouseout([[data],fn]) mouseover([[data],fn]) mouseup([[data],fn]) resize([[data],fn]) scroll([[data],fn]) select([[data],fn]) submit([[data],fn]) unload([[data],fn])1.8-

> 小练习
>
> 有一个块级元素div 宽 500px 高 400px ,当鼠标移动到div 上到时候 设置背景颜色，边框(css),移开的时候，删除样式

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script src="../js/jquery.min.js"></script>
        <script>
            //background-color: aqua

            $(function(){
                $('div').mouseover(function(){
                    $(this).css({
                        //'background-color':'aqua'
                        'backgroundColor':'aqua'
                    })
                }).mouseout(function(){
                    $(this).css({
                        'background-color':''
                        // 'backgroundColor':'aqua'
                    })
                })    
            })

        </script>
    </head>
    <body>

        <!-- -->
        <div style="width: 500px;height: 400px;border: solid 1px green;">12121</div>



    </body>
</html>
```

# [7.文档处理](https://jshand.gitee.io/#/course/front/jquery?id=_7文档处理)

## [7.1 元素的创建](https://jshand.gitee.io/#/course/front/jquery?id=_71-元素的创建)

 $('< div></ div>')

## [7.2内部插入](https://jshand.gitee.io/#/course/front/jquery?id=_72内部插入)

>  append(content|fn) 位置3
>
>  appendTo(content) 位置3
>
>  prepend(content|fn) 位置2
>
>  prependTo(content) 位置2

## [7.3外部插入](https://jshand.gitee.io/#/course/front/jquery?id=_73外部插入)

>  after(content|fn) 位置4
>
>  before(content|fn) 位置1
>
>  insertAfter(content)位置4
>
>  insertBefore(content)位置1

## [7.4 内部插入、外部插入样例](https://jshand.gitee.io/#/course/front/jquery?id=_74-内部插入、外部插入样例)

![img](https://jshand.gitee.io/imgs/jquery/dom-append.png)

------

![img](https://jshand.gitee.io/imgs/jquery/2020-11-07_165143.png)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script src="../js/jquery.min.js"></script>
        <script>
            $(function(){

                //将元素插入到元素内部开始的位置
                $('div').prepend('<span style="background-color: black;color:white">prepend</span>')
                // $('<span style="background-color: black;color:white">prependTo</span>').prependTo('div')


                //将元素插入到元素内部结束的位置,追加
                $('div').append('<span style="background-color: black;color:white">append</span>')
                // $('<span style="background-color: black;color:white">prependTo</span>').prependTo('div')



                //将元素插入到元素之前
                $('div').before('<span style="background-color: #996655;color:white">before</span>')
                // $('<span style="background-color: #996655;color:white">insertBefore</span>').insertBefore('div')


                // $('div').after('<span style="background-color: #996655;color:white">after</span>')
                $('<span style="background-color: #996655;color:white">insertAfter</span>').insertAfter('div')

            })


        </script>
    </head>
    <body>
        <div style="width: 300px;height: 200px;border: solid 1px green; margin: 20px 0px 0px 30px;">

        </div>

    </body>
</html>
```

## [7.5包裹](https://jshand.gitee.io/#/course/front/jquery?id=_75包裹)

>  wrap(html|ele|fn) 把所有匹配的元素用其他元素的结构化标记包裹起来。
>
>  unwrap() 这个方法将移出元素的父元素。这能快速取消 .wrap()方法的效果。匹配的元素（以及他们的同辈元素）会在DOM结构上替换他们的父元素。
>
>  wrapAll(html|ele) 将所有匹配的元素用单个元素包裹起来
>
>  wrapInner(html|ele|fn) 将每一个匹配的元素的子内容(包括文本节点)用一个HTML结构包裹起来

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script src="../js/jquery.min.js"></script>
        <script>

            $(function(){


                //每个元素包裹
                // $('a').wrap('<div></div>')

                //整体包裹
                // $('a').wrapAll(document.getElementById('p1'))



                // $('<span>span</span>').appendTo('#p1')

                //会改变文档结构
                // $('a').appendTo('#p1')


                //这个方法将移出元素的父元素。这能快速取消 .wrap()方法的效果。匹配的元素（以及他们的同辈元素）会在DOM结构上替换他们的父元素。
                // $('a').unwrap()
            })

        </script>
    </head>
    <body>


        <p>
            <a href="#">天猫</a>
            <a href="#">淘宝</a>
            <a href="#">拼多多</a>
            <a href="#">京东</a>
        </p>


        <a href="#">对</a>

        <p style="background-color: aqua;" id="p1">

        </p>


        <div>
    <p>Hello</p>
    <p>cruel</p>
    <p>World</p>
</div>

    </body>
</html>
```

## [7.6替换](https://jshand.gitee.io/#/course/front/jquery?id=_76替换)

>  replaceWith(content|fn) 将所有匹配的元素替换成指定的HTML或DOM元素。
>
>  replaceAll(selector) 用匹配的元素替换掉所有 selector匹配到的元素。

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script src="../js/jquery.min.js"></script>
        <script>

            $(function(){

                // $('<div>replace</div>').replaceAll('a')
                $('a').replaceWith('<div>replace2</div>')
            })

        </script>
    </head>
    <body>


            <a href="#">天猫</a>
            <a href="#">淘宝</a>
            <a href="#">拼多多</a>
            <a href="#">京东</a>


    </body>
</html>
```

## [7.7删除](https://jshand.gitee.io/#/course/front/jquery?id=_77删除)

>  empty() 删除匹配的元素集合中所有的子节点。
>
>  remove([expr]) 从DOM中删除所有匹配的元素。这个方法不会把匹配的元素从jQuery对象中删除，因而可以在将来再使用这些匹配的元素。但除了这个元素本身得以保留之外，其他的比如绑定的事件，附加的数据等都会被移除。
>
>  detach([expr]) 从DOM中删除所有匹配的元素。这个方法不会把匹配的元素从jQuery对象中删除，因而可以在将来再使用这些匹配的元素。与remove()不同的是，所有绑定的事件、附加的数据等都会保留下来。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script src="../js/jquery.min.js"></script>
        <script>

            $(function(){
                $('div').click(function(){
                    //empty 清空子元素
                    $(this).empty();
                })

                //给按钮添加click时间，执行 remove
                $('#rm').click(function(){
                    //$('div').remove();
                    let $div = $('div');
                    $div.remove();
                    setTimeout(function(){
                        $('body').append($div)
                    },1500)
                })

                //detach
                $('#de').click(function(){
                    let $div = $('div');
                    $div.detach();
                    setTimeout(function(){
                        $('body').append($div)
                    },1500)
                })
            })

        </script>
        <style>
            div{
                background-color: #000000; 
                width: 200px;
                height: 100px;
                cursor: pointer;
                color: white;
            }
        </style>
    </head>
    <body>
        <button id="rm">remove</button>
        <button id="de">detach</button>
        <div >
            <span>block</span>

        </div>

    </body>
</html>
```

## [7.8复制](https://jshand.gitee.io/#/course/front/jquery?id=_78复制)

>  clone([Even[,deepEven]]) 克隆匹配的DOM元素并且选中这些克隆的副本。

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script src="../js/jquery.min.js"></script>
        <script>

            $(function(){
                $('button').click(function(){

                    // $('body').append($('#btn').clone());//
                    $('body').append($('#btn').clone(true)); //副本上也可以点击
                });
            })

        </script>

    </head>
    <body>
        <button id="btn">onclick</button>
    </body>
</html>
```

# [8.CSS 外观](https://jshand.gitee.io/#/course/front/jquery?id=_8css-外观)

## [8.1 CSS](https://jshand.gitee.io/#/course/front/jquery?id=_81-css)

 css(name|pro|[,val|fn])1.9* 访问匹配元素的样式属性。 jQuery.cssHooks

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <!--  引入jQuery文档-->
        <script src="../js/jquery.min.js"></script>
        <script>

            $(function(){

                //css(name|pro|[,val|fn])

                //name , value
                // $('div').css('border','solid 1px red');

                //properties {key:value}
                $('div').css({'border':'solid 1px green'});

                $('div').css('background-color',function(i,value){
                    if(i == 1){
                        return 'aqua';
                    }
                    return value;
                })


                // 获取一个样式
                console.log($('#d1').css('border'))

                //获取多个属性的样式  返回Object json
                console.log($('#d1').css(['background-color','border']))

            });



        </script>
    </head>
    <body>

        <div id="d1"  style="background-color: aqua;">block</div>

        <div style="background-color: red;">block</div>

    </body>
</html>
```

## [8.2位置](https://jshand.gitee.io/#/course/front/jquery?id=_82位置)

>  offset([coordinates])
>
>  position()
>
>  scrollTop([val])
>
>  scrollLeft([val])

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <!--  引入jQuery文档-->
        <script src="../js/jquery.min.js"></script>
        <script>

            $(function(){
                let offset = $('.d1').offset(); //获取
                let position = $('.d1').position(); //获取
                console.log('offset',offset.top,offset.left);
                console.log('position',position.top,position.left);


                //设置偏移
                let timeid = null;
                $('#move').click(function(){

                    let timeid = null;
                    if($(this).text() == 'Move'){
                        $(this).text( 'Stop');
                        timeid = setInterval(function(){
                            let offset = $('.d1').offset(); //获取
                            $('.d1').offset({top:(offset.top+5),left:(offset.left+5)})
                        },100)
                    }else{
                        $(this).text( 'Move');
                        clearInterval(timeid)
                    }

                })

                $('#stop').click(function(){
                    clearInterval(timeid)
                })

            });



        </script>
        <style>
            .d1{
                position: absolute;
                border: solid 1px red;
                width: 200px;
                height: 50px;
                top: 30px;
            }
        </style>
    </head>
    <body>
        <div style="position:relative;top: 100px;left: 200px;">
            <button id="move">Move</button>
            <div class="d1">block</div>
        </div>
    </body>
</html>
```

## [8.3 尺寸](https://jshand.gitee.io/#/course/front/jquery?id=_83-尺寸)

>  height([val|fn])
>
>  width([val|fn])
>
>  innerHeight()
>
>  innerWidth()
>
>  outerHeight([options])
>
>  outerWidth([options])

```
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script src="../js/jquery.min.js"></script>
        <script>
            $(function(){
                let w = $('div').width();
                let h = $('div').height();

                console.log('宽',w)
                console.log('高',h);


                let innerW = $('div').innerWidth();
                let innerH = $('div').innerHeight();

                console.log('innerW',innerW)
                console.log('innerH',innerH);


                let outW = $('div').outerWidth();
                let outH = $('div').outerHeight();

                console.log('outW',outW)
                console.log('outH',outH);

            })


        </script>
        <style>
            div{
                box-sizing: border-box;  /** 边框是算作宽高的一部分**/
                /* box-sizing: content-box; */  /** 边框是+宽高是实际的 宽高**/
                border: solid 5px red;
                height: 200px;
                width: 400px;
            }    

        </style>
    </head>
    <body>

        <div >

        </div>


    </body>
</html>
```

# [9. 效果](https://jshand.gitee.io/#/course/front/jquery?id=_9-效果)

## [9.1基本](https://jshand.gitee.io/#/course/front/jquery?id=_91基本)

 show([s,[e],[fn]]) hide([s,[e],[fn]]) toggle([s],[e],[fn])

## [9.2滑动](https://jshand.gitee.io/#/course/front/jquery?id=_92滑动)

 slideDown([s],[e],[fn]) slideUp([s,[e],[fn]]) slideToggle([s],[e],[fn])

## [9.3 淡入淡出](https://jshand.gitee.io/#/course/front/jquery?id=_93-淡入淡出)

 fadeIn([s],[e],[fn]) fadeOut([s],[e],[fn]) fadeTo([[s],o,[e],[fn]]) fadeToggle([s,[e],[fn]])

## [9.4 自定义](https://jshand.gitee.io/#/course/front/jquery?id=_94-自定义)

 animate(p,[s],[e],[fn])1.8* stop([c],[j])1.7* delay(d,[q]) finish([queue])1.9+

## [设置](https://jshand.gitee.io/#/course/front/jquery?id=设置)

 jQuery.fx.off jQuery.fx.interval3.0-

# [10.Ajax](https://jshand.gitee.io/#/course/front/jquery?id=_10ajax)

## [10.1 原生的ajax](https://jshand.gitee.io/#/course/front/jquery?id=_101-原生的ajax)

### [10.1.1什么是 AJAX ？](https://jshand.gitee.io/#/course/front/jquery?id=_1011什么是-ajax-？)

AJAX = 异步 JavaScript 和 XML。

AJAX 是一种用于创建快速动态网页的技术。

通过在后台与服务器进行少量数据交换，AJAX 可以使网页实现异步更新。这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。

传统的网页（不使用 AJAX）如果需要更新内容，必需重载整个网页面。

有很多使用 AJAX 的应用程序案例：新浪微博、Google 地图、开心网等等。

------

### [10.1.2 AJAX 工作原理](https://jshand.gitee.io/#/course/front/jquery?id=_1012-ajax-工作原理)

![AJAX](https://www.runoob.com/wp-content/uploads/2013/09/ajax-yl.png)

------

### [10.1.3 使用步骤](https://jshand.gitee.io/#/course/front/jquery?id=_1013-使用步骤)

> 创建xhr对象

所有现代浏览器（IE7+、Firefox、Chrome、Safari 以及 Opera）均内建 XMLHttpRequest 对象。

创建 XMLHttpRequest 对象的语法：

```js
variable=new XMLHttpRequest();
```

老版本的 Internet Explorer （IE5 和 IE6）使用 ActiveX 对象：

```js
variable=new ActiveXObject("Microsoft.XMLHTTP");
```

为了应对所有的现代浏览器，包括 IE5 和 IE6，请检查浏览器是否支持 XMLHttpRequest 对象。如果支持，则创建 XMLHttpRequest 对象。如果不支持，则创建 ActiveXObject ：

```js
var xmlhttp;
if (window.XMLHttpRequest)
{
    //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
    xmlhttp=new XMLHttpRequest();
}
else
{
    // IE6, IE5 浏览器执行代码
    xmlhttp=new ActiveXObject("Microsoft.XMLHTTP");
}
```

> 打开连接
>
> 发送数据

如需将请求发送到服务器，我们使用 XMLHttpRequest 对象的 open() 和 send() 方法：

```js
xmlhttp.open("GET","ajax_info.txt",true);
xmlhttp.send();
```

| 方法                         | 描述                                                         |
| ---------------------------- | ------------------------------------------------------------ |
| open(*method*,*url*,*async*) | 规定请求的类型、URL 以及是否异步处理请求。 *method*：请求的类型；GET 或 POST *url*：文件在服务器上的位置 *async*：true（异步）或 false（同步） |
| send(*string*)               | 将请求发送到服务器。 *string*：仅用于 POST 请求              |

GET 还是 POST？

与 POST 相比，GET 更简单也更快，并且在大部分情况下都能用。

然而，在以下情况中，请使用 POST 请求：

- 无法使用缓存文件（更新服务器上的文件或数据库）
- 向服务器发送大量数据（POST 没有数据量限制）
- 发送包含未知字符的用户输入时，POST 比 GET 更稳定也更可靠

> 接受响应的数据

如需获得来自服务器的响应，请使用 XMLHttpRequest 对象的 responseText 或 responseXML 属性。

| 属性         | 描述                       |
| ------------ | -------------------------- |
| responseText | 获得字符串形式的响应数据。 |
| responseXML  | 获得 XML 形式的响应数据。  |

```js
document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
```

上面这种方式适合同步的方法，同步的方法是阻塞的，当请求处理完成才执行后面的代码，如果是异步的请求，需要检测请求的状态

当请求被发送到服务器时，我们需要执行一些基于响应的任务。

每当 readyState 改变时，就会触发 onreadystatechange 事件。

readyState 属性存有 XMLHttpRequest 的状态信息。

下面是 XMLHttpRequest 对象的三个重要的属性：

| 属性               | 描述                                                         |
| ------------------ | ------------------------------------------------------------ |
| onreadystatechange | 存储函数（或函数名），每当 readyState 属性改变时，就会调用该函数。 |
| readyState         | 存有 XMLHttpRequest 的状态。从 0 到 4 发生变化。 0: 请求未初始化 1: 服务器连接已建立 2: 请求已接收 3: 请求处理中 4: 请求已完成，且响应已就绪 |
| status             | 200: "OK" 404: 未找到页面                                    |

在 onreadystatechange 事件中，我们规定当服务器响应已做好被处理的准备时所执行的任务。

当 readyState 等于 4 且状态为 200 时，表示响应已就绪：

```js
xmlhttp.onreadystatechange=function()
{
    if (xmlhttp.readyState==4 && xmlhttp.status==200)
    {
        document.getElementById("myDiv").innerHTML=xmlhttp.responseText;
    }
}
```

### [10.1.4 ajax实例](https://jshand.gitee.io/#/course/front/jquery?id=_1014-ajax实例)

> json.txt

```
{
    "name":"jshand",
    "age":18
}
```

> stu.json

```json
{
    "name":"jshand",
    "age":18
}
```

> content.html

```html
<p>
    动态加载的内容
</p>
```

> ajax请求代码

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <script>
            function getContent() {
                //1  xhr对象
                let xmlhttp;
                if (window.XMLHttpRequest) {
                    //  IE7+, Firefox, Chrome, Opera, Safari 浏览器执行代码
                    xmlhttp = new XMLHttpRequest();
                } else {
                    // IE6, IE5 浏览器执行代码
                    xmlhttp = new ActiveXObject("Microsoft.XMLHTTP");
                }
                //2  使用xhr对象打开一个连接
                //async：true（异步）或 false（同步）
                 //完整路径 :http://127.0.0.1:8848/jquery/ajax/content.html
                 //相对路径 :content.html
                xmlhttp.open("GET", "content.html", true);
                //xhr 发送数据
                xmlhttp.send();

                //3 获取xhr响应的数据
                //alert(xmlhttp.responseText)  //同步的方法没有问题， 异步的可能取不到数据,需要监听readyState状态
                xmlhttp.onreadystatechange = function() {
                    if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
                        // alert(xmlhttp.responseText)
                        //2 追加到 body
                        let div = document.createElement('div')
                        div.innerHTML = xmlhttp.responseText
                        document.body.appendChild(div)
                    }
                }
            }
        </script>
    </head>
    <body>
        <button onclick="getContent()">记载内容，填充到doc</button>
        <div>已存在的内容</div>


    </body>
</html>
```

## [10.2 jQuery封装的Ajax](https://jshand.gitee.io/#/course/front/jquery?id=_102-jquery封装的ajax)

### [10.2.1 介绍](https://jshand.gitee.io/#/course/front/jquery?id=_1021-介绍)

jQuery 底层 AJAX 实现。简单易用的高层实现见 $.get, $.post 等。$.ajax() 返回其创建的 XMLHttpRequest 对象。大多数情况下你无需直接操作该函数，除非你需要操作不常用的选项，以获得更多的灵活性。

最简单的情况下，$.ajax()可以不带任何参数直接使用。

**注意**，所有的选项都可以通过$.ajaxSetup()函数来全局设置。

**回调函数**

如果要处理$.ajax()得到的数据，则需要使用回调函数。beforeSend、error、dataFilter、success、complete。

- beforeSend 在发送请求之前调用，并且传入一个XMLHttpRequest作为参数。
- error 在请求出错时调用。传入XMLHttpRequest对象，描述错误类型的字符串以及一个异常对象（如果有的话）
- dataFilter 在请求成功之后调用。传入返回的数据以及"dataType"参数的值。并且必须返回新的数据（可能是处理过的）传递给success回调函数。
- success 当请求之后调用。传入返回后的数据，以及包含成功代码的字符串。
- complete 当请求完成之后调用这个函数，无论成功或失败。传入XMLHttpRequest对象，以及一个包含成功或错误代码的字符串。

**数据类型**

$.ajax()函数依赖服务器提供的信息来处理返回的数据。如果服务器报告说返回的数据是XML，那么返回的结果就可以用普通的XML方法或者jQuery的选择器来遍历。如果见得到其他类型，比如HTML，则数据就以文本形式来对待。

通过dataType选项还可以指定其他不同数据处理方式。除了单纯的XML，还可以指定 html、json、jsonp、script或者text。

其中，text和xml类型返回的数据不会经过处理。数据仅仅简单的将XMLHttpRequest的responseText或responseHTML属性传递给success回调函数，

'''注意'''，我们必须确保网页服务器报告的MIME类型与我们选择的dataType所匹配。比如说，XML的话，服务器端就必须声明 text/xml 或者 application/xml 来获得一致的结果。

如果指定为html类型，任何内嵌的JavaScript都会在HTML作为一个字符串返回之前执行。类似的，指定script类型的话，也会先执行服务器端生成JavaScript，然后再把脚本作为一个文本数据返回。

如果指定为json类型，则会把获取到的数据作为一个JavaScript对象来解析，并且把构建好的对象作为结果返回。为了实现这个目的，他首先尝试使用JSON.parse()。如果浏览器不支持，则使用一个函数来构建。JSON数据是一种能很方便通过JavaScript解析的结构化数据。如果获取的数据文件存放在远程服务器上（域名不同，也就是跨域获取数据），则需要使用jsonp类型。使用这种类型的话，会创建一个查询字符串参数 callback=? ，这个参数会加在请求的URL后面。服务器端应当在JSON数据前加上回调函数名，以便完成一个有效的JSONP请求。如果要指定回调函数的参数名来取代默认的callback，可以通过设置$.ajax()的jsonp参数。

**注意**，JSONP是JSON格式的扩展。他要求一些服务器端的代码来检测并处理查询字符串参数。更多信息可以参阅 [最初的文章](https://bob.pythonmac.org/archives/2005/12/05/remote-json-jsonp/)。

如果指定了script或者jsonp类型，那么当从服务器接收到数据时，实际上是用了<script>标签而不是XMLHttpRequest对象。这种情况下，$.ajax()不再返回一个XMLHttpRequest对象，并且也不会传递事件处理函数，比如beforeSend。

**发送数据到服务器**

默认情况下，Ajax请求使用GET方法。如果要使用POST方法，可以设定type参数值。这个选项也会影响data选项中的内容如何发送到服务器。

data选项既可以包含一个查询字符串，比如 key1=value1&key2=value2 ，也可以是一个映射，比如 {key1: 'value1', key2: 'value2'} 。如果使用了后者的形式，则数据再发送器会被转换成查询字符串。这个处理过程也可以通过设置processData选项为false来回避。如果我们希望发送一个XML对象给服务器时，这种处理可能并不合适。并且在这种情况下，我们也应当改变contentType选项的值，用其他合适的MIME类型来取代默认的 application/x-www-form-urlencoded 。

**高级选项**

global选项用于阻止响应注册的回调函数，比如.ajaxSend，或者ajaxError，以及类似的方法。这在有些时候很有用，比如发送的请求非常频繁且简短的时候，就可以在ajaxSend里禁用这个。更多关于这些方法的详细信息，请参阅下面的内容。

如果服务器需要HTTP认证，可以使用用户名和密码可以通过username和password选项来设置。

Ajax请求是限时的，所以错误警告被捕获并处理后，可以用来提升用户体验。请求超时这个参数通常就保留其默认值，要不就通过jQuery.ajaxSetup来全局设定，很少为特定的请求重新设置timeout选项。

默认情况下，请求总会被发出去，但浏览器有可能从他的缓存中调取数据。要禁止使用缓存的结果，可以设置cache参数为false。如果希望判断数据自从上次请求后没有更改过就报告出错的话，可以设置ifModified为true。

scriptCharset允许给<script>标签的请求设定一个特定的字符集，用于script或者jsonp类似的数据。当脚本和页面字符集不同时，这特别好用。

Ajax的第一个字母是asynchronous的开头字母，这意味着所有的操作都是并行的，完成的顺序没有前后关系。$.ajax()的async参数总是设置成true，这标志着在请求开始后，其他代码依然能够执行。强烈不建议把这个选项设置成false，这意味着所有的请求都不再是异步的了，这也会导致浏览器被锁死。

$.ajax函数返回他创建的XMLHttpRequest对象。通常jQuery只在内部处理并创建这个对象，但用户也可以通过xhr选项来传递一个自己创建的xhr对象。返回的对象通常已经被丢弃了，但依然提供一个底层接口来观察和操控请求。比如说，调用对象上的.abort()可以在请求完成前挂起请求。

### [10.2.2 实例，资源参考原生ajax](https://jshand.gitee.io/#/course/front/jquery?id=_1022-实例，资源参考原生ajax)

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <title></title>
        <!--  引入jQuery文档-->
        <script src="../js/jquery.min.js"></script>
        <script>
            $(function(){
                $('button').click(function(){
                    // 1 使用jquery封装的ajax方法
                    let url = 'json.txt';// 访问文本类型
                    // let url = 'http://www.js.com/';  //触发error的地址
                    // let url = 'stu.json'; //访问json类型
                    $.ajax(url,{
                        //async:'',
                        type:'POST',
                        dataType:'json',
                        success:function(data, textStatus, jqXHR){
                            // 如果是文本的，但是格式确实是json格式，可以使用eval函数转换
                            // var obj =   eval( '('+ '{"name":"jshand"}'  +')');
                            // alert(obj.name)
                            alert(data.name)
                        },
                        error:function(xhr,msg){
                            console.log(msg)
                        },
                    })
                })
            })
        </script>
    </head>
    <body>
        <button >记载内容，填充到doc</button>
        <div>已存在的内容</div>
    </body>
</html>
```

### [10.2.3 ajax方法的参数](https://jshand.gitee.io/#/course/front/jquery?id=_1023-ajax方法的参数)

#### [**url,[settings\]** Object *V1.5*](https://jshand.gitee.io/#/course/front/jquery?id=urlsettings-object-v15)

**url**:一个用来包含发送请求的URL字符串。

**settings**:AJAX 请求设置。所有选项都是可选的。

### [V1.0 settings:选项](https://jshand.gitee.io/#/course/front/jquery?id=v10-settings选项)

#### [**accepts ** Map](https://jshand.gitee.io/#/course/front/jquery?id=accepts-map)

默认： 取决于数据类型。

内容类型发送请求头，告诉服务器什么样的响应会接受返回。如果accepts设置需要修改，推荐在$.ajaxSetup()方法中做一次。

#### [**async ** Boolean](https://jshand.gitee.io/#/course/front/jquery?id=async-boolean)

(默认: true) 默认设置下，所有请求均为异步请求。如果需要发送同步请求，请将此选项设置为 false。注意，同步请求将锁住浏览器，用户其它操作必须等待请求完成才可以执行。

#### [**beforeSend(XHR) ** Function](https://jshand.gitee.io/#/course/front/jquery?id=beforesendxhr-function)

发送请求前可修改 XMLHttpRequest 对象的函数，如添加自定义 HTTP 头。XMLHttpRequest 对象是唯一的参数。这是一个 [Ajax 事件](https://docs.jquery.com/Ajax_Events)。如果返回false可以取消本次ajax请求。

```
function (XMLHttpRequest) {
    this; // 调用本次AJAX请求时传递的options参数
}
```

#### [**cache ** Boolean](https://jshand.gitee.io/#/course/front/jquery?id=cache-boolean)

(默认: true,dataType为script和jsonp时默认为false) jQuery 1.2 新功能，设置为 false 将不缓存此页面。

#### [**complete(XHR, TS) ** Function](https://jshand.gitee.io/#/course/front/jquery?id=completexhr-ts-function)

请求完成后回调函数 (请求成功或失败之后均调用)。参数： XMLHttpRequest 对象和一个描述成功请求类型的字符串。 [Ajax 事件](https://docs.jquery.com/Ajax_Events)。

```
function (XMLHttpRequest, textStatus) {
    this; // 调用本次AJAX请求时传递的options参数
}
```

#### [**contents ** Map *V1.5*](https://jshand.gitee.io/#/course/front/jquery?id=contents-map-v15)

一个以"{字符串:正则表达式}"配对的对象，用来确定jQuery将如何解析响应，给定其内容类型。

#### [**contentType ** String](https://jshand.gitee.io/#/course/front/jquery?id=contenttype-string)

(默认: "application/x-www-form-urlencoded") 发送信息至服务器时内容编码类型。默认值适合大多数情况。如果你明确地传递了一个content-type给 $.ajax() 那么他必定会发送给服务器（即使没有数据要发送）

#### [**context ** Object](https://jshand.gitee.io/#/course/front/jquery?id=context-object)

这个对象用于设置Ajax相关回调函数的上下文。也就是说，让回调函数内this指向这个对象（如果不设定这个参数，那么this就指向调用本次AJAX请求时传递的options参数）。比如指定一个DOM元素作为context参数，这样就设置了success回调函数的上下文为这个DOM元素。就像这样：

```
$.ajax({ url: "test.html", context: document.body, success: function(){
    $(this).addClass("done");
}});
```

#### [**converters** map *V1.5*](https://jshand.gitee.io/#/course/front/jquery?id=converters-map-v15)

默认： {"* text": window.String, "text html": true, "text json": jQuery.parseJSON, "text xml": jQuery.parseXML}

一个数据类型对数据类型转换器的对象。每个转换器的值是一个函数，返回响应的转化值

#### [**cross Domain **map*V1.5*](https://jshand.gitee.io/#/course/front/jquery?id=cross-domain-mapv15)

默认： 同域请求为false

跨域请求为true如果你想强制跨域请求（如JSONP形式）同一域，设置crossDomain为true。这使得例如，服务器端重定向到另一个域

#### [**data ** Object,String](https://jshand.gitee.io/#/course/front/jquery?id=data-objectstring)

发送到服务器的数据。将自动转换为请求字符串格式。GET 请求中将附加在 URL 后。查看 processData 选项说明以禁止此自动转换。必须为 Key/Value 格式。如果为数组，jQuery 将自动为不同值对应同一个名称。如 {foo:["bar1", "bar2"]} 转换为 "&foo=bar1&foo=bar2"。

#### [**dataFilter **Function](https://jshand.gitee.io/#/course/front/jquery?id=datafilter-function)

给Ajax返回的原始数据的进行预处理的函数。提供data和type两个参数：data是Ajax返回的原始数据，type是调用jQuery.ajax时提供的dataType参数。函数返回的值将由jQuery进一步处理。

```
function (data, type) {
    // 对Ajax返回的原始数据进行预处理
    return data  // 返回处理后的数据
}
```

#### [**dataType **String](https://jshand.gitee.io/#/course/front/jquery?id=datatype-string)

预期服务器返回的数据类型。如果不指定，jQuery 将自动根据 HTTP 包 MIME 信息来智能判断，比如XML MIME类型就被识别为XML。在1.4中，JSON就会生成一个JavaScript对象，而script则会执行这个脚本。随后服务器端返回的数据会根据这个值解析后，传递给回调函数。可用值:

"xml": 返回 XML 文档，可用 jQuery 处理。

"html": 返回纯文本 HTML 信息；包含的script标签会在插入dom时执行。

"script": 返回纯文本 JavaScript 代码。不会自动缓存结果。除非设置了"cache"参数。'''注意：'''在远程请求时(不在同一个域下)，所有POST请求都将转为GET请求。(因为将使用DOM的script标签来加载)

"json": 返回 JSON 数据 。

"jsonp": [JSONP](https://bob.pythonmac.org/archives/2005/12/05/remote-json-jsonp/) 格式。使用 [JSONP](https://bob.pythonmac.org/archives/2005/12/05/remote-json-jsonp/) 形式调用函数时，如 "myurl?callback=?" jQuery 将自动替换 ? 为正确的函数名，以执行回调函数。

"text": 返回纯文本字符串

#### [**error ** Function](https://jshand.gitee.io/#/course/front/jquery?id=error-function)

(默认: 自动判断 (xml 或 html)) 请求失败时调用此函数。有以下三个参数：XMLHttpRequest 对象、错误信息、（可选）捕获的异常对象。如果发生了错误，错误信息（第二个参数）除了得到null之外，还可能是"timeout", "error", "notmodified" 和 "parsererror"。[Ajax 事件](https://docs.jquery.com/Ajax_Events)。

```
function (XMLHttpRequest, textStatus, errorThrown) {
    // 通常 textStatus 和 errorThrown 之中
    // 只有一个会包含信息
    this; // 调用本次AJAX请求时传递的options参数
}
```

#### [**global**Boolean](https://jshand.gitee.io/#/course/front/jquery?id=globalboolean)

(默认: true) 是否触发全局 AJAX 事件。设置为 false 将不会触发全局 AJAX 事件，如 ajaxStart 或 ajaxStop 可用于控制不同的 [Ajax 事件](https://docs.jquery.com/Ajax_Events)。

#### [**headers**map *V1.5*](https://jshand.gitee.io/#/course/front/jquery?id=headersmap-v15)

Default: {}

一个额外的"{键:值}"对映射到请求一起发送。此设置被设置之前beforeSend函数被调用;因此，消息头中的值设置可以在覆盖beforeSend函数范围内的任何设置。

#### [**ifModified**Boolean](https://jshand.gitee.io/#/course/front/jquery?id=ifmodifiedboolean)

(默认: false) 仅在服务器数据改变时获取新数据。使用 HTTP 包 Last-Modified 头信息判断。在jQuery 1.4中，他也会检查服务器指定的'etag'来确定数据没有被修改过。

#### [**isLocal**map *V1.5.1*](https://jshand.gitee.io/#/course/front/jquery?id=islocalmap-v151)

默认: 取决于当前的位置协议

允许当前环境被认定为“本地”，（如文件系统），即使jQuery默认情况下不会承认它。以下协议目前公认为本地：file, *-extension, and widget。如果isLocal设置需要修改，建议在$.ajaxSetup()方法中这样做一次。

#### [**jsonp**String](https://jshand.gitee.io/#/course/front/jquery?id=jsonpstring)

在一个jsonp请求中重写回调函数的名字。这个值用来替代在"callback=?"这种GET或POST请求中URL参数里的"callback"部分，比如{jsonp:'onJsonPLoad'}会导致将"onJsonPLoad=?"传给服务器。

#### [**jsonpCallback**String](https://jshand.gitee.io/#/course/front/jquery?id=jsonpcallbackstring)

为jsonp请求指定一个回调函数名。这个值将用来取代jQuery自动生成的随机函数名。这主要用来让jQuery生成度独特的函数名，这样管理请求更容易，也能方便地提供回调函数和错误处理。你也可以在想让浏览器缓存GET请求的时候，指定这个回调函数名。

#### [**mimeType**String *V1.5.1*](https://jshand.gitee.io/#/course/front/jquery?id=mimetypestring-v151)

一个mime类型用来覆盖XHR的 MIME类型。

#### [**password**String](https://jshand.gitee.io/#/course/front/jquery?id=passwordstring)

用于响应HTTP访问认证请求的密码

#### [**processData**Boolean](https://jshand.gitee.io/#/course/front/jquery?id=processdataboolean)

(默认: true) 默认情况下，通过data选项传递进来的数据，如果是一个对象(技术上讲只要不是字符串)，都会处理转化成一个查询字符串，以配合默认内容类型 "application/x-www-form-urlencoded"。如果要发送 DOM 树信息或其它不希望转换的信息，请设置为 false。

#### [**scriptCharset**String](https://jshand.gitee.io/#/course/front/jquery?id=scriptcharsetstring)

只有当请求时dataType为"jsonp"或"script"，并且type是"GET"才会用于强制修改charset。通常只在本地和远程的内容编码不同时使用。

#### [**statusCode**map *V1.5*](https://jshand.gitee.io/#/course/front/jquery?id=statuscodemap-v15)

默认: {}

一组数值的HTTP代码和函数对象，当响应时调用了相应的代码。例如，如果响应状态是404，将触发以下警报：

```
$.ajax({
  statusCode: {404: function() {
    alert('page not found');
  }
});
```

#### [**success(data, textStatus, jqXHR)**Function,Array](https://jshand.gitee.io/#/course/front/jquery?id=successdata-textstatus-jqxhrfunctionarray)

请求成功后的回调函数。参数：由服务器返回，并根据dataType参数进行处理后的数据；描述状态的字符串。还有 jqXHR（在jQuery 1.4.x的中，XMLHttpRequest） 对象 。在jQuery 1.5， 成功设置可以接受一个函数数组。每个函数将被依次调用。 [Ajax 事件](https://docs.jquery.com/Ajax_Events)。

```
function (data, textStatus) {
    // data 可能是 xmlDoc, jsonObj, html, text, 等等...
    this; // 调用本次AJAX请求时传递的options参数
}
```

#### [**traditional**Boolean](https://jshand.gitee.io/#/course/front/jquery?id=traditionalboolean)

如果你想要用传统的方式来序列化数据，那么就设置为true。请参考工具分类下面的jQuery.param 方法。

#### [**timeout**Number](https://jshand.gitee.io/#/course/front/jquery?id=timeoutnumber)

设置请求超时时间（毫秒）。此设置将覆盖全局设置。

#### [**type**String](https://jshand.gitee.io/#/course/front/jquery?id=typestring)

(默认: "GET") 请求方式 ("POST" 或 "GET")， 默认为 "GET"。注意：其它 HTTP 请求方法，如 PUT 和 DELETE 也可以使用，但仅部分浏览器支持。

#### [**url**String](https://jshand.gitee.io/#/course/front/jquery?id=urlstring)

(默认: 当前页地址) 发送请求的地址。

#### [**username**String](https://jshand.gitee.io/#/course/front/jquery?id=usernamestring)

用于响应HTTP访问认证请求的用户名

#### [**xhr**Function](https://jshand.gitee.io/#/course/front/jquery?id=xhrfunction)

需要返回一个XMLHttpRequest 对象。默认在IE下是ActiveXObject 而其他情况下是XMLHttpRequest 。用于重写或者提供一个增强的XMLHttpRequest 对象。这个参数在jQuery 1.3以前不可用。

#### [**xhrFields**map *V1.5*](https://jshand.gitee.io/#/course/front/jquery?id=xhrfieldsmap-v15)

一对“文件名-文件值”在本机设置XHR对象。例如，如果需要的话，你可以用它来设置withCredentials为true的跨域请求。

#### [10.2.4 示例](https://jshand.gitee.io/#/course/front/jquery?id=_1024-示例)

> 描述: 加载并执行一个 JS 文件。

> jQuery 代码:

```js
$.ajax({
  type: "GET",
  url: "test.js",
  dataType: "script"
});
```

> 描述:保存数据到服务器，成功时显示信息。

> jQuery 代码:

```js
$.ajax({
   type: "POST",
   url: "some.php",
   data: "name=John&location=Boston",
   success: function(msg){
     alert( "Data Saved: " + msg );
   }
});
```

> 描述: 装入一个 HTML 网页最新版本。
>
> jQuery 代码:

```js
$.ajax({
  url: "test.html",
  cache: false,
  success: function(html){
    $("#results").append(html);
  }
});
```

> 描述:同步加载数据。发送请求时锁住浏览器。需要锁定用户交互操作时使用同步方式。
>
> jQuery 代码:

```js
 var html = $.ajax({
  url: "some.php",
  async: false
 }).responseText;
```

> 描述:发送 XML 数据至服务器。设置 processData 选项为 false，防止自动转换数据格式。
>
> jQuery 代码:

```js
 var xmlDocument = [create xml document];
 $.ajax({
   url: "page.php",
   processData: false,
   data: xmlDocument,
   success: handleResponse
 });
```