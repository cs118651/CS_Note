### JS中的同步和异步1

#### 1.基本概念

先来看一段同步的例子

```
console.log(100)
alert(200)
console.log(300)
```

运行结果：

```
100
弹窗 200
300  //点击弹窗之后
```

可以看到程序按顺序执行，在 `alert(200)` 时程序暂停执行，等待用户点击弹窗之后，再执行下面的代码，这就是同步的例子。

再来看看异步的例子：

```
console.log(100)
setTimeout(function () {
	console.log(200);
},2000)
console.log(300)
```

运行结果：

```
100
300
200 //2s之后
```

程序执行过程：

```
1.执行第1行，打印100
2.执行第2行 setTimeout函数会被暂存到异步队列而不立即执行
3.执行最后一行，打印300
4.当所有同步操作执行完之后，线程会立即查看异步队列中的程序
5.发现异步队列中的setTimeout函数，并执行
```

这里 `setTimeout()` 函数就是一个异步的操作，它不会阻塞程序的运行，而是等待程序执行完同步操作之后再执行。

#### 2.异步的使用场景

1.定时任务：setTimeout、setInterval

2.网络请求：ajax请求、动态 <img> 加载

```
console.log('start')
$.get('./data1.json', function (data1) {
	console.log(data1)
})					  
console.log('end')  
//会先打印'start'再打印'end'再等待请求数据，网络请求结束时打印'data1'
```

3.事件绑定

```
console.log('start')
$('#btn1').on('click', function () {
	alert('clicked')
})				  
console.log('end') 
//会先打印'start'再打印'end'，再等待用户点击，当点击时弹出'clicked'
```

