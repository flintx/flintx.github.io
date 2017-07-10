---
title: JavaScript学习笔记04（BOM 与 DOM）
date: 2017-03-07 17:04:13
tags:
	- JavaScript
	- 前端
categories: JavaScript学习
---

> 参考 & 学习：
>
> [廖老师的JavaScript教程](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000)
>
> [阮一峰的网络日志](http://www.ruanyifeng.com/blog/)
>
> [JavaScript 参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference)
>
> [W3School JavaScript 教程](http://www.w3school.com.cn/js/index.asp)
>
> 由衷感谢这些资料的提供者与撰写者！

<!-- more -->



## 浏览器对象模型（BOM）



BOM 的全称是 **B**rowser **O**bject **M**odel（浏览器对象模型），即把**浏览器**当做一个对象来看待。BOM 是赖以Javascript的 Web 服务的真正核心，该模型提供了很多对象，用于调用与访问浏览器的各种功能与属性，而与网页内容无关。

BOM 提供了很多用于与浏览器交互操作的对象：`window` ，`location` ，`navigator` ，`screen` ，`history` 。



### window

`window` 是 BOM 的核心对象，其表示浏览器的一个实例，即使访问浏览器的一个接口，同时也是 ECMAScript 规定的 `Global` 对象。因此 `window.alert()` 可直接通过 `alert()` 调用。

#### 属性

```javascript
// 获取浏览器窗口宽高
alert('window inner size: ' + window.innerWidth + ' x ' + window.innerHeight);
// 1366 * 766
```



#### 操作

```javascript
// 打开一个新的url
// 简单模式
window.open('https://www.baidu.com');
// 复杂模式
// 第二个参数为新url的target属性，可以有_self, _parent, _top或_blank，也可以传入一个窗口命名字符串
// 第三个特性字符串不能有空格
var myWin = window.open('http://www.flintx.me', 'myWindow',
            'height=400,width=400,top=10,left=10,resizable=yes'); 

// 操作浏览器窗口大小的方法 resizeTo() 与 resizeBy() 通常被浏览器禁用.
// 操作浏览器窗口位置的方法 moveTo() 与 moveBy() 通常被浏览器禁用

// 关闭新打开的窗口
myWin.close();

// 超时调用
var timeoutId = setTimeout(function() {
    alert('Time Out!');
}, 1000);
// 撤销超时调用
clearTimeout(timeoutId);

// 间歇调用
var count = 0;
var intervalId = setInterval(function() {
    count++;
    console.log(count * 10 + ' 秒后, 第 ' + count + ' 次调用!');
}, 10000);
// 10 秒后, 第 1 次调用!
// 20 秒后, 第 2 次调用!
// 30 秒后, 第 3 次调用!
// 撤销间歇调用
clearInterval(intervalId);
```

#### 对话框

- `window.alert()`

  ```javascript
  alert('Hello, Flintx!');
  ```

  ![](http://okqi2ipwh.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170305151836.jpg)

- `window.confirm()`

  ```javascript
  if (confirm("To be or not to be?")) {
      alert('this is a question.');
  } else {
      alert('this is a question, too.');
  }
  ```

  ![](http://okqi2ipwh.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170305152236.jpg)

- `window.prompt()`

  ```javascript
  var result = prompt('道理我都懂，但JavaScript为啥这么坑？', '说出你的故事');
  if (result != null) {
      console.log(result); // 我也不知道，我也很绝望啊
  }
  ```

  ![](http://okqi2ipwh.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170305152601.jpg)



### location

`location` 是最有用的 BOM 对象之一，它提供了当前窗口的页面有关信息。

`location`，`window.location`，`document.location` 引用的是同一个对象。

`location` 可以将一个 URL 解析成独立的片段，并能通过属性来访问这些片段。

以一个 URL 为例：

```http
http://www.example.com:4000/blog/index.html?x=1&y=2#1a2c3b4d
```

片段分解：

```javascript
location.href; // http://www.example.com:4000/blog/index.html?x=1&y=2#1a2c3b4d
location.host; // www.example.com:4000
location.hostname; // www.example.com
location.port; // 4000
location.protocol; // http:
location.pathname; // /blog/index.html
location.search; // ?x=1&y=2
location.hash; // #1a2c3b4d
```

主要方法：

- `location.assign()` : 加载给定URL的内容资源到这个 `location` 对象所关联的实例上。
- `location.reload()` : 重新加载当前页面，若无参数页面会从浏览器缓存加载，传递参数 `true` 则强制从服务器重新加载。
- `location.replace()` : 用给定的URL替换掉当前的资源。与 `assign()` 方法不同的是用 `replace()`替换的新页面不会被保存在会话的历史中，这意味着用户将不能用后退按钮转到该页面。

### navigator

`navigator` 用以表示浏览器的信息。在 Web 开发领域，不同浏览器或者同一浏览器的不同平台版本总是各有所长各有所短，故客户端检测一直是无法回避的一种开发策略，而 `navigator` 对象的不少属性与方法为客户端检测提供了行而有效的支持。

关于各大浏览器之间的恩怨情仇与一些祖传bug可参考 [JavaScript高级程序设计（第3版）](https://book.douban.com/subject/10546125/) 的第9章，写得十分详细精彩，不得不说，这就是江湖。并且这一章最后提供了一个完整的客户端检测方案，即优先使用通用方法编程，进而使用能力检测、怪癖检测，最后使用用户代理检测（并附有完整检测代码），值得一阅。

`navigator` 对象属性方法中支持浏览器较多的一部分：

```javascript
navigator.appName; // "Netscape", 大部分浏览器都是"Netscape"
navigator.appCodeName; // "Mozilla", 通常是"Mozilla", 即使是非Mozilla浏览器
navigator.appVersion;
// "5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/56.0.2924.87 Safari/537.36"
// 一般不与实际浏览器对应
```

以上属性一般没用，原因在注释里。。囧。。

下面介绍有用且支持浏览器较多的：

```javascript
// 用户代理属性
navigator.userAgent;
/* "Mozilla/5.0 (Windows NT 10.0; Win64; x64) 
 * AppleWebKit/537.36 (KHTML, like Gecko) 
 * Chrome/56.0.2924.87 
 * Safari/537.36"
 */
// 和appVersion比较像, 但还是userAgent用的多一些, 客户端检测全靠它

// 检测插件
navigator.plugins.length; // 5
navigator.plugins[0].name; // "Widevine Content Decryption Module"
navigator.plugins[0].description
// "Enables Widevine licenses for playback of HTML audio/video content. (version: 1.4.8.962)"

// 检查cookie是否启用
navigator.cookieEnabled; // true

// 检查Java是否启用
navigator.javaEnabled(); // false
```



### screen

不同于 `window` 对象反映的是浏览器窗口的信息，`screen`  对象主要反映的是显示器的信息，用处不大【JavaScript高级程序设计（第3版）钦定的】。

主要用到的属性如下：

- `screen.width` ：屏幕宽度，以像素为单位；
- `screen.height` ：屏幕高度，以像素为单位；
- `screen.colorDepth` ：返回颜色位数，如8、16、24。

```javascript
console.log('Screen size = ' + screen.width + ' x ' + screen.height);
// Screen size = 1366 x 768
console.log('Screen color depth is ' + screen.colorDepth);
// Screen color depth is 24
```



### history

`history` 对象也不常用，调用 `history` 对象的 `back()` 或 `forward ()` 可以模仿浏览器的“后退”和“前进”按钮。但现在频繁交互的Web环境下，这个对象已经属于历史遗留了。当然了，它还有一个用途，即检测用户是否一开始就打开了你的页面：

```javascript
if (history.length === 0) {
    // 用户打开窗口之后的第一个页面
}
```



## 文档对象模型（DOM）

W3C对DOM的定义是：

> “一个与系统平台和变成语言无关的接口，程序和脚本可以通过这个接口动态地访问和修改文档的内容，结构，样式.”

有人说，**前端**的 JavaScript = ECMAScript（基本语法） + BOM（操作浏览器的API） + DOM（操作页面文档的API），虽有几分浮夸，但不无道理。

与 BOM 相比：

> BOM 是为了操作浏览器出现的 API，window 是其主要对象。
>
> 而 DOM 是为了操作文档出现的 API，document 是其主要对象；

关于 JavaScript DOM 操作的各种东西可以写[一本书](https://book.douban.com/subject/6038371/)了，这里只写比较基础的一些操作。

操作用的网页是自己的一篇文章：[Hello, My Blog](http://flintx.me/2017/02/02/hello/)

### 查找 HTML Element

通过 DOM 来操作页面的第一步：定位元素。

方法有二：

#### 通过 id 查找

```javascript
var imageTest = document.getElementById('图片插入测试');
imageTest;
```
查找出来是这样的：

```html
<h2 id="图片插入测试"><a href="#图片插入测试" class="headerlink" title="图片插入测试"></a>图片插入测试</h2>
```

#### 通过 Tag 查找

```javascript
var h2tag = document.getElementsByTagName('h2');
h2tag; // [h2#图片插入测试, h2#代码高亮测试, h2#数学公式测试]
```

### 通过 DOM 修改 HTML

**慎重**使用 `document.write()` 方法！！！

推荐使用修改元素 `innerHTML` 属性的方式。

#### 例1.修改文本

以本地修改新浪微博粉丝数为例：

**修改前**：

![](http://okqi2ipwh.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170306202348.jpg)

右键 -> 检查，得到这个数字 `233` 的元素：

```html
<strong class="W_f18">223</strong>
```

定位至该元素：

```javascript
var strongEle = document.getElementsByTagName('strong');
strongEle; // [strong.W_f18, strong.W_f18, strong.W_f18]
strongEle[0].innerHTML; // "217" --- 关注数
strongEle[1].innerHTML; // "223" --- 粉丝数
strongEle[2].innerHTML; // "342" --- 微博数
```

现在我们重写粉丝数的 `innerHTML` 属性：

```javascript
strongEle[1].innerHTML = '1000000';
```

**修改后**：

![](http://okqi2ipwh.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170306203200.jpg)

哈哈，百万粉留念 ~！

![](http://okqi2ipwh.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170306203236.jpg)

不是数字也没问题。嘛，可惜一刷新就没了。。



#### 例2.修改属性

关于改变一个元素的属性，用如下语法即可：

```javascript
document.getElementById(id).attribute = new value;
```

这里用这个demo网页来举例：[白学现场](http://flintx.me/laboratory/learnjs_04_01.html)

源代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>白学现场</title>
</head>
<body>
<img id="wa2" src="http://flintx.me/images/Kazusa.jpg" height="600" width="270">
<button id="btn">冬马小三！</button>
<h3 id="white_theory" align="center"></h3>
<script>
    var SetsunaURL = 'http://flintx.me/images/Setsuna.jpg';
    var KazusaURL = 'http://flintx.me/images/Kazusa.jpg';
    var WA2URL = 'http://flintx.me/images/WA2.jpg';
    var btn = document.getElementById('btn');
    var wa2Image = document.getElementById('wa2');
    var wa2Text = document.getElementById('white_theory');
    var times = 0;
    btn.addEventListener('click', function () {
        times++;
        if (times > 5) {
            btn.style.display = 'none';
            wa2Image.src = WA2URL;
            wa2Image.height = '720';
            wa2Image.width = '1366';
            wa2Text.innerHTML = '为什么会这样呢？明明是我先来的';
        } else if (times % 2 === 1) {
            btn.innerHTML = '雪菜碧池！';
            wa2Image.src = SetsunaURL;
        } else if (times % 2 === 0) {
            btn.innerHTML = '冬马小三！';
            wa2Image.src = KazusaURL;
        }
    });
</script>
</body>
</html>
```

效果就自己去感受吧哈哈 ~ （初次加载可能比较慢，多试几次风味更佳）



### 通过 DOM 修改 CSS

若需改变一个元素的样式（style），可用如下语法：

```javascript
document.getElementById(id).style.property = new style;
```

由于 CSS 基础还未学习（直接上手的JS。。），这里只能做一些很基础的演示，点按钮修改字体颜色大小：

[Change! Change! Change!](http://flintx.me/laboratory/learnjs_04_02.html)

代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Change! Change! Change!</title>
</head>
<body>
<p id="pid">
    Hello World!
</p>

<button id="btn1">变大</button>
<button id="btn2">变小</button>
<button id="btn3">变色</button>
<button id="btn4">消失</button>
<button id="btn5">出现</button>

<script>
    var p = document.getElementById('pid');
    var btn1 = document.getElementById('btn1');
    var btn2 = document.getElementById('btn2');
    var btn3 = document.getElementById('btn3');
    var btn4 = document.getElementById('btn4');
    var btn5 = document.getElementById('btn5');

    function getRandomInt(min, max) {
        return Math.floor(Math.random() * (max - min + 1) + min);
    }
    btn1.addEventListener('click', function () {
        var fontSize = p.style.fontSize || '20px';
        fontSize = parseInt(fontSize) + 2;
        p.style.fontSize = fontSize.toString() + 'px';
    });
    btn2.addEventListener('click', function () {
        var fontSize = p.style.fontSize || '20px';
        fontSize = parseInt(fontSize) - 2;
        p.style.fontSize = fontSize.toString() + 'px';
    });
    btn3.addEventListener('click', function () {
        var colors = ['red', 'blue', 'green', 'pink', 'gray', 'orange', 'yellow'];
        p.style.color = colors[getRandomInt(0, colors.length - 1)];
    });
    btn4.addEventListener('click', function () {
        p.style.visibility = 'hidden';
    });
    btn5.addEventListener('click', function () {
        p.style.visibility = 'visible';
    });
</script>
</body>
</html>
```

### 通过 DOM 监听事件（Event）

之前通过 DOM 来修改 HTML 和 CSS 其实已经用到了 `Event` 的概念，即 `onclick` （鼠标点击）。

接下来通过这个网页来做一些演示：[Events Test](http://flintx.me/laboratory/learnjs_04_03.html)

- `onload` 与 `onunload` : 会在用户进入或离开页面时被触发


- `onchange` : 常结合对输入字段的验证
- `onmouseover` 和 `onmouseout` : 用于在用户的鼠标移至 HTML 元素上方或移出元素时触发
- `onmousedown`、`onmouseup` 以及 `onclick` : 鼠标点击事件的所有部分

代码如下：

```html
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>Events Test</title>
    <link rel="stylesheet" type="text/css" href="mystyle.css">
</head>
<body onload="displayTime()" onunload="exitPage()">
<p></p>
<p id="p1">Flintx OS 尚未加载完成</p>
<p></p>
<h2>事件1：onchange</h2>
<p>请输入小写英文字母 <label for="input1"></label><input type="text" id="input1"></p>
<p>当离开输入字段时，文本将转换为大写。</p>
<p></p>

<h2>事件2：onmouseover 和 onmouseout</h2>
<div id="div1" class="div">鼠标移动到框中</div>
<p></p>

<h2>事件3：onmousedown 和 onmouseup</h2>
<img id="img1" src="http://omfq5kywz.bkt.clouddn.com/06.png">
<p>点击图像按住不放</p>
<script>
    document.getElementById('input1').addEventListener('change', function () {
       this.value = this.value.toUpperCase();
    });
    document.getElementById('div1').addEventListener('mouseover', function () {
        this.innerHTML = '哥们，你听说过安利吗？';
    });
    document.getElementById('div1').addEventListener('mouseout', function () {
       this.innerHTML = '别走啊，大兄弟';
    });
    document.getElementById('img1').addEventListener('mousedown', function () {
        this.src = 'http://omfq5kywz.bkt.clouddn.com/08.png';
    });
    document.getElementById('img1').addEventListener('mouseup', function () {
        this.src = 'http://omfq5kywz.bkt.clouddn.com/06.png';
    });

    function displayTime() {
        function check(x) {
            if (x < 10) {
                return '0' + x.toString();
            }
            return x.toString();
        }
        var time = new Date();
        var timePrint = check(time.getHours()) + ':' + check(time.getMinutes()) + ':' + check(time.getSeconds());
        document.getElementById('p1').innerHTML = 'Flintx OS 已启动\n当前时间：' + timePrint;
        setTimeout(displayTime, 500);
    }
    function exitPage() {
        alert('能量耗尽...');
    }
</script>
</body>
</html>
```

mystyle.css :

```css
h2 {
    color: darkslategray;
    font-size: medium;
    text-decoration: underline;
}

p {
    font-family: Consolas, "Liberation Mono", Courier, monospace;
    font-size: small;
}

.div {
    width: 300px;
    height: 100px;
    background-color: aqua;
    color: maroon;
    text-align: center;
    display: table-cell;
    vertical-align: middle;
}
```

