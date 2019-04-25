---

title: 关于JSON

date: 2018-07-02 11:24:10

categories: JavaScript

tag: JavaScript 

---

# 关于JSON

> 摘自：《JavaScript高级程序设计》

## 前言

​       `JSON`是`JavaScript`的一个严格的子集，利用了`JavaScript`中的一些模式来表示结构化数据。

​	关于`JSON`，最重要的是理解它是一种数据格式，不是一种编程语言。虽然具有相同的语法形式，但`JSON`并不从属于`JavaScript`。所以`JSON`在很多编程语言中都有针对`JSON`的解析器和序列化器，它其实只是一种数据格式。

## 语法

`JSON`的可以表示以下三种类型的值。

* **简单值：**使用与JavaScript相同的语法，可在JSON中表示字符串、数值、布尔值和null。JSON不支持JavaScript中的特殊值。
* **对象：**对象表示的是一组无序的键值对，每个键值对中的值可以是简单值，也可以是复杂数据类型的值。
* **数组：**数组表示的是一组有序的键值对，可以通过数值索引来访问其中的值。数组的值也可以是任意类型。

`JSON`不支持变量、函数或对象实例，它只是一种表示结构化数据的格式，虽然与`JavaScript`中表示数据的某些语法相同，但它并不局限于`JavaScript`的范畴。

### 简单值

​	最简单的数据形式就是简单值。

​	 `JavaScript`字符串和`JSON`字符串的最大区别在于，`JSON`字符串必须使用**双引号**（单引号会导致语法错误）。在实际应用中，`JSON`通常用来表示复杂的数据结构，简单值只是整个数据结构中的一部分。

### 对象

​	 `JSON`中对象与`JavaScript`字面量稍有不同。

 `JavaScript `对象字面量：

```javascript
var person = {
    name: "Andy",
    age: 25
};
```

 ` JSON`表示方式：

    ```json
{
    "name": "Andy",
    "age": 25
}
    ```

​	实际上，`JavaScript`中前面的键也可以加上引号，可有可无。但JSON的双引号是不可或缺的。` JSON`与`JavaScript`相比有两个不同：

* 没有变量声明，JSON中没有变量的概念。
* 没有末尾的分号。

### 数组

` JSON`数组采用的是`JavaScript`中的数组字面量形式。

`JavaScript`中的数组字面量：

```javascript
var values = [23, "hello", true];
```

`JSON`中的表示：

```json
[23, "hello", true]
```

同样，`JSON`数组也没有变量和分号。三者相结合，可以构成复杂的数据结构：

```json
[
    {
    "title": "Professional JavaScript",
    "authors": [
        "Nicholas C. Zakas"
    ],
    "edition": 3,
    "year": 2011 
    },
    {
         "title": "Professional JavaScript",
         "authors": [
              "Nicholas C. Zakas"
         ],
    "edition": 2,
    "year": 2009 
    },
    {
         "title": "Professional Ajax",
         "authors": [
             "Nicholas C. Zakas",
             "Jeremy McPeak",
             "Joe Fawcett"
         ],
         "edition": 2,
         "year": 2008
    }
]
```

## 序列化选项

`JSON`流行的重要原因是可以把`JSON`数据结构解析为有用的`JavaScript`对象。

### JSON对象

`JSON`对象有两个方法：

* `stringify()`：

  * 语法：`JSON.stringify(value[, replacer[, space]])`

  * 参数说明：

    * value 必需， 要转换的 JavaScript 值（通常为对象或数组）

    * replacer 过滤器 可选。用于转换结果的函数或数组。

      如果 replacer 为函数，则 JSON.stringify 将调用该函数，并传入每个成员的键和值。使用返回值而不是原始值。如果此函数返回 undefined，则排除成员。根对象的键是一个空字符串：""。

      如果 replacer 是一个数组，则仅转换该数组中具有键值的成员。成员的转换顺序与键在数组中的顺序一样。当 value 参数也为数组时，将忽略 replacer 数组。

    * space: 可选，文本添加缩进、空格和换行符，如果 space 是一个数字，则返回值文本在每个级别缩进指定数目的空格，如果 space 大于 10，则文本缩进 10 个空格。space 有可以使用非数字，如：\t。

* `parse()`

  * 语法： `JSON.parse(value[, reviver])`
  * 参数说明： 
    * value 要被解析成JavaScript值的字符串
    * reviver 还原函数 是一个函数，则规定了原始值如何被解析改造，在被返回之前

在最简单的情况下，这两个方法分别用于把`JavaScript`对象序列化为JSON字符串和把JSON字符串解析为原生JavaScript值。

例如：

```javascript
var book =  {
 	title: "Professional JavaScript",
 	authors: [
   		"Nicholas C. Zakas"
	],
    edition: 3,
    year: 2011
};
var jsonText = JSON.stringify(book);
```

默认情况下，`JOSN.stringify()`输出的`JSON`字符串不包含任何空格字符或缩进，因此保存在`jsonText`中的字符串如下所示：

```json
{"title":"Professional JavaScript","authors":["Nicholas C. Zakas"],"edition":3, "year":2011}
```

在序列化`JavaScript`对象时，所有函数及原型成员都会被有意忽略，不体现在结果中。此外，值未undefined的任何属性也都会被跳过。

 将`JSON` 字符串直接传递给`JSON.parse()`就可以得到相应的`JavaScript`值。

### 序列化选项

#### 过滤结果replacer

​	    如果过滤器参数是数组，那么`JSON.stringify()`的结果中将只包含数组中列出的属性。例如：

```javascript
var book = {
	"title": "Professional JavaScript",
 	"authors": [
    	"Nicholas C. Zakas"
 	],
	edition: 3,
	year: 2011 
};
var jsonText = JSON.stringify(book, ["title", "edition"]);
```

​	第二个参数是一个数组，其中包含`title`和`edition`。这两个属性与将要序列化的对象中的属性是对应的，因此在返回的结果字符串中，就只会包含两个属性：

```json
{"title":"Professional JavaScript","edition":3}
```

​	如果第二个参数是函数，行为会稍有不同。传入的函数接收两个参数，属性名和属性值。根据属性名可以知道如何处理要序列化的对象中的属性。属性名只能是字符串，而在值并非键值对结构的值时，键名可以是空字符串。函数返回的结果就是相应键的值。如果返回的是`undefined`，那么相应的属性会被忽略。

例子：

```javascript
var book = {
    "title": "Professional JavaScript",
    "authors": [
        "Nicholas C. Zakas"
    ],
    edition: 3,
    year: 2011 
};
var jsonText = JSON.stringify(book, function(key, value){ 		switch(key){
        case "authors":
            return value.join(",")
        case "year":
            return 5000;
        case "edition":
             return undefined;
    	default: 
        	return value;
	}
                                                        
});
```

序列化后的字符串如下：

```json
{"title":"Professional JavaScript","authors":"Nicholas C. Zakas","year":5000}
```

要序列化的每一个对象都要经过过滤器，因此数组中的每个带有这些属性的对象经过过滤之后，每个对象都会只包含`title`、`authors`和`year`属性。

### 字符串缩进

`JSON.stringify()`方法的第三个参数用于控制结果中的缩进和空白符。如果这个参数是一个数值，那它表示的是每个级别缩进的空格数。例如，要在每个级别缩进4个空格，可以这样：

```javascript
var book = {
	"title": "Professional JavaScript",
 	"authors": [
    	"Nicholas C. Zakas"
 	],
	edition: 3,
	year: 2011 
};
var jsonText = JSON.stringify(book, null, 4);
```

保存在jsonText中的字符串为

```json
{
    "title": "Professional JavaScript",
    "authors": [
        "Nicholas C. Zakas"
    ],
    "edition": 3,
    "year": 2011
}
```

`JSON.stringify()`也在结果字符串中插入了换行符提高可读性，只要传入有效的控制缩进的参数值，结果字符串就会包含换行符号。（只缩进不换行的意义不大。）最大缩进空格数为`10`，所有大于`10`的值都会被自动转换为`10`。	

如果缩进参数是一个字符串，那么这个字符串将被用作缩进字符。

例如：

```javascript
var jsonText = JSON.stringify(book, null, " --");
```

结果：

```json
{
 --"title": "Professional JavaScript",
 --"authors": [
 -- --"Nicholas C. Zakas"
 --],
 --"edition": 3,
 --"year": 2011
}
```

同样，缩进字符串长度不能超过`10`个字符长，如果超过，将只出现前`10`个字符。	

### toJSON()方法

​		有时，`JSON.stringify()`不能满足对某些对象进行自定义序列化的需求。在这些情况下，可以给对象定义`toJSON()`方法，返回其自身的JSON数据。

​	可以为任何对象添加`toJSON()`方法，比如：

```javascript
var book = {
	"title": "Professional JavaScript",
 	"authors": [
    	"Nicholas C. Zakas"
 	],
	edition: 3,
	year: 2011,
    toJSON: function() {
        return this.title;
    }
};
var jsonText = JSON.stringify(book);
// Professional JavaScript
console.log(jsonText);
```

​	以上代码`book`对象上定义了一个`toJOSN()`方法，方法返回图书的名字。		与`Date`对象类似，这个对象也将被序列化为一个简单的字符串而非对象。可以让`toJSON()`方法返回任何值，它都能正常工作。

​		比如，可以让这个方法返回`undefined`，此时如果包含它的对象嵌入在另一个对象中，会导致它的值变成`null`，而如果它是顶级对象，记过就是`undefined`。

`toJSON()`可以作为函数过滤器的补充，因此理解序列化的内部顺序十分重要。假设把一个对象传入`JSON.stringify()`，序列化的对象顺序如下：

1. 如果存在`toJSON()`方法而且能通过它取得有效的值，则调用该方法。否则返回对象本身。
2. 如果提供了第二个参数，应用这个函数过滤器。传入函数过滤器的值是第1步返回的值。
3. 对第2步返回的每个值进行相应的序列化。
4. 如果提供第三个参数，执行相应的格式化。

无论是考虑定义`toJSON()`还是使用函数过滤器，亦或需要同时使用两者，理解这个顺序至关重要。

## 解析选项

​        `JSON.parse()`方法可以接受另一个参数，该参数是一个函数，将在每个键值对调用。为了区别`JSON.stringify()`接收的替换（过滤）函数（replacer），这个函数被称为还原函数（reviver），但实际上这两个函数的签名是相同的,它们都接收两个参数，一个键和一个值，而且都需要返回一个值。

如果还原函数返回`undefined`，则表示要从结果中删除相应的键；如果返回其他值，则将该值插入到结果中。在将日期字符串转换为`Date`对象时，经常要用到还原函数。例如：

```javascript
var book = {
	"title": "Professional JavaScript",
 	"authors": [
    	"Nicholas C. Zakas"
 	],
	edition: 3,
	year: 2011,
    releaseDate: new Date(2011,11,1)
};
var jsonText = JSON.stringify(book);

var bookCopy = JSON.parse(jsonText, function(key, value){
    if (key == "releaseDate"){
        return new Date(value);
    } else {
        return value;
    }
});

// 2011
console.log(bookCopy.releaseDate.getFullYear());
```

​	以上代码`book`对象新增了一个`releaseDate`属性，该属性保存着一个`Date`对象。这个对象在经过序列化之后又变成了有效的JSON字符串，然后经过解析又在`bookCopy`中还原`Date`对象。还原函数在遇到`releaseDate`键时，会基于相应的值创建一个新的`Date`对象。结果就是`bookCopy.releaseDate`属性中会保存一个`Date`对象。正因为如此，才能基于这个对象调用`getFullYear()`方法。

