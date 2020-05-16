# BOM对象总结

>  读《JavaScript高级程序设计》

ECMAScript是JavaScript的核心，但如果要在Web中使用JavaScript，那么BOM（浏览器对象模型）则无疑才是真正的核心。

BOM对象提供了很多对象，如window、location、navigator，用于访问浏览器的功能，这些功能与任何网页内容无关。location、navigator实际上都是window对象的属性。

W3C为了把浏览器中JavaScript最基本的部分标准化，已经将BOM的主要方面纳入了HTML5的规范中。

## window对象

BOM的核心对象是window，它表示浏览器的一个实例。在浏览器中，window对象有双重角色，它既是通过JavaScript访问浏览器窗口的一个接口，又是ECMAScript规定的Global对象。

### 全局作用域

由于window对象同时扮演着ECMAScript中Global对象的角色，因此所有在全局作用域中声明的变量、函数都会变成window对象的属性和方法。

```javascript
var age = 29;
function sayAge() {
  alert(this.age);
}

alert(window.age); // 29
sayAge(); // 29
window.sayAge(); // 29
```

在全局作用域中定义了一个变量`age`和一个函数`sayAge()`，它们被自动归在了window对象名下。于是，可以通过`window.age`访问变量`age`，可以通过`window.sayAge()`访问函数`sayAge()`。由于`sayAge()`在全局作用域中，因此`this.age`被映射到`window.age`，最终结果是一致的。

抛开全局变量会成为window对象的属性不谈，定义全局变量与在`window`对象上直接定义属性有一些差别：全局变量不能通过`delete`操作符删除，而直接在`window`对象上定义的属性可以。

```javascript
var age = 29; 
window.color = "red"; 

delete window.age; // return false
delete window.color; //returns true 

alert(window.age); //29 
alert(window.color); //undefined
```

因为通过`var`语句添加的window属性的数据属性`[[Configurable]]`值是`false`，因此不可删除。

要记住的一点是，尝试访问未声明的变量会抛出错误，但是通过查询window对象，可以知道某个可能未声明的变量是否存在。

```javascript
// 报错，因为oldValue未定义
var newValue = oldValue;

// 不报错，因为这是一次属性查询，newValue的值是undefined
var newValue = window.oldValue;
```

### 窗口关系及框架

如果页面中包含框架`frame`，则每个框架都拥有自己的window对象，并且保存再frames集合中，可以通过索引或框架名称来访问相应的window对象。每个window对象都有一个`name`属性，其中包含框架的名称。

现在基本已经不使用frame了。

### 窗口位置

用来确定和修改window对象位置的属性和方法有很多。IE、Safari、Opera、Chrome都提供了`screenLeft`和`screenRight`属性，分别用于表示窗口相对于屏幕左边和上边的位置。FireFox在`screenX`和`screenY`中提供，Safari和Chrome也同时支持这两个属性。

下面代码可实现跨浏览器获取窗口左边和上边的位置

```javascript
var leftPos = (typeof window.screenLeft == "number") ?  window.screenLeft : window.screenX; 
var topPos = (typeof window.screenTop == "number") ? 
 window.screenTop : window.screenY;
```

不同的浏览器对于上述属性的定义不同，跨浏览器无法实现获取准确的坐标值。

有两个函数也可以设置窗口位置，`moveTo()`和`moveBy()`，这两个方法都接受两个参数，其中`moveTo()`接收的是新坐标的x和y坐标值，而`moveBy()`接收的是在水平和垂直方向上移动的像素数。

```javascript
// 将窗口移动到屏幕左上角
window.moveTo(0,0); 
// 将窗口向下移动100像素
window.moveBy(0,100); 
// 将窗口移动到200,300
window.moveTo(200,300); 
// 将窗口向左移动50像素
window.moveBy(-50,0);
```

这两个方法可能被浏览器禁用，且不适用于框架，只能对最外层的window对象使用。

### 窗口大小

IE9+、FireFox、Safari、Opera和Chrome提供了四个属性，`innerWidth`、`innerHeigh`、`outerWidth`和`outerHeight`。不同浏览器中的定义也不同，IE9+、Safari和FireFox中，`outerWidth`和`outerHeight`返回浏览器窗口本身尺寸，`innerWidth`和`innerHeight`表示该容器中页面视图区的大小（减去边框的宽度）。在Chrome中，`innerWidth`、`innerHeigh`与`outerWidth`、`outerHeight`返回相同的值，即视口（viewport）大小而非浏览器窗口大小。
