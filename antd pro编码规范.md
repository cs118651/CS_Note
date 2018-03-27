- ##### if后空格



- ##### for后空格



- ##### switch后空格



- ##### 对象每一项都加逗号


```
var obj = {
  name: 'cs',
  age: '20',
}
```



- ##### for in循环要判断key是否是被循环对象自身属性


```
for (key in obj) {
  if (obj.hasOwnProperty(key)) {
    ...
  }
}
```



- ##### 没有被重新分配( reassigned )的变量都用 `const` 声明



- ##### `=>` 前后都要空格



- ##### import语句放在一起，且最后一条import后要一个空行

##### - 箭头函数的参数需要一个括号

```
// bad
arr.forEach(item => {
  ...
})
// good
arr.forEach((item) => {
  ...
})
```





