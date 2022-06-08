# Bootstrap详解

---- 老孙

<hr>

**课程目标：**

1、bootstrap简介

2、bootstrap使用

3、案例

# 1.bootstrap简介

## 1.1 什么是Bootstrap？

- Bootstrap来自 Twitter，是目前最受欢迎的**响应式**前端框架。
- Bootstrap是基于 HTML、CSS、JavaScript的，它简洁灵活，使得 Web 开发更加快捷。

## 1.2 为什么使用 Bootstrap？

- 移动设备优先：自 Bootstrap3 起，框架包含了贯穿于整个库移动设备优先的样式。
  - 不是简单的增加一些可选的针对移动设备的样式，而是直接融合进了框架的内核中。
  - 也就是说，针对移动设备的样式融合进了框架的每个角落，而不是增加一个额外的文件。
- 浏览器支持：所有的主流浏览器都支持 Bootstrap。
- 容易上手：只要您具备 HTML 和 CSS 的基础知识，您就可以开始学习 Bootstrap。
- <b style='color:red'>响应式设计</b>：Bootstrap 的响应式 CSS 能够自适应于台式机、平板电脑和手机。
- 它为开发人员创建接口提供了一个简洁统一的解决方案。
- 它包含了功能强大的内置组件，易于定制。
- 它还提供了基于 Web 的定制。
- 它是开源的。

## 1.3 下载与使用

中文官网：https://www.bootcss.com/    

英文官网：http://getbootstrap.com

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\1.png)

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\2.png)

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\3.png)

下载好的，压缩包解压出来。就得到了bootstrap的资源文件了。

分别是css、js、font字体，全部放在项目的根目录即可

```html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="UTF-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1.0" />
    <meta http-equiv="X-UA-Compatible" content="ie=edge" />
    <link href="css/bootstrap.min.css" rel="stylesheet" />
    <title>拉勾有我-必须火</title>
  </head>

  <body style="padding:100px;">
      <h1>Hello, Bootstrap!</h1>
      <!-- 注意：这两个按钮的class样式是Bootstrap定义的 -->
      <button class="btn btn-primary">学习充电</button>
      <button class="btn btn-success">极速入职</button>
  </body>
</html>
```

# 2.Bootstrap的使用

## 2.1 表格

和之前的表格相比，标签更丰富更加语义化，效果更佳美观

### 2.1.1 丰富的标签

- \<table> 为表格添加基础样式
- \<thead> 表格**标题行**的容器元素（\<tr>）
- \<tbody> 表格**主体**中的表格行的容器元素（\<tr>）
- \<tr> 表格行
- \<td> 默认的表格单元格。
- \<th> 特殊的表格单元格，（居中和加粗的效果）。必须在\<thead> 内使用。
- \<caption> 关于表格存储内容的描述或**总结**。

### 2.1.2 好看的类样式

- .table 为任意 \<table> 添加基本样式 (只有横向分隔线)
- .table-striped 在 \<tbody> 内添加斑马线形式的条纹 ( IE8 不支持) ，**隔行变色**
- .table-bordered 为所有表格的单元格添加边框
- .table-hover 在 \<tbody> 内的任一行启用鼠标悬停状态，鼠标悬停高亮突出显示
- .table-condensed 让表格更加紧凑

### 2.1.3 情景色类样式

适合应用在\<th>、\<tr>，\<td>

- .active 激活效果（悬停颜色）
- .success 表示成功或积极的动作
- .info 表示普通的提示信息或动作
- .warning 表示警告或需要用户注意
- .danger 表示危险或潜在的带来的负面影响的动作

### 2.1.4 响应式表格

表格的父元素设置响应式，<b style="color:red">小于768px，出现边框</b>

```html
<table class="table table-responsive">
    <caption>经营产品大全</caption>
    <thead>
        <tr>
            <th>产品</th>
            <th>付款日期</th>
            <th>状态</th>
        </tr>
    </thead>
    <tbody>
        <tr class="info">
            <td>产品1</td>
            <td>2020-1-1</td>
            <td>待发货</td>
        </tr>
        <tr class="active">
            <td>产品2</td>
            <td>2020-1-2</td>
            <td>已发货</td>
        </tr>
        <tr class="success">
            <td>产品3</td>
            <td>2020-1-3</td>
            <td>未付款</td>
        </tr>
        <tr class="warning">
            <td>产品4</td>
            <td>2020-1-4</td>
            <td>已退货</td>
        </tr>
        <tr class="danger">
            <td>产品5</td>
            <td>2020-1-5</td>
            <td>已退款</td>
        </tr>
    </tbody>
</table>
```



## 2.2 表单

### 2.2.1 表单布局

#### 2.2.1.1 默认布局

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\5.png)

```html
<body style="padding:100px;">
    <form>
        <div class="form-group">
            <label>邮箱</label>
            <input type="email" class="form-control" placeholder="请输入邮箱">
        </div>
        <div class="form-group">
            <label>密码</label>
            <input type="passowd" class="form-control" placeholder="请输入密码">
        </div>
        <div class="form-group">
            <button class="btn btn-primary">提交</button>
        </div>
    </form>
</body>
```

#### 2.2.1.2 内联布局

- 让所有表单元素居于一行
- <b style="color:Red">注意</b>：当小于768px时,会自动还原成移动端样式.

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\6.png)

```html
<form class="form-inline">
```

### 2.2.2 表单控件

#### 2.2.2.1 输入框

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\7.png)

```html
<form>
    <div class="form-group">
        <label for="">输入框</label>
        <input type="text" class="form-control" placeholder="请输入文本...">
    </div>
</form>
```

#### 2.2.2.2 文本框

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\8.png)

```html
<div class="form-group">
    <label for="">文本框</label>
    <textarea class="form-control" cols="30" rows="5"></textarea>
</div>
```

#### 2.2.2.3 复选框

- checkbox 默认
- checkbox-inline 内联

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\9.png)

```html
<div class="checkbox">
    <label>
        <input type="checkbox"> 抽烟
    </label>
</div>
<div class="checkbox">
    <label>
        <input type="checkbox"> 喝酒
    </label>
</div>
    <hr>
<div class="checkbox-inline">
    <label>
        <input type="checkbox"> 洗澡
    </label>
</div>
<div class="checkbox-inline">
    <label>
        <input type="checkbox"> 烫头
    </label>
</div>
```

#### 2.2.2.4 复选按钮

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\11.png)

```html
<form>        
    <div class="btn-group" data-toggle="buttons">
        <label class="btn btn-primary">
            <input type="checkbox"> 音乐
        </label>
        <label class="btn btn-primary">
            <input type="checkbox"> 体育
        </label>
        <label class="btn btn-primary">
            <input type="checkbox"> 美术
        </label>
        <label class="btn btn-primary">
            <input type="checkbox"> 电脑
        </label>
    </div>
</form>

<script src="js/jquery-3.3.1.min.js"></script>
<script src="js/bootstrap.min.js"></script>
```



#### 2.2.2.5 单选框

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\10.png)

```html
<label class="radio">
    <input type="radio" name="sex"> 男
</label>
<label class="radio">
    <input type="radio" name="sex"> 女
</label>
<hr>
<label class="radio-inline">
    <input type="radio" name="gender"> 耕
</label>
<label class="radio-inline">
    <input type="radio" name="gender"> 织
</label>

<script src="js/jquery-3.3.1.min.js"></script>
<script src="js/bootstrap.min.js"></script>
```

#### 2.2.2.6 单选按钮

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\12.png)

```html
<div class="btn-group" data-toggle="buttons">
    <label class="btn btn-primary">
        <input type="radio" name="gender"> 男
    </label>
    <label class="btn btn-primary">
        <input type="radio" name="gender"> 女
    </label>
</div>

<script src="js/jquery-3.3.1.min.js"></script>
<script src="js/bootstrap.min.js"></script>
```

## 2.3 按钮

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\13.png)

```html
<button class="btn btn-default">默认按钮</button>
<button class="btn btn-primary">主要按钮</button>
<button class="btn btn-success">成功按钮</button>
<button class="btn btn-info">信息按钮</button>
<button class="btn btn-warning">警告按钮</button>
<button class="btn btn-danger">危险按钮</button>
<button class="btn btn-link">连接按钮</button>
```

- 按钮大小

  ![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\15.png)

```html
<button class="btn btn-primary btn-lg">超大按钮（高清大屏台式机）</button>
<button class="btn btn-primary">大按钮（笔记本屏幕）</button>
<button class="btn btn-primary btn-sm">小按钮（平板电脑）</button>
<button class="btn btn-primary btn-xs">超小按钮（手机）</button>
```

- 按钮状态
  - 激活状态：按钮在激活时将呈现为被按压的外观（深色的背景、深色的边框、阴影）;
  - 禁用状态：当您禁用一个按钮时，它的颜色会变淡 50%，并失去渐变;

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\14.png)

```html
<button class="btn btn-default">默认按钮</button>
<button class="btn btn-default active">激活按钮</button>
<button class="btn btn-default" disabled>禁用按钮</button>
```



## 2.4 图片

- .img-rounded：添加 border-radius:6px 来获得图片圆角。
- .img-circle：添加 border-radius:50% 来让整个图片变成圆形。
- .img-thumbnail：添加一些内边距（padding）和一个灰色的边框

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\16.png)

```html
<img src="img/1.jpg" class="img-rounded" width="33%">
<img src="img/1.jpg" class="img-circle" width="33%">
<img src="img/1.jpg" class="img-thumbnail" width="33%">
```

 响应式图片

```html
<img src="img/1.jpg" class="img-responsive">
```



## 2.5 下拉菜单组件

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\17.png)

```html
<div class="dropdown">
    <button class="btn btn-primary" data-toggle="dropdown">拉勾教育 <span class="caret"></span></button>
    <ul class="dropdown-menu">
        <li><a href="#">前沿技术</a></li>
        <li><a href="#">名师制作</a></li>
        <li><a href="#">24小时导师</a></li>
    </ul>
</div>

<script src="js/jquery-3.3.1.min.js"></script>
<script src="js/bootstrap.min.js"></script>
```

用法的关键核心

1. 外围容器使用 class="dropdown"包裹
2. 内部点击按钮事件绑定 data-toggle="dropdown"
3. 菜单元素使用 class="dropdown-menu"

## 2.6 分页组件

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\20.png)

```html
<ul class="pagination">
    <li class="previous"> <a href="#"> &laquo; </a> </li>
    <li class="active"> <a href="#">1</a> </li>
    <li> <a href="#">2</a> </li>
    <li> <a href="#">3</a> </li>
    <li class="next"> <a href="#">&raquo;</a> </li>
</ul>
```



![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\19.png)

```html
 <ul class="pager">
```

## 2.7 栅格系统

- bootstrap的栅格系统会将整个页面水平方向上平均分成12个小格子
- 当浏览器大小发生变化的时候，我们可以控制每行的元素占用几个格子，从而达到响应式的效果

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\25.png)

- 显示屏幕的扩大和缩小. 实现如下效果

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\24.png)

```html
<style>
    .a{
        height: 50px;
        border: 1px solid black;
        background-color: #eeeeee;
    }
</style>
<body>
    
    <div class="container">
        <div class="row">
            <!-- col-lg-3 当大屏幕时，一个div占3份，一行显示4个div -->
            <!-- col-md-4 当中屏幕时，一个div占4份，一行显示3个div -->
            <!-- col-sm-6 当小屏幕时，一个div占6份，一行显示2个div -->
            <!-- 超小屏幕时，一个div占12份，一行显示1个div，默认 -->
            
            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>
            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>
            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>
            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>

            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>
            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>
            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>
            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>

            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>
            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>
            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>
            <div class="col-lg-3 col-md-4 col-sm-6 a">x</div>
        </div>
    </div>
</body>
```



## 2.8 缩略图组件

- 配合响应式的栅格系统

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\21.png)

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\22.png)

```html
<div class="container">
        <div class="row">
            <div class="col-lg-3 col-md-4 col-sm-6">
                <div class="thumbnail">
                    <img src="img/2.jpg">
                </div>
            </div>
			<!-- 其余3张图片 -->
            <div class="col-lg-3 col-md-4 col-sm-6">
                <div class="thumbnail">
                    <img src="img/2.jpg">
                </div>
            </div>
            <div class="col-lg-3 col-md-4 col-sm-6">
                <div class="thumbnail">
                    <img src="img/2.jpg">
                </div>
            </div>
            <div class="col-lg-3 col-md-4 col-sm-6">
                <div class="thumbnail">
                    <img src="img/2.jpg">
                </div>
            </div>
        </div>
    </div>
```

- 自定义内容

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\23.png)

```html
<div class="container">
    <div class="row">

        <div class="col-lg-3 col-md-4 col-sm-6">
            <div class="thumbnail">
                <img src="img/2.jpg">
                <div class="caption">
                    <h3>视频标题</h3>
                    <p>视频介绍，美女众多...</p>
                    <p>
                        <a href="#" class="btn btn-primary">试看</a>
                        <a href="#" class="btn btn-warning">购买</a>
                    </p>
                </div>
            </div>
        </div>

        <!--其余3个省略-->

    </div>
</div>
```



## 2.9 模态框组件

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\30.png)

- data-toggle="modal"     触发类型:模态框modal
- data-target="#myModal" 触发的节点
- data-backdrop="static" 点击黑灰色背景，不会关闭模态框

```html
<button class="btn btn-warning" data-toggle="modal" data-target="#myModal">弹框</button>

<!-- 声明定义模态框组件 -->
<div class="modal" id="myModal" data-backdrop="static">
    <!-- 窗口声明 -->
    <div class="modal-dialog">
        <!-- 内容声明 -->
        <div class="modal-content">
            <!-- 1.框的标题 -->
            <div class="modal-header">
                <button class="close" data-dismiss="modal">
                    <span>&times;</span>
                </button>
                <h4 class="modal-title">友情提示</h4>
            </div>
            <!-- 2.框内信息 -->
            <div class="modal-body">
                服务器错误，请稍后再试！
                <hr>
                <img src="img/1.jpg" width="200px">
                <hr>
                <input type="text">
            </div>
            <!-- 3.框的按钮 -->
            <div class="modal-footer">
                <button class="btn btn-info">确定</button>
                <button class="btn btn-default" data-dismiss="modal">取消</button>
            </div>
        </div>
    </div>
</div>
```



# 3.案例

## 3.1 首页轮播大图

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\26.png)

1. 大小屏幕自动适应,扩大缩小浏览器
2. 滚动数字区点击左右切换图片
3. 左右区域点击切换
4. 默认5秒钟自动切换下一张
5. 最后一张,回到第一张

```html
<div id="myCarousel" class="carousel slide" data-ride="carousel">

    <!-- 滚动圆点区 -->
    <ol class="carousel-indicators">
        <li data-slide-to="0" data-target="#myCarousel"></li>
        <li data-slide-to="1" data-target="#myCarousel"></li>
        <li data-slide-to="2" data-target="#myCarousel"></li>
    </ol>

    <!-- 图片区 -->
    <div class="carousel-inner">
        <div class="item active">
            <img src="img/1.jpg">
        </div>
        <div class="item">
            <img src="img/2.jpg">
        </div>
        <div class="item">
            <img src="img/3.jpg">
        </div>
    </div>

    <!-- 左右切换 -->
    <a href="#myCarousel" data-slide="prev" class="carousel-control left">
        <span class="glyphicon glyphicon-chevron-left"></span>
    </a>
    <a href="#myCarousel" data-slide="next" class="carousel-control right">
        <span class="glyphicon glyphicon-chevron-right"></span>
    </a>

</div>
```

data 属性解释： 

1. data-slide 接受关键字 prev 或 next，用来改变幻灯片相对于当前位置的位置；

2. data-slide-to 来向轮播底部创建一个原始滑动索引，data-slide-to="2" 将把滑动块移动到一个特定的索引，索引从 0 开始计数。
3. data-ride="carousel"属性用户标记轮播在页面加载时开始动画播放

## 3.2 响应式导航条

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\27.png)

<hr>

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\29.png)

<hr>

![](F:\lagou\第三阶段 前端\02模块二前端高级(jQuery、ES6、Bootstrap)\06 任务六 (Bootstrap)\02_图片\28.png)

```html
<!-- 默认的导航条样式 -->
<div class="navbar navbar-default">
    <!-- 响应式的容器 -->
    <div class="container">
        <!-- 导航条的头部 -->
        <div class="navbar-header">
            <a href="#" class="navbar-brand">老孙科技股份有限公司</a>
            <!-- data-toggle="collapse"  数据切换的事件，特效是折叠-->
            <!-- data-target="#nav"  折叠效果的目标是下面的#nav容器-->
            <button class="navbar-toggle" data-toggle="collapse" data-target="#nav">
                <span class="sr-only"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
                <span class="icon-bar"></span>
            </button>
        </div>

        <!-- 导航项 -->
        <div id="nav" class="collapse navbar-collapse">
            <ul class="nav navbar-nav navbar-right">
                <li class="active"> <a href="#"> <span class="glyphicon glyphicon-home"></span> 主页</a> </li>
                <li> <a href="#"> <span class="glyphicon glyphicon-camera"></span> 产品</a> </li>
                <li> <a href="#"> <span class="glyphicon glyphicon-tint"></span> 竞争</a> </li>
                <li> <a href="#"> <span class="glyphicon glyphicon-earphone"></span> 联系我们</a> </li>
            </ul>
        </div>
    </div>
</div>
```
