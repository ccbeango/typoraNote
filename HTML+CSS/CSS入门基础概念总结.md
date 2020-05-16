# CSS入门基础概念总结

## CSS简介

CSS代表级联样式表。CSS是一种标准的样式表语言，用于描述网页的表示(即布局和格式)。

### CSS语法

1. CSS 规则由两个主要的部分构成：选择器，以及一条或多条声明。

```css
selector {declaration1; declaration2; ... declarationN }
```

选择器主要是需要改变样式的 HTML 元素。每条声明是由一个属性和一个值组成，属性和值使用`:`隔开。

例子：

```css
.box{
    /* 注释 */
    width:450px;
    height: 300px;
    background: pink;
    text-align: center;
    line-height: 300px;
    font-size: 20px;
}
```

2. CSS声明总是以分号(;)结束，声明组以大括号({})括起来。CSS注释以 `/*` 开始, 以 `*/` 结束。

3. CSS创建的方式有三种：
   * 外部样式表(External style sheet)
   * 内部样式表(Internal style sheet)
   * 内联样式(Inline style)

4. 多重样式的优先级：内联样式 >内部样式 >外部样式> 浏览器默认样式；如果外部样式放在内部样式的后面，则外部样式将覆盖内部样式。

## CSS定位

```html
<div class="box1">One</div>
<div class="box2">Two</div>
```

### Position

`position`属性指定了元素的定位类型，元素可以使用的`top`、`bottom` 、`left` 、`right`属性来定位，然而，这些属性单独是无法工作，必须先设定`position`属性。

`position`具有5个值：

* `static`：默认值。没有定位，元素出现在正常的流中，忽略 top, bottom, left, right 或者 z-index 声明。

  ```css
  div{
  	width:100px;
  	height: 100px;
  	background-color: pink;
  	position: static;
  }
  ```

* `fixed`：固定定位，生成固定定位的元素，相对于浏览器窗口进行定位；元素的位置相对于浏览器窗口是固定位置；常用于创建在滚动屏幕时仍固定在相同位置的元素。

  * 定位使元素的位置与文档流无关，因此不占据空间
  * 定位的元素会和其他元素重叠

  ```css
  div{
  	width:100px;
  	height: 100px;
  	background-color: pink;
  	position: fixed;
  	top:40px;
  }
  ```

* `relative`：相对定位，生成相对定位的元素，相对于其正常位置进行定位。相对于原来位置移动，元素设置此属性之后**仍然处在文档流中**，不影响其他元素的布局。

  ```css
  .box1, .box2 {
      width: 100px;
      height: 100px;
      background-color: pink;
  }
  
  .box2 {
      position: relative;
      top: -10px;
      left: 30px;
  }
  ```

* `absolute`：生成绝对定位的元素，相对于 `static` 定位以外的第一个父元素进行定位。绝对定位的元素的位置相对于最近的已定位父元素，如果元素没有已定位的父元素，那么它的位置相对于`<html>`。在布置文档流中其它元素时，绝对定位元素不占据空间，**绝对定位脱离了文档流**。

  * 因为默认定位方式是`static`，所以一般我们在使用绝对定位时会给祖先元素(经常是父级元素)加上`position:relative`，让将要定位的元素相对于已经定位的父元素进行绝对定位。

  ```css
  .box1, .box2 {
      width: 100px;
      height: 100px;
      background-color: pink;
      border: 2px solid #444;
  
  }
  /* 因为没有父级已定位的元素，所以会相对于html进行绝对定位。*/
  .box2 {
      position: absolute;
      top: 40px;
      left: 30px;
  }
  ```

* `sticky`： 粘性定位，该定位基于用户滚动的位置。

  * 粘性定位的元素是依赖于用户的滚动，在 position:relative 与 position:fixed 定位之间切换。

  * 它的行为就像相对定位，而当页面滚动超出目标区域时，它的表现就像fixed定位，它会固定在目标位置。

  * 元素定位表现为在跨越特定阈值前为相对定位，之后为固定定位。

  * 这个特定阈值指的是 top, right, bottom 或 left 之一，换言之，指定 top, right, bottom 或 left 四个阈值其中之一，才可使粘性定位生效。否则其行为与相对定位相同。

```css
body {
    height: 2000px;
}

.box1,
.box2 {
    width: 100px;
    height: 100px;
    background-color: pink;
    border: 2px solid #444;

}

.box2 {
    position: sticky;
    top: 30px;
    background-color: pink;
    border: 2px solid #444;
}
```

### z-index

**`z-index`属性指定一个元素的堆叠顺序。拥有更高堆叠顺序的元素总是会处于堆叠顺序较低的元素的前面。**

该属性设置一个定位元素沿 z 轴的位置，z 轴定义为垂直延伸到显示区的轴。如果为正数，则离用户更近，为负数则表示离用户更远。

注意：

1. 元素可拥有负的`z-index`属性值。
2. `z-index`仅能使用在进行定位元素`position:absolute, position:relative, 或 position:fixed`中。

```css
.box1, .box2 {
    width: 100px;
    height: 100px;
    background-color: pink;
    border: 2px solid #444;

}

.box2 {
    position: relative;
    top: -10px;
    left: 20px;
    background-color: greenyellow;
    border: 2px solid #444;
    z-index: -1;
}
```

### top right bottom left

1. `top`：规定元素的顶部边缘。该属性定义了一个定位元素的上外边距边界与其包含块上边界之间的偏移。
2. `right`：规定元素的右边缘。该属性定义了定位元素右外边距边界与其包含块右边界之间的偏移。
3. `bottom`：对于绝对定位元素，bottom属性设置单位高于/低于包含它的元素的底边；对于相对定位元素，bottom属性设置单位高于/低于其正常位置的元素的底边。
4. `left`：规定元素的左边缘。该属性定义了定位元素左外边距边界与其包含块左边界之间的偏移。

注意：

* 对于相对定义元素，如果`top`和`bottom`都是`auto`，其计算值则都是0；如果其中之一为`auto`，则取另一个值的相反数；如果二者都不是`auto`，`bottom`将取`top`值的相反数。

### clip

如果图像大于包含它的元素，会发生什么？-`clip`属性，让你指定一个绝对定位的元素，该尺寸应该是可见的，该元素被剪裁成这种形状并显示。

注意：: 如果先有`overflow：visible`，`clip`属性不起作用。

## CSS布局

### display

**定义了元素是否显示，以及生成哪种盒用于显示。**具有多个属性值，常用的有：

| 值           | 描述                                                 |
| ------------ | ---------------------------------------------------- |
| none         | 此元素不会被显示                                     |
| block        | 此元素将显示为块级元素，前后会带有换行符。           |
| inline       | 默认。此元素会被显示为内联元素，元素前后没有换行符。 |
| inline-block | 行内块元素                                           |
| list-item    | 此元素会作为列表显示                                 |
| inherit      | 规定应该从父元素继承 display 属性的值                |

**块级元素的特性**：

* 总是独占一行。表现为另起一行开始，而且其后的元素也必须另起一行显示。

* 宽度(width)、高度(height)、内边距(padding)和外边距(margin)都可控制;

* 默认为父级宽度的100%，支持全部样式。

* 常用原生标签：

  ```html
  body
  
  h1 , h2, h3, h4, h5, h6
  
  p
  
  div
  
  li (条目)
  
  ul(定义无序列表, 子标签li, 带点号)
  
  ol(定义有序列表, 子标签li, 带数字)
  
  dl (定义列表, 内部子标签为dt, dd, 带缩进)
  
  dt (标题)
  
  dd (内容)
  
  blockquote 
  
  center 
  
  pre 
  
  address , blockquote , center , dir , div , dl , fieldset , form , h1 , h2 , h3 , h4 , h5 , h6 , hr , isindex , menu , noframes , noscript , ol , p , pre , table , ul , li
  ```

**内联元素的特性**：

* 和相邻的内联元素在同一行。

* 宽度`width`、高度`height`、内边距的`padding-top/padding-bottom`和外边距的`margin-top/margin-bottom`都不可改变。即不支持宽高, 不支持`margin`上下, 也不支持`padding`上下。

* 原生标签：

  ```html
  a
  
  span
  
  em(语气强调,斜体)
  
  i(专业词汇, 斜体)
  
  b(关键词, 加粗)
  
  strong(非常重要, 加粗)
  
  input(输入框, 支持全部样式)
  
  img(图片, 支持全部样式)
  
  abbr , acronym , b , bdo , big , br , cite , code , dfn , em , font , i , img , input , kbd , label , q , s , samp , select , small , span , strike , strong , sub , sup ,textarea , tt , u , var
  ```

**内联块元素**：

* 没有原生的内联块元素，任何元素都可以转换为内联块元素
* 支持全部的样式

### float

**指定一个盒子（元素）是否应该浮动**。属性值有：

| 值      | 描述                        |
| ------- | --------------------------- |
| left    | 元素向左浮动                |
| right   | 元素向右浮动                |
| none    | 默认值。元素不浮动          |
| inherit | 从父元素继承 float 属性的值 |

注意：绝对定位的元素会忽略浮动。

### clear

**clear属性可用于去除浮动效果，它指定段落的左侧或右侧的元素不允许浮动。**

| 值      | 描述                                |
| ------- | ----------------------------------- |
| left    | 在左侧不允许浮动元素。              |
| right   | 在右侧不允许浮动元素。              |
| both    | 在左右两侧均不允许浮动元素。        |
| none    | 默认值。允许浮动元素出现在两侧。    |
| inherit | 规定应该从父元素继承clear属性的值。 |

### visibility

**visibility属性指定一个元素是否是可见的。**

| 值       | 描述                                                         |
| -------- | ------------------------------------------------------------ |
| visible  | 默认值。元素是可见的。                                       |
| hidden   | 元素是不可见的。                                             |
| collapse | 当在表格元素中使用时，此值可删除一行或一列，但是它不会影响表格的布局。被行或列占据的空间会留给其他内容使用。如果此值被用在其他的元素上，会呈现为 "hidden"。 |
| inherit  | 规定应该从父元素继承 visibility 属性的值。                   |

注意：即使不可见的元素也会占据页面上的空间。如果要创建不占据页面空间的不可见元素，使用`display`。

### overflow

**overflow属性规定当内容溢出元素框时发生的事情。**

| 值      | 描述                                                     |
| ------- | -------------------------------------------------------- |
| visible | 默认值。内容不会被修剪，会呈现在元素框之外。             |
| hidden  | 内容会被修剪，并且其余内容是不可见的。                   |
| scroll  | 无论是否需要滚动条，浏览器都会显示滚动条功能             |
| auto    | 如果内容被修剪，则浏览器会显示滚动条以便查看其余的内容。 |
| inherit | 规定应该从父元素继承overflow属性的值。                   |

## CSS尺寸

CSS 尺寸 (Dimension) 属性允许你控制元素的高度和宽度。同样，它允许你增加行间距。

### width、height、min-width、max-width、min-height、max-height

1. `width`：设置元素的宽度。

2. `height`：设置元素的高度。
3. `min-width`：设置元素的最小宽度。
4. `max-width`：设置元素的最大宽度。
5. `min-height`：设置元素的最小高度。
6. `max-height`：设置元素的最大高度。

注意：属性的长度均不包括填充`padding`，边框`border`和页边距`margin`。

### margin

外边距指的是围绕在元素边框的空白区域，可以通过margin属性来设置外边距，它接受任何长度单位、百分数值甚至负值。

margin在一个声明中设置所有外边距属性。该属性可以有 1 到 4 个值。

块级元素的垂直相邻外边距会合并，而行内元素实际上不占上下外边距。行内元素的左右外边距不会合并。同样地，浮动元素的外边距也不会合并。**允许指定负的外边距值**，不过使用时要小心。

| 属性          | 描述                                     |
| ------------- | ---------------------------------------- |
| margin        | 简写属性，在一个声明中设置所有外边距属性 |
| margin-bottom | 设置元素的下外边距                       |
| margin-left   | 设置元素的左外边距                       |
| margin-right  | 设置元素的右外边距                       |
| margin-top    | 设置元素的上外边距                       |

```css
margin:10px 5px 15px 20px;
	上边距是 10px

	右边距是 5px

	下边距是 15px

	左边距是 20px

margin:10px 5px 15px;

	上边距是 10px

	右边距和左边距是 5px

	下边距是 15px）

margin:10px 5px;

	上边距和下边距是 10px

	右边距和左边距是 5px

margin:10px;

	所有四个边距都是 10px
```

### padding

元素的内边距指的是在边框和内容区之间，通过padding属性定义元素边框与元素内容之间的空白区域。

padding属性定义元素的内边距。该属性可以有 1 到 4 个值。padding属性接受长度值或百分比值，但**不允许使用负值**。

| 属性           | 描述                                 |
| -------------- | ------------------------------------ |
| padding        | 简写属性，在一个声明中的所有填充属性 |
| padding-bottom | 设置元素的底部填充                   |
| padding-left   | 设置元素的左部填充                   |
| padding-right  | 设置元素的右部填充                   |
| padding-top    | 设置元素的顶部填充                   |

```css
padding:10px 5px 15px 20px;

	上填充是 10px

	右填充是 5px

	下填充是 15px

	左填充是 20px

padding:10px 5px 15px;

	上填充是 10px

	右填充和左填充是 5px

	下填充是 15px

padding:10px 5px;

	上填充和下填充是 10px

	右填充和左填充是 5px

padding:10px;

	所有四个填充都是 10px
```

## CSS背景

CSS 背景属性用于定义HTML元素的背景

### background

`background`属性是一个缩写属性，可以在一个声明中设置所有的背景属性。

CSS语法：

```css
background:bg-color bg-image position/bg-size bg-repeat bg-origin bg-clip bg-attachment initial|inherit;
```

可以设置的属性分别是：

* `background-color`： 指定要使用的背景颜色
* `background-position`：指定背景图像的位置 
* `background-size`：指定背景图片的大小
* `background-repeat`：指定如何重复背景图像
* `background-origin`：指定背景图像的定位区域
* `background-clip`：指定背景图像的绘画区域
* `background-attachment`：设置背景图像是否固定或者随着页面的其余部分滚动
* `background-image`：指定要使用的一个或多个背景图像

### background-color

background-color属性用于设置一个元素的背景颜色。元素的背景是元素的总大小，包括填充和边界，但不包括外边距。

| 属性值      | 描述                       |
| ----------- | -------------------------- |
| color       | 指定背景颜色。             |
| transparent | 默认值，指定背景颜色透明的 |
| inherit     | 指定背景颜色从父元素继承   |

### background-image

background-image属性用于设置一个元素的背景图像。

元素的背景是元素的总大小，包括填充和边界（但不包括边距）。

默认情况下，background-image放置在元素的左上角，并重复垂直和水平方向。

| 属性值  | 描述                     |
| ------- | ------------------------ |
| url     | 指定背景颜色。           |
| none    | 默认值，无图像背景会显示 |
| inherit | 指定背景图像从父元素继承 |

### background-repeat

background-image 属性设置如何平铺对象。

默认情况下，重复background-image的垂直和水平方向。

| 属性值    | 描述                                     |
| --------- | ---------------------------------------- |
| repeat    | 默认值，背景图像将向垂直和水平方向重复。 |
| repeat-x  | 只有水平位置会重复背景图像               |
| repeat-y  | 只有垂直位置会重复背景图像               |
| no-repeat | 无重复                                   |
| inherit   | 指定属性从父元素继承                     |

### background-attachment

background-attachment属性用于设置背景图像是否固定或者随着页面的其余部分滚动。

| 属性值  | 描述                                 |
| ------- | ------------------------------------ |
| scroll  | 默认值，背景图片随页面的其余部分滚动 |
| fixed   | 背景图像是固定的                     |
| inherit | 指定属性从父元素继承                 |

### background-position

background-position属性设置背景图像的起始位置。

这个属性设置背景原图像`background-image`的位置，背景图像如果要重复，将从这一点开始。

| 属性值                                                       | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| left   top<br/>left   center<br/>left   bottom<br/>right  top<br/>right  center<br/>right  bottom<br/>center top<br/>center center<br/>center bottom | 如果仅指定一个关键字，其他值将会是"center"                   |
| x% y%                                                        | 第一个值是水平位置，第二个值是垂直。左上角是0％0％。右下角是100％100％。如果仅指定了一个值，其他值将是50％。 默认值为：0％0％ |
| xpos ypos                                                    | 第一个值是水平位置，第二个值是垂直。左上角是0。单位可以是像素（0px 0px）或任何其他 CSS单位。如果仅指定了一个值，其他值将是50％。可以混合使用％和positions |
| inherit                                                      | 指定属性设置从父元素继承                                     |

### background-origin

background-origin属性规定`background-position`属性相对于什么位置来定位。

**注释：**如果背景图像的`background-attachment`属性为`fixed`，则该属性没有效果。

| 属性值      | 描述                         |
| ----------- | ---------------------------- |
| padding-box | 背景图像相对于内边距框来定位 |
| border-box  | 背景图像相对于边框盒来定位   |
| content-box | 背景图像相对于内容框来定位   |

### background-clip

background-clip 属性规定背景的绘制区域。

| 属性值      | 描述                 |
| ----------- | -------------------- |
| padding-box | 背景被裁剪到内边距框 |
| border-box  | 背景被裁剪到边框盒   |
| content-box | 背景被裁剪到内容框   |

### background-size

background-size 属性规定背景图像的尺寸，即指定背景图片的大小。

| 属性值     | 描述                                                         |
| ---------- | ------------------------------------------------------------ |
| length     | 设置背景图像的高度和宽度。第一个值设置宽度，第二个值设置高度。如果只设置一个值，则第二个值会被设置为 "auto"。 |
| percentage | 以父元素的百分比来设置背景图像的宽度和高度。第一个值设置宽度，第二个值设置高度。如果只设置一个值，则第二个值会被设置为 "auto"。 |
| cover      | 把背景图像扩展至足够大，以使背景图像**完全覆盖**背景区域。**背景图像的某些部分也许无法显示在背景定位区域中**。 |
| contain    | 把图像图像扩展至最大尺寸，以使其宽度和高度完全适应内容区域。(整个图片都包含在区域内) |

## CSS边框

元素的边框 (border) 是围绕元素内容和内边距的一条线或者多条线。

###  border

border属性是一个缩写属性，可以在一个声明中设置所有的边框属性。

可以设置的属性分别为：

* `border-width`：指定边框的宽度 

* `border-style`：指定边框的样式

* `border-color`：指定边框的颜色

* `inherit`

  如果不设置其中的某个值，也不会出问题，比如 border:solid #ff0000

```css
border:border-width|border-style|border-color|inherit
```

### border-width

border-width属性设置一个元素的四个边框的宽度。此属性可以有一到四个值，指定上、右、下、左边框的宽度。

| 属性值  | 描述                   |
| ------- | ---------------------- |
| thin    | 定义细的边框           |
| medium  | 默认值，定义中等的边框 |
| thick   | 定义粗的边框           |
| length  | 允许自定义边框的宽度   |
| inherit | 从父元素继承边框的宽度 |

```css
border-width:thin medium thick 10px;

border-width:thin medium thick;

border-width:thin medium;

border-width:thin;

```

### border-style

border-style属性设置一个元素的四个边框的样式。此属性可以有一到四个值。

| 属性值                    | 描述                                                         |
| ------------------------- | ------------------------------------------------------------ |
| none                      | 定义无边框。                                                 |
| hidden                    | 与 "none" 相同。不过应用于表时除外，对于表，hidden 用于解决边框冲突。 |
| dotted                    | 定义点状边框。在大多数浏览器中呈现为实线。                   |
| dashed                    | 定义虚线。在大多数浏览器中呈现为实线。                       |
| solid                     | 定义实线。                                                   |
| double                    | 定义双线。双线的宽度等于 border-width 的值。                 |
| groove/ridge/inset/outset |                                                              |
| inherit                   | 规定应该从父元素继承边框样式。                               |

### border-color

border-color属性设置一个元素的四个边框颜色，此属性可以有一到四个值

注意：请始终把 border-style 属性声明到 border-color 属性之前。元素必须在改变其颜色之前获得边框。

| 属性值      | 描述                               |
| ----------- | ---------------------------------- |
| color       | 指定边框背景颜色                   |
| transparent | 默认值，指定边框的颜色应该是透明的 |
| inherit     | 从父元素继承边框的颜色             |

### box-shadow

box-shadow 向框添加一个或多个阴影。

```css
box-shadow: h-shadow v-shadow blur spread color inset;

box-shadow: 10px 10px 5px #888888;
```

**注意：**boxShadow 属性把一个或多个下拉阴影添加到框上。该属性是一个用逗号分隔阴影的列表，每个阴影由 2-4 个长度值、一个可选的颜色值和一个可选的 `inset` 关键字来规定。省略长度的值是 0。

| 属性值   | 描述                                     |
| -------- | ---------------------------------------- |
| h-shadow | 必需的。水平阴影的位置。允许负值         |
| v-shadow | 必需的。垂直阴影的位置。允许负值         |
| blur     | 可选。模糊距离                           |
| spread   | 可选。阴影的大小                         |
| color    | 可选。阴影的颜色                         |
| inset    | 可选。将外部阴影 (outset) 改为内部阴影。 |

### border-radius

border-radius 属性是一个简写属性，用于设置四个`border-*-radius`属性。

每个半径的四个值的顺序是：左上角，右上角，右下角，左下角。

按此顺序设置每个 radii 的四个值。如果省略坐左下`bottom-left`，则与右上`top-right`相同。如果省略右下`bottom-right`，则与左上`top-left`相同。如果省略右上`top-right`，则与左上`top-left`相同。

```css
border-radius:length|%;
```

```css
p{
    width:100px;
    height: 100px;
    line-height: 100px;
    text-align: center;
    border:1px solid #ED28BB;
    border-radius:10px 54px 20px; 
}
```

### border-image

border-image是简写属性，可以用来同时设置以下属性：

- `border-image-source`：用在边框的图片的路径。
- `border-image-slice`：图片边框向内偏移。
- `border-image-width`：图片边框的宽度。
- `border-image-outset`：边框图像区域超出边框的量
- `border-image-repeat`：图像边框是否应平铺(repeated)、铺满(rounded)或拉伸(stretched)。

如果省略值，会设置其默认值，默认值为`none 100% 1 0 stretch`

### border-image-source

### border-image-slice

border-image-slice属性指定图像的边界向内偏移。

此属性指定顶部，右，底部，左边缘的图像向内偏移，分为九个区域：四个角，四边和中间。图像中间部分将被丢弃（完全透明的处理），除非填写关键字。如果省略第四个数字/百分比，它和第二个相同的。如果也省略了第三个，它和第一个是相同的。如果也省略了第二个，它和第一个是相同的

| 属性值 | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| number | 数字表示图像的像素（位图图像）或向量的坐标（如果图像是矢量图像） |
| %      | 百分比图像的大小是相对的：水平偏移图像的宽度，垂直偏移图像的高度 |
| fill   | 保留图像的中间部分                                           |

### border-image-width

border-image-width属性指定图像边界的宽度

注意： border-image -width的4个值指定用于把border图像区域分为九个部分。他们代表上，右，底部，左，两侧向内距离。如果第四个值被省略，它和第二个是相同的。如果也省略了第三个，它和第一个是相同的。如果也省略了第二个，它和第一个是相同的。负值是不允许的。

| 属性值 | 描述                                                         |
| ------ | ------------------------------------------------------------ |
| number | 表示相应的border-width 的倍数                                |
| %      | 边界图像区域的大小：横向偏移的宽度的面积，垂直偏移的高度的面积 |
| auto   | 如果指定了，宽度是相应的image slice的内在宽度或高度          |

### border-image-outside

border-image-outset用于指定在边框外部绘制 border-image-area 的量。

注意： border-image-outset用于指定在边框外部绘制 border-image-area 的量，包括上下部和左右部分。如果第四个值被省略，它和第二个是相同的。如果也省略了第三个，它和第一个是相同的。如果也省略了第二个，它和第一个是相同的

不允许border-image-outset拥有负值

| 属性值 | 描述                                              |
| ------ | ------------------------------------------------- |
| number | 代表相应的 border-width 的倍数                    |
| length | 设置边框图像与边框（border-image）的距离，默认为0 |

### border-image-repeat

border-image-repeat 属性用于图像边界是否应重复（repeated）、拉伸（stretched）或铺满（rounded）

该属性规定如何延展和铺排边框图像的边缘和中间部分。因此，可以规定两个值。如果省略第二个值，则采取与第一个值相同的值。

| 属性值  | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| stretch | 拉伸图像来填充区域，默认值                                   |
| repeat  | 平铺（repeated）图像来填充区域                               |
| round   | 类似 repeat 值，如果无法完整平铺所有图像，则对图像进行缩放以适应区域 |
| space   | 类似 repeat 值，如果无法完整平铺所有图像，扩展空间会分布在图像周围 |
| initial |                                                              |
| inherit |                                                              |

## CSS颜色

### color

color 属性规定文本的颜色。

这个属性设置了一个元素的前景色（在 HTML 表现中，就是元素文本的颜色）；光栅图像不受 color 影响。这个颜色还会应用到元素的所有边框，除非被 border-color 或另外某个边框颜色属性覆盖。

| 属性值     | 描述                                        |
| ---------- | ------------------------------------------- |
| color_name | 规定颜色值为颜色名称的颜色（`red`）         |
| hex_number | 规定颜色值为十六进制值的颜色（`#ff0000`）   |
| rgb_number | 规定颜色值为 rgb 代码的颜色(`rgb(255,0,0)`) |
| inherit    | 规定应该从父元素继承颜色。                  |

### opacity

opacity属性设置一个元素了透明度级别。

| 属性值  | 描述                                               |
| ------- | -------------------------------------------------- |
| value   | 指定不透明度。从0.0（完全透明）到1.0（完全不透明） |
| inherit | 规定应该从父元素继承透明度。                       |

## CSS字体

CSS 字体属性定义文本的字体系列、大小、加粗、风格（如斜体）和变形（如小型大写字母）。

### font

font 简写属性在一个声明中设置所有字体属性。

可设置的属性是（按顺序）： `font-style font-variant font-weight font-size/line-height font-family`。

font-size和font-family的值是必需的。如果缺少了其他值，如果有默认值的话，默认值将被插入。

### font-style

font-style属性指定文本的字体样式。

| 属性值  | 描述                         |
| ------- | ---------------------------- |
| normal  | 默认值。标准的字体样式。     |
| italic  | 斜体的字体样式               |
| oblique | 倾斜的字体样式               |
| inherit | 规定应该从父元素继承字体样式 |

italic使用的前提是，字体本身支持斜体的，如果不支持，是没有效果的。

oblique是让字体倾斜，都可以使用来设置字体倾斜。

### font-variant

font-variant 属性设置小型大写字母的字体显示文本，这意味着所有的小写字母均会被转换为大写，但是所有使用小型大写字体的字母与其余文本相比，其字体尺寸更小。

| 属性值     | 描述                         |
| ---------- | ---------------------------- |
| normal     | 默认值。标准的字体样式。     |
| small-caps | 小型大写字母的字体           |
| inherit    | 规定应该从父元素继承字体样式 |

### font-weight

font-weight 属性设置文本的粗细。

| 属性值                                                       | 描述                                                        |
| ------------------------------------------------------------ | ----------------------------------------------------------- |
| normal                                                       | 默认值。定义标准的字符。                                    |
| bold                                                         | 定义粗体字符。                                              |
| bolder                                                       | 定义更粗的字符。                                            |
| lighter                                                      | 定义更细的字符。                                            |
| 100<br/>200<br/>300<br/>400<br/>500<br/>600<br/>700 <br/>800 <br/>900 | 定义由粗到细的字符。400 等同于 normal，而 700 等同于 bold。 |
| inherit                                                      | 规定应该从父元素继承字体的粗细。                            |

### font-size

font-size属性设置字体大小。

| 属性值                                                       | 描述                                                         |
| ------------------------------------------------------------ | ------------------------------------------------------------ |
| xx-small<br/>x-small<br/>small <br/>medium<br/>large x-large<br/>xx-large | 把字体的尺寸设置为不同的尺寸，从 xx-small 到 xx-large。<br/>默认值：medium。 |
| smaller                                                      | 设置为比父元素更小的尺寸。                                   |
| larger                                                       | 设置为比父元素更大的尺寸。                                   |
| length                                                       | 设置为一个固定的值。                                         |
| %                                                            | 设置为基于父元素的一个百分比值。                             |
| inherit                                                      | 规定应该从父元素继承字体尺寸。                               |

### font-family

font-family 规定元素的字体系列。

font-family 可以把多个字体名称作为一个“回退”系统来保存。如果浏览器不支持第一个字体，则会尝试下一个。也就是说，font-family 属性的值是用于某个元素的字体族名称或/及类族名称的一个优先表。浏览器会使用它可识别的第一个值。

有两种类型的字体系列名称：

- `family-name`指定的系列名称：具体字体的名称，比如："times"、"courier"、"arial"。
- `generic-family`通常字体系列名称：比如："serif"、"sans-serif"、"cursive"、"fantasy"、"monospace"

使用逗号分割每个值，并始终提供一个类族名称作为最后的选择。

**注意：**使用某种特定的字体系列（Geneva）完全取决于用户机器上该字体系列是否可用；这个属性没有指示任何字体下载。因此，强烈推荐使用一个通用字体系列名作为后路。

如果字体名称包含空格，它必须加上引号

| 属性值                         | 描述                                                         |
| ------------------------------ | ------------------------------------------------------------ |
| family-name<br/>generic-family | 用于某个元素的字体族名称或/及类族名称的一个优先表。<br/>默认值：取决于浏览器 |
| inherit                        | 规定应该从父元素继承字体系列。                               |

## CSS文本 

CSS 文本属性可定义文本的外观。

通过文本属性，可以改变文本的颜色、字符间距，对齐文本，装饰文本，对文本进行缩进以及文本转换。

### text-transform

text-transform 属性控制文本的大小写。

这个属性会改变元素中的字母大小写，而不论源文档中文本的大小写。

| 属性值     | 描述                                           |
| ---------- | ---------------------------------------------- |
| none       | 默认。定义带有小写字母和大写字母的标准的文本。 |
| capitalize | 文本中的每个单词以大写字母开头。               |
| uppercase  | 定义仅有大写字母。                             |
| lowercase  | 定义仅有小写字母。                             |
| inherit    | 规定应该从父元素继承 text-transform 属性的值。 |

### white-space

white-space属性指定元素内的空白怎样处理。

| 属性值   | 描述                                                        |
| -------- | ----------------------------------------------------------- |
| normal   | 默认。空白会被浏览器忽略。                                  |
| pre      | 空白会被浏览器保留。其行为方式类似 HTML 中的 pre标签。      |
| nowrap   | 文本不会换行，文本会在在同一行上继续，直到遇到 br标签为止。 |
| pre-wrap | 保留空白符序列，但是正常地进行换行。                        |
| pre-line | 合并空白符序列，但是保留换行符。                            |
| inherit  | 规定应该从父元素继承 white-space 属性的值。                 |

### tab-size

tab-size 属性规定制表符（tab）字符的空格长度。

| 属性值  | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| number  | 默认值是 8。规定每个制表符（tab）字符要显示的空格字符的数量。 |
| length  | 规定制表符（tab）字符的长度。几乎所有的主流浏览器都不支持该属性值。 |
| initial |                                                              |
| inherit | 从父元素继承该属性                                           |

### word-break

word-break属性指定非中日韩脚本的单词换行规则。

| 属性值    | 描述                             |
| --------- | -------------------------------- |
| normal    | 默认值。使用浏览器默认的换行规则 |
| break-all | 允许在单词内换行                 |
| keep-all  | 只能在半角空格或连字符处换行     |

### word-wrap

word-wrap 属性允许长单词或 URL 地址换行到下一行。

| 属性值     | 描述                            |
| ---------- | ------------------------------- |
| normal     | 默认值。只在允许的断字点换行    |
| break-word | 在长单词或 URL 地址内部进行换行 |

### text-align

text-align属性指定元素文本的水平对齐方式。

text-align不会作用块级元素，如div

| 属性值  | 描述                                     |
| ------- | ---------------------------------------- |
| left    | 默认值，把文本排列到左边                 |
| right   | 把文本排列到右边                         |
| center  | 把文本排列到中间                         |
| justify | 实现两端对齐文本效果                     |
| inherit | 规定应该从父元素继承 text-align 属性的值 |

justify对最后一行没有效果，所以文本假如说只有一行，是没有任何效果的。

### text-align-last

text-align-last 属性规定如何对齐文本的最后一行。

注意：text-align-last 属性只有在 text-align 属性设置为 `justify`时才起作用。

| 属性值  | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| auto    | 默认值，最后一行被调整，并向左对齐。                         |
| left    | 最后一行向左对齐。                                           |
| right   | 最后一行向右对齐。                                           |
| center  | 最后一行居中对齐。                                           |
| justify | 最后一行被调整为两端对齐                                     |
| start   | 最后一行在行开头对齐（如果 text-direction 是从左到右，则向左对齐；如果 text-direction 是从右到左，则向右对齐） |
| end     | 最后一行在行末尾对齐（如果 text-direction 是从左到右，则向右对齐；如果 text-direction 是从右到左，则向左对齐） |
| initial |                                                              |
| inherit | 从父元素继承该属性                                           |

### text-justify

目前只有IE支持

text-justify 属性规定当 text-align 被设置为 text-align 时的齐行方法。

### word-spacing

word-spacing 属性增加或减少单词间的空白（即字间隔）。

该属性定义元素中字之间插入多少空白符。针对这个属性，“字” 定义为由空白符包围的一个字符串。如果指定为长度值，会调整字之间的通常间隔；所以，normal 就等同于设置为 0。允许指定负长度值，这会让字之间挤得更紧。

| 属性值  | 描述                           |
| ------- | ------------------------------ |
| normal  | 默认值，定义单词间的标准空间。 |
| length  | 定义单词间的固定空间           |
| inherit |                                |

### letter-spacing

letter-spacing 属性增加或减少字符间的空白（字符间距）。

该属性定义了在文本字符框之间插入多少空间。由于字符字形通常比其字符框要窄，指定长度值时，会调整字母之间通常的间隔。因此，normal 就相当于值为 0。

| 属性值  | 描述                                 |
| ------- | ------------------------------------ |
| normal  | 默认值，规定字符间没有额外的空间。   |
| length  | 定义字符间的固定空间（允许使用负值） |
| inherit |                                      |

### text-indent

text-indent 属性规定文本块中首行文本的缩进。

注意： 负值是允许的。如果值是负数，将第一行左缩进。

| 属性值  | 描述                                      |
| ------- | ----------------------------------------- |
| length  | 定义固定的缩进。默认值：0                 |
| %       | 定义基于父元素宽度的百分比的缩进          |
| inherit | 规定应该从父元素继承 text-indent 属性的值 |

### vertical-align

vertical-align 属性设置一个元素的垂直对齐方式。

该属性定义行内元素的基线相对于该元素所在行的基线的垂直对齐。允许指定负长度值和百分比值。这会使元素降低而不是升高。在表单元格中，这个属性会设置单元格框中的单元格内容的对齐方式。

| 属性值      | 描述                                                         |
| ----------- | ------------------------------------------------------------ |
| baseline    | 默认。元素放置在父元素的基线上。                             |
| sub         | 垂直对齐文本的下标。                                         |
| super       | 垂直对齐文本的上标                                           |
| top         | 把元素的顶端与行中最高元素的顶端对齐                         |
| text-top    | 把元素的顶端与父元素字体的顶端对齐                           |
| middle      | 把此元素放置在父元素的中部。                                 |
| bottom      | 把元素的底端与行中最低的元素的顶端对齐。                     |
| text-bottom | 把元素的底端与父元素字体的底端对齐。                         |
| length      |                                                              |
| %           | 使用 "line-height" 属性的百分比值来排列此元素。允许使用负值。 |
| inherit     | 规定应该从父元素继承 vertical-align 属性的值                 |

### line-height

line-height 属性设置行间的距离（行高）。

不允许使用负值。

该属性会影响行框的布局。在应用到一个块级元素时，它定义了该元素中基线之间的最小距离而不是最大距离。

| 属性值  | 描述                                                 |
| ------- | ---------------------------------------------------- |
| normal  | 默认。设置合理的行间距。                             |
| number  | 设置数字，此数字会与当前的字体尺寸相乘来设置行间距。 |
| length  | 设置固定的行间距。                                   |
| %       | 基于当前字体尺寸的百分比行间距。                     |
| inherit | 规定应该从父元素继承 line-height 属性的值。          |

行高是作用在每一个行框盒子(line-box)上的，而行框盒子则是由内联盒子组成，因此，行高与内联元素可以说是非常紧密，行高直接决定了内联元素的高度（注意：这里的内联元素不包括替换元素）；对于块级元素和替换元素，行高是无法决定最终高度的，只能决定行框盒子的最小高度。

## CSS文本装饰

### text-decoration

text-decoration 属性规定添加到文本的修饰。

| 属性值       | 描述                                            |
| ------------ | ----------------------------------------------- |
| none         | 默认。定义标准的文本。                          |
| underline    | 定义文本下的一条线。                            |
| overline     | 定义文本上的一条线。                            |
| line-through | 定义穿过文本下的一条线。                        |
| blink        | 定义闪烁的文本。                                |
| inherit      | 规定应该从父元素继承 text-decoration 属性的值。 |

### text-shadow

text-shadow 属性是用于向文本设置阴影的。

```css
text-shadow: h-shadow v-shadow blur color;
```

注意：text-shadow 属性向文本添加一个或多个阴影。该属性是逗号分隔的阴影列表，每个阴影有两个或三个长度值和一个可选的颜色值进行规定。省略的长度是 0。

| 值       | 描述                             |
| -------- | -------------------------------- |
| h-shadow | 必需。水平阴影的位置。允许负值。 |
| v-shadow | 必需。垂直阴影的位置。允许负值。 |
| blur     | 可选。模糊的距离。               |
| color    | 可选。阴影的颜色。               |

### CSS书写模式

### direction

direction属性指定文本方向/书写方向。

| 属性值  | 描述                                      |
| ------- | ----------------------------------------- |
| ltr     | 默认，文本方向从左到右。                  |
| rtl     | 文本方向从右到左。                        |
| inherit | 规定应该从父元素继承 direction 属性的值。 |

### unicode-bidi ？

unicode-bidi 属性与 direction 属性一起使用，来设置或返回文本是否被重写，以便在同一文档中支持多种语言。

| 属性值        | 描述                                                    |
| ------------- | ------------------------------------------------------- |
| normal        | 默认。不使用附加的嵌入层面。                            |
| embed         | 创建一个附加的嵌入层面。                                |
| bidi-override | 创建一个附加的嵌入层面。重新排序取决于 direction 属性。 |
| initial       | 设置该属性为它的默认值。                                |
| inherit       | 从父元素继承该属性。                                    |

### writing-mode ？

writing-mode 属性定义了文本在水平或垂直方向上如何排布。

- horizontal-tb：水平方向自上而下的书写方式。即 left-right-top-bottom
- vertical-rl：垂直方向自右而左的书写方式。即 top-bottom-right-left
- vertical-lr：垂直方向内内容从上到下，水平方向从左到右
- sideways-rl：内容垂直方向从上到下排列
- sideways-lr：内容垂直方向从下到上排列

## CSS列表样式

### list-style

list-style 简写属性在一个声明中设置所有的列表属性。

list-style 简写属性在一个声明中设置所有的列表属性。

可以设置的属性（按顺序）：`list-style-type`、` list-style-position`、 `list-style-image`。

可以不设置其中的某个值，比如 "list-style:circle inside;" 也是允许的。未设置的属性会使用其默认值。

| 值                  | 描述                                       |
| :------------------ | :----------------------------------------- |
| list-style-type     | 设置列表项标记的类型。                     |
| list-style-position | 设置在何处放置列表项标记。                 |
| list-style-image    | 使用图像来替换列表项的标记。               |
| inherit             | 规定应该从父元素继承 list-style 属性的值。 |

### list-style-type

list-style-type 属性设置列表项标记的类型。

| 属性值               | 描述                                                        |
| -------------------- | ----------------------------------------------------------- |
| none                 | 无标记。                                                    |
| disc                 | 默认。标记是实心圆。                                        |
| circle               | 标记是空心圆。                                              |
| square               | 标记是实心方块。                                            |
| decimal              | 标记是数字。                                                |
| decimal-leading-zero | 0开头的数字标记。(01, 02, 03, 等。)                         |
| lower-roman          | 小写罗马数字(i, ii, iii, iv, v, 等。)                       |
| upper-roman          | 大写罗马数字(I, II, III, IV, V, 等。)                       |
| lower-alpha          | 小写英文字母The marker is lower-alpha (a, b, c, d, e, 等。) |
| upper-alpha          | 大写英文字母The marker is upper-alpha (A, B, C, D, E, 等。) |
| lower-greek          | 小写希腊字母(alpha, beta, gamma, 等。)                      |
| lower-latin          | 小写拉丁字母(a, b, c, d, e, 等。)                           |
| upper-latin          | 大写拉丁字母(A, B, C, D, E, 等。)                           |
| hebrew               | 传统的希伯来编号方式                                        |
| armenian             | 传统的亚美尼亚编号方式                                      |
| georgian             | 传统的乔治亚编号方式(an, ban, gan, 等。)                    |
| cjk-ideographic      | 简单的表意数字                                              |
| hiragana             | 标记是：a, i, u, e, o, ka, ki, 等。（日文片假名）           |
| katakana             | 标记是：A, I, U, E, O, KA, KI, 等。（日文片假名）           |
| hiragana-iroha       | 标记是：i, ro, ha, ni, ho, he, to, 等。（日文片假名）       |
| katakana-iroha       | 标记是：I, RO, HA, NI, HO, HE, TO, 等。（日文片假名）       |

### list-style-position

list-style-position属性指示如何相对于对象的内容绘制列表项标记。

| 值      | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| inside  | 列表项目标记放置在文本以内，且环绕文本根据标记对齐。         |
| outside | 默认值。保持标记位于文本的左侧。<br/>列表项目标记放置在文本以外，且环绕文本不根据标记对齐。 |
| inherit | 规定应该从父元素继承 list-style-position 属性的值。          |

### list-style-image

list-style-image 属性使用图像来替换列表项的标记。

这个属性指定作为一个有序或无序列表项标志的图像。图像相对于列表项内容的放置位置通常使用 list-style-position 属性控制。

**注意：**请始终规定一个 "list-style-type" 属性以防图像不可用。

| 值      | 描述                                             |
| :------ | ------------------------------------------------ |
| URL     | 图像的路径。                                     |
| none    | 默认。无图形被显示。                             |
| inherit | 规定应该从父元素继承 list-style-image 属性的值。 |

## CSS表格

### table-layout

table-layout属性为表设置表格布局算法。

| 值        | 描述                                       |
| --------- | ------------------------------------------ |
| automatic | 默认。列宽度由单元格内容设定               |
| fixed     | 列宽由表格宽度和列宽度设定                 |
| inherit   | 规定应该从父元素继承 table-layout 属性的值 |

### border-collapse

border-collapse属性设置表格的边框是否被合并为一个单一的边框，还是像在标准的HTML中那样分开显示。

| 值       | 描述                                          |
| -------- | --------------------------------------------- |
| collapse | 如果可能，边框会合并为一个单一的边框          |
| separate | 默认值。边框会被分开。                        |
| inherit  | 规定应该从父元素继承 border-collapse 属性的值 |

### border-spacing

border-spacing属性设置相邻单元格的边框间距离。(仅用于"边框分离"模式）

| 值            | 描述                                                         |
| ------------- | ------------------------------------------------------------ |
| length length | 规定相邻单元的边框之间的距离。使用 px、cm 等单位。不允许使用负值。<br>如果定义一个 length 参数，那么定义的是水平和垂直间距。<br>如果定义两个 length 参数，那么第一个设置水平间距，而第二个设置垂直间距。 |
| inherit       | 指定应该从父元素继承border - spacing属性的值                 |

### caption-side

caption-side属性设置表格标题的位置。

| 值      | 描述                                       |
| ------- | ------------------------------------------ |
| top     | 默认值。把表格标题定位在表格之上           |
| bottom  | 把表格标题定位在表格之下                   |
| inherit | 规定应该从父元素继承 caption-side 属性的值 |

### empty-cells

empty-cells属性设置是否显示表格中的空单元格。（仅用于"分离边框"模式）

| 值      | 描述                                      |
| ------- | ----------------------------------------- |
| hide    | 不在空单元格周围绘制边框                  |
| show    | 默认值。在空单元格周围绘制边框            |
| inherit | 规定应该从父元素继承 empty-cells 属性的值 |

## CSS内容

### content

content 属性与 `:before` 及 `:after` 伪元素配合使用，来插入生成内容。

| 值              | 描述                                                         |
| --------------- | ------------------------------------------------------------ |
| none            | 设置Content，如果指定成Nothing                               |
| normal          | 设置content，如果指定的话，正常，默认是"none"（该是nothing） |
| counter         | 设定计数器内容                                               |
| attr(attribute) | 设置Content作为选择器的属性之一。                            |
| string          | 设置Content到你指定的文本                                    |
| open-quote      | 设置Content是开口引号                                        |
| close-quote     | 设置Content是闭合引号                                        |
| no-open-quote   | 如果指定，移除内容的开始引号                                 |
| no-close-quote  | 如果指定，移除内容的闭合引号                                 |
| url(url)        | 设置某种媒体（图像，声音，视频等内容）                       |
| inherit         | 指定的content属性的值，应该从父元素继承                      |

### counter-increment

counter-increment属性递增一个或多个计数器值。

counter-increment属性通常用于counter-reset属性和content属性。

| 值        | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| none      | 没有计数器将递增                                             |
| id number | id 定义将增加计数的选择器、id 或 class。number 定义增量。number 可以是正数、零或者负数 |
| inherit   | 指定counter-increment属性的值，应该从父元素继承              |

### counter-reset

counter-reset属性创建或重置一个或多个计数器。

counter-reset属性通常是和counter-increment属性，content属性一起使用。

| 值        | 描述                                                         |
| --------- | ------------------------------------------------------------ |
| none      | 没有计数器将递增                                             |
| id number | id 定义将增加计数的选择器、id 或 class。number 定义增量。number 可以是正数、零或者负数 |
| inherit   | 指定counter-increment属性的值，应该从父元素继承              |

### quotes

quotes属性设置嵌套引用的引号类型。

| 值                          | 描述                                                         |
| --------------------------- | ------------------------------------------------------------ |
| none                        | 规定 "content" 属性的 "open-quote" 和 "close-quote" 的值不会产生任何引号。 |
| string string string string | 定义要使用的引号。前两个值规定第一级引用嵌套，后两个值规定下一级引号嵌套。 |
| inherit                     | 规定应该从父元素继承 quotes 属性的值。                       |

## CSS用户界面

### appearance

appearance 属性可以使元素看上去像标准的用户界面元素

| 值     | 说明                             |
| ------ | -------------------------------- |
| normal | 正常呈现元素                     |
| icon   | 作为一个小图片的呈现元素         |
| window | 作为一个视口呈现元素             |
| button | 作为一个按钮，呈现元素           |
| menu   | 作为一个用户选项设定呈现元素选择 |
| field  | 作为一个输入字段呈现元素         |

### text-overflow

text-overflow属性指定当文本溢出包含它的元素，应该发生什么。

| 值       | 描述                                 |
| -------- | ------------------------------------ |
| clip     | 修剪文本。                           |
| ellipsis | 显示省略符号来代表被修剪的文本。     |
| string   | 使用给定的字符串来代表被修剪的文本。 |

### outline

outline（轮廓）是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用。

outline简写属性在一个声明中设置所有的轮廓属性。

可以设置的属性分别是（按顺序）：outline-color, outline-style, outline-width

如果不设置其中的某个值，也不会出问题，比如`outline:solid #ff0000;`也是允许的。

outline不是元素尺寸的一部分，因此元素的宽度和高度属性不包含轮廓的宽度。

| **值**        | 描述                                             |
| ------------- | ------------------------------------------------ |
| outline-color | 规定边框的颜色。参阅：outline-color 中可能的值。 |
| outline-style | 规定边框的样式。参阅：outline-style 中可能的值。 |
| outline-width | 规定边框的宽度。参阅：outline-width 中可能的值。 |
| inherit       | 规定应该从父元素继承 outline 属性的设置。        |

### outline-width

outline-width指定轮廓的宽度。

**注意：** 请始终在outline-width属性之前声明outline-style属性。元素只有获得轮廓以后才能改变其轮廓的宽度。

| 值      | 描述                               |
| ------- | ---------------------------------- |
| thin    | 规定细轮廓                         |
| medium  | 默认。规定中等的轮廓               |
| thick   | 规定粗的轮廓                       |
| length  | 允许规定轮廓粗细的值               |
| inherit | 规定应该从父元素继承轮廓宽度的设置 |

### outline-color

outline-color属性指定轮廓颜色。

**注意：** 请始终在outline-width属性之前声明outline-style属性。元素只有获得轮廓以后才能改变其轮廓的宽度。

| 值      | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| color   | 指定轮廓颜色。在 CSS颜色值寻找颜色值的完整列表。             |
| invert  | 默认。执行颜色反转（逆向的颜色）。可使轮廓在不同的背景颜色中都是可见。 |
| inherit | 规定应该从父元素继承轮廓颜色的设置。                         |

### outline-style

outline-style属性指定outline的样式。

| 值      | 描述                                                |
| ------- | --------------------------------------------------- |
| none    | 默认。定义无轮廓。                                  |
| dotted  | 定义点状的轮廓。                                    |
| dashed  | 定义虚线轮廓。                                      |
| solid   | 定义实线轮廓。                                      |
| double  | 定义双线轮廓。双线的宽度等同于 outline-width 的值。 |
| groove  | 定义 3D 凹槽轮廓。此效果取决于 outline-color 值。   |
| ridge   | 定义 3D 凸槽轮廓。此效果取决于 outline-color 值。   |
| inset   | 定义 3D 凹边轮廓。此效果取决于 outline-color 值。   |
| outset  | 定义 3D 凸边轮廓。此效果取决于 outline-color 值。   |
| inherit | 规定应该从父元素继承轮廓样式的设置。                |

### outline-offset

outline-offset属性设置轮廓框架在 border 边缘外的偏移。

Outlines在两个方面不同于边框：

- Outlines 不占用空间
- Outlines 可能非矩形

| 值      | 描述                                       |
| ------- | ------------------------------------------ |
| length  | 轮廓与边框边缘的距离                       |
| inherit | 规定应从父元素继承 outline-offset 属性的值 |

### cursor

cursor属性定义了鼠标指针放在一个元素边界范围内时所用的光标形状。

| 值        | 描述                                                         |
| :-------- | :----------------------------------------------------------- |
| url       | 需使用的自定义光标的 URL。注释：请在此列表的末端始终定义一种普通的光标，以防没有由 URL 定义的可用光标 |
| default   | 默认光标                                                     |
| auto      | 默认。浏览器设置的光标                                       |
| crosshair | 光标呈现为十字线                                             |
| pointer   | 光标呈现为指示链接的指针（一只手）                           |
| move      | 此光标指示某对象可被移动                                     |
| e-resize  | 此光标指示矩形框的边缘可被向右（东）移动                     |
| ne-resize | 此光标指示矩形框的边缘可被向上及向右移动（北/东）            |
| nw-resize | 此光标指示矩形框的边缘可被向上及向左移动（北/西）            |
| n-resize  | 此光标指示矩形框的边缘可被向上（北）移动                     |
| se-resize | 此光标指示矩形框的边缘可被向下及向右移动（南/东）            |
| sw-resize | 此光标指示矩形框的边缘可被向下及向左移动（南/西）            |
| s-resize  | 此光标指示矩形框的边缘可被向下移动（北/西）                  |
| w-resize  | 此光标指示矩形框的边缘可被向左移动（西）                     |
| text      | 此光标指示文本                                               |
| wait      | 此光标指示程序正忙（通常是一只表或沙漏）                     |
| help      | 此光标指示可用的帮助（通常是一个问号或一个气球）             |

### zoom

设置或检索对象的缩放比例

| 值         | 描述                               |
| ---------- | ---------------------------------- |
| normal     | 使用对象的实际尺寸。               |
| number     | 用浮点数来定义缩放比例。不允许负值 |
| percentage | 用百分比来定义缩放比例。不允许负值 |

### box-sizing

box-sizing 属性允许你以某种方式定义某些元素，以适应指定区域。

| 值          | 说明                                                         |
| ----------- | ------------------------------------------------------------ |
| content-box | 宽度和高度分别应用到元素的内容框;在宽度和高度之外绘制元素的内边距和边框 |
| border-box  | 为元素指定的任何内边距和边框都将在已设定的宽度和高度内进行绘制。 |
| inherit     | 指定box-sizing属性的值，应该从父元素继承                     |

### resize

resize属性指定一个元素是否是由用户调整大小的。

| 值         | 描述                       |
| ---------- | -------------------------- |
| none       | 用户无法调整元素的尺寸     |
| both       | 用户可调整元素的高度和宽度 |
| horizontal | 用户可调整元素的宽度       |
| vertical   | 用户可调整元素的高度       |

### user-select

user-select属性设置或检索是否允许用户选中文本。

| 值      | 描述                                                         |
| ------- | ------------------------------------------------------------ |
| none    | 文本不能被选择                                               |
| text    | 可以选择文本                                                 |
| all     | 当所有内容作为一个整体时可以被选择。如果双击或者在上下文上点击子元素，那么被选择的部分将是以该子元素向上回溯的最高祖先元素 |
| element | 可以选择文本，但选择范围受元素边界的约束                     |

### pointer-events

## CSS链接

链接的样式的设置可以用任何CSS中的属性比如颜色，字体，背景等。

特别的链接，可以有不同的样式，这取决于他们是什么状态。

这四个链接状态是：

- `a:link`：正常，未访问过的链接
- `a:visited`：用户已访问过的链接
- `a:hover`：当用户鼠标放在链接上时
- `a:active`： 链接被点击的那一刻

## CSS盒子

## CSS浮动

## CSS选择器

### 元素选择符

#### 通配选择符



### 关系选择器

### 属性选择器

### 伪类选择器

### 伪对象选择器

## CSS函数

### attr()

attr() 函数返回选择元素的属性值。

语法：

```css
attr(attribute-name)
```

| 值             | 描述                      |
| -------------- | ------------------------- |
| attribute-name | 必须。HTML 元素的属性名。 |

### calc()

calc() 函数用于动态计算长度值。

* calc()函数使用标准的数学运算优先级规则，支持加减乘除四则运算；
* 任何长度值都可以使用calc()函数进行计算；
* 运算符前后都需要保留一个空格，例如：`width: calc(100% - 10px);`



## CSS语法与规则