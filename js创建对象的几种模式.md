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

####   3.1 基本原理

```
function Person(){
}
Person.prototype.name = "Nicholas";
Person.prototype.age = 29;
Person.prototype.job = "Software Engineer";
Person.prototype.sayName = function(){
 alert(this.name);
}; 
var person1 = new Person();
person1.sayName(); //"Nicholas"
var person2 = new Person();
person2.sayName(); //"Nicholas"
alert(person1.sayName == person2.sayName); //true 
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

##### 	在读取某个对象的属性或者方法时，原型链的实现流程是这样的：

​	1.首先检查对象实例上是否存在该属性。 //person1

​	2.若不存在，检查对象实例的原型对象上是否存在该属性。 //Person.prototype

​	3.若不存在，检查原型对象的原型对象上是否存在该属性。 //Object.prototype

​	4.若不存在，则返回null。 //null是原型链的结束

更直观的看，本例中的原型链为：

​	**person1----------->Person.prototype----------->Object.prototype----------->null**

####   3.2 关于原型对象和实例的一些操作符和方法

 1.  `isPrototypeOf()` 方法：

     **用法：** 原型对象.isPrototypeof(实例)

     **作用：** 检测实例和原型对象对应关系

```
alert(Person.prototype.isPrototypeOf(person1)); //true
alert(Person.prototype.isPrototypeOf(person2)); //true 
```

2. `Object.getPrototypeOf()` 

   **用法：** Object.getPrototypeOf(实例)

   **作用：** 返回实例的原型对象

   ```
   alert(Object.getPrototypeOf(person1) === Person.prototype); //true 
   alert(Object.getPrototypeOf(person1) === person1.__proto__); //true 
   ```

3. `hasOwnProperty()` 

   **用法：** 实例.hasOwnProperty(属性)

   **作用：** 检测该属性是否来自实例本身(返回true)或者来自其原型对象(返回false)

   ```
   person1.name = 'andy'; //给实例属性name赋新值 屏蔽原型中的name值(没有覆盖)

   alert(person1.name); //"andy"——来自实例
   alert(person1.hasOwnProperty("name")); //true

   alert(person2.name); //"Nicholas"——来自原型
   alert(person2.hasOwnProperty("name")); //false

   delete person1.name; //delete操作符可删除实例上的属性

   alert(person1.name); //"Nicholas"——来自原型
   alert(person1.hasOwnProperty("name")); //false 
   ```

4. `in` 操作符

   **用法：** `属性 in 实例` 

   **作用：** in 操作符会在通过对象能够访问给定属性时返回 true，无论该属性存在于实例中还是原型中。

   ```
   person1.name = "andy"; 
   alert("name" in person1); //true 而且name来自实例
   alert("name" in person2); //true 而且name来自原型
   ```

#### 3.3 原型模式的另一种写法

```
function Person(){
}
Person.prototype = {
	name : "Nicholas",
 	age : 29,
 	job: "Software Engineer",
 	sayName : function () {
 	alert(this.name);
 	}
}; 
```

将 `Person.prototype` 设置为等于一个以对象字面量形式创建的新对象，但是这样做会带来一个后果，就是 `Person.prototype` 的 `constructor` 属性改变了 ，不再指向 `Person` 构造函数，而是指向了`Object` 这个构造函数。

因为每创建一个函数，就会同时创建它的 prototype 对象，这个对象也会自动获得 constructor 属性。

```
var friend = new Person();
alert(friend instanceof Object); //true
alert(friend instanceof Person); //true
alert(friend.constructor == Person); //false
alert(friend.constructor == Object); //true 
```

这里注意，发现一个问题，我运行了下面的代码：

```
console.log(friend.constructor === Person.prototype.constructor)  //true
```

说明不止原型对象中有 `constructor` 属性，实例中也有，并且同样指向 `Person` 构造函数。这里我大胆的猜测一下，其实 `Person.prototype` 和实例 `firend` 其实本质上是没有区别的，它们都是构造函数 `Person` 的一个实例而已。

**回到主题**，那 `constructor ` 属性指向不按照预期怎么办呢，有两种处理方式：

```
function Person(){
}
Person.prototype = {
	constructor : Person,   //显示地声明了constructor属性就等于构造函数
	name : "Nicholas",
	age : 29,
	job: "Software Engineer",
	sayName : function () {
	alert(this.name);
 	}
};
```

​	但是这样做会带来一个副作用，导致 `constructor` 属性的 [[Enumerable]] (可枚举的)特性被设置为 true。默认情况下，原生的 constructor 属性是不可枚举的，如果不想出现这种情况，可以采用如下方式：

```
//重设构造函数，只适用于 ECMAScript 5 兼容的浏览器
Object.defineProperty(Person.prototype, "constructor", {
 enumerable: false,  // 这里声明constructor属性不可枚举
 value: Person  //这里将constructor值设置为Person
});
```

##### 	原型模式的问题在于所有的属性和方法都被实例所共享，当我们需要一些私有的属性和方法时就要组合使用两种模式，当然这也是最常用的一种模式。

### 4.构造函数模式+原型模式

```
//独有的属性方法定义在构造函数中
function Person(name, age, job){
 	this.name = name;
 	this.age = age;
 	this.job = job;
 	this.friends = ["Shelby", "Court"];
}
//共享的属性方法定义在原型中
Person.prototype = {
	constructor : Person,
	sayName : function(){
	alert(this.name);
	}
} 
```

这种方式看起来或许不是那么有整体性，毕竟对象不应该是一个整体的封装吗，所以js高程中描述了这样一种更加一体化的对象创建语法，“动态原型模式”

```
function Person(name, age, job){
	//属性
	this.name = name;
	this.age = age;
	this.job = job;
	//方法
	if (typeof this.sayName != "function"){
 		Person.prototype.sayName = function(){
 			alert(this.name);
 		};
	}
}
var friend = new Person("Nicholas", 29, "Software Engineer");
friend.sayName();
```

这里只在 `sayName()` 方法不存在的情况下，才会将它添加到原型中。这段代码只会在初次调用构造函数时才会执行。此后，原型已经完成初始化，不需要再做什么修改了。不过要记住，这里对原型所做的修改，能够立即在所有实例中得到反映。因此，这种方法确实可以说非常完美。