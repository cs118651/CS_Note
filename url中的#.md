## url中的##

#### 1.#的含义

代表网页中的一个位置。其右面的字符，就是该位置的标识符。比如，

```
http://www.example.com/index.html#print
```

就代表网页index.html的print位置。浏览器读取这个URL后，会自动将print位置滚动至可视区域。

为网页位置指定标识符，有两个方法。一是使用锚点，比如<a name="print"></a>，二是使用id属性，比如<div id="print" >。

#### 2.HTTP请求不包括####

'#'是用来指导浏览器动作的，对服务器端完全无用。所以，HTTP请求中不包括#。

比如，访问下面的网址，

```
http://www.example.com/index.html#print
```

实际的http请求是这样的：

```
GET /index.html HTTP/1.1
Host: www.example.com
```

#### 3.#后面的字符

在第一个#后面出现的任何字符，都会被浏览器解读为位置标识符。这意味着，这些字符都不会被发送到服务器端。

比如，下面URL的原意是指定一个颜色值：

```
http://www.example.com/?color=#fff
```

但是，浏览器实际发出的请求是：

```
GET /?color= HTTP/1.1
Host: www.example.com
```

只有将#转码为 `%23` 才能被正确识别

#### 4.改变#不触发网页重载但会改变浏览器访问历史

```
http://www.example.com/index.html#location1
```

到：

```
http://www.example.com/index.html#location2
```

浏览器并不会像服务器重新请求index.html。

但是，每一次改变#后的部分，都会在浏览器历史中增加一个记录，这，利用这一特性，就可以解决ajax中前进后退的问题。

#### 5.web API

利用 `window.location.hash` 属性来读取或写入#值。

读取时可用于判断网页状态是否改变，写入时则会在不重载页面的情况下创造一条历史访问记录。

利用HTML5新增的全局事件 `onhashchange` 来监听hash值的变化，hash一旦变化就会触发这个事件。使用方法有三种：

```
window.onhashchange = func;
<body onhashchange="func();">
window.addEventListener("hashchange", func, false);
```

