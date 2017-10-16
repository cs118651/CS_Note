# js创建对象的几种模式

### 1.工厂模式

```
function createPerson(name, age, job){
	var o = new Object();
 	o.name = name;
 	o.age = age;
 	o.job = job;
 	o.sayName = function(){
 	alert(this.name);
	};
	return o;
}
var person1 = createPerson("Nicholas", 29, "Software Engineer");
var person2 = createPerson("Greg", 27, "Doctor"); 
```

### 2.构造函数模式

```
function Person(name, age, job){
	this.name = name;
	this.age = age;
	this.job = job;
	this.sayName = function(){
	alert(this.name);
 	};
}
var person1 = new Person("Nicholas", 29, "Software Engineer");
var person2 = new Person("Greg", 27, "Doctor"); 
```

构造函数模式中的  `new` 操作符在创建一个对象实例的过程中做了如下4个步骤：

​	1.创建一个新对象。  // var o = new Object();

​	2.将构造函数作用域赋给新对象(改变this指向)。

​	3.执行代码(添加属性和方法)。

​	4.返回新对象。       // return o;

在前面的例子中，两个实例person1和person2拥有了一个 `constructer` 属性，指向 `Person` 构造函数。

```
alert(person1.constructor == Person); //true
alert(person2.constructor == Person); //true 
//可以用于检测对象类型，即对象是那个构造函数创建的
```

但是检测对象类型更可靠的方式是 `instanceof` 操作符。

> **person1 instanceof Object      //true**
>
> **person1 instanceof Person      //true**

#### 	构造函数模式的缺陷在于不够灵活，如果需要拥有相同方法和属性的对象，就会造成资源的浪费。

### 3.原型模式

```
function Person(){
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
 alert(this.name);
}; 
```

​	无论任何时候，只要创建了一个新函数，就会根据特定规则为该函数创建一个 `prototype` 属性，这个属性是一个指针，指向函数的原型对象。即 `Person.prototype === Person.prototype` ，我的理解是等式左边意为 “**构造函数的 prototype 属性**” ，等式右边要看做一个**整体**，即 **构造函数的原型对象** 。

​	当然同时，原型对象也将并且**唯一**获得一个 `constructor` 属性，它指向构造函数(与前面正好相对) ，原型对象还会从 Object 上继承其他的方法。

​	此外，当用 `new` 操作符创建一个实例之后，又涉及到一个属性(指针)，即 `__proto__` ，它指向构造函数的原型对象。

##### 至此，一共出现了3个概念和3个指针，他们之间的关系是：

​	构造函数.prototype ------------>  原型对象

​	原型对象.constructor ----------------> 构造函数

​	实例.__proto_\_  ----------------------> 构造函数

用一张著名的图来表示：

![](http://oxx2l7d61.bkt.clouddn.com/js1.png)

图中我们可以发现一个问题，两个实例都不包含 **sayName()** 方法但却可以调用该方法，这就涉及到查找对象属性的过程，即依据**原型链**来查找。
