---

title: Session和Cookie

date: 2019-03-12 12:03:10

categories: Network

tag:

- Network 
- Http

---

# Session和Cookie

HTTP协议是互联网上应用最广泛的一种网络协议，所有Web都必须遵守这个协议。由于HTTP协议是无状态的，服务器无从得知两次请求是否来自同一个用户，所以需要某种机制来区分用户。`Session`和`Cookie`就是常用的会话跟踪技术，`Cookie`通过在客户端记录信息确定用户身份，`Session`在服务器端记录用户信息确定用户身份。

用一个例子来说明两者之间的区别与联系：

假如去村口理发店找托尼老师理发，托尼老师告诉我们说理发5次免费送1次，本来五十块钱只能理5次，现在变成6次了，然而不可能直接被干5次，这时候，想象一下也就一下几种方案：

1. 托尼老师是个奇才，村里面找他理发的人只要一进去，他就知道这人理过几次，还剩几次，这次剃头是不是要收钱。这种做法是协议本身支持状态。
2. 托尼老师给每个人发一张卡片，上面记录理发次数，一般托尼老师还搞一个期限。每次理发时，顾客出示这张卡片，则此次理发就会与以前或以后的理发消费相关联起来。这种做法是客户端保持状态。
3. 托尼老师让大家办VIP卡，除了卡号之外什么信息也不记录，每次理发时出示卡片，托尼就会在理发店的记录本上找到这个卡号对应的记录添加一些消费信息。这种做法就是在服务器端保持状态。

由于HTTP协议是无状态的，而出于种种考虑也不希望使之成为有状态的，因此，后面两种方案就成为现实的选择。具体来说`Cookie`机制采用的是在客户端保持状态的方案，而`Session`机制采用的是在服务器端保持状态的方案。同时我们也看到，由于采用服务器端保持状态的方案在客户端也需要保存一个标识，所以`Session`机制可能需要借助于`Cookie`机制来达到保存标识的目的，但实际上它还有其他选择。

## Cookie

### 什么是Cookie

`Cookie`意为小甜点，是由W3C组织提出，最早由`Netscape`社区发展的一种机制。

**Cookie的机制**：`Cookie`分发是通过扩展`HTTP`协议来实现的，服务器通过在`HTTP`的响应头中加上一行特殊的指示以提示浏览器按照指示生成相应的`Cookie`。而`Cookie`的使用是由浏览器按照一定的原则在后台自动发送给服务器的。浏览器检查所有存储的`Cookie`，如果某个`Cookie`所声明的作用范围大于等于将要请求的资源所在的位置，则把该`Cookie`附在请求资源的`HTTP`请求头上发送给服务器。Cookie是一个很小的纯文本信息，它存储在客户端。

所以就是，由于HTTP是一种无状态协议，服务器但从网络上无从得知客户端身份，那么，给客户端颁发一个通行证，无论谁访问都需要携带通行证，此通行证对于每个人来说都是独一无二的，这样服务器就能够从通行证中确认客户端身份，针对不同的访问来做出不同的响应。

![](https://raw.githubusercontent.com/ccbeango/blogImages/master/NetWork/Session和Cookie01.png)

Cookie可以包含任意的信息，不仅仅是id，客户端会记录服务器返回来的`Set-Cookie`首部中的Cookie内容。并将Cookie存储在浏览器的Cookie数据库中，当用户访问同一站点时，浏览器就会挑选当时该站点颁发的`id=XXX`的身份证Cookie，并在Cookie请求首部发送过去。

#### **服务器发送Cookie给客户端**

从服务器端，发送cookie给客户端，是对应的`Set-Cookie`。包括了对应的cookie的名称、值以及各个属性。

```shell
Set-Cookie: lu=Rg3vHJZnehYLjVg7qi3bZjzg; Expires=Fri, 15 Mar 2019 01:45:01 GMT; Path=/; Domain=.ccbeango.com; HttpOnly

Set-Cookie: made_write_conn=1295214458; Path=/; Domain=.ccbeango.com

Set-Cookie: reg_fb_gate=deleted; Expires=Fri, 15 Mar 2019 01:45:01 GMT; Path=/; Domain=.ccbeango.com; HttpOnly 
```

#### 从客户端把Cookie发送到服务器

从客户端发送cookie给服务器的时候，是不发送cookie的各个属性的，而只是发送对应的名称和值。

```shell
GET /index.html HTTP/1.1  

Host: www.ccbeango.com  

Cookie: name=value; name2=value2  

Accept: */*  
```

### Cookie的类型

可以按照过期时间分为两类：会话cookie和持久cookie。会话cookie是一种临时cookie，用户退出浏览器，会话Cookie就会被删除了，持久cookie则会储存在硬盘里，保留时间更长，关闭浏览器，重启电脑，它依然存在，通常是持久性的cookie会维护某一个用户周期性访问服务器的配置文件或者登录信息。

持久cookie设置一个特定的过期时间（Expires）：

```she
Set-Cookie: id=12345; Expires=Wed, 21 Oct 2019 07:28:00 GMT;
```

也可以设置有效期（Max-Age），这个是现在推荐使用的，在Cookie的属性中有介绍。

### Cookie的属性

**Cookie的域**

域表示当前Cookie所属于哪个域或子域下面。Cookie是不可跨域名的。域名`www.google.com`颁发的Cookie不会被提交到域名`www.baidu.com`去。这是由Cookie的隐私安全机制决定的。隐私安全机制能够禁止网站非法获取其他网站的Cookie。

对于服务器返回的Set-Cookie中，如果没有指定Domain的值，那么其Domain的值是默认为当前所提交的http的请求所对应的主域名的。比如访问` http://www.ccbeango.com`，返回一个cookie，没有指名domain值，那么其为值为默认的`http://www.ccbeango.com`。

正常情况下，同一个一级域名下的两个二级域名如`www.ccbeango.com`和`images.ccbeango.com`也不能交互使用`Cookie`，因为二者的域名并不严格相同。如果想所有`ccbeango.com`名下的二级域名都可以使用该`Cookie`，就需要设置`domain`参数。

```shell
Set-Cookie: name="12345"; domain=".ccbeango.com"
```

注意：`name`相同但`domain`不同的两个Cookie是两个不同的Cookie。如果想要两个域名完全不同的网站共有Cookie，可以生成两个Cookie，`domain`属性分别为两个域名，输出到客户端。

**Cookie的有效期**

Cookie的`maxAge`决定着Cookie的有效期，单位为秒（Second）。它的值可以为正数，表示此Cookie从创建到过期所能存在的时间，此Cookie会存储到客户端电脑，以Cookie文件形式保存，不论关闭浏览器或关闭电脑，直到时间到才会过期。 可以为负数，表示此Cookie只是存储在浏览器内存里，为临时性Cookie，只要关闭浏览器，此Cookie就会消失。`maxAge`默认值为`-1`。 还可以为`0`，表示从客户端电脑或浏览器内存中删除此Cookie。

要想修改Cookie只能使用一个同名的Cookie来覆盖原来的Cookie，达到修改的目的。删除时只需要把maxAge修改为`0`即可。

注意：从客户端读取Cookie时，包括maxAge在内的其他属性都是不可读的，也不会被提交。浏览器提交Cookie时只会提交`name`与`value`属性。maxAge属性只被浏览器用来判断Cookie是否过期。

**Cookie的Path**

`domain`属性决定运行访问Cookie的域名，而`path`属性决定允许访问Cookie的路径`ContextPath`。如果设置为`/user/`，则只有`contextPath`为`/user`的程序可以访问该Cookie。如果设置为`/`，则本域名下`contextPath`都可以访问该Cookie。注意：最后一个字符必须为`/`。

例如：`www.ccbeango.com` 和 `www.ccbeango.com/user/`这两个url。 `www.ccbeango.com` 设置cookie

```
Set-cookie: id="123432";domain="www.ccbeango.com";
```

`www.ccbeango.com/user/` 设置cookie：

```
Set-cookie：user="wang", domain="www.ccbeango.com"; path=/user/
```

访问其他路径`www.ccbeango.com/other/`会获得

```
cookie: id="123432"
```

但如果访问`www.ccbeango.com/user/`就会获得

```
  cookie: id="123432"
  cookie: user="wang"
```

**Cookie的secure**

HTTP协议不仅是无状态的，而且是不安全的。使用HTTP协议的数据不经过任何加密就直接在网络上传播，有被截获的可能。使用HTTP协议传输很机密的内容是一种隐患。如果不希望Cookie在HTTP等非安全协议中传输，可以设置Cookie的`secure`属性为`true`。浏览器只会在`HTTPS`和`SSL`等安全协议中传输此类Cookie。

```shell
Set-Cookie: id=abcdefg; Expires=Wed, 21 Oct 2015 07:28:00 GMT; Secure;
```

**Cookie的httponly**

表示此cookie必须用于http或https传输。这意味着，浏览器脚本，比如javascript中，是不允许访问操作此cookie的。

Cookie是保存在浏览器端的，因此浏览器具有操作Cookie的先决条件。浏览器可以使用脚本程序如JavaScript或者VBScript等操作Cookie。这里以JavaScript为例介绍常用的Cookie操作。例如下面的代码会输出本页面所有的Cookie。

```javascript
<script>document.write(document.cookie);</script>
```

由于JavaScript能够任意地读写Cookie，有些好事者便想使用JavaScript程序去窥探用户在其他网站的Cookie。不过这是徒劳的，W3C组织早就意识到JavaScript对Cookie的读写所带来的安全隐患并加以防备了，W3C标准的浏览器会阻止JavaScript读写任何不属于自己网站的Cookie。换句话说，A网站的JavaScript程序读写B网站的Cookie不会有任何结果。

### 第三方Cookie

通常`Cookie`的域和浏览器地址的域匹配，这被称为第一方Cookie。那么第三方Cookie就是Cookie的域和地址栏中的域不匹配，这种Cookie通常被用在第三方广告网站。为了跟踪用户的浏览记录，并且根据收集的用户的浏览习惯，给用户推送相关的广告。

![](https://raw.githubusercontent.com/ccbeango/blogImages/master/NetWork/Session和Cookie02.png)

如上图（a）：用户访问服务器1的一个页面`index.html`，这个页面和第三方广告网站合作，这个页面还有一张`www.advertisement.com`域名下的一张广告图`ad1.jpg`，当请求这张`ad1.jpg`图片的时候，`www.advertisement.com`这个服务器会给用户设置`cookie`

```
Set-Cookie: user="wang";like="a"; domain="advertisement.com"
```

记录用户的浏览记录，分配一个`user`来表示用户的身份。

图（b）：用户访问服务器2的一个`index.html`页面，这个页面也和同一家广告商合作，这个页面也包含一张`www.advertisement.com`域名下的一张广告图`ad2.jpg`，当请求这张`ad2.jpg`图片的时候，浏览器就会向`www.advertisement.com`发送`cookie`

```
Cookie: user="wang"; like="a";
```

`www.advertisement.com`收到浏览器发送的cookie识别了用户的身份，同时又用这个页面用户的浏览数据设置`cookie`

```
Set-Cookie: buy="b"; domain="advertisement.com"
```

图（c）：很巧，用户访问服务器3的一个`index.html`页面，这个页面也和那一家广告商合作，这个页面也包含一张`www.advertisement.com`域名下的一张广告图`ad3.jpg`，当请求这张`ad3.jpg`图片的时候，浏览器就会向`www.advertisement.com`发送`cookie`

```
Cookie: user="wang"; like="a"; buy="b"
```

这样广告公司就可以根据用户的浏览习惯，给用户推送合适的广告。

## Session

### 什么是Session

`Session`机制是一种服务器端的机制，服务器使用一种类似于散列表的结构（也可能就是使用散列表）来保存信息。

**Session的机制**：当程序需要为某个客户端的请求创建一个`Session`的时候，服务器首先检查这个客户端的请求里是否已包含了一个`Session`标识 - 称为`session id`，如果已包含一个`session id`则说明以前已经为此客户端创建过`Session`，服务器就按照`session id`把这个`Session`检索出来使用（如果检索不到，可能会新建一个），如果客户端请求不包含`session id`，则为此客户端创建一个`Session`并且生成一个与此`Session`相关联的`session id`，`session id`的值应该是一个既不会重复，又不容易被找到规律以仿造的字符串，这个`session id`将被在本次响应中返回给客户端保存。

如果说`Cookie`机制是通过检查客户身上的“通行证”来确定客户身份的话，那么`Session`机制就是通过检查服务器上的“客户明细表”来确认客户身份。`Session`相当于程序在服务器上建立的一份客户档案，客户来访的时候只需要查询客户档案表就可以了。

如果`Cookie`被禁用，可以使用URL重写或者隐藏表单来将`session id`回传给服务器。

#### Session的生命周期

在谈论`Session`机制的时候，常常听到这样一种误解“只要关闭浏览器，`Session`就消失了”。其实可以想象一下会员卡的例子，除非顾客主动对店家提出销卡，否则店家绝对不会轻易删除顾客的资料。对`Session`来说也是一样的，除非程序通知服务器删除一个`Session`，否则服务器会一直保留，程序一般都是在用户做登出的时候发个指令去删除`Session`。然而浏览器从来不会主动在关闭之前通知服务器它将要关闭，因此服务器根本不会有机会知道浏览器已经关闭，之所以会有这种错觉，是大部分`Session`机制都使用会话`Cookie`来保存`session id`，而关闭浏览器后这个`session id`就消失了，再次连接服务器时也就无法找到原来的`Session`。如果服务器设置的`Cookie`被保存到硬盘上，或者使用某种手段改写浏览器发出的`HTTP`请求头，把原来的`session id`发送给服务器，则再次打开浏览器仍然能够找到原来的`Session`。

恰恰是由于关闭浏览器不会导致`Session`被删除，迫使服务器为`Seesion`设置了一个失效时间，当距离客户端上一次使用`Session`的时间超过这个失效时间时，服务器就可以认为客户端已经停止了活动，才会把`Session`删除以节省存储空间。

Session保存在服务器端。**为了获得更高的存取速度，服务器一般把Session放在内存里。每个用户都会有一个独立的Session。如果Session内容过于复杂，当大量客户访问服务器时可能会导致内存溢出。因此，Session里的信息应该尽量精简。**

**Session生成后，只要用户继续访问，服务器就会更新Session的最后访问时间，并维护该Session**。用户每访问服务器一次，无论是否读写Session，服务器都认为该用户的Session“活跃（active）”了一次。

由于会有越来越多的用户访问服务器，因此Session也会越来越多。**为防止内存溢出，服务器会把长时间内没有活跃的Session从内存删除。这个时间就是Session的超时时间。如果超过了超时时间没访问过服务器，Session就自动失效了。**

