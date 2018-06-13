# ES6小技巧

> 文章部分摘自 [ES6 的几个小技巧](https://juejin.im/post/5ab9a8196fb9a028b86e0615) 

### 1. 数组方法reduce妙用

相对于map、forEach、filter等函数，我平时用到reduce的场景几乎没有，因为不熟悉，觉得平时用其他的方法就够用了，今日看了掘金上的一篇文章深受启发：

##### 用reduce代替 map + filter

设想你有这么个需求：要把数组中的值进行计算后再滤掉一些值，然后输出新数组。很显然我们一般使用 map 和 filter 方法组合来达到这个目的，但这也意味着你需要迭代这个数组两次。

来看看我们如何使用 reduce 只迭代数组一次，来完成同样的结果。下面这个例子我们需要把数组中的值乘 2 ，并返回大于 50 的值：

```javascript
const numbers = [10, 20, 30, 40];
const doubledOver50 = numbers.reduce((finalList, num) => {
  
  num = num * 2; //double each number (i.e. map)
  
  //filter number > 50
  if (num > 50) {
    finalList.push(num);
  }
  return finalList;
}, []);

doubledOver50; // [60, 80]
```

reduce函数在mdn的定义如下：

![reduce函数定义](http://oxx2s9vy2.bkt.clouddn.com/reduce%E5%87%BD%E6%95%B0%E5%AE%9A%E4%B9%89.png)

将reduce函数的迭代初始值指定为一个空数组(即第二个参数)来存储满足条件的元素集合。

### 使用 reduce 检测括号是否对齐封闭

需求：实现一个接受一个字符串并判断括号是否对齐的函数

思路：考虑使用reduce，将迭代初始值指定为数值0，每次迭代遇到 `(` 时accumulator加1，反之遇到 `)` 时accumulator减1，如果括号是封闭的话，最终返回值应该是0。

```javascript
//Returns 0 if balanced.
const isParensBalanced = (str) => {
  return str.split('').reduce((counter, char) => {
    if(counter < 0) {
      return counter;
    } else if(char === '(') {
      return ++counter;
    } else if(char === ')') {
      return --counter;
    }  else { //matched some other char
      return counter; //<-- 返回counter以便下次迭代
    }
    
  }, 0); //<-- 初始值指定为0
}
```

