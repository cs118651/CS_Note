# ES6小技巧

> 文章部分摘自 [ES6 的几个小技巧](https://juejin.im/post/5ab9a8196fb9a028b86e0615) 

### 1. 数组方法reduce妙用

相对于map、forEach、filter等函数，我平时用到reduce的场景几乎没有，因为不熟悉，觉得平时用其他的方法就够用了，今日看了掘金上的一篇文章深受启发：

##### 用reduce代替 map + filter

设想你有这么个需求：要把数组中的值进行计算后再滤掉一些值，然后输出新数组。很显然我们一般使用 map 和 filter 方法组合来达到这个目的，但这也意味着你需要迭代这个数组两次。

来看看我们如何使用 reduce 只迭代数组一次，来完成同样的结果。下面这个例子我们需要把数组中的值乘 2 ，并返回大于 50 的值：

```
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

