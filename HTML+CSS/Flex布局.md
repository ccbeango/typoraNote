# Flex布局

## 认知Flex布局

Flex布局时目前Web开发中使用最多的布局方案：

* flex布局（Flexible布局，弹性布局）
* 目前特别在移动端用的最多，目前PC端也使用越来越多了

Flex中的两个概念：

* 开启了Flex布局的元素叫**flex container**
* flex container里面的直接子元素叫做**flex items**

设置`display`属性为`flex`或者`inline-flex`可以成为flex container

* flex： flex container以块级（block-level）形式存在
* inline-flex：flex container以行级（inline-level）形式存在

开启Flex布局的方法：

```html
<style>
    .box {
        display: flex;
    }
</style>

<div class="box">
    <div class="child_box_1"></div>
    <div class="child_box_2"></div>
    <div class="child_box_3"></div>
</div>
```

任何一个容器都可以指定为 Flex 布局：

块级：

```css

.box{
  display: flex;
}
```

行内元素：

```css

.box{
  display: inline-flex;
}
```

设为 Flex 布局以后，子元素的`float`、`clear`和`vertical-align`属性将失效。

## Flex布局模型

## Flex相关属性

应用在flex container上的CSS属性：

* flex-flow
* flex-direction
* flex-wrap
* justify-content
* align-items
* align-content

应用在flex items上的CSS属性：

* flex
* flex-grow
* flex-basis
* flex-shrink
* order
* align-self

flex items默认是沿着main axis(主轴)从main start 开发往main end方向排布。

## flex container上的相关属性

### flex-direction

flex-direction可以改变主轴的方向：

* row：默认值，主轴从左到右
* row-reverse: 主轴从右到左
* column：主轴从上到下
* column-reverse：主轴从下到上

### justify-content

justify-content决定了flex items在主轴mian axis上的对齐方式：

* flex-start：默认值，与mian start对齐
* flex-end：与main edn对齐
* center：居中对其
* space-between：
  * flex items之间的距离相等
  * 与main start、main end两端对齐
* space-evently：
  * flex items之间的距离相等
  * flex items与mian start、mian end之间的距离等于flex items之间的距离note
* space around：
  * flex items之间的距离相等
  * flex items与main start、main end之间的距离是flex items之间距离的一半，也就是说，每个项目两侧的间隔相等。所以，项目之间的间隔比项目与边框的间隔大一倍。

### align-items

align-times决定了flex items在交叉轴cross axis上的对齐方式

* normal：默认值，在弹性布局中，效果和strech一样
* strech：当flex items在cross axis方向的size为auto或没有指定高度时，会自动拉伸填充fkex container
* flex-start：与cross start对齐
* flex-end：与cross end对其
* center：居中对齐
* baseline：与基准线对齐，基线永远是第一行文本

### flex-wrap

默认情况下，所有的flex items都会在同一行显示，放不下时每个item会收缩。

flex wrap决定了flex container是单行还是多行

* nowrap：默认值，单行
* wrap：多行，第一行在上方
* wrap-reverse：多行，第一行在下方

### flex-flow

`flex-flow`属性是`flex-direction`属性和`flex-wrap`属性的简写形式，默认值为`row nowrap`。

```css
.box {
  flex-flow: <flex-direction> || <flex-wrap>;
}
```

### align-content

align content 决定了多行flex items在cross axis上的对齐方式，用法与justify-content类似

* stretch：默认值，与align-items的stretch类似
* flex-start：与cross start对齐
* flex-end：与cross end对齐
* center：居中对齐
* space-between：
  * flex items之间的距离相等
  * 与cross start、ctoss end两端对齐

* space-around：
  * flex items之间的距离相等
  * flex items与cross start、corss end之间的距离是flex items之间距离的一半
* space evenly：
  * flex items之间的距离相等
  * flex items与cross start、cross end之间的距离等于flex items之间的距离

## flex items上的相关属性

### order

order决定flex items的排布顺序

* 可以设置任意整数（正、负、0），值越小就越排在前面
* 默认值是0

### align-self

flex items可以通过align-self覆盖flex container设置的align-items

* auto：默认值，遵从flex container的align-items设置
* stretch、flex-start、flex-end、center、baseline，效果跟align-items一致

### flex-grow

flex-grow决定了flex items如何扩展，即定义flex items的放大比例

* 可以设置任意非负数字（正小数、正整数、0），默认值是0
* 当flex-container在main axis方向上有剩余size时，flex-grow属性才会有效

如果所有flex items的flex-grow总和sum超过1，每个flex item扩展的size为

* flex container的剩余size * flex-grow / sum

如果所有flex items的flex-grow总和sum不超过1，每个flex item扩展的size为

* flex container的剩余size * flex-grow

flex items扩展后的最终size不能超过max-width\max-height

### flex-shrink

flex-shrink决定了flex items如何收缩

* 可以设置任意非负数字（正小数、正整数、0），默认值是1
* 当flex items在main axis方向上超过了flex container的size，flex-shrink属性才会有效

如果所有flex items的flex-shrink总和超过1，每个flex item收缩的size为

* flex items超出flex container的size * 收缩比例 / 所有flex items的收缩比例之和

如果所有flex items的flex-shrink总和sum不超过1，每个flex item收缩的size为

* flex items超出flex container的size * num * 收缩比例 / 所有flex items的收缩比例之和
* 收缩比例 = flex-shrinl * flex item的base size
* base size就是flex item放入flex container之前的size

flex items收缩后的最终size不能小于min-width\min-height

### flex-basis

flex-basis用来设置flex items在main axis方向上的base size

* auto：默认值、
* number：具体的宽度数值，如100px

决定flex items最终base size的因素，优先级从高到低

* max-width\max-height\min-width\min-height
* flex-basis
* width\height
* 内容本身的size

### flex

flex 是 flex-grow || flex-shrink || flex-basis 的简写，flex 属性可以指定 1个，2个或3个值。

单值语法: 值必须为以下其中之一：

* 一个无单位数（number）:它会被当作flex-grow的值
* 一个有效的宽度(width)值:它会被当作flex-basis的值。
*  关键字none , auto或initial.

 双值语法:

* 第一必须为一个无单位数，并且它会被当作flex-grow的值。
*  第二个值必须为以下之一: 
  * 一个无单位数：它会被当作flex-shrink的值。
  *  一个有效的宽度值：它会被当作flex-basis的值。

三值语法：

* 第一个值必须为一个无单位数，并且它会被当作flex-grow的值。
* 第二个值必须为一个无单位数，并且它会被当作flex-shrink的值。 
* 第三个值必须为一个有效的宽度值，并且它会被当作flex-basis的值