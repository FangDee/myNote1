## 一、什么是 HTML？

HTML 是用来描述网页的一种语言。

- HTML 指的是超文本标记语言 (**H**yper **T**ext **M**arkup **L**anguage)
- HTML 不是一种编程语言，而是一种*标记语言* (markup language)
- 标记语言是一套*标记标签* (markup tag)
- HTML 使用*标记标签*来描述网页

## [#](https://ydlclass.com/doc21xnv/javaweb/html/#二、常见的浏览器)二、常见的浏览器

| 浏览器  | 内核           | 备注                                                         |
| ------- | -------------- | ------------------------------------------------------------ |
| IE      | Trident        | IE、猎豹安全、360极速浏览器、百度浏览器                      |
| firefox | Gecko          | 可惜这几年已经没落了，打开速度慢、升级频繁、猪一样的队友flash、神一样的对手chrome。 |
| Safari  | webkit         | 从Safari推出之时起，它的渲染引擎就是Webkit，一提到 webkit，首先想到的便是 chrome，可以说，chrome 将 Webkit内核 深入人心，殊不知，Webkit 的鼻祖其实是 Safari。 |
| chrome  | Chromium/Blink | 在 Chromium 项目中研发 Blink 渲染引擎（即浏览器核心），内置于 Chrome 浏览器之中。Blink 其实是 WebKit 的分支。大部分国产浏览器最新版都采用Blink内核。二次开发 |
| Opera   | blink          | 现在跟随chrome用blink内核。                                  |

## [#](https://ydlclass.com/doc21xnv/javaweb/html/#三、html-标签)三、HTML 标签

HTML 标记标签通常被称为 HTML 标签 (HTML tag)。

- HTML 标签是由*尖括号*包围的关键词，比如 `<html>`
- HTML 标签通常是*成对出现*的，比如 `<b>` 和 `</b>`
- 标签对中的第一个标签是*开始标签*，第二个标签是*结束标签*
- 开始和结束标签也被称为*开放标签*和*闭合标签*

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_1、html-文档-网页)1、HTML 文档 = 网页

- HTML 文档*描述网页*
- HTML 文档*包含 HTML 标签*和纯文本
- HTML 文档也被称为*网页*

Web 浏览器的作用是读取 HTML 文档，并以网页的形式显示出它们。浏览器不会显示 HTML 标签，而是使用标签来解释页面的内容：

```html
<html>
<body>

<h1>我的第一个标题</h1>

<p>我的第一个段落。</p>

</body>
</html>
```

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_2、html-基本文档)2、HTML 基本文档

```html
<!DOCTYPE html>
<html>
<head>
<title>文档标题</title>
</head>
<body>
可见文本...
</body>
</html>
```

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_3、html头部)3、HTML头部

<head> 元素包含了所有的头部标签元素。在 <head>元素中你可以插入脚本（scripts）, 样式文件（CSS），及各种meta信息。

可以添加在头部区域的元素标签为: <title>, <style>, <meta>, <link>, <script>, 和 <base>。

------

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_4、html-title-元素)4、HTML <title> 元素

<title> 标签定义了不同文档的标题。

<title> 在 HTML/XHTML 文档中是必须的。

<title> 元素:

- 定义了浏览器工具栏的标题
- 当网页添加到收藏夹时，显示在收藏夹中的标题
- 显示在搜索引擎结果页面的标题

一个简单的 HTML 文档:

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_5、html-base-元素)5、HTML <base> 元素

<base> 标签描述了基本的链接地址/链接目标，该标签作为HTML文档中所有的链接标签的默认链接，可以不加:

```text
<head>
<base href="http://www.runoob.com/images/" target="_blank">
</head>
```

------

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_6、html-link-元素)6、HTML <link> 元素

<link> 标签定义了文档与外部资源之间的关系。

<link> 标签通常用于链接到样式表:

```text
<head>
<link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
```

------

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_7、html-style-元素)7、HTML <style> 元素

<style> 标签定义了HTML文档的样式文件引用地址.

在<style> 元素中你也可以直接添加样式来渲染 HTML 文档:

```css
<head>
<style type="text/css">
    body {background-color:yellow}
    p {color:blue}
</style>
</head>
```

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_8、html-meta-元素)8、HTML <meta> 元素

meta标签描述了一些基本的元数据。

<meta> 标签提供了元数据.元数据也不显示在页面上，但会被浏览器解析。

META 元素通常用于指定网页的描述，关键词，文件的最后修改时间，作者，和其他元数据。

元数据可以使用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他Web服务。

<meta> 一般放置于 <head> 区域

> <meta> 标签- 使用实例

为搜索引擎定义关键词:

```html
<meta name="keywords" content="HTML, CSS, XML, XHTML, JavaScript">
```

为网页定义描述内容:

```html
<meta name="description" content="好好学java的网站">
```

定义网页作者:

```html
<meta name="author" content="itnanls">
```

每30秒钟刷新当前页面:

```html
<meta http-equiv="refresh" content="30">
```

语法：

```html
方式一：
<meta charset="utf-8">

方式二：
<meta http-equiv="Content-Type" content="text/html; charset=UTF-8">
```

说明：http-equiv传送HTTP通信协议的标头。

HTML5 中默认的字符集是 UTF-8。 所有的 HTML 4 处理器都支持 UTF-8，所有的 HTML5 和 XML 处理器都支持 UTF-8 和 UTF-16。

------

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_9、html-script-元素)9、HTML <script> 元素

<meta> 标签提供了元数据.元数据也不显示在页面上，但会被浏览器解析。

<script> 元素在以后的章节中会详细描述。

------

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_10、基本标签-basic-tags)10、基本标签（Basic Tags）

```html
<h1>最大的标题</h1>
<h2> . . . </h2>
<h3> . . . </h3>
<h4> . . . </h4>
<h5> . . . </h5>
<h6>最小的标题</h6>
 
<p>这是一个段落。</p>
<br> （换行）
<hr> （水平线）
<!-- 这是注释 -->
```

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_11、文本格式化-formatting)11、文本格式化（Formatting）

```html
<b>粗体文本</b>
<code>计算机代码</code>
<em>强调文本</em>
<i>斜体文本</i>
<kbd>键盘输入</kbd> 
<pre>预格式化文本</pre>
<small>更小的文本</small>
<strong>重要的文本</strong>
 
<abbr> （缩写）
<address> （联系信息）
<bdo> （文字方向）
<blockquote> （从另一个源引用的部分）
<cite> （工作的名称）
<del> （删除的文本）
<ins> （插入的文本）
<sub> （下标文本）
<sup> （上标文本）
```

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_12、链接-links)12、链接（Links）

```html
普通的链接：<a href="http://www.example.com/">链接文本</a>
图像链接： <a href="http://www.example.com/"><img src="URL" alt="替换文本"></a>
邮件链接： <a href="mailto:webmaster@example.com">发送e-mail</a>
书签：
<a id="tips">提示部分</a>
<a href="#tips">跳到提示部分</a>
```

------

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_13、图片-images)13、图片（Images）

```text
<img loading="lazy" src="URL" alt="替换文本" height="42" width="42">
```

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_14、样式-区块-styles-sections)14、样式/区块（Styles/Sections）

```html
<style type="text/css">
h1 {color:red;}
p {color:blue;}
</style>
<div>文档中的块级元素</div>
<span>文档中的内联元素</span>
```

------

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_15、无序列表)15、无序列表

```html
<ul>    
    <li>项目</li>    
    <li>项目</li> 
</ul>
```

------

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_16、有序列表)16、有序列表

```html
<ol>    
    <li>第一项</li>    
    <li>第二项</li> 
</ol>
```

------

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_17、定义列表)17、定义列表

```html
<dl>  
    <dt>项目 1</dt>    
    <dd>描述项目 1</dd>  
    <dt>项目 2</dt>    
    <dd>描述项目 2</dd> 
</dl>
```

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_18、表格-tables)18、表格（Tables）

```html
<table border="1">  
    <thead>
        <tr>     
            <th>表格标题</th>     
            <th>表格标题</th>   
        </tr>
    </thead>
    <tbody>
       <tr>     
         <td>表格数据</td>     
         <td>表格数据</td>   
       </tr> 
    </tbody>
</table>
```

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_19、框架-iframe)19、框架（Iframe）

```html
<iframe src="https://www.itnanls.cn"></iframe>
```

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_20、表单-forms)20、表单（Forms）

```html
<form> 
    <input type="text" name="email" size="40" maxlength="50"> 
    <input type="password"> 
    <input type="checkbox" checked="checked"> 
    <input type="radio" checked="checked"> 
    <input type="submit" value="Send"> 
    <input type="reset"> <input type="hidden"> 
    
    <select> 
        <option>苹果</option> 
        <option selected="selected">香蕉</option> 
        <option>樱桃</option> 
    </select> 
    
    <textarea name="comment" rows="60" cols="20"></textarea>  
</form>
```

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_21、实体-entities)21、实体（Entities）

```html
&lt; 等同于 < 
&gt; 等同于 > 
&nbsp; 等同于空格
```

## [#](https://ydlclass.com/doc21xnv/javaweb/html/#四、html5-新元素)四、HTML5 新元素

> HTML5 新元素

自1999年以后HTML 4.01 已经改变了很多,今天，在HTML 4.01中的几个已经被废弃，这些元素在HTML5中已经被删除或重新定义。HTML5 在 2012 年已形成了稳定的版本

为了更好地处理今天的互联网应用，HTML5添加了很多新元素及功能，比如: 图形的绘制，多媒体内容，更好的页面结构，更好的形式 处理，和几个api拖放元素，定位，包括网页 应用程序缓存，存储，网络工作者等，咱们选择一些进行学习。

> 新多媒体元素

| 标签    | 描述                         |
| :------ | :--------------------------- |
| <audio> | 定义音频内容                 |
| <video> | 定义视频（video 或者 movie） |

------

> 新的语义和结构元素

HTML5提供了新的元素来创建更好的页面结构：

| 标签     | 描述                              |
| :------- | :-------------------------------- |
| <footer> | 定义 section 或 document 的页脚。 |
| <header> | 定义了文档的头部区域              |
| <nav>    | 定义导航链接的部分。              |
| <aside>  | 定义页面的侧边栏内容。            |
| <time>   | 定义日期或时间。                  |

当然html还有很多新增的标签和特性，有兴趣可以去看看

还有html5还移除了一部分标签，这里不再赘述。

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_1、浏览器支持)1、浏览器支持

![Internet Explorer](https://www.runoob.com/images/compatible_ie.gif)![Firefox](https://www.runoob.com/images/compatible_firefox.gif)![Opera](https://www.runoob.com/images/compatible_opera.gif)![Google Chrome](https://www.runoob.com/images/compatible_chrome.gif)![Safari](https://www.runoob.com/images/compatible_safari.gif)

Internet Explorer 9+, Firefox, Opera, Chrome, 和 Safari 支持 `<video>`元素.

**注意:** Internet Explorer 8 或者更早的IE版本不支持 `<video>` 元素。

------

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_2、html5-视频)2、HTML5 (视频)

如需在 HTML5 中显示视频，您所有需要的是：

> 实例

```html
<video width="320" height="240" controls>   
    <source src="movie.mp4" type="video/mp4">  您的浏览器不支持Video标签。 
</video>

<video> 与</video> 标签之间插入的内容是提供给不支持 video 元素的浏览器显示的。
<video> 元素支持多个 <source> 元素. <source> 元素可以链接不同的视频文件。浏览器将使用第一个可识别的格式：
```

同时 <video> 元素也提供了 width 和 height 属性控制视频的尺寸.如果设置的高度和宽度，所需的视频空间会在页面加载时保留。如果没有设置这些属性，浏览器不知道大小的视频，浏览器就不能再加载时保留特定的空间，页面就会根据原始视频的大小而改变。

#### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_1-视频格式与浏览器的支持)（1）视频格式与浏览器的支持

当前，<video> 元素支持三种视频格式： MP4, WebM, 和 Ogg:

| 浏览器            | MP4                  | WebM | Ogg  |
| :---------------- | :------------------- | :--- | :--- |
| Internet Explorer | YES                  | NO   | NO   |
| Chrome            | YES                  | YES  | YES  |
| Firefox           | YES                  | YES  | YES  |
| Safari            | YES                  | NO   | NO   |
| Opera             | YES (从 Opera 25 起) | YES  | YES  |

- MP4 = 带有 H.264 视频编码和 AAC 音频编码的 MPEG 4 文件
- WebM = 带有 VP8 视频编码和 Vorbis 音频编码的 WebM 文件
- Ogg = 带有 Theora 视频编码和 Vorbis 音频编码的 Ogg 文件

------

#### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_2-视频格式)（2）视频格式

| 格式 | MIME-type  |
| :--- | :--------- |
| MP4  | video/mp4  |
| WebM | video/webm |
| Ogg  | video/ogg  |

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_3、html5-audio-音频)3、HTML5 Audio(音频)

------

HTML5 提供了播放音频文件的标准。

------

> 互联网上的音频

直到现在，仍然不存在一项旨在网页上播放音频的标准。

今天，大多数音频是通过插件（比如 Flash）来播放的。然而，并非所有浏览器都拥有同样的插件。

HTML5 规定了在网页上嵌入音频元素的标准，即使用 `<audio>` 元素。

------

#### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_1-浏览器支持)（1）浏览器支持

![Internet Explorer](https://www.runoob.com/images/compatible_ie.gif)![Firefox](https://www.runoob.com/images/compatible_firefox.gif)![Opera](https://www.runoob.com/images/compatible_opera.gif)![Google Chrome](https://www.runoob.com/images/compatible_chrome.gif)![Safari](https://www.runoob.com/images/compatible_safari.gif)

Internet Explorer 9+, Firefox, Opera, Chrome, 和 Safari 都支持 `<audio> `元素.

**注意:** Internet Explorer 8 及更早IE版本不支持 `<audio>` 元素.

------

#### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_2-audio使用)（2）Audio使用

如需在 HTML5 中播放音频，你需要使用以下代码：

> 实例

```html
<audio controls>   
    <source src="horse.ogg" type="audio/ogg">           <source src="horse.mp3" type="audio/mpeg"> 您的浏览器不支持 audio 元素。 
</audio>

<audio> 元素允许使用多个 <source> 元素. <source> 元素可以链接不同的音频文件，浏览器将使用第一个支持的音频文件
```

control 属性供添加播放、暂停和音量控件。

在<audio> 与</audio> 之间你需要插入浏览器不支持的`<audio>`元素的提示文本 。

#### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_3-音频格式及浏览器支持)（3）音频格式及浏览器支持

目前, <audio>元素支持三种音频格式文件: MP3, Wav, 和 Ogg:

| 浏览器               | MP3  | Wav  | Ogg  |
| :------------------- | :--- | :--- | :--- |
| Internet Explorer 9+ | YES  | NO   | NO   |
| Chrome 6+            | YES  | YES  | YES  |
| Firefox 3.6+         | YES  | YES  | YES  |
| Safari 5+            | YES  | YES  | NO   |
| Opera 10+            | YES  | YES  | YES  |

------

#### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_4-音频格式的mime类型)（4）音频格式的MIME类型

| Format | MIME-type  |
| :----- | :--------- |
| MP3    | audio/mpeg |
| Ogg    | audio/ogg  |
| Wav    | audio/wav  |

------

#### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_5-html5-audio-标签)（5）HTML5 Audio 标签

| 标签     | 描述                                                         |
| :------- | :----------------------------------------------------------- |
| <audio>  | 定义了声音内容                                               |
| <source> | 规定了多媒体资源, 可以是多个，在 <video> 与 <audio>标签中使用 |

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_4、html5-新的-input-类型)4、HTML5 新的 Input 类型

HTML5 拥有多个新的表单输入类型。这些新特性提供了更好的输入控制和验证。

本章全面介绍这些新的输入类型：

- **color**

  ```text
  选择你喜欢的颜色: <input type="color" name="favcolor">
  ```

- **date**

  ```text
  生日: <input type="date" name="bday">
  ```

- **datetime**

  ```text
  生日 (日期和时间): <input type="datetime" name="bdaytime">
  ```

- **datetime-local**

  ```text
  生日 (日期和时间): <input type="datetime-local" name="bdaytime">
  ```

- **email**

  ```text
  E-mail: <input type="email" name="email">
  ```

- **month**

  ```text
  生日 (月和年): <input type="month" name="bdaymonth">
  ```

- **number**

  ```text
  数量 ( 1 到 5 之间 ): <input type="number" name="quantity" min="1" max="5">
  ```

  使用下面的属性来规定对数字类型的限定：

  | 属性      | 描述                       |
  | :-------- | :------------------------- |
  | disabled  | 规定输入字段是禁用的       |
  | max       | 规定允许的最大值           |
  | maxlength | 规定输入字段的最大字符长度 |
  | min       | 规定允许的最小值           |
  | pattern   | 规定用于验证输入字段的模式 |
  | readonly  | 规定输入字段的值无法修改   |
  | required  | 规定输入字段的值是必需的   |
  | size      | 规定输入字段中的可见字符数 |
  | step      | 规定输入字段的合法数字间隔 |
  | value     | 规定输入字段的默认值       |

- **range**

  ```text
  <input type="range" name="points" min="1" max="10">
  ```

  请使用下面的属性来规定对数字类型的限定：

  - max - 规定允许的最大值
  - min - 规定允许的最小值
  - step - 规定合法的数字间隔
  - value - 规定默认值

- **search**

  ```text
  Search Google: <input type="search" name="googlesearch">
  ```

- **tel**

  ```text
  电话号码: <input type="tel" name="usrtel">
  ```

- **time**

  ```text
  选择时间: <input type="time" name="usr_time">
  ```

- **url**

  ```text
  添加您的主页: <input type="url" name="homepage">
  ```

- **week**

  ```text
  选择周: <input type="week" name="week_year">
  ```

### [#](https://ydlclass.com/doc21xnv/javaweb/html/#_5、html5-新的表单属性)5、HTML5 新的表单属性

HTML5 的 <form> 和 <input>标签添加了几个新属性.

**<input>新属性**：

- **autocomplete**

  自动提示以前输入过的内容

- **autofocus**

  autofocus 属性是一个 boolean 属性.

  autofocus 属性规定在页面加载时，域自动地获得焦点。

  ```text
  First name:<input type="text" name="fname" autofocus>
  ```

- **height 与 width**

  height 和 width 属性规定用于 image 类型的 标签的图像高度和宽度。

  **注意:** height 和 width 属性只适用于 image 类型的 标签。

  ```text
  <input type="image" src="img_submit.gif" alt="Submit" width="48" height="48">
  ```

- **min 与 max**

  min、max 和 step 属性用于为包含数字或日期的 input 类型规定限定（约束）

  **注意:** Internet Explorer 9及更早 IE 版本，Firefox 不支持 input 标签的 max 和 min 属性。

  ```text
  <input type="number" name="quantity" min="1" max="5">
  ```

- **pattern (regexp)**

  pattern 属性描述了一个正则表达式用于验证 元素的值。

  ```text
  Country code: <input type="text" name="country_code" pattern="[A-Za-z]{3}" title="Three letter country code">
  ```

- **placeholder**

  placeholder 属性提供一种提示（hint），描述输入域所期待的值。

  简短的提示在用户输入值前会显示在输入域上

  ```text
  <input type="text" name="fname" placeholder="First name">
  ```

- **required**

  **注意：** Internet Explorer 9及更早 IE 版本，或 Safari 不支持 input 标签的 required 属性。

  required 属性是一个 boolean 属性.

  required 属性规定必须在提交之前填写输入域（不能为空）。

  ```text
  Username: <input type="text" name="usrname" required>
  ```

**<form>新属性：**

- **autocomplete**

  autocomplete 属性规定 form 或 input 域应该拥有自动完成功能。

  当用户在自动完成域中开始输入时，浏览器应该在该域中显示填写的选项。

- **novalidate**

  novalidate 属性是一个 boolean(布尔) 属性.

  novalidate 属性规定在提交表单时不应该验证 form 或 input 域。