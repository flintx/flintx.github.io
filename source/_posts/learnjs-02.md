---
title: JavaScript学习笔记02（Map与Set、函数）
date: 2017-02-24 22:04:13
tags:
	- JavaScript
	- 前端
categories: JavaScript学习
---

> 参考 & 学习：
>
> [廖老师的JavaScript教程](http://www.liaoxuefeng.com/wiki/001434446689867b27157e896e74d51a89c25cc8b43bdb3000)
> [JavaScript 参考文档](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference)
> [W3School JavaScript 教程](http://www.w3school.com.cn/js/index.asp)
>
> 由衷感谢这些资料的提供者与撰写者！

<!-- more -->

## Map 与 Set

### Map

> JavaScript的默认对象表示方式`{}`可以视为其他语言中的`Map`或`Dictionary`的数据结构，即一组键值对。
>
> 但是JavaScript的对象有个小问题，就是键必须是字符串。但实际上Number或者其他数据类型作为键也是非常合理的。
>
> 为了解决这个问题，最新的ES6规范引入了新的数据类型`Map`。

`Map`实例的主要属性包括`size`一个，而不是`length`

```javascript
var m = new Map([['No.1', 'Alice'], ['No.2', 'Bob'], ['No.3', 'Candy']]);
console.log(m); // Map {"No.1" => "Alice", "No.2" => "Bob", "No.3" => "Candy"}
console.log(m.size); // 3
```

`Map`实例的主要方法有：

> [`Map.prototype.clear()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/clear)
>
> 移除`Map`对象的所有键/值对 。
>
> [`Map.prototype.delete(key)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/delete)
>
> 移除任何与键相关联的值，并且返回该值，该值在之前会被`Map.prototype.has(key)`返回为`true`。之后再调用`Map.prototype.has(key)`会返回`false`。
>
> [`Map.prototype.entries()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/entries)
>
> 返回一个新的 `Iterator` 对象，它按插入顺序包含了`Map`对象中每个元素的 **[key, value]** **数组**。
>
> [`Map.prototype.forEach(callbackFn, thisArg)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/forEach)
>
> 按插入顺序，为 `Map`对象里的每一键值对调用一次`callbackFn`函数。如果为`forEach`提供了`thisArg`，它将在每次回调中作为`this`值。
>
> [`Map.prototype.get(key)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/get)
>
> 返回键对应的值，如果不存在，则返回`undefined`。
>
> [`Map.prototype.has(key)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/has)
>
> 返回一个布尔值，表示`Map`实例是否包含键对应的值。
>
> [`Map.prototype.keys()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/keys)
>
> 返回一个新的 `Iterator`对象， 它按插入顺序包含了Map对象中每个元素的**键 **。
>
> [`Map.prototype.set(key, value)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/set)
>
> 设置`Map`对象中键的值。返回该`Map`对象。
>
> [`Map.prototype.values()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Map/values)
>
> 返回一个新的`Iterator`对象，它按插入顺序包含了`Map`对象中每个元素的**值** 。

```javascript
var m = new Map();

var keyNum = 1024,
	keyStr = 'This is a string.',
	keyObj = {},
	keyFun = function () {};

// 添加键key
m.set(keyNum, '数值键1024对应的值');
m.set(keyStr, "字符串键'This is a string.'对应的值");
m.set(keyObj, '对象键keyObj对应的值');
m.set(keyFun, '函数键keyFun对应的值');

console.log(m); 
// Map {
// 	1024 => "数值键1024对应的值", 
// 	"This is a string." => "字符串键'This is a string.'对应的值", 
// 	Object {} => "对象键keyObj对应的值", 
// 	function => "函数键keyFun对应的值"
// }
console.log(m.size);
// 4

// 读取值value
console.log(m.get(keyNum)); // 数值键1024对应的值
console.log(m.get(keyStr)); // 字符串键'This is a string.'串对应的值
console.log(m.get(keyObj)); // 对象键keyObj对应的值
console.log(m.get(keyFun)); // 函数键keyFun对应的值

console.log(m.get(1024)); // 数值键1024对应的值
console.log(m.get('This is a string.')); // 字符串键'This is a string.'串对应的值
console.log(m.get({})); // undefined, 因为keyObj !== {}
console.log(m.get(function () {})); // undefined, 因为keyFunc !== function () {}

// 通过Array来构造Map
var arr = [["key1", "value1"], ["key2", "value2"]];
var aMap = new Map(arr);
console.log(aMap.get('key1')) // 'value1';

// 一些迭代技巧
var myMap = new Map();
myMap.set(0, "zero");
myMap.set(1, "one");
for (var [key, value] of myMap) {
  	console.log(key + " = " + value);
}
// 将会显示两个log。一个是"0 = zero"另一个是"1 = one"
for (var key of myMap.keys()) {
  	console.log(key);
}
// 将会显示两个log。 一个是 "0" 另一个是 "1"
// keys()方法返回一个可迭代对象Iterator，包含了Map对象中每个元素的键
for (var value of myMap.values()) {
  	console.log(value);
}
// 将会显示两个log。 一个是 "zero" 另一个是 "one"
// values()方法返回一个可迭代对象Iterator，包含了Map对象中每个元素的值
for (var [key, value] of myMap.entries()) {
  	console.log(key + " = " + value);
}
// 将会显示两个log。 一个是 "0 = zero" 另一个是 "1 = one"
// entries()方法以键值对形式返回一个可迭代对象Iterator

// 移除
myMap.delete(1);
console.log(myMap); // Map {0 => "zero"}

// 清空
myMap.clear();
console.log(myMap); // Map {}

// 判断是否存在某个键
myMap.set(0, "zero");
myMap.set(1, "one");
console.log(myMap.has(0)); // true
console.log(myMap.has(2)); // false
```



### Set

> `Set`对象是值的集合，你可以按照插入的顺序迭代它的元素。 `Set`中的元素只会出现一次，即 `Set` 中的元素是唯一的。

`set`对象中元素是唯一的，因而需要判断两个元素是否相等。判断相等的算法略不同于`===`运算符，其中`+0`与`-0`是被视为不同的元素，尽管`+0 === -0`为`true`（`ECMAScript6`中这点已修改，`+0`与`-0`已被视为相同元素）。除此之外，`NaN`与`NaN`被视为相同元素，尽管`NaN !== NaN`为`true`。

```javascript
var s = new Set();
s.add(+0);
console.log(s); // Set {0}
s.add(-0);
console.log(s); // Set {0}   Chrome 56.0+
s.add(NaN);
console.log(s); // Set {0, NaN}
s.add(NaN);
console.log(s); // Set {0, NaN}
```

同`Map`实例一样，`set`实例的主要属性包括`size`一个，而不是`length`.

```javascript
var aSet = new Set([1, 2, 3, 3, 1.1, 1.10, undefined, undefined, NaN, NaN, '3']); // 通过Array构造一个Set
console.log(aSet); // Set {1, 2, 3, 1.1, undefined, NaN, '3'}
console.log(aSet.size); // 7
```

`Set`实例包括如下主要方法：

> [`Set.prototype.add(value)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set/add)
>
> 在`Set`对象尾部添加一个元素。返回该`Set`对象。
>
> [`Set.prototype.clear()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set/clear)
>
> 移除`Set`对象内的所有元素。
>
> [`Set.prototype.delete(value)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set/delete)
>
> 移除`Set`的中与这个值相等的元素，返回`Set.prototype.has(value)`在这个操作前会返回的值（即如果该元素存在，返回`true`，否则返回`false`）。`Set.prototype.has(value)`在此后会返回`false`
>
> [`Set.prototype.entries()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set/entries)
>
> 返回一个新的迭代器对象，该对象包含`Set`对象中的**按插入顺序排列的**所有元素的值的[value, value]数组。为了使这个方法和`Map`对象保持相似，不过每个值的键和值相等。
>
> [`Set.prototype.forEach(callbackFn, thisArg)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set/forEach)
>
> 按照插入顺序，为`Set`对象中的每一个值调用一次`callBackFn`。如果提供了`thisArg`参数，回调中的`this`会是这个参数。
>
> [`Set.prototype.has(value)`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set/has)
>
> 返回一个布尔值，表示该值在`Set`中存在与否。
>
> [`Set.prototype.keys()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set/keys)
>
> 与`values()`方法相同，返回一个新的迭代器对象，该对象包含`Set`对象中的**按插入顺序排列的**所有元素的值。
>
> [`Set.prototype.values()`](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Global_Objects/Set/values)
>
> 返回一个新的迭代器对象，该对象包含`Set`对象中的**按插入顺序排列的**所有元素的值。

```javascript
// 使用Set对象
var mySet = new Set();
var undefinedVar;
mySet.add(1);
mySet.add(true);
mySet.add('a string.');
mySet.add(undefinedVar);
mySet.add(5);
console.log(mySet); // Set {1, true, "a string.", undefined, 5}

// 判断Set中是否存在某元素
mySet.has(Math.sqrt(25)); // true
mySet.has('A string.'.toLowerCase()); // true
mySet.has(undefined); // true
mySet.has(0.2 + 0.1 === 0.3); // false, 表达式结果为false, mySet里没有false元素，故返回false

// 删除Set中的元素
mySet.delete(5);
mySet.has(5); // false

// 遍历Set中的元素
// for .. of 方式
for (let item of mySet) {
	console.log(item);
}
// 1, true, 'a string.', undefined

for (let item of mySet.keys()) {
	console.log(item);
}
// 1, true, 'a string.', undefined

for (let item of mySet.values()) {
	console.log(item);
}
// 1, true, 'a string.', undefined

for (let [key, value] of mySet.entries()) {
	console.log(`${key} ==> ${value}`);
}
// 1 ==> 1, true == > true, 'a string.' ==> 'a string.', undefined ==> undefined
// 键与值相等

// forEach() 方法
mySet.forEach(function (value) {
	console.log(value);
});
// 1, true, 'a string.', undefined
```



## JavaScript中的函数

### 函数的定义方式

- 方式一（面向过程/函数声明风格）

  ```javascript
  function sum(a, b) {
  	return a + b; 
  }
  ```

  ​

- 方式二（函数表达式风格）

  ```javascript
  var sum = function (a, b) {
  	return a + b; 
  };
  ```

  ​

- 方式三（Function构造器，不推荐）

  ```javascript
  var sum = new Function("a", "b", "return a + b;");
  ```

- 此外还有函数生成器、箭头函数表达式等函数定义方式，属于JS较新特性或高级函数技巧，后续再加以学习。

### arguments关键字与rest参数

调用函数时，JavaScript允许传入任意个参数而不影响调用，即使传入参数多于或少于定义参数也没有问题（多余参数会被忽略）。

```javascript
Math.max(); // -Infinity
Math.max(1); // 1
Math.max(1, 2); // 2
Math.max(1, 2, 3); // 3
Math.max([1, 2, 3]); // NaN
Math.max(1, 2, 3, undefined, NaN, 'a string'); // NaN
```

除了任意传参之外，JavaScript还提供了与这个特性配套的`arguments`关键字对象与`rest`参数。

- `arguments` 只在函数内部起作用，通过 `arguments` 可获得调用者传入函数的所有参数。

  `arguments` 与 `Array` 比较相似，但不等于 `Array` ，只具有 `length` 属性，但可以通过一些方式转换为 `Array`。

  ```javascript
  function mySum() {
  	var res = 0;
  	for (var i = 0; i < arguments.length; i++) {
  		res += arguments[i];
  	}
  	return res;
  }

  mySum(); // 0
  mySum(1); // 1
  mySum(1, 2, 3); // 6
  mySum(' is ', 'zero.'); // "0 is zero."
  mySum(Infinity, -Infinity); // NaN

  // 通过arguments连接多个字符串
  function myConcat(separator) {
    	var args = Array.prototype.slice.call(arguments, 1);
  	return args.join(separator);
  }

  myConcat(", ", "red", "orange", "blue"); // "red, orange, blue"
  myConcat("; ", "elephant", "giraffe", "lion", "cheetah"); // "elephant; giraffe; lion; cheetah"

  // 将arguments转化为Array
  // 通过 from() 方法
  var args = Array.from(arguments);
  // 通过spread运算符
  var args = [...arguments];
  ```

  `arguments` 值永远与对应命名参数的值保持同步：

  ```javascript
  function add(num1, num2) {
  	arguments[1] = 10;
  	console.log(num1 + num2);
  }

  add(1, 2); // 11
  ```

  ​

- `rest` 参数在ES6标准中被引入，若传入参数多于定义参数，多余的参数将以数组形式交给`rest`，若少于或等于，则 `rest` 为空数组.

  ```javascript
  function fun1(a, b, ...rest) {
  	console.log(a);
  	console.log(b);
  	console.log(rest);
  }

  fun1(1, 2, 3, 4, 5);
  // 1
  // 2
  // [3, 4, 5]
  fun1(1, 2);
  // 1
  // 2
  // []
  fun1(1);
  // 1
  // undefined
  // []

  // 利用rest，将传入参数返回为一个排序后的数组
  function sortRestArgs(...theArgs) {
  	var sortedArgs = theArgs.sort();
  	console.log(sortedArgs);
  }

  sortRestArgs(3, 5, 7, 1); // [1, 3, 5, 7]
  ```

### 作用域问题

- 一般变量的作用域为整个函数体，在函数体外不可引用该变量，并且不同函数内部的同名变量互相独立，互不影响；

  ```javascript
  'use strict';
  function myFun() {
  	var x = 1;
  	x += 2;
  }

  x += 2; // ReferenceError: x is not defined
  ```

- 全局变量/函数均绑定在 `window` 对象上；

- 内部函数可以访问外部函数定义的变量。若内部函数和外部函数的变量名重名，则使用内部函数变量（**由内向外**原则）；

  ```javascript
  function outFun() {
  	var aVar = 100;
  	function inFun() {
  		var aVar = 200;
  		console.log(aVar);
  	}
  	inFun();
  	console.log(aVar);
  }

  outFun(); // 200 100
  ```

  ​

- 变量提升，JS的一个坑。。JavaScript会把整个函数体内部的所有变量提升到顶部进行声明，即以下代码：

  ```javascript
  function myFun() {
  	var x = 10;
  	x += 10;
  	var y = 7;
  	y += 20;
  	return x + y;
  }

  // 以上代码等效于

  function myFun() {
  	var y;
  	var x = 10;
  	x += 10;
  	y = 7;
  	y += 20;
  	return x + y;
  }
  ```

  这就会导致一个变量可能在声明前就已被使用，但程序可以照常运行，并不符合使用前必须声明的语言规则。因而最好严格遵守 **”在函数内部首先申明所有变量”** 这一规则。

  ```javascript
  // 这段代码由于变量提升的特性可以正常运行，但这是不符合规则的bad code
  function myFun() {
    	// var y; y的定义被提升到了此处
  	var x = 10;
  	x += 10;
  	y = 7; // 此时按照代码文本，y尚未定义；然而实际上y的定义被提升到了开头处
  	var y;
  	y += 20;
  	return x + y;
  }
  myFun(); // 47
  ```

  ​

- 若一个变量的作用范围仅为类似 `for` 、`while` 循环内部的局部范围，则此作用范围称之为 `局部作用域` / `块级作用域` ，然而 `var` 定义的变量范围为函数内部，于局部范围外依然可以引用：

  ```javascript
  function myFun() {
  	for (var i = 0; i < 10; i++) {
  		;
  	}
  	console.log(i); // 循环外部依然可用
  }
  // 以上代码等价为:
  function myFun() {
    	var i;
  	for (i = 0; i < 10; i++) {
  		;
  	}
  	console.log(i);
  }
  myFun(); // 10
  ```

  为了解决这个问题，ES6引入了 `let` 关键字来用以声明只具备块级作用域的局部变量：

  ```javascript
  function myFun() {
  	for (let i = 0; i < 10; i++) {
  		;
  	}
  	console.log(i); // i 已不可使用
  }
  myFun(); // ReferenceError: i is not defined
  ```

  故在新标准下，局部变量应使用 `let` 来声明。

- 与 `let` 关键字一起，ES6标准还引入了用于声明 **常量** 的 `const` 关键字，在此之前，常量只能通过全字母大写来人为定义，并不能限制程序对于一个常量的修改行为。`const` 定义的变量不可以修改，而且必须初始化。

  ```javascript
  // 过去
  var PI = 3.1415926;
  PI += 1.0; // OK
  // 现在
  const PI = 3.1415926;
  PI += 1.0; // TypeError: Assignment to constant variable.
  ```

  **注** ：`const` 定义的常量不可用 `delete` 删除。

### this关键字

`this` 关键字是什么？

答：即编程语言中的“我”的概念。

```javascript
var Flint = {
  	age : 22,
  	sayThis : function () {
      	console.log(this);
  	}
};
Flint.sayThis(); // Object {age: 22}
```

 `this` 指向 `Flint` ，即调用 `sayThis` 方法的“我”。

鉴于这又是JS的一个大坑（孔乙己：JavaScript ES6的函数有六种调用方式你知道吗？）更多的概念自己也没完全理解，感觉可以另写一篇文章来说明了，于是扔链接跑路=_=

关于对 `this` 以及通过 `apply()`，`call()` 方法来调用函数的更深入理解还是查看以下文档吧：

> [MDN - this 关键字](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Operators/this)
>
> [this 的值到底是什么？一次说清楚](https://zhuanlan.zhihu.com/p/23804247)
>
> [JS中this关键字详解](http://www.cnblogs.com/lisha-better/p/5684844.html)

**ps.** 听说记住这三点就不会被坑了：

> 1. 当函数作为对象的方法调用时，`this` 就是该对象;
> 2. 当函数作为单纯函数调用时，严格模式下，`this` 是 `undefined` ，非严格模式下是全局对象，浏览器中就是 `window` ;
> 3. `this` 不是变量，嵌套函数中的`this` 不会从外层继承 `this` 值！

附：利用 `apply()` 统计方法调用次数（函数装饰器的简单使用）。

```javascript
var count = 0;
var oldParseInt = parseInt; // 保存原函数

window.parseInt = function () {
    count += 1;
    return oldParseInt.apply(null, arguments); // 调用原函数
};

// 测试:
parseInt('10');
parseInt('20');
parseInt('30');
count; // 3
```



### 高阶函数

高阶函数又称算子或泛函，在计算机科学被定义为中至少满足下列一个条件的函数：

> 1. 接受一个或多个函数作为输入；
> 2. 输出一个函数。

一个简单的高阶函数例子：

```javascript
function sum(a, b, fun) {
	return fun(a) + fun(b);
}

sum(1, -1, Math.abs); // 2

function addOne(x) {
	return x + 1;
}

sum(0, 0, addOne); // 2
```

#### map/reduce

- 在JavaScript中，`Array.prototype.map()` 方法返回一个由原 `Array` 中的每个元素调用指定方法后的返回值组成的新 `Array`。

  ```javascript
  // 计算Array中每个元素的平方根
  var numbers = [1, 4, 9, 16];
  var roots = numbers.map(Math.sqrt); 
  roots; // [1, 2, 3, 4]

  // 打印字符串中每个字符的ASCII码值
  var map = Array.prototype.map;
  var arr = map.call("Hello World", function(c) {
  	return c.charCodeAt(0);
  });
  arr; // [72, 101, 108, 108, 111, 32, 87, 111, 114, 108, 100]

  // 反转字符串
  var str = '12345';
  Array.prototype.map.call(str, function(c) {
    return c;
  }).reverse().join(''); // "54321"

  // 将不规范英文名变为首字母大写其余字母小写的形式
  function normalize(arr) {
  	function f(s) {
  		return s.toLowerCase()[0].toUpperCase() + s.slice(1, s.length).toLowerCase();
  	}
  	return arr.map(f);
  }
  normalize(['adam', 'LISA', 'barT']); // ["Adam", "Lisa", "Bart"]

  // 传递函数有多个参数时，需要通过自定义用户函数来作为传递函数
  ["1", "2", "3"].map(parseInt); // [1, NaN, NaN], 原因是parseInt需要还需要第二个不可忽略参数：进制
  // 解决办法
  function myParseInt(x) {
  	return parseInt(x, 10);
  }
  ["1", "2", "3"].map(myParseInt); // [1, 2, 3]
  ```

  ​

- `Array.prototype.reduce()` 按从左到右的顺序把函数结果继续和序列的下一个元素按指定方法做累积计算。

  ```javascript
  // [x1, x2, x3, x4].reduce(f) ==> f(f(f(x1, x2), x3), x4)
  [1, 2, 3, 4].reduce(function (a, b) {
  	return a + b;
  }); // 10
  ```

  直觉上 `reduce()` 只需传入一个参数，即一个传入两个参数返回一个参数的 `callback` 函数.

  实际上该 `callback` 函数拥有4个参数：`accumulator`, `currentValue`, `currentIndex`, `array` ；而除了 `callback`

  函数这个参数外， `reduce()` 本身还有一个叫 `initialValue` 的可选参数：

  ```javascript
  var initialValue = 'alpha: ';
  ['A', 'B', 'C', 'D'].reduce(function (accumulator, currentValue, currentIndex, array) {
  	return accumulator + currentValue;
  }, initialValue); // "alpha: ABCD"
  ```

|  call  | `accumulator`  | `currentValue` | `currentIndex` |        `array`| `return value`  |
| :----: | :------------: | :------------: | :------------: | :--------------------: | :-------------: |
| first  |  `'alpha: '`   |     `'A'`      |      `1`       | `['A', 'B', 'C', 'D']` |  `'alpha: A'`   |
| second |  `'alpha: A'`  |     `'B'`      |      `2`       | `['A', 'B', 'C', 'D']` |  `'alpha: AB'`  |
| third  | `'alpha: AB'`  |     `'C'`      |      `3`       | `['A', 'B', 'C', 'D']` | `'alpha: ABC'`  |
| fourth | `'alpha: ABC'` |      `'D'`       |      `4`       | `['A', 'B', 'C', 'D']` | `'alpha: ABCD'` |

  一些 `reduce()` 的简单应用：

  ```javascript
  // 数组扁平化
  [[0, 1], [2, 3], [4, 5]].reduce( function(a, b) {
      return a.concat(b);
  }); // [0, 1, 2, 3, 4, 5]

  // 统计Array中各个值出现次数
  var colors = ['blue', 'red', 'orangr', 'green', 'red', 'red', 'blue'];
  var countedColors = colors.reduce(function (allColors, color) {
  	if (color in allColors) {
  		allColors[color]++;
  	} 
  	else {
  		allColors[color] = 1;
  	}
  	return allColors;
  }, {});
  countedColors; // Object {blue: 2, red: 3, orangr: 1, green: 1}
  ```

#### filter

`filter()` 方法使用指定的函数测试所有元素，并创建一个包含所有通过测试的元素的新数组。

```javascript
// 将Array中的小写字母筛去
function isUpper(c) {
	return c.charCodeAt(0) >= 'A'.charCodeAt(0) && c.charCodeAt(0) <= 'Z'.charCodeAt(0);
}

['A', 'b', 'c', 'D', 'e', 'F'].filter(isUpper); // ["A", "D", "F"]

// 将Array中重复元素删去
var elements = [3.14, 'pi', 'e', 2.718281828, 'e', 'e', 3.14, 3.1415926];
elements.filter(function (element, index, self) {
	return self.indexOf(element) === index;
}); // [3.14, "pi", "e", 2.718281828, 3.1415926]

// 筛素数 1 ~ 100
var num = [...Array(100).keys()];
num.filter(function (x) {
	for (let i = 2; i <= Math.sqrt(x + 0.5); i++) {
		if (x % i === 0) {
			return false;
		}
	}
	return x !== 1 && x !== 0;
});
// [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37, 41, 43, 47, 53, 59, 61, 67, 71, 73, 79, 83, 89, 97]
```



#### sort

和很多语言不太一样，`sort()` 方法默认排序顺序是先将数组元素转化为字符串，再根据字符串Unicode码值来排序（即字典序）。

因为这个默认的排序规则，新手使用 `sort()` 方法常常会遇到一些可怕的事：

```javascript
[4, 30, 2, 10, 9].sort(); // [10, 2, 30, 4, 9] ！！！
// 数值排序在欢声笑语中打出了GG
```

好在 `sort()` 方法有着高阶函数的尊严，可以自定义排序规则：

```javascript
// 数值升序排序
[4, 30, 2, 10, 9].sort(function (a, b) {
	return a > b;
}); // [2, 4, 9, 10, 30]

// 数值降序排序
[4, 30, 2, 10, 9].sort(function (a, b) {
	return b - a;
}); // [30, 10, 9, 4, 2]

// 忽略字母大小写的字典序排序
var companys = ['Apple', 'Facebook', 'Microsoft', 'cisio', 'amazon', 'Tecent', 'baidu', 'Alibaba', 'Google'];
companys.sort(function (a, b) {
	var s1 = a.toLowerCase();
	var s2 = b.toLowerCase();
	return s1 > s2;
});
// ["Alibaba", "amazon", "Apple", "baidu", "cisio", "Facebook", "Google", "Microsoft", "Tecent"]

// 按字符串长度排序，若相等则按字典序排序
var companys = ['Apple', 'Facebook', 'Microsoft', 'cisio', 'amazon', 'Tecent', 'baidu', 'Alibaba', 'Google'];
companys.sort(function (a, b) {
	var len1 = a.length;
	var len2 = b.length;
	return len1 > len2 || (len1 === len2 && a > b);
});
// ["Apple", "baidu", "cisio", "Google", "Tecent", "amazon", "Alibaba", "Facebook", "Microsoft"]
```



### 闭包（Closure）

一个较为直观的定义：

> 闭包就是能够读取其他函数内部变量的函数

感觉智商被掏空。。还需要多学习多学习：

> [MDN - 闭包](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Closures)
>
> [学习Javascript闭包（Closure） - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2009/08/learning_javascript_closures.html)

闭包多用来模拟其他编程语言的 `private` 属性：

```javascript
var Counter = (function() {
var privateCounter = 0;
    function changeBy(val) { // 实际上changeBy()方法成为私有方法
        privateCounter += val;
    }
    return {
        increment: function() {
            changeBy(1);
        },
        decrement: function() {
            changeBy(-1);
        },
        value: function() {
            return privateCounter;
        }
    }   
})();

console.log(Counter.value()); // 0
Counter.increment();
Counter.increment();
console.log(Counter.value()); // 2
Counter.decrement();
console.log(Counter.value()); // 1
```



### Arrow Function

虽然ES6才有的这货长得很像 `lambda` 函数表达式，但实际上JavaScript本来就是一门大量借鉴了函数式编程思想语言，比起其他语言半残废的 `lambda` ，JS很早就支持类似概念的匿名函数了。

具体使用方法：

```javascript
(param1, param2, …, paramN) => expression
// or
(param1, param2, …, paramN) => { statements }
```

```javascript
var add = (x, y) => x + y;
add(1, 2); // 3

var pow = x => x * x;
pow(2.5); // 6.25

var sayHi = () => {console.log('Hi!');}
sayHi(); // "Hi"
```

更多特性与使用技巧参考：[MDN - Arrow Function](https://developer.mozilla.org/zh-CN/docs/Web/JavaScript/Reference/Functions/Arrow_functions)



### yield 关键字 与 generator 对象

这个概念之前学Python时接触过。。懒得填这个坑了，将来接触 `function*` 和 `AJAX` 再单独写文章说明吧。。

简单说调用 `generator` 的优势在于不立即执行并保存上次执行状态。


