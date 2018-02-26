---
title: 巩固JavaScript系列--01数据类型和函数
date: 2017-08-12 19:25:01
comments: true

tags:
    - JavaScript
categories:
     - 渣技术
---

最近感觉校招也快到了，感觉自己的知识体系还是挺乱的，所以拿起了红宝书再仔细读读，对知识点再巩固巩固，想每次学习完对整体的一个知识点总结总结，写下了自己的JavaScript系列学习。

<!-- more -->

在开始写数据类型之前，想对&lt;script&gt;标签记下几点内容。

## script标签

&lt;script&gt;有以下6个属性
1. async：可选。表示应该立即下载脚本，但不妨碍页面中的其他操作，比如可以下载其他资源或等待加载其他脚本。只对外部脚本文件有效。

2. charset：可选。表示通过src属性指定的代码的字符集。由于大多数浏览器会忽略它的值，所有很少用

3. defer：可选。表示脚本可以延迟到文档完全被解析和显示之后再执行。只对外部文件有效。

4. language：已放弃。

5. src：可选。表示包含要执行的外部文件。

6. type：可选。表示编码使用的脚本语言的内容类型

这里重点记下两个比较特殊的属性**async和defer**：

defer：设置defer的脚本会在整个页面都解析完毕之后再执行。

async：指定async属性的目的是不让页面等待其脚本下载和执行，从而异步加载页面的其他内容。

## 数据类型

ECMAScript中有5种简单的数据类型(也称为基本数据类型)：**Undefined、Null、Boolean、Number和String**。

还有一种复杂数据类型：**Object**。Object本质是由一组无序的名值(键值)组成。

### 1. Undefined类型

Undefined类型的值就只有一个undefined。
``` bash
var name;
alert(name == undefined)    //true
```
从上面可以看出，name只定义，未初始化。它的默认值为undefined。与下面代码一致。
``` bash
var name=undefined;
alert(name == undefined)    //true
```
其实我们再没必要为它赋值undefined，因为它默认的值就是undefined。

### 2. Null类型

Null类型的值也只有一个，即null。从逻辑角度来看，null值表示一个空对象指针，这也是使用typeof操作符检测null值时会返回object类型的原因。
``` bash
var people=null
alert(typeof people)  //object
```
**如果定义的变量准备在将来用于保存对象，那么最好将该变量初始化为null而不是其他值(像默认的undefined)**

**undefined值是派生自null值的**，因此下面代码返回的是true
``` bash
alert(alert( null == undefined))  //true
```
但如果是全等操作的话，返回的是false
``` bash
alert(alert( null === undefined))  //false
```
**无论什么时候都没有必要把一个变量的值显式的设置为undefined**，可是同样的规则对应null却是不适用。换句话说，**只要意在保存对象的变量还没有真正的保存对象，就应该为其设置null值**。这样做的目的不仅体现null做为空对象指针的惯例，而且也有助于进一步区分null和undefined。这也是null和undefined的只要区别。

### 3. Boolean类型
Boolean类型有两个值即true和false。Boolean类型的知识点主要是下面这个:

可以对任何数据类型调用Boolean()函数，而且总会返回一个Boolean值。

**除了 0、-0、''、NaN、null、undefined、false对其使用Boolean函数返回的是假(false)，其余返回的是真(true)**

在流控制语句中(如if语句)，它会自动执行相应的Boolean转换。请看下面代码：
``` bash
var name='hmz';
if (name){
    alert('my name is hmz')  //my name is hmz
}
```
因为name被自动执行相应的Boolean转换，返回的是true

### 4. Number类型
1. 浮点数值： 浮点数值需要的内存空间是保存**整数值的两倍**，因此ECMAScript会不失时机地将浮点数值转换为整数值。浮点数值计算会**产生舍入误差**，比如0.1+0.2不会等于0.3，而是等于0.3000000...0004，所以千万不要使用以下代码，你会发现代码并不会去实现alert：
``` bash
var a=0.1,b=0.2;
if(a+b == 0.3){       //不要做这样的测试
    alert('go this')
}
```
2. 数值范围： isFinite()函数在参数位于最大值(Number.MAX_VALUE)和最小值(Number.MIN_VALUE)之间会返回true。

3. NaN： NaN(Not a  Number)是一个特殊的数值。NaN有两个特点①任何涉及NaN的操作都会返回NaN；②NaN跟任何值都不相等，包括他自己本身。

根据NaN的两个特点ECMAScript定义了isNaN()函数。isNaN()函数传入的参数会被想转换为数值再判断，**参数不会转换为数值的函数会返回true，否则函数会返回false**。
``` bash
alert(isNaN(NaN))  //true     不能被转换为数值
alert(isNaN(10))   //false    10是数值
alert(isNaN('blue'))  //true  不能被转换为数值
alert(isNaN('10')) //false   '10'会被转换为10
alert(isNaN(true))  //false   true会被转换为1
```

4. 数值转换
可以转换为数值的函数有Number()、parseInt()、parseFloat()。Number()函数可以用于任何数据类型，而parseInt()和parseFloat()专门用于把字符串转换为数值。

parseInt()转化为整数，可以传入第二个参数，告诉浏览器按几进制进行解析数据，如果不传第二个参数，默认是10进制
parseFloat()转化为浮点数，只解析整数，故没有第二个参数。

### 5. String类型
转换为字符的函数有：toString()和String()。

toString()：①数值、布尔、对象、字符串都有toString()方法。但是**null和undefined**没有toString()方法。②多数情况下，toString()方法不必传参，但也可以为其传一个参数，该参数可以将num.toString(2|8|10|16)返回几进制的字符串表示

String()：在不知道要转换的值是不是undefined和null的情况下，还可以使用String()函数。这个函数能将任何类型的值转换为字符串。

### 6. Object类型
ECMAScript中的对象其实就是一组数据和功能的集合。

Object的每个实例都有下列的属性和方法：
1. constructor：保存着用于创建当前对象的函数，即构造器。
2. hasOwnProperty：用于检查给定的属性在当前对象实例中是否存在(而不是在实例的原型中)。如：people.hasOwnProperty('name')
3. isPrototypeOf(object): 检测传入的对象是否是当期对象的原型
4. propertyIsEnumerable(propertyName): 检测传入的属性能否使用for-in进行枚举
5.toLocaleString()：返回对象的字符串表示，该字符串与执行环境对应
6. toString()：返回对象的字符串表示
7. valueOf：返回对象的字符串、数组或布尔的表示。通常与toString()的返回值相同

在ECMAScript中Object是所有对象的基础，因此所有对象都具有上面这些基本的属性和方法。


## 函数
函数中个人感觉想要去注意的是函数的返回值和函数的参数。
1. 函数的返回值：函数中要么让函数始终都返回一个值，要么永远都不要返回值。如果未指定返回值的函数，那么它返回的是一个特殊的undefined值
2. 理解参数：

ECMAScript函数传递参数比较灵活，定义函数参数也比较灵活。原因在于ECMAScript的参数在内部是用一个**数组来表示**的。函数接收到的是这个数组，而不关心数组中包含哪些参数。实际上在函数体内可以通过arguments对象来访问这个参数数组，从而获取传递给函数的每一个参数。

arguments对象只是与数组类似(他并不是Array的实例)，因为可以使用[]来访问它的每一个元素，使用length属性可以确定传递进来多少参。arguments的length由传入的参数个数决定的，不是由定义函数时的命名参数的个数决定的。对于没有传递值的命名参数将被自动赋予undefined，这就跟定义了变量但没有初始化一样。

### 函数实现伪重载

``` bash
function doAdd() {
    if (arguments.length == 1){
        alert(arguments[0])
    }
    else if (arguments.length == 2){
        alert(arguments[0] + arguments[1])
    }
}
doAdd(10)   //10
doAdd(10,20)  //30
```
上面**利用arguments的length可以实现伪重载**，但ECMAScript中是**没有真正的重载**(方法名一样，接收的参数的类型或数量不同)，因为ECMAScript不存在函数签名(函数签名由参数个数与其类型组成)的特性！！！函数名相同的情况下ECMAScript中前面定义的函数会被后面定义的函数所覆盖，故而没有真正的重载，但可以使用上面的代码实现伪重载。


## 结尾
时间过的很快，不知不觉快要八月中旬了，各个公司也开始陆续开放网申了，不由的会想太多东西，故而感觉最近的节奏感没跟上，上周在学Vue感觉也是不是很好，没有把真正的心思放在学Vue上面，搞七搞八的一周也过去了。现在自己就想静静，外面的世界如何如何，你的能力有限，大可不必在乎，现在呢要做的只有好好的静下心，坚持吧，一切顺其自然！再说写这个JavaScript系列，我也不知道我会写多少，当我还是会尽自己最大的努力去总结。


于 厦门高崎新村 自己的小屋　


本文首发于个人博客>> [shineTomorrow--一个路上慢慢行走的前端人](https://blog.huangmingzhao.cn)



参考文献：《JavaScript高级程序设计》

















