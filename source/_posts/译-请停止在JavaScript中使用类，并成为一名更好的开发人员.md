---
title: '[译]请停止在JavaScript中使用类，并成为一名更好的开发人员'
date: 2020-02-01 17:34:33
tags: javascript
comments: true
---

>原文链接：[Please stop using Classes in JavaScript and become a better developer](https://medium.com/javascript-in-plain-english/please-stop-using-classes-in-javascript-and-become-a-better-developer-a185c9fbede1) by Michael Krasnov

多年来，OOP（面向对象编程）一直是软件工程中的标准。类，多态性，继承和封装的概念主导了开发过程，并对其产生了革命性的影响。但是没有什么是一成不变的，包括编程范例。在本文中，我将讨论为什么首先引入类，为什么在JavaScript中使用类是一个坏主意，以及一些替代方法。

我不会谈论OOP为何逐渐消失，但是您可以查看[这篇很棒的文章](https://medium.com/better-programming/object-oriented-programming-the-trillion-dollar-disaster-92a4b666c7c7)以获取更多信息。

# ES6之前的类
即使自ES6（ECMAScript 2015）起才将class关键字添加到JavaScript中，但之前就有人使用**class**了。通过构造函数和原型委托实现此目的。为了确切地说明我的意思，我将在ES5和ES6环境中实现相同的类。考虑一个类`Car`和另一个继承自`Car`的`SportsCar`类。它们都具有`make`和`model`属性和`start`方法，但`SportsCar`还具有`turbocharged`属性并覆盖`start`方法：
```javascript
// "class" declaration
function Car(make, model) {
  this.make = make;
  this.model = model;
}

// the start method
Car.prototype.start = function() {
  console.log('vroom');
}

// overriding the toString method
Car.prototype.toString = function() {
  console.log('Car - ' + this.make + ' - ' + this.model);
}

// inheritance example
function SportsCar(make, model, turbocharged) {
  Car.call(this, make, model);
  this.turbocharged = turbocharged;
}

// actual inheritance logic
SportsCar.prototype = Object.create(Car.prototype);
SportsCar.prototype.constructor = SportsCar;

// overriding the start method
SportsCar.prototype.start = function() {
  console.log('VROOOOM');
}

// Now testing the classes
var car = new Car('Nissan', 'Sunny');
car.start(); // vroom
console.log(car.make); // Nissan

var sportsCar = new SportsCar('Subaru', 'BRZ', true);
sportsCar.start(); // VROOOOM
console.log(car.turbocharged); // true
```
您可能已经猜到了，`Car`（第2行）和`SportsCar`（第18行）函数是构造函数。使用`this`关键字定义属性，使用`new`创建实例。如果您不熟悉`prototype`，这是每个JS对象都必须委派常见行为的特殊属性。例如，原型为数组对象所具有的功能，你可能很熟悉：`map`，`forEach`，`find`等原型为字符串具有的功能`replace`，`substr`等等。

在第33行上创建Car对象之后，可以访问其属性和方法。从第34行调用`start`产生以下操作：

1. JS引擎询问`car`对象上的一个键值`start`。
2. 对象说它没有这样的值
3. JS引擎询问`car.prototype`上的一个键值`start`。
4. 在`car.prototype`返回的`start`功能，JS引擎立即执行。

访问`make`和`model`属性的操作类似，只是它们直接在`Car`对象上定义而不是在`prototype`上定义。

在第24-25行解决了这个有点棘手的继承功能。这里最重要的function是`Object.create`。它接受一个对象并返回一个全新的对象，其原型设置为作为参数传递的对象。现在，如果JS引擎在`sportsCar`对象或上找不到值`sportsCar.prototype`，它将查询`sportsCar.prototype.prototype`也就是`Car`对象的原型。

# ES6中的关键字--*Class*

随着2015年ES6的发布，期待已久的`class`关键字出现在JavaScript中。这样做是根据社区的众多要求完成的，因为人们对来自面向对象的语言感到不自在。但是他们忽略了一个重点。
> JavaScript has no idea what classes are

JavaScript不是一种面向对象的语言，因此类的概念绝对不适用于它。尽管JS中的所有内容确实都是对象，但这些对象与Java或C＃中的对象不同。在JS中，对象只是具有[某种复杂查找过程](https://medium.com/javascript-scene/3-different-kinds-of-prototypal-inheritance-es6-edition-32d777fa16c9)的Map数据结构。就是这样。当我说一切都是对象时，我是说真的：甚至函数都是对象。您可以通过以下代码片段进行检查：
```javascript
function iAmAnObject() {}

console.log(iAmAnObject.name); // iAmAnObject
console.log(Object.keys(iAmAnObject)); // Array []
```
这一切都很好，但是`class`关键字如何工作？很高兴你问。您还记得前面的`Car`和`SportsCar`示例吗？好吧，`class`关键字最重要的是语法糖。换句话说，类在概念上产生相同的代码，并且仅用于美学和可读性目的。正如我之前所承诺的，这是ES6中这些相同类的示例：
```javascript
class Car {
  constructor(make, model) {
    this.make = make;
    this.model = model;
  }
  
  start() {
    console.log('vroom');
  }
  
  toString() {
    console.log(`Car - ${this.make} - ${this.model}`);
  }
}

class SportsCar extends Car {
  constructor(make, model, turbocharged) {
    super(make, model);
    this.turbocharged = turbocharged;
  }
  
  start() {
    console.log('VROOOOM');
  }
}


// Actual usage remains the same
var car = new Car('Nissan', 'Sunny');
car.start(); // vroom
console.log(car.make); // Nissan

var sportsCar = new SportsCar('Subaru', 'BRZ', true);
sportsCar.start(); // VROOOOM
console.log(car.turbocharged); // true
```
这些示例相同，并且产生相同的结果。有趣的是，它们在后台生成（几乎）相同的代码。我不会在这里写出来，但是如果您很好奇，请转至[在线Babel转译器](https://babeljs.io/en/repl#?browsers=&build=&builtIns=false&spec=false&loose=false&code_lz=MYGwhgzhAEDCYCdoG8BQ1rAPYDsIBcEBXYfLBACgFswBrAUwBpoqsATekAShXQ2nwALAJYQAdDQbQAvCzr0A3HwxDRE9pxksNIJRgC-fPgUT4KPNP0y4IWEPTEgsAcwoByAG4IsWKm6560IYYfGQAyoTCOK4WytZ4dg5OrgAG8EgAtNAAJMiq4pL0-tBZufnqHCD6KQF8hoaooJAwYQAO5PgQ6dD0AB749DhsMN2W8QTEpOTU8syslcz4RAgARljAgojO9GyxVhBErfSUhXM6tVblS6vrmwjbbFrXaxtbO4HB0Mb4pua8VtgEvZHC53AA1ABKAHkYVCALL-D6oBqoAD0qOgAEFSEQwCBoEQIGBttAEPQaFEYEJ6NAiVR6KgPIhMMzZDh6AB3OCIdwAOVERJwbmYbjCRBwOAAnojGogxCYEGYAtB0dAvD4qI0bIkQa5gHLCsrVfyoGAcKhGcyIO1FV1WdB2Vy2h07ZRRUQVogiMLoG4AEIQgBaPsIRHotWtLvS8p-ivMChVGMhsPhWqBSVB-oQYmetzeuwTqtD9CAA&debug=false&forceAllTransforms=false&shippedProposals=false&circleciRepo=&evaluate=true&fileSize=false&timeTravel=false&sourceType=module&lineWrap=true&presets=es2015&prettier=true&targets=&version=7.8.0&externalPlugins=)，然后查看输出。

# 为什么不？
现在，您应该了解JS中的类以及它们如何工作。现在，借助所有这些知识，我可以解释为什么在JS中使用类是一个坏主意。

1. **具有约束力的问题**。当类构造函数与`this`关键字紧密处理时，它可能会引入潜在的绑定问题，特别是如果您尝试将类方法作为回调传递给外部例程（您好，React devs👋）
2. **性能问题**。由于类的实现，众所周知，它们很难在运行时进行优化。尽管我们现在喜欢高性能机器，但[摩尔定律逐渐消失](https://en.wikipedia.org/wiki/Moore%27s_law)的事实可以改变所有这一切。
3. **私有变量**。首先，私有变量的巨大优点和主要原因之一就是JS中不存在私有变量。
4. **严格的层次结构**。类引入了从上到下的直接顺序，并使更改难以实现，这在大多数JS应用程序中是不可接受的。
5. [**因为React团队告诉您不要这样做**](https://reactjs.org/docs/hooks-intro.html)。尽管它们尚未明确弃用基于类的组件，但它们可能会在不久的将来出现。

所有这些问题都可以通过JS对象和原型委托得到缓解。JS提供了更多的功能，而类却可以做到，但是大多数开发人员对此视而不见。如果您想真正掌握JS，则需要拥护它的哲学，并摆脱基于教条式的基于对象思维。
