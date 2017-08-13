---
title: 理解JavaScript原型
date: 2017-07-30 20:42:42
tags:
---
JavaScript的`原型`是JavaScript中的重要一环，根据网上的一些资料和自己的理解，对`原型`做一个解释。

<!--more-->

##prototype属性的引入
`对象`可以使用`new`操作符后跟一个`构造函数`来创建的。构造函数如下：

```
function Person(age) {
  this.name = "meng";
  this.age = age;
}
```
构造函数始终都应该以一个大写字母开头，而非构造函数则应该以一个小写字母开头。要创建一个新的实例对象的时候，可以使用`new`操作符。

```
var person1 = new Person(12);
var person2 = new Person(13);
```
在构建的两个实例对象的时候，person1和person2有一个共有属性name;当改变其中一个实例对象的name的时候，另一个不会发生改变。

```
person1.name = "jing";
console.log(person1.name);  //jing
console.log(person2.name);  //meng
```
每个实例对象都有自己的属性和方法的副本，改变其中的一个并不会影响另一个，这就造成了资源和空间的浪费，也无法实现数据的共享。

为了解决上面的问题，作者Brendan Eich决定使用构造函数设置一个`prototype`属性，可以让所有对象实例共享它所包含的属性和方法。

上面的例子可以写成：

```
function Person(age) {
  this.age = age;
}
Person.prototype.name = "meng";

var person1 = new Person(12);
var person2 = new Person(13);
Person.prototype.name = "jing";
console.log(person1.name);  //jing
console.log(person2.name);  //jing
```
这时person1和person2就共享了`Person.prototype.name`这个属性，只要其中一个改变，就会同时影响两个实例对象。

##原型是什么
只要创建了一个新函数，就会根据一组特定的规则为该函数创建一个prototype属性，这个属性指向函数的原型对象。所有原型对象都会获得一个指向`prototype`对象的`constructor`属性。如图：

![14805248662081.jpg](http://upload-images.jianshu.io/upload_images/1561693-8c18d7417b86f024.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
拿上面的例子来说，Person.prototype.constructor又指向了Person。

####_proto_、prototype和constructor
每一个生成的对象都有一个`__proto__`属性，当调用构造函数创建一个新的实例之后，`__proto__`就指向了构造函数的原型对象，即构造函数的`prototype`属性。

下图所示：当定义person1是一个数组时，person1自带一个`__proto__`属性。
![14805256492639.jpg](http://upload-images.jianshu.io/upload_images/1561693-056169526e257b50.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下图所示：当定义Person构造函数时，Person自带一个`prototype`属性。`prototype`属性自带一个`constructor`属性，至于其他的属性，都是从Object继承而来的。

![14805260237657.jpg](http://upload-images.jianshu.io/upload_images/1561693-2f4d857269574c20.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
下图所示：当`new`一个实例对象person1时，`_proto_`就指向了构造函数Person的原型对象，即构造函数的`prototype`属性。可以对比一个person1的`_proto_`属性在实例化之前和之后的变化。

![14805259038416.jpg](http://upload-images.jianshu.io/upload_images/1561693-fc467a193bd8d7bc.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

下图是展示了Person构造函数、Person的原型属性以及Person现有的两个实例之间的关系。
![14805245325304.jpg](http://upload-images.jianshu.io/upload_images/1561693-be72bc34903ea2ef.jpg?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


##new运算符的工作原理
new 运算符接受一个函数`F`及其参数：`new F(arguments...)`。这一过程分为三步：

创建类的实例。这步是把一个空的对象的`__proto__`属性设置为 `F.prototype`。
初始化实例。函数`F`被传入参数并调用，关键字`this`被设定为该实例。
返回实例。
现在我们知道了`new`是怎么工作的，我们可以用JS代码实现一下：

```
function New (f) {
    var n = { '__proto__': f.prototype }; /*第一步*/
    return function () {
        f.apply(n, arguments);            /*第二步*/
        return n;                         /*第三步*/
    };
}
```
##hasOwnProperty函数

`hasOwnProperty`用来判断该属性属于实例自定义的属性而不是原型链上的属性。

```
function Person(age) {
  this.age = age;
}
Person.prototype.name = "meng";

var person1 = new Person(12);
person1.hasOwnProperty('age');  //true
person1.hasOwnProperty('name'); //false
```

##JavaScript引擎如何来查找属性

以下代码展示了JS引擎如何查找属性:

```
function getProperty(obj, prop) {
    if (obj.hasOwnProperty(prop))
        return obj[prop]

    else if (obj.__proto__ !== null)
        return getProperty(obj.__proto__, prop)

    else
        return undefined
}
```
通过上面的代码可以看出来，js先查找自身有没有该属性，如果没有的话，就查找__proto__属性指向的原型对象中有没有，如果没有的话，就去查它的原型的原型中有没有,一直到原型链的最顶端为止。

参考：
[阮一峰:Javascript继承机制的设计思想](http://www.ruanyifeng.com/blog/2011/06/designing_ideas_of_inheritance_mechanism_in_javascript.html)
[JavaScript的原型机制](http://www.jianshu.com/p/9d6aaba12a63)