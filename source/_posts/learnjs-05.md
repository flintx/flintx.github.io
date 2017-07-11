---
title: JavaScript学习笔记05（jQuery选择器与DOM操作）
date: 2017-07-10 23:04:13
tags:
	- JavaScript
	- 前端
	- jQuery
categories: JavaScript学习
---

> 参考 & 学习：
>
> [锋利的jQuery](https://book.douban.com/subject/10792216/)
>
> [廖老师的JavaScript教程](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000)
>
> [jQuery 教程 | 菜鸟教程](http://www.runoob.com/jquery/jquery-intro.html)
>
> [阮一峰的网络日志](http://www.ruanyifeng.com/blog/)
>
> [JavaScript 参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference)
>
> [W3School JavaScript 教程](http://www.w3school.com.cn/js/index.asp)
>
> 由衷感谢这些资料的提供者与撰写者！

<!-- more -->

jQuery 是一个 JavaScript 库，jQuery 库为使用者提供了以下功能：

- HTML 元素选择器（selector）
- HTML DOM 遍历
- HTML 元素操作、CSS操作
- HTML 事件（event）
- 一些封装好的动画与特效
- AJAX 方法
- 丰富的插件



## jQuery 使用

使用方式有三：

1. 下载后通过 `<script src="jquery-x.x.x.min.js"></script>` 引入，下载地址：[http://jquery.com/download/](http://jquery.com/download/)

2. CDN 引入。

   jQuery 官方的：[link](https://code.jquery.com/)

   国内网站推荐：

   - 七牛云的：[link](https://www.staticfile.org/)
   - 又拍云的：[link](http://www.bootcdn.cn/)
   - 百度的（优点是稳定，劣势是版本不够新，仅支持http）：[link](http://cdn.code.baidu.com/)

   国际网站推荐：

   - 微软的：[link](https://docs.microsoft.com/en-us/aspnet/ajax/cdn/overview#jQuery_Releases_on_the_CDN_0)
   - cdnjs.com：[link](https://cdnjs.com/)
   - Google的：[link](https://developers.google.com/speed/libraries/)

3. 在 Console 控制台中引入 jQuery

   ```javascript
   jQuery; // Uncaught ReferenceError: jQuery is not defined
   var jq = document.createElement('script');
   jq.src = "https://ajax.googleapis.com/ajax/libs/jquery/1.9.0/jquery.min.js";
   document.getElementsByTagName('head')[0].appendChild(jq);
   jQuery // function (e,t){return new st.fn.init(e,t,X)}
   ```

   ​



## jQuery 语法

### $ 符号

jQuery 把所有功能封装在一个全局变量 `jQuery` 中，而 `$` 是 `jQuery` 的别名。

在实际开发中，往往直接使用 `$` 符号。

如果引入多个 js 框架造成了该符号被相互占用，也可以使用 `noConflict()` 方法来解除对 `$` 的占用，交还该符号的占用权。

```javascript
$ === jQuery; // true
typeof($); // "function"
$; // function (e,t){return new st.fn.init(e,t,X)}
var tmp = $.noConflict(); // 创建一个新的别名tmp, 用以在接下来的库中使用 jQuery 对象
$; // undefined
tmp; // function (e,t){return new st.fn.init(e,t,X)}
```

`$` 本质上是一个函数。

但 js 中函数也是一个对象，故 `$` 拥有众多属性与方法。

它的基本语法如下：

```javascript
$(selector).action();
// $ -- 即全局变量jQuery
// selector -- 选择器，用于查询 HTML element
// action -- 对 element 所执行的操作

/* for example */
$("p").hide() // 隐藏所有 <p> 元素
$("p.test").text("Hello World") // 将所有 class="test" 的 <p> 元素的文本设置为 "Hello World"
$("a#test").attr("href", "http://flintx.me") // 将所有 id="test" 的 <a> 元素的链接地址设置为本博客
```

### 文档就绪事件

为了防止网页在文档加载完成之前就运行 jQuery 代码（可能会导致操作一个不存在的元素、获得未完全渲染的元素等），在运行 jQuery 之前我们往往需要加上判断。

```javascript
// 写法 1
$(document).ready(function(){
 
   // do some things...
 
});

// 写法 2
$(function(){
 
   // do some things...
 
});
```

写法 1 可读性较好，写法 2 较简洁，推荐使用写法 1。

`ready()` 方法与原生 JavaScript 中的 `window.onload()` 方法较为类似，但略有不同，如下表所示：

|      |             window.onload             |            ready             |
| :--: | :-----------------------------------: | :--------------------------: |
| 执行时机 |              网页所有内容加载完毕               | 网页所有 DOM 结构绘制完毕（关联内容不一定加载完毕） |
| 调用次数 | 只能调用一次（多次编写 `onload` 方法只会执行最后编辑的方法内容） |            可多次调用             |



```javascript
window.onload = function () {
	alert('first test');
};

window.onload = function () {
	alert('second test');
};

// 结果只输出 'second test'

$(document).ready(function () {
	alert('hello, world!');
});

$(document).ready(function () {
	alert('hello again');
});

// 两个结果依次输出
```



## jQuery 选择器（Selector）

选择器是 jQuery 的核心，它帮助我们快速定位到一个或多个 DOM 节点。



### 基本选择器

基本选择器主要包括：

- `$("#id")` 根据给定的id匹配一个元素；
- `$(".class")` 根据给定的类名匹配元素；
- `$("element")` 根据给定的元素名匹配元素；
- `$("[attr='xxx']")` 根据给定的属性值匹配元素；
- `$("selector1, selector2, ..., selectorN")` 将每一个选择器匹配到的元素合并后一起返回；

下面以 [百度首页](https://www.baidu.com/) 为例，来展示 jQuery 选择器的使用（百度首页已引入 jQuery，可直接使用）。

#### 按 id 查找

如果某个DOM节点有`id`属性，我们可以利用 `$("#id")` 来对其进行定位。

利用 Chrome 的开发者工具，可以快速定位到百度首页 logo 图片所在的 element：

```html
<div id="lg">
  <img hidefocus="true" src="//www.baidu.com/img/bd_logo1.png" width="270" height="129">
</div>
```

执行如下语句：

```javascript
$("#lg") // [div#lg, context: document, selector: "#lg"]
```

可准确定位到 logo 图片所在的 div 元素。

随后我们让其消失：

```javascript
$('#lg').hide();
```

![](http://okqi2ipwh.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170410172657.png)



再让其出现：

```javascript
$('#lg').show(); // 上述操作与使用两次$('#lg').toggle();是同样的效果
```

![](http://okqi2ipwh.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170410172616.png)

#### 按 tag 查找

```javascript
// 基本语法

$('tag_name').method();

// 让百度首页所有链接消失
$('a').fadeToggle();
```



#### 按 class 查找

```javascript
// 基本语法
$('.class_name').method();

// 让b站(www.bilibili.com)所有图片改成ac娘
$('img').attr('src', 'http://omfq5kywz.bkt.clouddn.com/10.png');
```



![](http://okqi2ipwh.bkt.clouddn.com/QQ%E6%88%AA%E5%9B%BE20170508151343.png)

#### 按属性查找

其实 id 也好，class 也好，都可以看做一个 DOM 节点的属性，使用属性来查找往往会更加便捷。

```javascript
// 选取带有href属性的元素
$("[href]")

// 选取target属性为"_blank"的元素
$("[target='_blank']")

// 选取target属性不为"_blank"的a元素
$("a[target!='_blank']")
```



### 层次选择器

![](http://img.blog.csdn.net/20160221180840819)



`$("ancestor descendant")` 与 `("parent > child")` 的区别在于：前者选取 `ancestor` 节点的所有后代中的 `descendant` 元素，后者选取 `parent` 节点的下一层子节点中的 `child` 元素

此外，在 jQuery 中还可以用 `next()` 方法来替代 `$('prev + next')` ，用 `nextAll()` 方法来替代 `$('prev ~ siblings')` 

一些使用示例：

```javascript
// 将 body 内的所有 div 元素背景色变为 Miku 色 (图1)
$('body div').css("background","#39c5bb");

// 将 body 下一层节点中的所有 div 元素背景色变为 Miku 色 (图2)
$('body>div').css("background","#39c5bb");

// 将 id 为 lga 的节点之后的下一个兄弟(同级) div 节点隐藏，这句话可以在Google首页试一下:)
$('#lga+div').toggle()

// 将 id 为 cst 的节点之后的所有兄弟(同级) div 节点隐藏，这句话可以在Google首页试一下:)
$('#cst~div').toggle()
```



图1：

![](http://okqi2ipwh.bkt.clouddn.com/%E5%B1%82%E6%AC%A1%E9%80%89%E6%8B%A9%E5%99%A81.png)



图2：

![](http://okqi2ipwh.bkt.clouddn.com/%E5%B1%82%E6%AC%A1%E9%80%89%E6%8B%A9%E5%99%A82.png)

### 过滤选择器

#### 基本过滤选择器

![](http://okqi2ipwh.bkt.clouddn.com/%E5%9F%BA%E6%9C%AC%E8%BF%87%E6%BB%A4%E9%80%89%E6%8B%A9%E5%99%A8.png)

#### 内容过滤选择器

![](http://okqi2ipwh.bkt.clouddn.com/%E5%86%85%E5%AE%B9%E9%80%89%E6%8B%A9%E8%BF%87%E6%BB%A4%E5%99%A8.gif)

#### 属性过滤选择器

![](http://okqi2ipwh.bkt.clouddn.com/%E5%B1%9E%E6%80%A7%E9%80%89%E6%8B%A9%E8%BF%87%E6%BB%A4%E5%99%A8.jpg)

#### 表单过滤选择器

![](http://okqi2ipwh.bkt.clouddn.com/%E8%A1%A8%E5%8D%95%E8%BF%87%E6%BB%A4%E9%80%89%E6%8B%A9%E5%99%A8.png)





###  补充

1. 当有傻逼在属性值中使用了 `.` `#` `()` `[]` `:` 等字符，这时候选择器很容易出问题，需要使用转义字符（即在特殊字符前加一个 `\\` ）。

   ```html
   <div id="s#b[id]"></div>

   <script>
   	$('#s#b[id]') // 怕是要跪。。
   	$('#s//#b//[id//]') // It's OK.
   </script>
   ```

   ​

2. 还有一个要注意的是：选择器里不要有多余的空格！！！多一个空格少一个空格，选择器表达的意义就会大有不同。

   ```javascript
   $('#test :hidden') // 选取 id 为 test 的元素后代中的隐藏元素
   $('#test:hidden') // 选取 id 为 test 的隐藏元素
   ```



### 示例

1. 给网页中所有 `<p>` 元素添加 `onClick()` 事件

   ```javascript
   // 原生写法
   var items = document.getElementsByTagName("p");// 获取页面中的所有p元素
   for(var i = 0; i < items.length; i++) {	
   	items[i].onclick = function(){  // 给每一个p添加onclick事件
   		// do something
   	}
   }

   // jQuery写法
   $('p').click(function(){
     	// do something
   });
   ```

   ​

2. 使一个表格隔行变色

   ```javascript
   // 原生写法
   var item = document.getElementById("tb1");			// 获取id为tb1的元素(table)
   var tbody = item.getElementsByTagName("tbody")[0];	// 获取表格的第一个tbody元素
   var trs = tbody.getElementsByTagName("tr");	        // 获取tbody元素下的所有tr元素
   for(var i = 0; i < trs.length; i++){
   	if (i % 2 === 0) {
   		trs[i].style.backgroundColor = "#cccccc"; // 改变tr元素背景色.
   	}
   }

   // jQuery写法
   $('#tb1 tbody tr:even').attr('background', '#cccccc');
   ```

   ​

3. 有一个复选框，输出选中项个数

   ```javascript
   // 原生写法
   var arrays = new Array();
   var items = document.getElementsByName("check");  // 获取name为check的一组元素(checkbox)
   for(var i = 0; i < items.length; i++) {
   	if (items[i].checked) {      // 判断是否选中
   		arrays.push(items[i].value);
   	}
   }
   alert("选中的个数为：" + arrays.length);

   // jQuery写法
   alert("选中的个数为：" + $("input[name='check']:checked").length);
   ```





## jQuery DOM 操作



### 插入节点（append）

插入一个节点最常用的方法是 `append()` ，语法一般是 `$(selector).append(html)`  ，即在节点中插入一个子节点。

需要区别的就是 `append()` 与 `after()` 方法，前者插入位置是是在节点中的后面，后者是节点后。

```html
<ul>
  <li>...</li>
  <li>...</li>
  <!-- $('ul').append('<li>...</li>') 的插入位置 -->
</ul>
<!-- $('ul').after('<li>...</li>') 的插入位置 -->
```



插入方法一览：



![](http://okqi2ipwh.bkt.clouddn.com/%E6%8F%92%E5%85%A5%E8%8A%82%E7%82%B91.png)

![](http://okqi2ipwh.bkt.clouddn.com/%E6%8F%92%E5%85%A5%E8%8A%82%E7%82%B92.png)

### 删除节点（remove）

删除节点的方法有三个：`remove()` ，`detach()` ，`empty()` 

- remove() : 删除内容包含所有匹配的节点以及其后代节点，返回已删除节点的引用。

  ```javascript
  var $delDiv = $('div').remove(); // 删除页面中所有 div 元素
  $delDiv.appendTo('body'); // 将删除的所有节点重新加入到页面中

  $('ul li').remove('li:gt(2)'); // 删除表中索引值大于2的li元素
  ```

  ​

- detach() : 使用方法等同于 `remove()` ，不同在于 `detach()` 将会保留与被删除元素绑定的事件与数据等，恢复后可以继续调用其绑定事件，而 `remove()` 方法必须重新绑定。

  ```javascript
  // 给p元素绑定click事件
  $('p').click(function(){
    	alert($(this).html());
  });

  var $delP1 = $('p').detach();
  $delP1.appendTo('body'); // 点击p元素，可以触发之前绑定的click事件

  var $delP2 = $('p').remove();
  $delP2.appendTo('body'); // 点击p元素，不可以触发之前绑定的click事件
  ```

  ​

- empty() : 该方法并非删除节点，而是清空节点内容，即将匹配到的元素标签 `<xx>` 与 `</xx>` 之间的内容清除，包括子节点。

  ```html
  <div id='div1'>
    <p>
      3.141592653589793238462643383279502884197169...
    </p>
  </div>

  <script>
  	$('#div1').empty(); // ==> <div id='div1'></div>
  </script>

  <script>
  	$('p').empty(); // ==> <div id='div1'><p></p></div>
  </script>
  ```

  ​

### 复制节点（clone）

复制节点主要用到了 `clone()` 方法.

- clone() : 一般语法为 `$(selector).clone()` ，可以选择参数 `true` 或者 `false` ，分别表示复制绑定方法与不复制绑定方法，默认为 `false` 。

  ```javascript
  // 复制索引值为4（第5个）的节点，并将其追加到<ul>元素中
  $('ul li:eq(4)').clone().appendTo('ul'); 

  // 将id为btn1的节点复制一份，追加到id为btn1的节点之后，并复制其绑定方法
  $('#btn1').after($('#btn1').clone(true));
  ```

  ​

### 替换节点（replace）

替换节点有 `replaceWith()` 与 `replaceAll()` 两个方法。这两个方法作用相同，只不过前者参数是替换内容，后者是被替换节点。

```javascript
// <div><p> hahaha:) </p><div> ==> <div><h1>Hello, world!</h1></div>
$('p').replaceWith('<h1>Hello, world!</h1>');
$('<h1>Hello, world!</h1>').replaceAll('p'); // 两种方法功能相同
```



### 包装节点（wrap）

所谓包装节点，又称包裹节点，就是把选取的节点外层再包裹一层标签。

实现这一功能的方法是 `wrap()` 、`wrapAll()` 与 `wrapInner()` 。分别对应着单独包裹选取的每一个节点、统一包裹选取的所有节点、单独包裹选取节点的子节点。

具体使用方法如下：

```html
<p>first element.</p>
<p>second element.</p>
<p>third element.</p>

<script>
	function wrap1() {
      	$('p').wrap('<b></b>');
	}
  
  	function wrap2() {
      	$('p').wrapAll('<b></b>');
	}
  
  	function wrap3() {
      	$('p').wrapInner('<b></b>');
	}
</script>
```



- 执行wrap1() : 

  ```html
  <b><p>first element.</p></b>
  <b><p>second element.</p></b>
  <b><p>third element.</p></b>
  ```

- 执行wrap2() : 

  ```html
  <b>
    <p>first element.</p>
    <p>second element.</p>
    <p>third element.</p>
  </b>
  ```

- 执行wrap3() : 

  ```html
  <p><b>first element.</b></p>
  <p><b>second element.</b></p>
  <p><b>third element.</b></p>
  ```

需要注意的是，使用 `wrapAll()` 方法时，如果选取元素之间有其他元素，那么其他元素将被放到包裹元素之后。

```html
<!-- 包裹前 -->
<p>first element.</p>
<div>A div.</div>
<p>second element.</p>

<script>
	$('p').wrapAll('<b></b>');
</script>

<!-- 包裹后 -->
<b>
  <p>first element.</p>
  <p>second element.</p>
</b>
<div>A div.</div>
```



此外，三个wrap函数的参数内容其实就是一个jQuery选择器，这里举一个以供思考的例子：

```html
<!-- 包裹前 -->
<p>first element.</p>
<div>A div.</div>
<p>second element.</p>

<script>
	$('p').wrap('div');
</script>

<!-- 包裹后 -->
<div>
  A div.
  <p>first element.</p>
</div>
<div>A div.</div>
<div>
  A div.
  <p>first element.</p>
  <p>second element.</p>
</div>
<!-- 被双层包裹 -->
```



### 遍历节点（traversal）

访问临近DOM节点常用的方法一般有：

- `children()` ：返回选择器匹配元素的子元素集合，不考虑任何子元素的后代元素；
- `next()` ：返回紧邻在选择器匹配元素之后的同辈元素；
- `prev()` ：返回紧邻在选择器匹配元素之前的同辈元素；
- `siblings()` ：返回选择器匹配元素的所有前后同辈元素；
- `closest()` ：返回选择器匹配元素的最近匹配参数中选择器的元素。

此外还有很多方法，如下表所示：

![](http://okqi2ipwh.bkt.clouddn.com/%E9%81%8D%E5%8E%86%E8%8A%82%E7%82%B9.png)



### 属性操作（attribute）

这部分比较简单，就不予赘述了。

- 获取属性值：`attr(attr_name)` 
- 设置属性值：`attr(attr_name, attr_value)`
- 删除属性：`removeAttr(attr_name)`



### 样式操作（style）

包括两方面内容，class相关与style相关：

1. class相关

   - `addClass()` ：追加class样式（如果要设置class样式应该用 `attr()` 方法）；
   - `removeClass()` ：移除class样式，参数值为想要移除的class样式，当无参数时则移除所有class样式；
   - `toggleClass()` ：切换class样式，若类名存在则删除，若不存在则追加；
   - `hasClass()` ：判断是否含有某class样式。

2. style相关

   - `css(css_name)` ：获取style属性值；

   - `css(css_name, css_value)` ：设置style属性值；

   - `css({css_name1:css_value1, css_name2:css_value2, ...})` ：设置多个style属性值；

     ```javascript
     $('p').css('color'); // 获取p元素的样式颜色值
     $('p').css('color', 'red'); // 设置p元素的样式颜色为红色
     $('p').css({'color':'red', 'font-size':'24px'}); // 设置p元素字体大小为24像素，样式颜色为红色

     // font-size不带引号时应使用驼峰写法，即
     $('p').css(fontSize, '24px');
     // 若带引号则两种写法皆可，即
     $('p').css('fontSize', '24px');
     $('p').css('font-size', '24px');
     ```

除上述几个方法外，还有一些常用的内建方法，如 `height()` 、`width()` 、`offset()` 等，如下所示：

![](http://okqi2ipwh.bkt.clouddn.com/%E5%B1%9E%E6%80%A7%E6%93%8D%E4%BD%9C.png)

### 内容操作（value）

内容操作主要就是三个方法的辨析：`html()` , `text()` , `val()` .

- html() : 设置或返回指定某个标签开始到结束标签之间的所有内容（包括标签和文本内容）；
- text() : 设置或返回被选元素的纯文本内容；
- value() : 设置或者获取设置或者获取元素的 value 值，该方法大多用于 input 元素。

如果只是这么简单的一段无嵌套纯文本html：

```html
<p>
  苟利国家生死以，岂因祸福避趋之。
</p>
```

前两者的返回均为：`苟利国家生死以，岂因祸福避趋之。` ，`val()` 返回为 `""`  。

