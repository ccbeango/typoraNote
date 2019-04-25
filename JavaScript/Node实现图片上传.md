---

title: Node实现canvas图片上传

date: 2018-03-01 14:20:10

categories: JavaScript

tag: JavaScript 

---

# Node实现canvas图片上传

> 转自：https://cnodejs.org/topic/4f939c84407edba2143c12f7

## 需求

​	目前流行的“你画我猜”应用，你有没有想过使用HTML5来实现过？那么不可避免的需要解决canvas保存图片到硬盘或mongodb之类的数据库。本文主要介绍使用nodejs将html5 canvas base64编码图片保存为文件，同时提供两种解决方案。

​	html5 canvas属于客户端API，没有权限去保存图片到硬盘，只有canvas . toDataURL()这一个接口可导出画布的base64编码，以提供给服务端进行处理保存，据我所知.net和php都有方法或类来进行简单的处理保存。nodejs呢？是的，没错！nodejs同样有能力来保存base64编码的图片。

## 解决方案一

使用new Buffer来创建对应编码的缓冲，通过fs模块将Buffer写成一个文件。

优点：简单易用，无需其它模块的支持。

缺点：不能对图片的尺寸，水印，压缩，格式等进行处理。

**注意点：**

1. new Buffer接收到base64编码，不能带data:URL，而使用canvas . toDataURL()导出的base64编码会带data:URL，所以需要先过滤掉类似这样的一段“data:image/png;base64,iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0”需过滤成：“iVBORw0KGgoAAAANSUhEUgAAABQAAAAUCAYAAACNiR0”
2. ’binary’ – 一种只使用每个字符前8个字节将原始的二进制数据编码进字符串的方式。这个方式已经废弃，应当尽量使用buffer 对象。这个编码将会在未来的node 中删除。==看到有人把base64声明的Buffer再转换成binary，这个是完全没必要的。==

3、生成的图片有size变化，但是打开后是一个无效的图像，这个看本文的第三点。

**使用express搭建的/upload (POST)上传保存接口，完成代码如下：**

```js
var express = require('express');
var fs = require("fs");
var app = module.exports = express();
//配置
app.configure(function(){
  app.use(express.bodyParser());
  app.use(express.methodOverride());
  app.use(express.cookieParser('keyboard cat'));
  app.use(express.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/up')); //静态文件目录
  app.use(express.errorHandler({ dumpExceptions: true, showStack: true }));
});
//保存base64图片POST方法
app.post('/upload', function(req, res){
	//接收前台POST过来的base64
	var imgData = req.body.imgData;
	//过滤data:URL
	var base64Data = imgData.replace(/^data:image\/\w+;base64,/, "");
	var dataBuffer = new Buffer(base64Data, 'base64');
	fs.writeFile("out.png", dataBuffer, function(err) {
		if(err){
		  res.send(err);
		}else{
		  res.send("保存成功！");
		}
	});
});
if (!module.parent) {
  app.listen(8000);
  console.log('Express started on port 8000');
}
```

##解决方案二：

使用node-canvas模块进行图片处理和保存。

​	优点：能对图片像html5 canvas一样进行处理，尺寸调整、水印、图片反转色、格式转换

​	缺点：需安装模块支持、当base64编码有误不能解析成图片时会报错并停止nodejs服务。

​	注意点：canvas透明背景，默认为黑色；使用base64给img.src赋值时，需带上data:URL

**使用express搭建的/upload (POST)上传保存接口，完成代码如下：**

```js
var Canvas = require('canvas'); //需安装canvas模块
var express = require('express');
var fs = require("fs");
var app = module.exports = express();
//配置
app.configure(function(){
  app.use(express.bodyParser());
  app.use(express.methodOverride());
  app.use(express.cookieParser('keyboard cat'));
  app.use(express.session());
  app.use(app.router);
  app.use(express.static(__dirname + '/up')); //静态文件目录
  app.use(express.errorHandler({ dumpExceptions: true, showStack: true }));
});

app.post('/upload', function(req, res){
	var base64Data = req.body.imgData;
	var img = new Canvas.Image;

	img.onload = function(){
		var w = img.width;
		var h = img.height;
		var canvas = new Canvas(w, h);
		var ctx = canvas.getContext('2d');
		ctx.drawImage(img, 0, 0);

		var out = fs.createWriteStream(__dirname + '/crop.jpg');
		var stream = canvas.createJPEGStream({
			bufsize : 2048,
			quality : 80
		});

		stream.on('data', function(chunk){
			out.write(chunk);
		});

		stream.on('end', function(){
			out.end();
			res.send("上传成功！");
		});
	}

	img.onerror = function(err){
		res.send(err);
	}

	img.src = base64Data;
});
if (!module.parent) {
  app.listen(8000);
  console.log('Express started on port 3000');
}
```

**容易出现的错误（base64编码中，不容忽视的“+”号）**

1. 如果canvas没有任何像素，则返回值为：“data:,”，这是最短的data:URL，代码中最好做一下保护。
2. 使用解决方案一实现图片保存，生成的图片有size，但是打开后却是不能识别的无效图像。

使用解决方案二实现图片保存，nodejs直接报错，并且服务挂掉。

**原因：**

这个问题，花了我很长时间才找到原因，根本原因是base64编码，使用express接收POST值后，base64编码字符串中的“+”号被替换成空格了，引起编码出错，img.src = base64Data;直接把nodejs服务挂掉。如果你出现类似问题，请console.log(base64Data);看字符串是否有空格。

**解决办法：**

将空格替换回“+”号

```js
var base64Data = imgData.replace(/\s/g,"+");
```



​	

