---
title: JavaScript学习笔记03（面向对象编程）
date: 2017-03-05 00:34:13
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

##  JavaScript面向对象的基本概念

JavaScript是一门基于对象的语言，可以说所有数据皆为对象。然而不同于C++/Java/C#，JavaScript没有类（Class）的概念，不区分类和对象实例。换而言之，"student" 和 "the student" 在JS中是没有区别的。

ECMA-262将对象定义为”无序属性的集合，其属性可以是基本值、对象和函数。“故我们可以将JS中的对象想象成散列表，由键值对组成，其中值可以是数据或函数。

### 对象的创建与生成

对象创建有多种模式，这里从最原始的模式讲起。

#### 1.原始模式

原始模式，即通过字面量直接创建对象实例。

```javascript
var cat1 = {
	name : 'Tom',
	color : 'yellow',
	sayHi : function () {
		console.log('喵喵喵~');
	}
};
// or
var cat2 = new Object();
cat2.name = 'Henry';
cat2.color = 'gray';
cat2.sayHi = function () {
	console.log('喵喵喵~');
};

cat1.sayHi === cat2.sayHi; // false
```

原始模式的缺点很明显：重复代码过多，同一类的对象实例之间无关联。

#### 2.工厂模式

```javascript
function createCat(name, color) {
	var o = new Object();
	o.name = name;
	o.color = color;
	o.sayHi = function () {
		console.log('喵喵喵~');
	}
	return o;
}

var cat1 = createCat('Tom', 'yellow-brown');
console.log(cat1.name); // Tom
console.log(cat1.color); // yellow-brown
cat1.sayHi(); // 喵喵喵~

var cat2 = createCat('Henry', 'gray');
cat1 === cat2; // false
```

工厂模式虽然解决了创建多个相似对象的问题，但是无法解决对象识别的问题，无法知道一个对象实例具体属于什么类。

#### 3.构造函数模式

```javascript
function Cat(name, color) {
	this.name = name;
	this.color = color;
	this.sayHi = function () {
		console.log('喵喵喵~');
	};
  	// 等效于 new Function("console.log('喵喵喵~')");
}

var cat1 = new Cat('azusa', 'black');
var cat2 = new Cat('yui', 'white');
cat1.sayHi(); // 喵喵喵~

// 也可以在另一对象作用域调用
var cat3 = new Object();
Cat.call(cat3, 'aki', 'yellow');
cat3.sayHi(); // 喵喵喵~

cat1 instanceof Cat; // true
cat1.sayHi === cat2.sayHi; // false
```

构造函数模式的优点比较明显，即利用创建 `Function` 对象增加了一个类（Class）的概念，可以有效解决对象识别问题，而且构造函数还能当作普通函数来调用，区别于是否使用 `new` 运算符。

然而构造函数模式并非没有缺点，每个方法在实例上都要重新创建一遍，存在一个浪费内存的问题。虽然 `cat1` 与 `cat2` 都有一个 `sayHi()` 的方法，但属于不同的 `Function` 实例。同样的方法要创建不同的实例来实现，十分浪费内存。

#### 4.原型模式

```javascript
function Cat(name, color) {
	this.name = name;
	this.color = color;
}
Cat.prototype.master = 'Flint';
Cat.prototype.sayHi = function () {
	console.log('喵喵喵~');
};

var cat1 = new Cat('azusa', 'black');
var cat2 = new Cat('yui', 'white');
cat1.sayHi(); // 喵喵喵~
console.log(cat2.master); // Flint

cat1.sayHi === cat2.sayHi; // true

cat1.hasOwnProperty('name'); // true
cat1.hasOwnProperty('sayHi'); // false
```

准确来说，这是构造函数模式和原型模式的组合使用（因为实例一般或多或少会有独属于自己的属性，故很少有人单纯使用原型模式）。

除此之外，原型模式还有一点语法糖：

```javascript
function Cat(name, color) {
	this.name = name;
	this.color = color;
}
Cat.prototype.master = 'Flint';
Cat.prototype.sayHi = function () {
	console.log('喵喵喵~');
};
// 上述代码可以写作：

function Cat(name, color) {
	this.name = name;
	this.color = color;
}
Cat.prototype = {
	master : 'Flint',
	sayHi : function () {
		console.log('喵喵喵~');
	}
};
```



#### 5.class关键字

在ES6中新引入了 `class` 关键字，让类的定义不再依赖于原型来实现，不过目前支持ES6的浏览器还不够多，成为主流代码风格还需要一段时间。

```javascript
function Student(name) {
  	this.name = name;
}
Student.prototype.sayHi = function() {
  	console.log(`Hello, I am ${this.name}!`);
}
```

上述代码可利用 `class` 关键字改写为：

```javascript
class Student {
	constructor(name) {
		this.name = name
	}

	sayHi() {
		console.log(`Hello, I am ${this.name}!`);
	} // 定义原型函数不需要function关键字
}

var stu1 = new Student('小明');
stu1.sayHi(); // Hello, I am 小明!
var stu2 = new Student('小红');
stu1.sayHi === stu2.sayHi; // true
```



### 继承

ECMAScript 仅支持实现继承，不支持接口继承。而继承方法有如下几种：

#### 1.原型链继承

原型链的概念如下所示：

```javascript
new Cat() ==> Cat.prototype ==> Animal.prototype ==> Object.prototype ==> null
```

即新建的实例对象 `new Cat()` 继承了 `Cat` 的原型方法，进而继承了 `Animal` 的原型方法，最后继承了 `Object` 的原型方法（每一个对象实例继承的顶层对象均为 `Object` ）。

原型链继承的实现方式就是把 `Cat` 的原型指向一个 `new Animal()` ，于是所有的 `Cat` ( `cat1` 、`cat2` 、 `cat3` ...) 就可以调用 `Animal` 的原型方法了：

```javascript
function Animal() {
	this.species = '动物';
}
Animal.prototype.eat = function (food) {
	console.log(`I'm eating some ${food}.`);
}

function Cat(name, color) {
	this.name = name;
	this.color = color;
}
Cat.prototype = {
	master : 'Flint',
	sayHi : function () {
		console.log('喵喵喵~');
	}
};

Cat.prototype = new Animal();
Cat.prototype.constructor = Cat; //构造函数的纠正
var cat1 = new Cat('Tom', 'gray');
console.log(cat1.species); // 动物
cat1.eat('fish'); // I'm eating some fish.
```

关于构造函数的纠正概念参考了[阮一峰老师的文章](http://www.ruanyifeng.com/blog/2010/05/object-oriented_javascript_inheritance.html)。

原型链继承可以使父对象的原型方法与属性被子对象调用，但子对象构造函数无法向父对象构造函数传参。故实践中单纯的原型链模式很少使用。

#### 2.经典继承（构造函数绑定）

该方法将父对象的构造函数绑定在子对象上，相对于原型链继承有一个优势，即可以在子对象构造函数中向父对象构造函数传参：

```javascript
function Animal(species) {
	this.species = species;
}
Animal.prototype.eat = function (food) {
	console.log(`I'm eating some ${food}.`);
}

function Cat(name, color) {
	Animal.call(this, '猫科动物');
  	// Animal.apply(this, ['猫科动物']);
	this.name = name;
	this.color = color;
}
Cat.prototype = {
	master : 'Flint',
	sayHi : function () {
		console.log('喵喵喵~');
	}
};

function Dog(name, age) {
	Animal.call(this, '犬科动物');
  	// Animal.apply(this, ['猫科动物']);
	this.name = name;
	this.age = age;
}

var cat1 = new Cat('Tom', 'gray');
console.log(cat1.species); // 猫科动物

var dog1 = new Dog('Bey', 7);
console.log(dog1.species); // 犬科动物

cat1.sayHi(); // 喵喵喵~
cat1.eat('fish'); // Uncaught TypeError: cat1.eat is not a function
```

可以看到，猫与狗都使用了动物 `Animal` 的构造函数，并传入了不同的参数。然而这个经典继承的缺点也很明显，即子对象无法使用父对象的原型函数与原型属性，仅可使用构造函数

#### 3.组合继承

原型链继承可以让子对象使用父对象的原型函数与属性，但不能使用父对象的构造函数，即无法继承父对象的实例方法与属性。而经典继承则反之，可以继承父对象实例方法与属性，但无法继承父对象原型方法和属性。于是取二者之长就有了组合继承法：

```javascript
function Animal(species) {
	this.species = species;
}
Animal.prototype.eat = function (food) {
	console.log(`I'm eating some ${food}.`);
}

function Cat(name, color) {
	Animal.call(this, '猫科动物'); // 经典继承
	this.name = name;
	this.color = color;
}
Cat.prototype = {
	master : 'Flint',
	sayHi : function () {
		console.log('喵喵喵~');
	}
};

Cat.prototype = new Animal(); // 原型链继承
Cat.prototype.constructor = Cat;

var cat1 = new Cat('Tom', 'gray');
console.log(cat1.species); // 猫科动物
cat1.eat('fish'); // I'm eating some fish.
```

组合继承融合了二者优点，是JavaScript目前最为常用的继承模式。

然而组合继承也不是没有缺点，由上例可以看到，`Cat` 在继承 `Animal` 的时候，需要调用两次父对象（ `Animal` ）的构造函数（经典继承一次，原型链继承一次）。

#### 4.寄生组合式继承

寄生组合式继承源于原型式继承与寄生式（parasitic）继承，由JSON作者道格拉斯提出并推广，其基本模式如下：

```javascript
function inheritPrototype(subType, superType) {
	var prototype = Object(superType.prototype); // 创建对象
	prototype.constructor = subType; // 增强对象
	subType.prototype = prototype; // 指定对象
}
```

第一步是创建父对象原型的一个副本；

第二步是为创建的原型副本增添 `constructor` 方法，从而弥补因原型重写失去的 `constructor` ；

第三步是将新创建的对象（即父对象原型副本）赋给子对象原型。

> 关于 `construcor` ，举个例子：
>
> ```javascript
> function Tree(name) {
>    this.name = name;
> }
>
> var theTree = new Tree("Redwood");
> console.log( "theTree.constructor is " + theTree.constructor );
> ```
>
> 输出：
>
> ```javascript
> theTree.constructor is function Tree(name) {
>     this.name = name;
> }
> ```

这种继承方式的使用举例：

```javascript
function inheritPrototype(subType, superType) {
	var prototype = Object(superType.prototype); // 创建对象
	prototype.constructor = subType; // 增强对象
	subType.prototype = prototype; // 指定对象
}

function Animal(species) {
	this.species = species;
}
Animal.prototype.eat = function (food) {
	console.log(`I'm eating some ${food}.`);
}

function Cat(name, color) {
	Animal.call(this, '喵星人');
	this.name = name;
	this.color = color;
}
Cat.prototype = {
	master : 'Flint',
	sayHi : function () {
		console.log('喵喵喵~');
	}
};

inheritPrototype(Cat, Animal);
cat1 = new Cat('小仙女', '琥珀色');
cat1.eat('fish'); // I'm eating some fish.
console.log(cat1.species); // 喵星人

Cat.prototype.sayName = function () {
	console.log(this.name);
}

Animal.prototype.saySpecies = function () {
	console.log(this.species);
}

cat1.sayName(); // 小仙女
cat1.saySpecies(); // 喵星人
```

可以看到，这个例子的高效率体现在只调用了一次 `Animal` 构造函数，并且可以维持原型链保持不变，开发人员普遍认为寄生组合式继承是最理想的继承范式。

在`YUI` 库中这个继承范式是这样实现的：

```javascript
function extend(Child, Parent) {
	var F = function(){};
	F.prototype = Parent.prototype;
	Child.prototype = new F();
	Child.prototype.constructor = Child;
	Child.uber = Parent.prototype;
}
```

相较于之前的 `inheritPrototype` 方法，`extend` 利用了一个空函数来作为中介。并通过最后一行代码，在子对象上打开了一条通道，可以直接调用父对象的方法（这一行放在这里，只是为了实现继承的完备性，纯属备用性质）

#### 5.class继承

在ES6中新加入的 `class` 关键字使得JavaScript的继承更加简洁友好，不需要再编写复杂的原型链代码。

```javascript
class Animal {
	constructor(species) {
		this.species = species;
	}

	eat(food) {
		console.log(`I'm eating some ${food}.`);
	}
}

class Cat extends Animal{
	constructor(name, color) {
		super('喵星人');
		this.name = name;
		this.color = color;
	}

	sayHi() {
		console.log('喵喵喵');
	}
}

var cat1 = new Cat('大毛', '灰色');
cat1.eat('meat'); // I'm eating some meat.
console.log(cat1.species); // 喵星人
```



## 标准对象简介

### Date

`Date` 对象毫无疑问是用来表示日期（date）和时间（time）的。

`Date` 使用自UTC的1970年1月1日午夜0点开始经过的毫秒数来保存日期，可保存的日期时间范围为1970.1.1之前或之后的 `100 000 000` 年

方法和属性列表可查阅：

> [JavaScript Date 对象](http://www.w3school.com.cn/jsref/jsref_obj_date.asp)
>
> [MDN - Date](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Date)

简单使用举例：

```javascript
/* 获取当前时间 */
var now = new Date();
now; // Sat Mar 04 2017 19:29:52 GMT+0800 (中国标准时间) -- 不同浏览器显示效果有差异
Date.now(); // 1488627189112

/* 构造一个Date实例 */
// new Date(year, month[, day[, hour[, minutes[, seconds[, milliseconds]]]]]);
var ago = new Date(1644, 1, 1, 0, 0, 0);
ago; // Mon Feb 01 1644 00:00:00 GMT+0800 (中国标准时间)

// new Date(value);
var begin = new Date(0); // 这里传入的是毫秒数
begin; // Thu Jan 01 1970 08:00:00 GMT+0800 (中国标准时间)

// new Date(dateString);
// 1.
var war2 = new Date("8/15/1945");
war2; // Wed Aug 15 1945 00:00:00 GMT+0800 (中国标准时间)
// 2.
war2 = new Date("1945-08-15T09:00:00");
war2; // Wed Aug 15 1945 17:00:00 GMT+0800 (中国标准时间)
war2.getTimezoneOffset(); // -480, 即与UTC时间相差分钟数
war2 = new Date("1945-08-15T09:00:00+08:00");
war2; // Wed Aug 15 1945 09:00:00 GMT+0800 (中国标准时间)
// 3.
war2 = new Date("August 15, 1945 03:24:00");
war2; // Wed Aug 15 1945 03:24:00 GMT+0800 (中国标准时间)

/* 时间设置与读入 */
var birthday = new Date(94, 10, 19);
birthday; // Sat Nov 19 1994 00:00:00 GMT+0800 (中国标准时间)
birthday.getFullYear(); // 1994
birthday.getMonth(); // 10
birthday.getDate(); // 19
birthday.getDay(); // 6
// ...
birthday.setHours(15);
birthday; // Sat Nov 19 1994 15:00:00 GMT+0800 (中国标准时间)

/* 时间格式化输出 */
// 无format方法，请参考http://blog.csdn.net/vbangle/article/details/5643091
```



**注** ：有个务必要注意的坑，月份的取值范围是0~11，而星期和日期的取值范围却偏偏是1~7和1~28/29/30/31。因而，情人节是 `new Date(2017, 1, 14)` ，而不是 `new Date(2017, 2, 14)` ！！！

### RegExp

> JS正则表达式的学习推荐这个视频：[JavaScript正则表达式](http://www.imooc.com/learn/706)
>
> 正则表达式可视化：[Regexper](https://regexper.com/)



JavaScript 可以用类似 perl 的语法来创建一个正则表达式：

```javascript
var exp = / pattern / flags ;
```

也可以用创建 `RegExp` 对象的方式来构建一个正则表达式：

```javascript
var exp = new RegExp(pattern, flags);
```

#### 修饰符

`flags` 主要有如下几个，并且支持组合使用：

- `g` : 全局模式（global），将模式应用于所有字符串。若关闭则在发现第一个匹配项即停止；

- `i` : 忽略大小写模式（ignore）;

- `m` : 多行模式（multiline），开启后 `^ ` 和 `$` 可以匹配字符串中每一行的开始和结束（行是由 `\n` 或 `\r` 分割的），而不只是整个输入字符串的最开始和最末尾处。

  举个例子：

  ```javascript
  var str = `abc
  def
  ghi`;
  var pattern = /\w+$/;
  str.match(pattern); // ["ghi"]
  pattern = /\w+$/m;
  str.match(pattern); // ["abc"]
  pattern = /\w+$/gm;
  str.match(pattern); // ["abc", "def", "ghi"]
  ```

#### 元字符

|  元字符  |                    含义                    |
| :---: | :--------------------------------------: |
|  `.`  | 匹配除了换行符 （ `\n` `\r` `\u2028` 或 `\u2029` ）之外的任意**单个**字符 |
| `\d`  |          匹配任意一个数字字符，等价于 `[0-9]`          |
| `\w`  | 匹配任意来一个字母或数字字符，以及下划线。等价于 `[A-Za-z0-9_]`  |
| `\s`  |  匹配一个空白符，包括空格、制表符、换页符、换行符和其他 Unicode 空格  |
| ` \b` |  匹配单词边界，例如，/\bno/ 匹配 "at noon" 中的 "no"   |

以上元字符若改为大写则表示匹配一个不是该元字符的字符（例如 `\D` 表示一个非数字字符）。



#### 集合

- `[xyz]` : 表示一个字符集合。匹配集合中的任意一个字符。也可以使用连字符 `-` 指定一个范围，如 `[0-9]` 表示 `[0123456789]` ，`[a-d]` 表示 `[abcd]` 。
- `[^xyz]` : 表示一个字符集合的非集合。匹配不在集合中的任意一个字符。



#### 量词

|    量词    |            含义            |
| :------: | :----------------------: |
|   `n+`   | 匹配前面的模式一次或多次，等价于 `n{1,}` |
|   `n*`   | 匹配前面的模式零次或多次，等价于 `n{0,}` |
|   `n?`   |       匹配前面的模式零次或一次       |
|  `n{x}`  |      匹配前面的模式 `x` 次       |
| `n{x,}`  |     匹配前面的模式至少 `x` 次      |
| `n{x,y}` | 匹配前面的模式至少 `x` 次，至多 `y` 次 |
|  `?=n`   |   匹配一个紧接指定模式 `n` 的字符串    |
|   `n$`   |     匹配一个结尾为 `n` 的字符串     |
|   `^n`   |     匹配一个开头为 `n` 的字符串     |

#### 分组

用 `()` 可以达到分组的功能，使量词作用于分组，而不是只是紧跟的单个字符。

```javascript
'a1b2c3d4'.match(/[a-z]\d{3}/g); // null
'a1b2c3d4'.match(/([a-z]\d){3}/g); // ["a1b2c3"]
```

分组还可以用于反向引用，这是正则表达式中很重要的一个概念。

所谓反向引用，即以变量的形式来替换源字符串中的匹配项。

看个例子：

> 将“年-月-日”的日期形式替换为“月/日/年”的形式
>
> 如：2017-03-04 ==> 03/04/2017

利用反向引用来实现替换：

```javascript
var pattern = /(\d{4})-(\d{2})-(\d{2})/g;
"2017-03-04".replace(pattern, '$2/$3/$1'); // "03/04/2017"
"1994-11-19".replace(pattern, '$2/$3/$1'); // "11/19/1994"
// x-x-x ==> x年x月x日
"2008-08-08".replace(pattern, '$1年$2月$3日'); // "2008年08月08日"
```



#### RegExp 的方法

- `test()` : 检测一个字符串是否匹配某个模式。如果字符串中有匹配项返回 true ，否则返回 false。不过需要注意的一点是一直向前匹配，对于同一个RegExp对象执行多次，其 `lastIndex` 属性，即上一次匹配到的最后索引（非全局模式不生效）可能会一直更新，故执行 `test()` 方法的RegExp对象最好不要开启全局模式g。

  ```javascript
  var pattern = /\w/g;
  pattern.test('ab'); // true
  pattern.test('ab'); // true
  pattern.test('ab'); // false

  while (pattern.test('NBA')) {
      console.log('last index is ' + pattern.lastIndex);
  }
  // last index is 1
  // last index is 2
  // last index is 3
  ```

- `exec()` : 用于检索字符串中的正则表达式的匹配。返回一个数组，其中存放匹配的结果数组，数组第一项是匹配项，后续项是匹配项中的分组。如果未找到匹配，则返回值为 `null`。

  ```javascript
  var patt1 = /\w(\d)(\w)/;
  var patt2 = /\w(\d)(\w)/g;
  var str = 'a1Ab2Bc3Cd4De5E';

  patt1.exec(str); // ["a1A", "1", "A"]
  patt1.exec(str); // ["a1A", "1", "A"]
  patt1.exec(str); // ["a1A", "1", "A"]

  patt2.exec(str); // ["a1A", "1", "A"]
  patt2.exec(str); // ["b2B", "2", "B"]
  patt2.exec(str); // ["c3C", "3", "C"]
  patt2.exec(str); // ["d4D", "4", "D"]
  patt2.exec(str); // ["e5E", "5", "E"]
  patt2.exec(str); // null
  patt2.exec(str); // ["a1A", "1", "A"]
  ```

#### 支持正则表达式的String方法

- `search()`
- `match()`
- `replace()`
- `split()`

### Math

直接查阅 [MDN - Math](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Math) 即可。

这里贴一点JavaScript生成随机数的办法：

```javascript
// 返回一个大于等于0，小于1的伪随机数
function getRandom() {
    return Math.random();
}

// 返回一个介于min和max之间的随机数
function getRandomArbitrary(min, max) {
    return Math.random() * (max - min) + min;
}

// 返回一个介于min和max之间的整型随机数
function getRandomInt(min, max) {
    return Math.floor(Math.random() * (max - min + 1) + min);
}
```



### JSON

方法就两个：

- [`JSON.parse()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/parse) : 解析JSON字符串, 可以选择改变前面解析后的值及其属性，然后返回解析的值。


- [`JSON.stringify()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) : 返回指定值的 JSON 字符串，可以自定义只包含某些特定的属性或替换属性值。

后续用到再写吧。。