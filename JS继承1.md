### JS高级程序设计读书笔记 - JS继承

*下文中所提及的 “类型”  一词均指构造函数所代表的一种抽象，比如 Supertype 类型* 

#### 1.原型链继承

```
function SuperType(){
  this.property = true;
} 
SuperType.prototype.getSuperValue = function(){
  return this.property;
};
function SubType(){
  this.subproperty = false;
}
//继承了 SuperType
SubType.prototype = new SuperType();
SubType.prototype.getSubValue = function (){
  return this.subproperty;
};
var instance = new SubType();
alert(instance.getSuperValue()); //true 
```

  以上代码定义了两个构造函数：`SuperType` 和 `Subtype` 。通过将 `SuperType` 的一个实例赋给 `Subtype` 的原型对象来实现继承，此时的 `Subtype.prototype` 拥有了 `Subtype` 实例的所有属性和方法并且它的 `__proto__` 属性会指向 `SuperType.prototype` ，此时:

```
instance.__proto__.__proto__ === SuperType.prototype   //true
```

##### 但是，原型链继承存在一些问题：首先我们知道一个常识，当某个属性存在于原型对象中且为引用类型时，这个类型的实例会共享这些属性：

```
function SuperType(){
  this.property = true;
  this.supArr = [1,2,3];
}
... //省略号表示和上文代码一致
function SubType(){
  this.subproperty = false;
  this.subArr = [4,5,6]
}
... 
var instance1 = new SubType();
var instance2 = new SubType();

*********************************chrome devtool*********************************
> instance1.supArr === instance2.supArr
< true
> instance1.subArr === instance2.subArr
< false
```

而原型链继承的核心：

```
child.prototype = new parent()
```

故当父类型实例中存在引用类型的属性时，不论该属性来自父类型构造函数或是原型对象，此时通过上面语句的执行，都变成了子类型原型对象中的属性了，这时候需要另一种继承方法：借用构造函数。

#### 2.借用构造函数继承

```
function SuperType(name){
  this.name = name;
  this.colors = ["red", "blue", "green"];
}
function SubType(){
  //继承了 SuperType
  SuperType.call(this,'andy');
}
var instance1 = new SubType();
instance1.colors.push("black");
alert(instance1.colors); //"red,blue,green,black"
var instance2 = new SubType();
alert(instance2.colors); //"red,blue,green" 
```

通过使用 `call()` 或 `apply()` 方法，我们实际上是在（未来将要）新创建的 SubType 实例的环境下调用了 SuperType 构造函数。这样一来，就会在新 SubType 对象上执行 SuperType()函数中定义的所有对象初始化代码。

这种方式有一个很大的优势，即可以在子类型构造函数中向父类型构造函数传递参数，如上面的`name` 参数。

但是这样一来，子类型的每个实例都有自己的方法和属性(占据不同的内存空间)，那函数复用也就无从谈起了，所以在实际应用中，更多的还是使用组合继承的方法。

#### 3.组合继承

```
function SuperType(name){
  this.name = name;
  this.colors = ["red", "blue", "green"];
}
SuperType.prototype.sayName = function(){
  alert(this.name); 
};
 
function SubType(name, age){
  //继承属性
  SuperType.call(this, name);
  this.age = age;
}

//继承方法
SubType.prototype = new SuperType();
SubType.prototype.constructor = SubType; //修复constructor指向，之前指向Supertype
```

即通过原型链继承私有的属性方法， 通过构造函数继承共享的属性方法。