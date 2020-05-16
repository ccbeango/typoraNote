# HTML常用元素

HTML提供了大量元素，没一个元素都有特殊的用途，保证网页的丰富多样性：

* 区块：div
* 区分：span
* 文本：p、h1~h6、em、dt、dd
* 表格：table、tbody、thead、tr、td、th、tfoot、caption
* 表单：form、input、label、textarea、select
* 链接：a
* 图片：img
* 文档：html、head、title、body、meta
* 列表：ul、ol、li、dlside、footer、nav
* 其他：br、hr、iframe
* 结构：header、section、a、strong、pre、adderss、q、blcokquote、cite、code

## 文档声明

`<!DOCTYPE html>`

## HTML的基本元素

### html元素

是HTML文档的根元素，一个文档中只有一个，其他元素都是它的后代元素

W3C标准建议为html元素增加`lang`属性

```html
<html lang="en"></html>
```

作用是：

* 帮助语音合成工具确定要使用的发音
* 帮助翻译工具确定要使用的翻译规则

### head元素

head元素里面的内容是一些元数据（描述数据的数据）

一般用于描述网页的各种信息，如字符编码、网页标题。网页图标

* title
* meta 
* style
* link

### h、p、strong、code、br、hr



### 字符实体

HTML中有一些字符是预留出来作特殊用途的，比如

* 小于号(<)
* 大于号（>）

想要在网页中正确地显示这些预留字符，必须使用字符实体，书写格式一般有两种

* &entity_name: &nbsp(空格) &lt(小于号)
* &#entity_number：&#160(空格) &#60（小于号）

### span元素

默认情况下，跟普通文本几乎没差别

用于区分特殊文本和普通文本，比如用来显示一些关键字。

对普通的文本进行归类。

### div元素

一般作为其他元素的容器，把其他元素包住，代表一个整体。

用于把网页分割为多个独立的部分。

### img元素

img元素是单标签

img元素的属性：

* src：设置图片的路径，可以使用绝对路径或相对路径。
* alt
* width
* height：使用很少

### a元素

定义超链接，用于打开新的URL

属性：

* href：链接地址。如果不写href属性，会被识别为普通文本。
* target：打开方式
  * `_self`
  * `_blank`
  * `_parent`：与iframe配合使用
  * `_top`：与iframe一起使用

a元素和base元素可以结合使用：

如果页面中要访问的url的域名或ip相同，可以抽离到base中。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <base href="https://www.baidu.com" target="_blank">
</head>
<body>
    <a href="/">百度一下</a>
    <a href="/img/bd_logo1.png">百度logo</a>
</body>
</html>
```

#### 锚点

```html
<!-- 只有#号的锚点会跳转到页面顶部 -->
<a href="#">顶部</a>
<a href="#title1">标题1</a>
<a href="#title2">标题2</a>
<a href="#title3">标题3</a>

<h2 id="title1">我是内容1</h2>
<h2 id="title2">我是内容2</h2>
<h2 id="title3">我是内容3</h2>
```

#### 伪链接

有时候点击链接的时候不希望打开新的URL，而是希望做点别的事情，这时候就可以使用伪链接。

伪链接：没有指明具体链接地址的链接

点击伪链接之后具体来做什么事情，要编写对应的js代码。

如果暂时不需要做任何事情，可以用下面的形式

```html
<a href="#" onclick="return false;">伪链接1</a>
<a href="javascript:">伪链接2</a>
```



### iframe

现在使用很少。

在网页中嵌套网页。

### 标签语义化的重要性

标签语义化指的是选择标签的时候尽量让每一个标签都有正确的语义。

虽然很多标签之间互换之后也能实现功能，但还要遵守标签语义化原则。

## 列表

HTMl提供了3组常用的用来展示列表的元素：

* 有序列表 ol、li
* 无序列表 ul、li
* 定义列表： dl、dt、dd

### 有序列表

`ol` (ordered list) 有序列表，直接子元素只能是`li`(list item)

```html
<ol>
  <li>海王</li>
  <li>海贼王</li>
  <li>上海堡垒</li>
  <li>星际穿越</li>
</ol>
```

对于列表，Chorme浏览器默认会加上如下样式元素。

```css
/* user agent stylesheet */
ol {
  display: block;
  list-style-type: decimal;
  margin-block-start: 1em;
  margin-block-end: 1em;
  margin-inline-start: 0px;
  margin-inline-end: 0px;
  padding-inline-start: 40px;
}

/* user agent stylesheet */
li {
  display: list-item;
  text-align: -webkit-match-parent;
}
```

浏览器默认加上了边距的样式，加边距的时候，Chrome使用了`margin-block-**`，而不是使用`margin-left`或其他的，是因为有些国家或地区在阅读时，可能是从右向左，这样设置更容易进行适配。

### 无序列表

`ul` (unordered list) 有序列表，直接子元素只能是`li`(list item)。

默认样式等，与有序列表基本相同。

### 定义列表

`dl`(definition list)定义列表，直接子元素只能是dt(definition term)、dd(definition description)。

`dt`：列表中每一项的项目名

`dd`：列表中每一项的具体描述，是对`dt`的描述、解释、补充。

* 一个`dt`后面一般紧跟着一个或多个`dd`。

`dt`、`dd`常见的组合是：

* 事务的名称、事务的描述
* 问题、答案

* 类别名、归属于这类的各种事务

```html
<dl>
   <dt>红饮料</dt>
   <dd>西瓜汁</dd>

   <dt>黑饮料</dt>
   <dd>咖啡</dd>

   <dt>白饮料</dt>
   <dd>牛奶</dd>
 </dl>
```

## 表格

`table`: 表格

`tr`：表格中的行

`td`：行中的单元格

### table相关属性

table的常用属性

| 属性        | 说明                                      |
| ----------- | ----------------------------------------- |
| border      | 边框的宽度                                |
| cellpadding | 单元格内部的间距                          |
| cellspacing | 单元格之间的间距                          |
| width       | 表格的宽度                                |
| align       | 表格的水平对齐方式<br>left、center、right |

```html
 <table border="1" cellspacing="10" width="500" align="center">
   <tr>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
   </tr>
   <tr>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
   </tr>
   <tr>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
   </tr>
   <tr>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
     <td>内容</td>
   </tr>
 </table>
```

### tr和td相关属性



# Emmet语法

## ！ 和 html:x

使用`!`或`html[:x]`，x可以不写，或5或xml，能够直接生成html模板。

## > 子代 和 + 兄弟

```html
<!-- 结构 div>p>span>strong -->
<div>
  <p><span><strong></strong></span></p>
</div>

<!-- 结构 h2+div+p+ -->
<h2></h2>
<div></div>
<p></p>

<!-- div>h2+a+p>span -->
<div>
  <h2></h2>
  <a href=""></a>
  <p><span></span></p>
</div>
```

## *  多个 和 ^ 上一级

```html
<!-- div>p*3 -->
<div>
  <p></p>
  <p></p>
  <p></p>
</div>

<!-- div>span^div>p -->
<div><span></span></div>
<div>
  <p></p>
</div>

<!-- div>p*2>span^^h1+strong -->
<div>
  <p><span></span></p>
  <p><span></span></p>
</div>
<h1></h1>
<strong></strong>
```

## () 分组

```html
<!-- div>(p>span)+h2+strong -->
<p><span></span></p>
<h2></h2>
<strong></strong>

<!-- 转换 div>p*2>span^^h1+strong -->
<!-- 为 (div>(p*2>span))+h1+strong -->
<div>
  <p><span></span></p>
  <p><span></span></p>
</div>
<h1></h1>
<strong></strong>
```

感觉还是找上一层`^`好用。

## 属性(id属性、class属性、普通属性) 和  数字

```html
<!-- div#main -->
<div id="main"></div>

<!-- div.box  -->
<div class="box"></div>

<!-- div[title] -->
<div title=""></div>

<!-- div[title="哈哈"] -->
<div title="哈哈"></div>

<!-- 添加多个属性 div#main.box1.box2[title="test"]-->
  <div id="main" class="box1 box2" title="test"></div>

<!-- 练习div#main>div.box+p.p1+span.title^div#footer>div.box2 -->
  <div id="main">
    <div class="box"></div>
    <p class="p1"></p>
    <span class="title"></span>
  </div>
  <div id="footer">
    <div class="box2"></div>
  </div>
```

## {} 内容

```html
<!-- div.box{我是内容} -->
<div class="box">我是内容</div>
```

## $ 数字

```html
<!-- 属性数字 div.box$*4 -->
<div class="box1"></div>
<div class="box2"></div>
<div class="box3"></div>
<div class="box4"></div>

<!-- ul>li.item$$$*5 -->
<ul>
  <li class="item001"></li>
  <li class="item002"></li>
  <li class="item003"></li>
  <li class="item004"></li>
  <li class="item005"></li>
</ul>


<!--  内容数字 div.box{我是内容$}*3 -->
<div class="box">我是内容1</div>
<div class="box">我是内容2</div>
<div class="box">我是内容3</div>
```

## 隐式标签

一些约定俗成，挡在不指定元素的时候，会根据情况生成元素。

一般情况下，隐式标签是代表`div`的，不过也可以代表`li`或`table`

```html
<!-- #main -->
<div id="main"></div>

<!-- .box -->
<div class="box"></div>

<!-- div>.wrap>.content -->
<div>
  <div class="wrap">
    <div class="content"></div>
  </div>
</div>

<!-- ul>.item${列表元素$}*3 -->
<ul>
  <li class="item1">列表元素1</li>
  <li class="item2">列表元素2</li>
  <li class="item3">列表元素3</li>
</ul>


<!-- table>#row$*4>[colspan=2] -->
<table>
  <tr id="row1">
    <td colspan="2"></td>
  </tr>
  <tr id="row2">
    <td colspan="2"></td>
  </tr>
  <tr id="row3">
    <td colspan="2"></td>
  </tr>
  <tr id="row4">
    <td colspan="2"></td>
  </tr>
</table>
```

## CSS的Emmet语法

部分举例。

```css
.box {
  /* w200+h150+m20+p30 */
  /* w200 */
  width: 200px;
  /* h200 */
  height: 150px;
  /* m20  */
  margin: 20px;
  /* p30  */
  padding: 30px;
}

.box2 {
  /* m20-20-40-50 */
  margin: 20px 20px 40px 50px;
}

.box3 {
  /* p-10-20--30 */
  padding: -10px 20px -30px;
}

.box4 {
  /* m10px20px */
  margin: 10px 20px;
}

.box5 {
  /* m10px-20 */
  margin: 10px -20px;
}
```

字体的

```css
.box{
 /* fz20 */
 font-size: 20px;

  /* fz1.5 */
  font-size: 1.5em;

  /* fw700 */
  font-weight: 700;
  /* lh40 */
  line-height: 40;
  /* lh40px */
  line-height: 40px;

  /* bgc#333 */
  background-color: #333333;
}
```





# CSS

CSS全称是Cascading Style Sheets。

CSS3：是CSS2.x以后对某一些CSS模块进行升级更新后的称呼，比如CSS Color Module Level 3、Select Level 3、CSS Namespaces Module Level 3。目前并不存在真正意义的CSS 3。

常见的CSS属性的具体用途，大致可以分类为：

* 文本：color、direction、letter-spacing、word-spacing、line-height、text-align、text-indent、text-transform、text-decoration、white-space
* 字体：font、font-family、font-style、font-variant、font-weight
* 背景：background、background-color、background-image、background-repeat、background-attachment、background-position
* 盒子模型：width、height、border、margin、padding
* 列表：list-style
* 表格：border-collapse
* 显示：display、visibility、overflow、opacity、filter
* 定位：vertical-align、position、left、top、right、bottom、float、clear

## CSS样式应用



CSS样式应用到元素上有3种方法：

* 内联样式 （inline style）
* 文档样式表（document style sheet）、内嵌样式表（embed style sheet）
* 外部样式表 （external style sheet）

在Chrome中的调试窗口可以看到有些样式右上角标注`user agent stylesheet`，这是用户代理样式，即浏览器默认的标签样式。

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <link rel="stylesheet" href="./css/style.css">
  <style>
    /* 文档样式表 */
    .document-style {
      color: blue;
      font-size: 60px;
    }
  </style>
</head>
<body>
  <!-- 内联样式 inline -->
  <h1 style="color: red; font-size: 50px">哈哈哈</h1>

  <!-- 文档样式表 document style sheet -->
  <h1 class="document-style">呵呵呵</h1>

  <!-- 外部样式表 （external style sheet） -->
  <h1 class="external-style">咯咯咯</h1>
</body>
</html>
```

### css编码设置

关于外部样式表，推荐使用`utf-8`

这样可以避免浏览器解析样式，出现中文时的编解码错误。如：`font-family: '华文宋体'`，如果不设置编码，可能就会出错。

```css
@charset: "utf-8"
```

### 使用@import导入外部样式

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Document</title>
  <style>
    @import: url('./css/style.css');
  </style>
</head>
<body>
  <h1 class="external-style">咯咯咯</h1>
</body>
</html>
```

使用`@import`引入和`<link>`进行引入都可以。

### 设置网页图标

```css
<link rel="icon" type="image/x-icon" href="">
```

link元素可以用来设置网页的图标

* link元素的`rel`属性不能省略，用来指定文档与链接资源的关系
* 一般rel若确定，响应的type也会默认确定，所以可以省略type
* 网页图标支持图片格式是`ico`、`png`，常用大小是`16*16`、`24*24`、`32*32`像素

## CSS选择器 （selector）

CSS选择器就是按照一定的规则，选择出符合条件的元素，为之添加CSS样式。

选择器种类大概可以分为：

* 通用选择器（universal selector）
* 元素选择器（type selectors）
* 类选择器（class selectors）
* id选择器（id selectors）
* 属性选择器（attribute selectors）
* 组合（combinators）
* 伪类（pseudo-classes）
* 伪元素（pseudo-elements）

### 通用选择器

选择所有的元素。

```css
* {
  color: red;
}
```

一般是用来给所有元素做一些通用性的设置。

```css
* {
  margin: 0;
  padding: 0;
}
```

### 元素选择器

选择指定的元素。

```css
div {
  color: red;
}
```

### class选择器

注意点：

* 一个元素可以有多个class，每个class之间用空格隔开
* class值如果由多个单词组成，`单词之间可以使用中划线

### id选择器

通过id选择选择指定元素，每个id值在一个文件中应该只出现一次。

### 属性选择器

根据属性进行选择，比如

```css
[title="test"] {
  color: red;
}

[title*="test"] {
  color: red;
}
```



### 后代选择器

又叫组合选择器，包含直接和间接所有的。

```html
<style>
div span {
  color: red;
}
</style>

<span>文字内容1</span>
<div class="box">
  <span>文字内容2</span>
  <p>
    <span>文字内容3</span>
  </p>
  <div>
    <span>文字内容4</span>
  </div>
  <span>文字内容5</span>
</div>
<span>文字内容6</span>
```

选中div下面span元素，所以选中的是2/3/4/5。

### 子代选择器

选择直接的子元素，只包含子代的。

```html
<style>
div > span {
  color: red;
}
</style>

<span>文字内容1</span>
<div class="box">
  <span>文字内容2</span>
  <p>
    <span>文字内容3</span>
  </p>
  <div>
    <span>文字内容4</span>
  </div>
  <span>文字内容5</span>
</div>
<span>文字内容6</span>
```

所以符合规则的是2/4/5

p标签中不可以包含div，包含的话结构会直接乱掉。

### 相邻兄弟选择器

div元素后面紧挨着的元素（且div、p元素必须是兄弟关系）

```html
<style>
  div+p {
    color: red;
  }
</style>
<p>文字内容1</p>
<div>
  <p>文字内容2</p>
</div>
<p>文字内容3</p>
<p>文字内容4</p>
```

符合规则的是3

### 全体兄弟选择器

div元素后面的p元素（且div、p元素必须是兄弟关系）

```html
<style>
  div~p {
    color: red;
  }
</style>
<p>文字内容1</p>
<div>
  <p>文字内容2</p>
</div>
<p>文字内容3</p>
<p>文字内容4</p>
```

符合规则的是3/4

### 选择器组

#### 交集选择器

同时符合2个条件的元素：div元素、class值有one的元素

```css
 <style>
    div.one[title="test"] {
      color: red;
    }
  </style>
  <div class="one">
    <p>文字内容1</p>
  </div>
  <div class="two">文字内容2</div>
  <p class="one">文字内容3</p>
	<div class="one" title="test">文字内容5</div>
</body>
```

符合规则的是5

#### 并集选择器

所有的div元素 + 所有class值有one的元素 + 所有title属性值等于test的元素。

```html
<style>
  div, .one, [title="test"] {
    color: red;
  }
</style>
<div class="one">文字内容1</div>
<span title="test">文字内容2</span>
<p class="one">文字内容3</p>
```

符合条件的有1/2/3

### 伪类选择器（pseudo-classes）

常见的伪类有

* 动态伪类（dynamci pseudo-classes）
  * `:link`、`:visited`、`:hover`、`:active`、`:focus`
* 目标伪类（target pseudo-classes)
  * `:target`

* 语言伪类(language pseudo-classes) 使用较少
  * `:lang()`
* 元素状态伪类(UI element state pseudo-classes) 
  * `:enabled`、`:disabled`、`:checked`
* 结构伪类(structural pseudo-classes)
  * `:nth-child()`、`:nth-last-child()`、`:nth-of-type()`、`:nth-last-of-type()`
  * `:first-child`、`:last-child`、`first-of-type`、`:last-of-type`
  * `:root`、`:only-child`、`:only-of-type`、`:empty`
* 否定伪类(negation pseudo-classes)
  * `:not()`

#### 目标伪类

使用较少，一般在锚点中使用。

比如点击某个锚点，让选中的标题变颜色。

```css
/* 选中的锚点字体变成红色 */
:target {
  color: red;
}
```

#### 元素状态伪类

使用较少

设置某个元素处于某种状态时设置样式

```html
<style>
  button:enabled {
    color: green;
  }

  button:disabled {
    color: blue;
  }


</style>
<button>我是按钮1</button>
<button disabled>我是按钮2</button>
```

设置按钮在可用时是绿色，不可用时是蓝色。

#### 动态伪类

##### a元素上的使用

使用举例，可以在a链接上设置。

* `a:link`：未访问的链接
* `a:visited`：已访问的链接
* `a:hover`：鼠标移动到链接上
* `a:active`：激活的链接（鼠标在链接上长按住未松开）

```html
<style>
  /* 给a元素设置样式，会应用到所有伪类 */
  a {
  	font-size: 20px  
  }
  
  a:link {
    color: red;
  }

  a:visited {
    color: gray;
  }

  a:hover {
    color: blue;
  }

  a:active {
    color: orange;
  }
</style>
<a href="">Google一下</a>
```

如果给a元素设置样式，相当于给a元素的所有动态伪类都设置了。

要注意：

* `:hover`必须放在`:link`和`:visited`后面才能生效。
* `:active`必须放在`:hover`后面才能完全生效。

官方推荐的顺序是上面的顺序link visted hover acitve。

记忆小技巧：女朋友看到LV后，ha ha 大笑，哈哈哈。

##### 其他元素上的使用

`:hover`、`:active`也可以用到其他元素上：

```html
<style>
  strong:hover {
    color: blue;
  }

  strong:active {
    font-size: 30px;
  }

</style>
<strong>哈哈</strong>
```

##### :focus的使用

:focus指当前拥有输入焦点的元素（能接收键盘输入）

文本输入框聚焦后，北京变为灰色

```css
input:foucs {
  background: gray;
}
```

因为链接a元素可以被键盘Tab键选中聚焦，所以`:focus`也适用于a元素

```html
<style>
  a:focus {
    color: yellowgreen;
  }
</style>
<a href="">哈哈</a>
```

所以此时链接a上可以有五种动态伪类，建议的编写顺序为：

`:link`、`:visited`、`:focus`、`:hover`、`:active`

记忆：女朋友看到LV包包后，Feng了一样地ha ha大笑。

##### 去掉a元素的 :focus

开发过程中，a标签很多时候不希望获得foucs获取焦点。可以使用一些css的方法：

```css
a:focus {
  outline: none
}
```

这种方法其实是选中了，只是看不出来效果。

第二种办法，给a标签加上属性`tabindex`，这个属性是添加Tab键的选中顺序。

```html
<a tabindex="-1" href="">Google一下</a>
```

设置成`-1`之后，Tab键是无法选中的。

#### 结构伪类

##### :nth-child()

子元素选择器，选中第几个子元素改变样式。

```html
<style>
  /*
  	交集选择器
     * 是一个p元素
     * 同时p元素作为子元素的第三个元素
  */
  p:nth-child(3) {
    color: red;
  }
</style>
<body>
  <div>
    <p>文字内容1</p>
    <p>文字内容2</p>
    <p>文字内容3</p>
    <p>文字内容4</p>
    <p>文字内容5</p>
  </div>
  <div>我是个div</div>
  <p>文字内容6</p>
</body>
```

文字内容3和6会变为红色。

`:nth-child()`根据n选择设置颜色，可以根据自己的需求，进行相关的计算。

```html
<style>
  /* 所有p的子元素设置为绿色 */
  p:nth-child(n) {
    color: green;
  }

  /* 偶数p红色 */
  p:nth-child(2n) {
    color: red;
  }

  /* 偶数p红色 */
  p:nth-child(even) {
    color: red;
  }

  /* 奇数p蓝色 */
  p:nth-child(2n+1) {
    color: blue;
  }

  /* 奇数p蓝色 */
  p:nth-child(odd) {
    color: blue;
  }
  
  /* 选中前5个设置紫色 */
  p:nth-child(-n+5) {
    color: purple;
  }
</style>
<div>
  <p>文字内容1</p>
  <p>文字内容2</p>
  <p>文字内容3</p>
  <p>文字内容4</p>
  <p>文字内容5</p>
  <p>文字内容6</p>
  <p>文字内容7</p>
  <p>文字内容8</p>
  <p>文字内容9</p>
</div>
```

一个例子

```html
<style> 
  p:nth-child(4n+1) {
    color: red;
  }

  p:nth-child(4n+2) {
    color: blue;
  }

  p:nth-child(4n+3) {
    color: green;
  }

  p:nth-child(4n+4) {
    color: orange;
  }
</style>
<body>
  <div>
    <div>div</div>
    <p>文字内容1</p>
    <p>文字内容2</p>
    <p>文字内容3</p>
    <p>文字内容4</p>
    <span>span</span>
    <p>文字内容5</p>
    <p>文字内容6</p>
  </div>
</body>
```

p标签的颜色从上至下依次是：蓝、绿色、橘色、红色、绿色、橘色。

##### :nth-last-child()

`:nth-child()`是正着数，`:nth-last-child()`是倒着数。

其他的特性和`:nth-child()`是一样的。

`:nth-last-child(1)`倒数第一个子元素；

`:nth-last-child(2n)`选中偶数的子元素；

`nth-last-child(-n+2)`，代表最后两个子元素。

##### :nth-of-type()

`:nth-of-type()`用法和`:nth-child()`类似，不同点是`:nth-of-type()`计数时只计算同种类型的元素。

忽略其他类型，只选择同类的子元素进行样式添加。

```html
<style> 
  p:nth-child(3) {
    color: blue;
  }

  p:nth-of-type(3) {
    color: red;
  }
</style>
<body>
  <div>
    <div>div</div>
    <p>文字内容1</p>
    <p>文字内容2</p>
    <p>文字内容3</p>
    <p>文字内容4</p>
    <span>span</span>
    <p>文字内容5</p>
    <p>文字内容6</p>
  </div>
</body>
```

文字内容2变成蓝色，文字内容3变成红色。`:nth-child()`会在所有的子元素中进行选择，`:nth-of-type()`只会在同类的子元素中进行选择。

`nth-of-type(2n)`或`:nth-of-type(2n+1)`表示从相同的子类中选择偶数或奇数。使用同上面的是一个原理。

假如说这样写就表示找每个类型的偶数。

```css
:nth-of-type(2n) {
  color:red;
}
```

再来看这例子，样式改变是什么哪些：

```html
<style>
  p:nth-of-type(2) {
    color: red;
  }
</style>

<body>

  <div>
    <p>文字内容1</p>
    <div>
      <p>文字内容2</p>
    </div>
    <div>
      <div>
        <p>文字内容3</p>
      </div>
      <p>文字内容4</p>
      <p>文字内容5</p>
    </div>
    <p>文字内容6</p>
  </div>

</body>
```

变红色字体的是5和6

##### :nth-last-of-type()

同理`nth-of-type()`，唯一不同是，倒过来找。不再赘述。

##### 其他伪类

* `:first-child` 等同于`:nth-child(1)`

* `:last-child`等同于`:nth-last-child(1)`

* `:first-of-type`等同于`:nth-last-of-type(1)`

* `:only-child`是父元素中唯一的子元素，选中。

  ```html
  
  <style>
    body :only-child {
      color: red;
    }
  </style>
  <!-- 文字内容2和3变红 -->
  <body>
    <p>文字内容1</p>
    <div>
      <p>文字内容2</p>
    </div>
    <div>
      <div>
        <p>文字内容3</p>
      </div>
      <p>文字内容4</p>
      <p>文字内容5</p>
    </div>
    <p>文字内容6</p>
  
  </body>
  ```

* `only-of-type`，是父元素中唯一的这种类型的子元素。

  ```html
  <style>
    body :only-of-type {
      color: red;
    }
  </style>
  <!-- 文字内容1、4和5变红 -->
  <body>
    <div>
      <p>
        <span>文字内容1</span>
      </p>
      <div>文字内容2</div>
      <div>文字内容3</div>
    </div>
    <div>
      <strong>文字内容4</strong>
      <a href="">
        <span>文字内容5</span>
      </a>
    </div>
  </body>
  ```

* `:root`根元素，也就是html元素。下面两种写法等价

  ```css
  html {
    
  }
  
  :root {
    
  }
  ```

* `:empty` 选中元素内容为空的元素。

  ```html
  <style>
    :empty {
      height: 20px;
      background-color: red;
    }
  </style>
  <!-- 空元素p和div会被选中 -->
  <body>
    <div>
      <p></p>
      <span>文字内容1</span>
    </div>
    <div>
      <strong>文字内容2</strong>
      <a href="">文字内容3</a>
      <div></div>
    </div>
  </body>
  
  ```

  

##### :not() 否定伪类

`:not()`的格式是`:not(x)`，表示除`x`以外的元素。

`x`是一个选择器，可以是：元素选择器、通用选择器、属性选择器、类选择器、id选择器、伪类（除否定伪类）。

```html
<style>
  body :not(div) {
    color: red;
  }
</style>
<!-- 文字内容2和3变红 -->
<body>
  <div>文字内容1</div>
  <p>文字内容2</p>
  <span>文字内容3</span>
  <div>文字内容4</div>
</body>

```

`:not()`中的参数也可以是类名或者id名

### 伪元素选择器 (pseudo-elements)

伪元素可以看成是行内元素。

常用的伪元素有：

* `:first-line`、`::first-line`
* `:fitst-letter`、`::first-letter`
* `:before`、`::before`
* `:after`、`::after`

为了区分伪元素和伪类，建议伪元素使用2个冒号，如`::first-line` 

#### ::first-line

选中第一行，针对首行文本设置属性

```css
div::first-line {
  color: blue;
  text-decoration: underline;
}
```

只有下列属性可以应用到`::first-line`上：

* 字体属性、颜色属性、背景属性
* `word-spacing`、`letter-spacing`、`text-decoration`、`text-transform`、`line-height`

#### ::first-letter

选中第一个字母，针对首字母设置属性

```css
div::first-letter {
  color: blue;
  font-size: 30px;
}
```

只有下列属性可以应用在`::first-letter`上：

* 字体属性、margin属性、padding属性、border属性、颜色属性、背景属性
* `text-decoration`、`text-transform`、`letter-spacing`、`word-spacing`(适当的时候)、`line-height`、`float`、`vertical-align`(只有当float是none时)

#### ::before 和 ::after

`::before`和`::after`用来在一个元素内容之前或之后插入其他内容(可以是文字、图片)。

```html
  span::before {
    content: "1";
    color: red;
    margin-right: 5px;
  }  

  span::after {
    content: "abc";
    color: purple;
    margin-left: 5px;
  }
</style>

<body>
  <span>我是一个span</span>
</body>

```

这个元素中，不能省略content属性，即使里面是空字符串，属性是不能删除的。

```css
div::before {
  content: "";
  display: inline-block;
  width: 100px;
  height: 20px;
  background-color: red;
}

div::before {
  content: url("bg001.png")
}
```

## CSS常用属性

* color 前景色，不止字体颜色，例如还可以作用border边框，或text-decoration:line-through。
* font-size 字体大小
* background-color 背景颜色
* width/height 宽高

​	**宽度和高度不适用与非替换行内元素**

### 颜色设置的方法

1. 基本颜色关键字red、black、yellow、blue等，只有上百种基本颜色的关键字。
2. RGB颜色十进制：rgb()
3. RGBA颜色：和十进制之前相同，a指alpha，透明度。如：rgb()
4. RGB颜色十六进制：#FFFFFF

### 查看网站布局的技巧

在浏览器中打开开发者工具，然后添加一个新的div全局样式属性，增加`outline`就可以进行布局结构的查看

```css
div {
    outline: 2px solid red !important;
}
```

### CSS属性-文本

#### text-transform

转换文本大小写

#### 文本 text-decoration

text-decoration 用于设置文字的装饰线

很多时候用来去掉`a`标签的下划线。

```css
a {
  text-decoration: none;
}
```

还有就是比如用来加删除划线，划掉原来的价格。

html中的u标签，是设置了这个属性

```css
/* u标签默认样式 */
u {
    text-decoration: underline;
}
```

#### 文本 letter-sapcing

设置之母之间的间距 

#### 文本 word-spacing

设置单词之间的间距

#### 文本 text-ident

设置文本首行缩进。

可以使用但是`em`进行首行缩进。比如说，一段文本中的字体大小是`20px`，想要首行缩进2个字，可以使用`text-indent: 40px;`或`text-indet: 2em;`

```css
p {
  text-indent: 40px;
  text-indet: 2em;
}
```



#### 文本 text-align

设置元素内容在元素中的水平对齐方式。

```css
div {
  background-color: #0f0;
  text-align: center;
}
```

可以用户设置div中的元素，文本，图片等。

可以设置元素内容居中，但是不能设置div居中。因为div是块级元素，父级的盒子会认为它自己内部的盒子本身宽度和自己是一致的，所以是不会对内部的盒子产生作用。

```html
<style>
    .box {  
      background-color: #0f0;
      text-align: center;
    }
    
    .inner {
      background-color: purple;
      width: 200px;
    }
</style>

<div class="box">
   <div class="inner">我是div</div>
</div>
```

如果不设置inner的宽度，看起来好现实box中设置的元素内容居中作用在了inner的div盒子上，其实不是的，由于继承，inner盒子也有了文本设置属性，他只是作用在了inner盒子的内容上。

可以这样改

```css
.inner {
  background-color: purple;
  width: 200px;
  display: inline-block
}
```

### CSS属性-字体

Google浏览器设置字体，最小是12px，再小其实就看不清楚了。

浏览器默认字体的大小是浏览器自己设置的，例如chrome浏览器我们就可以自己在设置中进行字体样式的相关设置。

#### 字体 font-size

决定文字的大小。字体默认浏览器设置的大小是16px

常用设置：

具体数值+单位，如100px

也可以使用em单位，如`1em`代码100%，`2em`代表200%，`0.5em`代表50%。或者使用rem单位，现在移动端使用很多。

```html
<style>
    .box {
      font-size: 18px;
    }

    p {
      /* 相对于父级box的字体大小，所以字体大小是2*18px */
      font-size: 2em;
    }
  </style>
</head>
<body>
  <div class="box">
    <span>我是span元素</span>
    <p>我是段落，哈哈哈</p>
  </div>
</body>
```

#### 字体 font-family

用于设置字体的类型。如微软雅黑

当设置某种字体的时候，浏览器会去读取操作系统中是否有这种字体，Windows下会去`C:\Windows\Fonts`目录下寻找设置的字体是否存在，如果有就会去使用这种字体。

如果没有这种字体，那么设置的字体将失效。为了防止设置的字体刚好操作系统不存在，一般会一次性设置多个字体

```css
font-family: Arial, Helvetica, sans-serif; 
```

字体中间有空格，可以使用单引号包含起来。

浏览器会从左至右依次选择设置的字体，直到找到可用的字体，如果都不存在，那么浏览器会使用操作系统的默认字体。

一般情况下，英文字体只适用于英文，中文字体同时适用于中文和英文。所以在开发中，中英文需要使用不同的字体，可以将英文字体写在前面，中文字体写在后面。

```css
div {
  font-family: "Courier New", "宋体"
}
```



#### 字体 font-weight

设置文本的粗细。

使用某些html标签，如h1~h6、b、strong，其实是浏览器给这些标签设置了font-weight属性，显示出来的字体就是加粗的。标签也是设置了css样式而已。

```css
/* h1的默认样式 */
h1 {
    display: block;
    font-size: 2em;
    margin-block-start: 0.67em;
    margin-block-end: 0.67em;
    margin-inline-start: 0px;
    margin-inline-end: 0px;
    font-weight: bold;
}
/* b标签默认样式 */
b {
    font-weight: bold;
}
```

#### 字体 font-style

用于设置文字的常规、斜体显示。

如使用i标签，是加上了这个属性。

```css
i {
    font-style: italic;
}
```

我们很少使用这种标签，一般会使用字体样式来设置。

i标签使用很多，都会使用i标签和伪类配合，来设置小图标之类的。

#### 字体 font-variant

影响小写字母的显示形式。

`small-caps`将小写字母替换为缩小过的大写字母规格。

#### 字体 line-height

用于设置文本的最小行高。

##### 简单理解

行高可以简单理解为一行文本所占据的高度。

例子：

```html
<style>
  .box {
    background: green;
    font-size: 18px;
  }
</style>
<div class="box">
  我是div元素
</div>
```

设置的文本，在浏览器中显示，文本在盒子中占据了一定的高度，但要注意的是，**文本占据的高度并不等于文字的高度**，我们的直观印象会认为，一行文本占据的高度等于文本的高度，但这是不一定的。

打开浏览器选中“我是div元素”文本，我们可以看到，默认情况下文本占据的高度其实是大于文字的高度的，文本上下都多出来一部分区域。

上面的div我们并没有设置高度，不过我们都知道是有高度的，这个高度我们通常会说是里面的内容撑起来的。准确来说，撑起来div高度的就是文本的行高。

为什么要有行高呢？比如说我们在读一首诗时，这首诗是竖着读还是横着读，我们会自动根据这个诗字体间的间距去决定该怎样去读。就是因为这些文字之间有间距，我们潜移默化地会去决定怎样读这段文字。

##### 严格定义

行高的严格定义是，两行文字基线(baseline)之间的间距。

baseline：与小写字母`x`最底部对齐的线。

行距：当前文本底线和下一行文本顶线之间的距离。

![](https://raw.githubusercontent.com/ccbeango/blogImages/master/HTML%2BCSS/CSS入门基础概念总结02.png)

根据行高的定义，我们知道了行高就是右侧标出是行高的那根黑线即两条基线之间的高度，我们又可以知道其实右侧三条黑色箭头代表的高度都是行高。

我们要注意区分`height`和`line-height`的区别：

* height：元素的整体高度
* line-height：元素中每一行文字所占据的高度

行高一个最常用的实例就是，让一行文字在div内部进行垂直居中。

```html
<style>
  .box {
    font-size: 20px;
    height: 50px;
    line-height: 50px;
    background: green;
  }
</style>
<div class="box">
  我是div元素
</div>
```

上面的文字在div中是垂直居中的。

假如说当前的行高是30px，文本的高度是20px，现在行距就是10px，文本在排布的时候，上下距离各5px。所以上面的例子，设置的行高应该就是div盒子的高度，那么文字正好是居中的，距离上下各15px。

所以文本为什么可以居中，就是因为行高设定之后，行距会等分。

这个时候，其实`height`是可以不写的，行高已经将盒子撑起来了。当然，没有文本时，行高也没有意义。

#### 字体 font 多写属性

可以同时设置多个属性

```css
font: font-style font-variant font-weight font-size/line-height font-family
```

比如

```css
.box {
  font-size: 18px;
  font-family: "宋体";
  font-weight: bold;
  font-style: oblique;
  font-variant: small-caps;
  line-height: 50px;
}
/* 可写成 */
.box {
  font: oblique small-caps bold 30px/50px "宋体";
}
```

其中`font-style`、`font-variant`、`font-weight`可以随意调换顺序，也可以省略，不设置。

`/line-height`可以省略，如果不省略，必须跟在`font-size`后面。

### CSS属性-列表

列表常见的CSS属性有四个，列表属性用的很少。

* list-style-type
* list-style-image
* list-style-position
* list-style

它们都可以继承，所以设置给`ol`、`ul`元素，默认也会应用到`li`元素。

#### 列表 list-style-type

设置li元素前面标记的样式

* **none** 去除标记样式

* dics 实心圆
* circle 空心圆
* square 实心方块
* decimal 阿拉伯数字
* lower-roman 小写罗马数字
* upper-roman 大写罗马数字

#### 列表 list-style-image

设置某张图片为`li`元素前面的标记，会覆盖`list-style-type`的设置。

#### 列表 list-style-position

设置li元素前面标记的位置：

* outside
* inside

#### 列表 list-style

是`list-tyle-type`、`list-tyle-image`、`list-tyle-position`的缩写属性。

```css
ul {
 list-style: outside url("image/dot.png"); 
}
```

一般最常用对还是直接设置为`none`，去掉前面的默认标记`list-style: none`。

### CSS属性-表格



## CSS特性

### 继承

CSS中有些属性是可以继承的。一个元素如果没有设置某属性，就会跟随父元素的值。当然，一个元素如果有设置某属性的值，就使用自己设置的值。

不能继承的属性，一般可以使用inherit值强制继承。

浏览器的开发者工具也会标识出哪些样式是继承过来的`Inherited from ***`。

要注意的是，CSS继承过来的是计算值，

```html
<style>
  .box1 {
    font-size: 60px;
  }

  .box2 {
     /* 30px */
    font-size: 0.5em;
  }
</style>

<div class="box1">
  <div class="box2">
      <!-- p中文字30px -->
    <p>文字内容</p>
  </div>
</div>
```

### 层叠和权重

CSS允许相同名字的CSS属性层叠在同一个元素上。

层叠后的结果是只有一个CSS属性会生效。

哪个CSS属性会生效，取决于CSS属性所处环境的优先级高低。

浏览器的开发者工具非常清晰地显示了哪个CSS属性会生效。

 **基本层叠**，使用了相同的选择器，后面一定会把前面的层叠掉。

下面文字内容是橘色。

```html
<style>
  .box1 {
    color: red;
  }

  .box2 {
    background-color: green;
    color: purple;
  }

  .box3 {
    width: 300px;
    color: orange;
  }
</style>
<body>
  <div class="box1 box2 box3">文字内容1</div>
</body>
```

当选择器不同时，需要按照选择器的权重来层叠，谁的权重越大，谁就优先显示。

下面文字内容是蓝色。

```html
<style>
 #mian {
   color: blue;
 }

 .box {
   color: green;
 }

 div {
   color: red;
 }
  
</style>
<body>
  <div class="box" id="main">文字内容</div>
</body>
```

#### CSS属性的优先级

按照经验，为了方便比较CSS属性的优先级，可以给CSS属性所处的环境定义一个权重。

* `!important`：
* 内联样式：1000
* id选择器：100
* 类选择器、属性选择器、伪类：10
* 元素选择器、伪元素：1
* 通配符：0

比较优先级的严谨方法：

* 从权值最大的开始比较每一种权值的数量多少，数量多的则优先级高，即可结束比较
* 如果数量相同，比较下一个较小的权值，以此类推。

* 如果所有权值比较完毕后，发现数量相同，就采取就近原则。

```css
  /* 如果这2个color是作用在同一个标签上，哪个优先级高？ */

  /* 2个id选择器、2个类选择器 */
  .five#radio .one #three {
    color: blue;
  }

  /* 2个id选择器、1个类选择器、2个元素选择器 */
  #box #btn .four div span {
    color: black;
  }
```



要理解的是，权重只是为了方便记忆，并不是真的按照值去比较权重。

**为什么"我是个a"是蓝色。**

```html
<style>
  div {
    color: red !important;
  }
</style>

<body>
  <div class="box1 box2 box3">
    我是div的内容 <br>
    <span>我是一个span</span> <br>
    <a href="">我是个a</a>
  </div>
</body>
```

因为a标签本身给自己设置了颜色，所以会使用自己的颜色，而不是继承div中的红色，即使设置了`!important`

**为什么div中的文本没有变成红色**

```html
<style>
  p div {
    color: red !important;
  }
</style>

<body>
  <p>
    <div>我是一个div</div>
  </p>
</body>
```

浏览器本身不支持p中嵌套div，可以在浏览器中查看，html结构是乱掉的，和声明的不一样。

### CSS属性的使用经验

为什么有时候编写的CSS属性不生效，有可能是因为：

* 选择器的优先级太低
* 选择器没选中对应的元素
* CSS属性的使用形式不对
  * 元素不支持此CSS属性，比如span默认是不支持width和heigt的
  * 浏览器不支持此CSS属性，如旧版本的浏览器不支持CSS3的某些属性
  * 被同类的CSS属性覆盖，比如font覆盖font-size

建议：

充分利用浏览器开发者工具进行调试（增加、修改样式）、查错。

