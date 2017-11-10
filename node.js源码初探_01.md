### node.js源码初探_01

​	最近刚开始学习node.js，很多教程都是从 http 这个模块开始讲解一个实例，因为通过 http 模块，我们可以用寥寥几行代码在本地创建一个最简易的 http-server ：

```
//引入http模块
var http = require('http')  

//调用http模块的creatServer方法
//此方法接收一个回调函数作为参数
http.createServer(function (req, res) {
	//回调函数接收 req(request)和res(response) 两个参数
	//调用res的writeHead方法写入http响应头
    res.writeHead(200, {'Content-Type': 'text/plain'})
    //调用res的end方法结束响应 并输出字符串
    res.end('Hello World\n')
}).listen(1337, '127.0.0.1')
// 链式调用http模块的listen方法来监听主机'127.0.0.1'的1337端口
console.log('Server running');
```

在node命令行运行这串代码，然后浏览器输入相应的地址就会看到页面中出现 'Hello World' 字样

这么一个 http-server 就实现完毕了，看上去十分简单，但其背后node.js却做了大量的工作，为了一探究竟，我决定去看一看node.js的 http 模块的源码来初步研究一下这样一个服务端程序做了哪些工作：

首先打开 github 搜索 node.js，找到 http.js 这个文件，先搜索一下关键字 `createServer` ，找到这个函数的定义：

![](http://oxx2l7d61.bkt.clouddn.com/nodejs%E6%BA%90%E7%A0%81%E5%88%9D%E6%8E%A2_01.png)

发现它只是接收了 `requestListener` 回调并将回调传递给一个 `Server` 实例并返回，然后再看一下这个 `Server` 实例是怎么定义的

![](http://oxx2l7d61.bkt.clouddn.com/nodejs%E6%BA%90%E7%A0%81%E5%88%9D%E6%8E%A2_01_2.png)

原来是在代码开头引入的 `_http_server` 模块，接着来看  `_http_server`  模块，打开 _http_server.js 并找到 `Server` 构造函数，发现当接收到回调时，监听了一个 `request` 事件，那这个事件是在哪里触发的呢？接着搜索...

![](http://oxx2l7d61.bkt.clouddn.com/nodejs%E6%BA%90%E7%A0%81%E5%88%9D%E6%8E%A2_01_3.png)

发现在同一文件里的 `parserOnIncoming` 函数中触发了该事件：

![](http://oxx2l7d61.bkt.clouddn.com/nodejs%E6%BA%90%E7%A0%81%E5%88%9D%E6%8E%A2_01_4.png)

写不下去了.... 