##javascript操作符类型转换
一直自以为很简单的类型转换，最近一次却混淆了“==”导致的隐式类型转换相关知识，故google操作符类型转换相关资料，做此总结复习，也方便大家学习和交流。
<br>
<br>
话不多说先上题

```js
if ('0' == false) {
	console.log('"0" is false');
}
if ('0') {
	console.log('"0" is true');
}
```
这个结果很有意思，两者都输出了，那 '0' 到底是真还是假呢？

其实js在'=='比较的时候，其转化有一些规则：

* 首先看双等号左右有没有NaN，如果存在NaN，一律返回false。
* 如果比较的两者中有bool，会把 bool 先转换为对应的 number，即 0 和 1
* 如果比较的双方中有一方为number一方为string，会把string转换为数字
* 把string直接转换为bool的时候，空字符串''转换为 false，除此之外的字符串转换为 true
* 如果有对象对象使用toString()或者valueOf()进行转换；
* null, undefined不会进行类型转换, 但它们俩相等(null == undefined 为true)

####运算符的类型转换
在研究'=='的隐式类型很容易联想到运算符相关的类型转换<br>
还是先上题<br>
####乘法:
```js
console.log(6 * '6'); 	//36  将字符转化为数字进行相乘
console.log(6 * 'a'); 	//NaN	调用Number()将字符转化，结果为NaN，所以相乘为NaN
console.log(6 * null); 	//0  	Number()将null转化为0
console.log(6 * undefined); 	//NaN	Number()将undefined转化为0
console.log(6 * NaN); 	//NaN	
```
关于答案我也不多啰嗦，以下是乘法的隐形转换原则

* 如果2个数值都是数字，那么直接进行乘法运算，但是如果结果超过了js数值范围，则返回Infinity（正无穷)或者 -Infinity（负无穷）
* 如果有一个数是NaN则结果为NaN
* Infinity乘0结果是0
* 如果运算符两边有不为数值的类型，则调用Number()来将其转化为数值类型。如果转化之后不为数值类型（NaN）,
则结果为NaN
<br>
<br>

####除法:
```js
 console.log(6 / '6');		//1    将字符转化为数字进行相除
 console.log(6 / 'a');		//NaN   将“a”用Number()函数进行转化，出来的值是NaN，结果就是NaN
 console.log(6 / NaN);		//NaN
 console.log(6 / null);		//Infinity  null用Number()函数进行转化，结果是0，那么6/0是正无穷
 console.log(null / 6);		//0  同上0/6是0
 console.log(6 / undefined);	//NaN   undefined 用Number()进行转化，结果是NaN
 console.log(6 / 0);		//Infinity
 console.log(0 / 6);		//0
 console.log(0 / 0);		//NaN 	0除以0结果是NaN
```
除法和乘法规则差不太多，只需要注意运算规则，最后一点是个小坑
<br>
<br>

####减法:
```js
console.log(10 - '6');	//4
console.log(6 - 'a');	//NaN
console.log(6 - NaN);	//NaN
console.log(6 - null);	//6
console.log(6 - undefined);	//NaN
console.log(6 - 6);		//0
console.log(6 - true);	//5
console.log(6 - 'true');	//NaN
console.log(6 - '');	//6
console.log(6 - Infinity);	//-Infinity
console.log(Infinity - Infinity);		//NaN
console.log('a' + 6 - 6);	//NaN   先相加为字符串， 字符串减数字为NaN
```
关于减法，最大的坑就是在无穷的运算。

* Infinity - Infinity结果是NaN
* -Infinity - Infinity结果是-Infinity
* 数字减Infinity结果是-Infinity
* Infinity - （-Infinity）结果是Infinity
* 如果操作数是对象，则先调用valueOf方法，如果结果是NaN,则最终结果为NaN, 如果返回的结果不是数值类型，则再调用toString，转换为字符串再进行相关计算;
<br>
<br>

####加法:
```js
console.log(10 + '6'); 	//106
console.log(6 + 'a');	//6a
console.log(6 + NaN);	//NaN
console.log(6 + null); //6
console.log('6' + null);	//6null  null转化为'null'
console.log(6 + undefined);	//NaN
console.log(null + undefined);	//NaN
console.log(6 + 6);	//12
console.log('两数和为' + 6 + 6;	//两数和为66
console.log('两数和为' + (6 + 6));	//两数和为12 先相加
```

加法隐性转换规则：

* 如果有一个是字符串，则另一个也会转换为字符串进行拼接。包括null和undefined
* 如果一个数字和null或者undefined相加，则是把null/undefined进行Number()转换
<br>
<br>

####取模
```js
console.log(10 % '6'); 	//1  '6'通过Number()转化为6
console.log(6 % 'a');	//NaN
console.log(6 % NaN);	//NaN
console.log(6 % null);	//NaN  将null通过Number()转化为0， 6 % 0 结果为NaN
console.log(null % 6);	//0  0 % 6，结果是0
console.log(6 % undefined);	//NaN
console.log(6 % 0);	//NaN
console.log(0 % 6);	//0
console.log(0 % 0);	//NaN
console.log(Infinity % Infinity);	//NaN
console.log(6 % Infinity);	//6
console.log(Infinity % 6); 	//NaN
```
这个没有啥好啰嗦的啦，就是转化和取模的运算规则
<br>
<br>

####toString()和valueOf()
关于类型转化，肯定又多多少少会牵扯到toString()和valueOf()这两个方法

```js
var oArr = [1];
oArr.toString = function () {
	console.log('toString');
	return 1;
}
oArr.valueOf = function () {
	console.log('valueOf');
	return 1;
}

console.log(oArr - 1);	//输出为valueOf 和 0


var oArr2 = [1];
oArr2.toString = function () {
	console.log('toString');
	return 1;
}
oArr2.valueOf = function () {
	console.log('valueOf');
	return this;
}

console.log(oArr2 - 1);	//输出为valueOf toString 和 0
```

由此可见转换首先调用的是valueOf()，如果valueOf()返回值不是基本类型，那就会调用toString进行转换，而不是将一个对象转换为字符串调用toString()，转换为数值调用valueOf()。大多数对象隐式转换为值类型都是首先尝试调用valueOf()方法。
<br>

最后提及一道相关题目：
<br>
<br>
**加法柯里化:**
<br>
&nbsp;&nbsp;&nbsp;&nbsp;如何实现add函数，使add(1)(2)(3) 最后返回加法结果呢

答案:

```js
function add(x) {
	var sum = x;
	var tmp = function (y) {
		sum = sum + y;
		return tmp;
	};

	tmp.toString = function () {
		return sum;
	};
	return tmp;
}
```
<br><br><br><br>
转载请联系~<br>
邮箱：lynnprosper@163.com

 
