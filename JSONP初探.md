#### JSONP初探

  jsonp(json with padding)虽然与json仅一字之差，但意义却大相径庭。json是一种基于文本的数据交换方式，而jsonp则是json的一种“使用模式”，用于解决跨域数据访问。

##### jsonp的产生

1.众所周知，由于浏览器标准中的同源策略，ajax直接请求普通文件会存在跨域无法请求的问题。

2.然而 <script>、<link>、<img> 等等带有href或者src属性的标签却不受同源策略的限制。

3.这样子解决方案就呼之欲出了，web客户端通过与调用脚本一模一样的方式，来调用跨域服务器上动态生成的js格式文件（一般以JSON为后缀），显而易见，服务器之所以要动态生成JSON文件，目的就在于把客户端需要的数据装入进去。

4.客户端在对JSON文件调用成功之后，也就获得了自己所需的数据，剩下的就是按照自己需求进行处理和展现了，这种获取远程数据的方式看起来非常像 `AJAX` ，但其实并不一样。

5.为了便于客户端使用数据，逐渐形成了一种非正式传输协议，人们把它称作JSONP，该协议的一个要点就是允许用户传递一个 `callback` 参数给服务端，然后服务端返回数据时会将这个 `callback` 参数作为函数名来包裹住JSON数据，这样客户端就可以随意定制自己的函数来自动处理返回数据了。

##### jsonp的具体实现

**1.我们知道，哪怕跨域js文件中的代码（当然指符合web脚本安全策略的），web页面也是可以无条件执行的。**

 远程服务器remoteserver.com根目录下有个remote.js文件代码如下：

```
alert('我是远程文件');
```

本地服务器localserver.com下有个jsonp.html页面代码如下：

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title></title>
    <script type="text/javascript" src="http://remoteserver.com/remote.js"></script>
</head>
<body>

</body>
</html>
```

毫无疑问，页面将会弹出一个提示窗体，显示跨域调用成功。

**2.现在我们在jsonp.html页面定义一个函数，然后在远程remote.js中传入数据进行调用。**

**jsonp.html页面代码如下：**

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title></title>
    <script type="text/javascript">
    var localHandler = function(data){
        alert('我是本地函数，可以被跨域的remote.js文件调用，远程js带来的数据是：' + data.result);
    };
    </script>
    <script type="text/javascript" src="http://remoteserver.com/remote.js"></script>
</head>
<body>

</body>
</html>
```

remote.js文件代码如下：

```
localHandler({"result":"我是远程js带来的数据"});
```

运行之后查看结果，页面成功弹出提示窗口，显示本地函数被跨域的远程js调用成功，并且还接收到了远程js带来的数据。很欣喜，跨域远程获取数据的目的基本实现了，但是又一个问题出现了，我怎么让远程js知道它应该调用的本地函数叫什么名字呢？毕竟是jsonp的服务者都要面对很多服务对象，而这些服务对象各自的本地函数都不相同啊？我们接着往下看。

**3.聪明的开发者很容易想到，只要服务端提供的js脚本是动态生成的就行了呗，这样调用者可以传一个参数过去告诉服务端“我想要一段调用XXX函数的js代码，请你返回给我”，于是服务器就可以按照客户端的需求来生成js脚本并响应了。**

看jsonp.html页面的代码：

```
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <title></title>
    <script type="text/javascript">
    // 得到航班信息查询结果后的回调函数
    var flightHandler = function(data){
        alert('你查询的航班结果是：票价 ' + data.price + ' 元，' + '余票 ' + data.tickets + ' 张。');
    };
    // 提供jsonp服务的url地址（不管是什么类型的地址，最终生成的返回值都是一段javascript代码）
    var url = "http://flightQuery.com/jsonp/flightResult.aspx?code=CA1998&callback=flightHandler";
    // 创建script标签，设置其属性
    var script = document.createElement('script');
    script.setAttribute('src', url);
    // 把script标签加入head，此时调用开始
    document.getElementsByTagName('head')[0].appendChild(script); 
    </script>
</head>
<body>

</body>
</html>
```

这次的代码变化比较大，不再直接把远程js文件写死，而是编码实现动态查询，而这也正是jsonp客户端实现的核心部分，本例中的重点也就在于如何完成jsonp调用的全过程。

我们看到调用的url中传递了一个code参数，告诉服务器我要查的是CA1998次航班的信息，而callback参数则告诉服务器，我的本地回调函数叫做flightHandler，所以请把查询结果传入这个函数中进行调用。

OK，服务器很聪明，这个叫做flightResult.aspx的页面生成了一段这样的代码提供jsonp.html（服务端的实现这里就不演示了，与你选用的语言无关，说到底就是拼接字符串）

```
flightHandler({
    "code": "CA1998",
    "price": 1780,
    "tickets": 5
});
```

我们看到，传递给flightHandler函数的是一个json，它描述了航班的基本信息。运行一下页面，成功弹出提示窗口，jsonp的执行全过程顺利完成！