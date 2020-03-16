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
*  

### id选择器

通过id选择选择指定元素，每个id值在一个文件中应该只出现一次。

## 最常见的css属性

* color 前景色，不止字体颜色，例如还可以作用border边框，或text-decoration:line-through。
* font-size 字体大小
* background-color 背景颜色
* width/height 宽高

​	**宽度和高度不适用与非替换行内元素**

### 颜色设置的方法：

1. 基本颜色关键字red、black、yellow、blue等，只有上百种基本颜色的关键字。

2. RGB颜色十进制：rgb()

3. RGBA颜色：和十进制之前相同，a指alpha，透明度。如：rgb()

4. RGB颜色十六进制：#FFFFFF

   





