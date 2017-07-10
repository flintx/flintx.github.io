---
title: JavaScript学习笔记01（基本语法、数据类型、运算符）
date: 2017-02-23 12:40:58
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

## 基本语法 & 数据类型

### 变量名

- 变量名是大小写英文、数字、`$`和`_`的组合，且不能用数字开头，使用var进行基本赋值：`var x = 1;`

### 字符串（String）

-  字符串（String）类型可以使用单引号`'`或双引号`"`来指示文本范围，并且可以在单引号中使用双引号，或是双引号中使用单引号。

   ```javascript
    console.log("My name is '233', and you?"); // My name is '233', and you?
    console.log('Nice to meet you, "666"'); // Nice to meet you, "666"
   ```

-  如果需要存储多行字符串，可使用``来表示.

-  多个字符串的链接可以使用`+`操作符，或采用ES6新增的模板字符串.

   ```javascript
        var s = `Esl
        Psy
        Congroo`;
        console.log(s);
        /*
   * Esl
   * Psy
   * Congroo
   */

   var name = 'Flint';
   var age = '22';
   var s1 = 'My name is ' + name + ' and I\'m ' + age + ' years old.';
   console.log(s1); // My name is Flint and I'm 22 years old.
   var s2 = `My name is ${name} and I\'m ${age} years old.`;
   console.log(s2); // My name is Flint and I'm 22 years old.
   s1 === s2; // true
   ```
-  **注意**：模板字符串仅可用于``包括的字符串.

-  **特别注意**：字符串是不可变的，String 类定义的方法都不能改变字符串的内容。虽然对字符串的某个索引赋值不会有错误提示，但是也没有任何效果。诸如toUpperCase() 方法，返回的是全新的字符串，而不是修改原始字符串。

-  关于字符串的各种操作，今后使用中再进行整理归纳。

       详细资料：[JavaScript String 对象属性与方法](http://www.w3school.com.cn/jsref/jsref_obj_string.asp)

### 数值（Number）

- 基本类型包括：

  ```javascript
  123; // 整数123
  0.456; // 浮点数0.456
  6.66666e4; // 科学计数法表示的6.66666x10^4，等同于66666.6
  -789; // 负数
  NaN; // NaN表示Not a Number，当无法计算结果时用NaN表示
  Infinity; // Infinity表示无限大，当数值超过了最大值时，就表示为Infinity

  /*基本运算*/
  1 + 1; // 2
  4 - 5.1; // -1.0999999999999996
  3 / 2 // 1.5, JS中没有整除运算，需要整除运算可使用Math.floor或Math.ceil
  5 % 3 // 2, 取余数
  7.5 % 1 // 0.5, 浮点数亦可
  2 / 0; // Infinity
  0 / 0; // NaN
  ```

  ​

- 值得注意的是，JS的浮点数运算有一个固有bug，即精度误差：`0.1 + 0.2 == 0.3` 返回结果为`false`，利用`console.log(0.2 + 0.1)`，得到结果`0.30000000000000004`。这是由于二进制运算机制造成的（部分小数无法用有限位二进制表示，比如`0.1` `0.33` 等），几乎所有的编程语言都会有类似精度误差的问题，包括C/C++/Java/Python等。解决此类问题可以参考[通过isEqual工具方法判断数值是否相等](http://madscript.com/javascript/javscript-float-number-compute-problem/)，或者给出明确的精度要求：

  ```javascript
  // 利用fix
  0.1 + 0.2 === 0.3 // false
  parseFloat((0.1 + 0.2).toFixed(2)) === 0.3; // true

  // 利用abs
  1 / 3 === (1 - 2 / 3); // false
  Math.abs(1 / 3 - (1 - 2 / 3)) < 0.0000001; // true
  Math.abs(1 / 3 - (1 - 2 / 3)) < 1e-7; // true
  ```

  ​

- 此外，JS中所有数字均为**64**位。

  详细资料：[JavaScript Number 对象属性与方法](http://www.w3school.com.cn/jsref/jsref_obj_number.asp)

### 布尔（Boolean）

- 布尔（Boolean）类型只有`true`、`false`两种值，以及与`&&`或`||`非`!`三种运算。

  详细资料：[JavaScript Boolean 对象属性与方法](http://www.w3school.com.cn/jsref/jsref_obj_boolean.asp)

### “空”（undefined、null）

- JS中有两种表示"空"的类型，即`null`与`undefined`，类似于C/C++的`NULL`或Python的`None`。其中`null`表示一个空的值，而`undefined`表示值未定义。
- `null`不同于`0`以及`''`，`0`是数值0，`''`表示长度为0的字符串

### 数组（Array）

- 数组（Array）类型与Python的list比较相似，可以存储不同数据类型的元素，并能直接用[]操作符或`new Array()`方法来构造一个数组。

  ```javascript
  var array = ['dog', '利', '郭嘉', 'live' === 'die', 1];
  console.log(array); // ["dog", "利", "郭嘉", false, 1]
  console.log(array.length); // 5
  var arr = new Array(array, 'Not because of bad fortune to avoid the trend', 'haha');
  console.log(arr); // [Array[5], "Not because of bad fortune to avoid the trend", "haha"]
  console.log(arr.length); // 3
  arr[0]; // ["dog", "利", "郭嘉", false, 1]
  arr[1]; // "Not because of bad fortune to avoid the trend"
  arr[3]; // undefined, 超出索引范围返回undefined
  ```

- Array 的 `length` 属性是可读写的，可以通过设置这个属性来移除数组尾部元素与增添新的数组元素。

  ```javascript
  var colors = ['blue', 'red', 'green'];
  console.log(colors.length); // 3
  colors.length = 2;
  console.log(colors[2]); // undefined
  colors.length = 3;
  console.log(colors[2]); // undefined, 'green'已被删除
  colors[2] = 'gray';
  colors[colors.length] = 'pink';
  colors[colors.length] = 'orange';
  console.log(colors); // ["blue", "red", "gray", "pink", "orange"]
  ```

- 所有对象均有 `toLocaleString()` 、`toString()` 、 `valueOf()` 方法用于转换，默认转换方法为 `toString()` 。除此之外，默认分隔符为逗号，但可以使用 `join()` 方法来使用不同分隔符构建字符串。

  ```javascript
  var colors = ['blue', 'red', 'green'];
  console.log(colors.join('! ')); // blue! red! green
  console.log(colors.join(' + ')); // blue + red + green
  console.log(colors.join('\n'));
  // blue
  // red
  // green
  ```

- Array 通过 `concat()` 方法来构建新数组，通过 `slice()` 方法对数组进行切片。

  ```javascript
  var colors = ['blue', 'red', 'green'];
  colors.concat('orange', ['pink', 'gray']);
  // ["blue", "red", "green", "orange", "pink", "gray"]
  colors.slice(0, 3);
  // ["blue", "red", "green"]
  ```

  ​

- Array 具有五种迭代方法，分别是：

  - `every()` : 对Array的每一个元素运行指定函数，若该函数对每一项都返回 `true` ，则返回 `true` .
  - `filter()` : 对Array的每一个元素运行指定函数，返回运行该函数返回 `true` 的元素组成的Array.
  - `forEach()` : 对Array的每一个元素运行指定函数，无返回值.
  - `map()` : 对Array的每一个元素运行指定函数，返回每次函数调用结果组成的Array.
  - `some()` : 对Array的每一个元素运行指定函数，若该函数存在任一项返回 `true` ，则返回 `true` .

  ```javascript
  var colors = ['blue', 'red', 'green', 'orange', 'pink'];
  colors.every(function (element) {
  	if (element.length > 3) {
  		return true;
  	} else {
  		return false;
  	}
  }); // false

  colors.some(function (element) {
  	if (element.length > 3) {
  		return true;
  	} else {
  		return false;
  	}
  }); // true

  colors.map(function (element) {
  	return element.toUpperCase();
  }); // ["BLUE", "RED", "GREEN", "ORANGE", "PINK"]

  colors.filter(function (element) {
  	return element.length === 4;
  }); // ["blue", "pink"]

  colors.forEach(function (element) {
  	var s = element[0].toUpperCase() + element.slice(1, element.length);
  	console.log(s);
  }); 
  // Blue
  // Red
  // Green
  // Orange
  // Pink
  ```

- Array 可通过 `push()` 与 `pop()` 方法实现栈操作，通过 `shift()` 与 `push()` 方法（后增前删） 或 `unshift()` 与 `pop()` 方法（前增后删）实现队列操作。

- Array 可通过 `reverse()` 方法逆转数组，通过高级函数 `sort()` 实现自定义比较关系排序（默认字典序）。

- Array 可通过 `indexOf()` 和 `lastIndexOf()` 方法来查找一个数组中的元素，前者从前往后查找，后者反之，使用全等操作法 `===` 来比较待查找元素，返回元素在数组中的位置，未找到返回 `-1` 。第一个参数为查找项，第二个可选参数为查找起点位置。

  ```javascript
  var colors = ['blue', 'red', 'green', 'orange', 'pink'];
  colors.indexOf('pink'); // 4
  colors.lastIndexOf('red'); // 1
  colors.indexOf('red', 2); // -1
  ```

  ​

- 详细资料：[JavaScript Array 对象属性与方法](http://www.w3school.com.cn/jsref/jsref_obj_array.asp)

### 对象（Object）

- 对象（Object）感觉更像Python里面的dict，是一种无序的集合数据类型，由若干`(key: value)`的键值对组成。

- 基本上 JavaScript 里的任何东西都是对象。

- 用面向对象（Object - Oriented）的观点来看，对象由属性与方法组成，其本质上也还是键值对，即`(属性名/PropertyName: 属性值)` 与 `(方法名/MethodName: 函数表达式或Function对象)`。

  ```javascript
  var myDog = {
    	name: '233',
    	age: 3,
    	'back-color': 0x3f3f3f, // 如果属性名包含特殊字符，必须使用''括起来
    	bark: function(){
        console.log("汪汪汪!");
    	}
  };

  /*访问对象属性与方法*/
  console.log(myDog.name); // 通过.操作符访问对象属性
  console.log(myDog['age']); // 通过[]操作符访问对象属性
  console.log(myDog['back-color']); // 若属性名包含特殊字符, 则只能用[]操作符来访问
  myDog.bark(); // 通过.操作符访问对象方法
  console.log(myDog.girlfrind); // undefined, 不存在该属性

  /*增删对象属性与方法*/
  myDog.girlfrind = '666'; // 新增一个girlfriend属性
  console.log(myDog.girlfriend); // 666
  myDog.girlfriend = 'QAQ'; // 修改girlfriend属性
  console.log(myDog.girlfriend); // QAQ
  delete myDog.girlfriend; // 删除girlfriend属性, delete myDog['girlfrind']亦可
  console.log(myDog.girlfriend); // undefined, 不存在该属性

  /*验证对象是否存在某属性或方法*/

  // 检测对象是否拥有某一属性，可以用in操作符
  'name' in myDog; // true
  'girlfriend' in myDog; // false
  'toString' in myDog; // true, 该属性继承自object对象

  // 判断一个属性是否为对象自身拥有，而非继承得到，可使用hasOwnProperty()方法
  myDog.hasOwnProperty('name'); // true
  myDog.hasOwnProperty('toString'); // false
  ```



## 条件判断 & 循环控制

### 条件判断

条件判断，与多数编程语言大同小异，就是`if` `else` `else if`，以及`switch`,并且支持嵌套。

需要注意的就是`if`后的条件判断语句，一般是值为 `true` 或 `false` 的表达式，但JavaScript并没有像Java一样强制要求是逻辑判断表达式，条件语句的位置可能会有其他语句出现。

于是有如下准则：

> 所有**不是** `undefined`、`null`、` 0`、`NaN`、空字符串 (`""`) 的任意对象，包括值为`false`的Boolean对象， 在条件语句中都为**true**。

```javascript
var b = new Boolean(false);
if (b) // 表达式的值为true
  console.log(true);
else
  console.log(false);
/* 
 * 反观C++，若bool b = false;
 * 则if (b) 表达式值为false.
 */

var s = '123';
var es = '';
if (s.length) { // 条件计算结果为3, 表达式值为true
  console.log(true);
} else {
  console.log(false);
}
if (s) { // '123'表达式值为true
  console.log(true);
} else {
  console.log(false);
}
if (es) { // ''表达式值为false
  console.log(true);
} else {
  console.log(false);
}

var zero = 0;
if (zero) // 表达式结果为false
  console.log(true);
else
  console.log(false);
if (!zero) // 表达式结果为true
  console.log(true);
else
  console.log(false);

// !!!这仅是实验，极其不推荐在条件表达式中单纯的使用赋值语句
var x = 1, y = 0;
if (x = y) // 表达式结果为false
  console.log(true);
else
  console.log(false);
var x = 1, y = 2;
if (x = y) // 表达式结果为true
  console.log(true);
else
  console.log(false);
// 在赋值语句作为条件表达式的情况下，先执行赋值语句，然后将赋值后对象作为条件
```

总结一下，就是：

> 尽量使用值为 true 或 false 的逻辑表达式作为condition
>
> "空"（`undefined`、`null`、` 0`、`NaN`、`""`）的东西是**false**，除此之外一切对象皆为**true**。



### 迭代器`for`

迭代器`for`的使用方式和C++/Java基本一致（除了赋值语句）

```javascript
 for ([initialization]; [condition]; [final-expression])
    statement

 /*example*/
 for (var i = 0; i < 5; i++) {
   console.log(i);
 }
 /*
 0
 1
 2
 3
 4
 */
 for (var i = 0; i < 5; i++) {
   if (i % 2 == 0)
     continue;
   console.log(i);
 }
 /*
 1
 3
 */
 for (var i = 0; i < 5; i++) {
   if (i > 2)
     break;
   console.log(i);
 }
 /*
 0
 1
 2
 */
```

### 迭代器`for ... in`

迭代器`for ... in`用于以**任意序**迭代一个对象的可枚举属性，其迭代顺序依赖于执行环境。

```javascript
var obj = {
    name: 'Flint',
    age: 22,
  	sex: 'male',
    city: 'Wuhan',
  	cry: function() {
      console.log('QAQ');
  	}
};
for (var key in obj) {
    if (obj.hasOwnProperty(key)) { // 过滤掉继承的属性
      console.log(key); // 'name', 'age', 'sex', 'city', 'cry'.
    }
}

var arr = ['H', 'He', 'Li', 'Be', 'B'];
for (var i in arr) {
  console.log(i); // '0', '1', '2', '3', '4'. 注意i是string不是number
  console.log(arr[i]); // 'H', 'He', 'Li', 'Be', 'B'
}
```

### 迭代器`for ... of`

`for ... of`语句是在可迭代对象基础上创建的一个循环。不同于`for ... in`遍历对象属性的名称，`for ... of`仅遍历一个对象的可迭代部分并且直接遍历属性值而非属性名。区别如下：

```javascript
var arr = ['apple', 'banana', 'cat', 'dog'];
arr.name = 'alpha';

for (let key in arr) {
  console.log(key); // '0', '1', '2', '3', 'name'
  console.log(arr[key]) // 'apple', 'banana', 'cat', 'dog', 'alpha'
}

for (let key of arr) {
  console.log(key) // 'apple', 'banana', 'cat', 'dog'
}
```

### `forEach()`方法

`forEach`是大部分可迭代对象`iterable`内置的一个方法，是一种比较推荐的遍历方式。

原型如下（以`Array`为例）：

```javascript
array.forEach(callback(currentValue, index, array){
    //do something
}, this)
// or
array.forEach(callback[, thisArg])
```

> #### 参数
>
> - `callback`为数组中每个元素执行的函数，该函数接收三个参数：
>
>   - `currentValue(当前值)` ：数组中正在处理的当前元素。
>   - `index(索引)` ：数组中正在处理的当前元素的索引。
>   - `array` ：`forEach()`方法正在操作的数组。 
>
> - `thisArg`为可选参数。当执行回调函数时用作`this`值。
>
>   ​
>
> #### 返回值
>
> - `undefined`

使用示范：

```javascript
var arr = ['apple', 'banana', 'cat', 'dog'];
arr.forEach(function (element, index, array) {
  console.log('arr[' + index + '] = ' + element);
});
/*
 * arr[0] = apple
 * arr[1] = banana
 * arr[2] = cat
 * arr[3] = dog
 */

function logArrayElements(item, i, arr) {
  console.log('array[' + i + '] = ' + item + ";");
  if (i === arr.length - 1) {
    console.log('array is : ' + arr);
  }
}
['Aa', 'Bb', 'Cc'].forEach(logArrayElements);
/*
 * array[0] = Aa;
 * array[1] = Bb;
 * array[2] = Cc;
 * array is : Aa,Bb,Cc
 */

// Set与Map的forEach()方法各有其定义
var s = new Set(['A', 'B', 'C', undefined]);
s.forEach(function (value, sameValue, set) {
  console.log(`${value} and ${sameValue};`);
});
// A and A;
// B and B;
// C and C;
// undefined and undefined;

var m = new Map([['No.1', 'Alice'], ['No.2', 'Bob'], ['No.3', 'Candy']]);
m.forEach(function (value, key, map) {
  console.log(`${key} ==> ${value};`);
});
// No.1 ==> Alice;
// No.2 ==> Bob;
// No.3 ==> Candy;
```

