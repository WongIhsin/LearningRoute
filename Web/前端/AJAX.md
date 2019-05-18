# AJAX

Ajax不是JavaScript的规范，它只是一个人的发明的缩写，Asynchronous JavaScript and XML，意思是用JavaScript执行异步网络请求

Form表单的提交，一旦用户点击Submit按钮，表单开始提交，浏览器就会刷新页面，然后再新的页面里告诉你操作是成功还是失败。如果网络很慢，会得到一个404页面

一次HTTP请求对应一个页面

如果要让用户留在当前页面中，同时发出新的HTTP请求，就必须使用JavaScript发送这个新请求，收到数据后，再用JavaScript更新页面，这样一来，用户感觉自己仍然停留在当前页面，但是数据却可以不断地更新

最早大规模使用AJAX的就是Gmail，Gmail的页面再首次加载后，剩下的数据都依赖于AJAX来更新

用JavaScript写一个完整的AJAX代码并不复杂，但需要注意：Ajax请求是异步执行的，也就是说，要通过回调函数获得响应

在现代浏览器上写AJAX主要依靠XMLHttpRequest对象

```javascript
'use strict';
function success(text) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = text;
}
function fail(code) {
    var textarea = document.getElementById('test-response-text');
    textarea.value = 'Error code: ' + code;
}
var request = new XMLHttpRequest(); // 建立新XMLHttpRequest对象
request.onreadystatechange = function () { // 状态发生变化时，函数被回调
    if (request.readyState === 4) { // 成功完成
        //判断响应结构：
        if (request.status === 200) {
            //成功，通过responseText拿到响应的文本：
            return success(request.responseText);
        } else {
            //失败，根据响应码判断失败原因
            return fail(request.status);
        }
    } else {
        //HTTP请求还在继续...
    }
}
//发送请求
request.open('GET', '/api/categories');
request.send();
alert('请求已发送，请等待响应...')
```

对于低版本的IE，需要换一个ActiveXObject对象

```javascript
var request = new ActiveXObject('Microsoft.XMLHTTP'); // 新建Microsoft.XMLHTTP对象
//后续同上
```

混合标准写法和IE写法在一起，可以这么写

```javascript
var request;
if (window.XMLHttpRequest) {
    request = new XMLHttpRequest();
} else {
    request = new ActiveXObject('Microsoft.XMLHTTP');
}
```

`XMLHttpRequest`对象的`open()`方法有3个参数，第一个参数是指定GET还是POST，第二个参数是指定URL，第三个参数是指定是否使用异步调用，默认是true，所以不用写

调用`send()`的时候才是真正的发送请求，GET请求不需要参数，POST请求需要把body部分以字符串或者`FormData`对象传进去

---

#### 安全限制

JavaScript使用的URL，即request.open()使用的URL是相对路径，如何使用其他的绝对路径，比如`http://www.baidu.com`再运行，会报错，这是因为浏览器的**同源策略**导致的

默认情况下，JavaScript在发送AJAX请求时，URL的域名必须和当前页面完全一致

域名要相同、协议要相同、端口号要相同，部分浏览器可能会允许端口不同，不过大多数浏览器都会严格遵守这个限制

不过JavaScript还是可以请求外域的资源的，多种方法：

+ 一是通过Flash插件发送HTTP请求，这种方式可以绕过浏览器的安全限制，但必须安装Flash，并且和Flash交互，不过现在Flash用的越来越少

+ 二是通过在同源域名下架设一个代理服务器来转发，JavaScript负责把请求发送到代理服务器，代理服务器再把结果返回，但这需要服务器端额外做开发**`/proxy?url=http://www.sina.com.cn`**

+ 三是JSONP，限制是只能使用GET请求，并且要求返回JavaScript，这种方式跨域实际上是利用了浏览器允许跨域引用JavaScript资源

  JSONP通常以函数调用的形式返回，例如返回的JavaScript内容如下`foo('data');`，这样一来，如果在页面中先准备好`foo()`函数，然后给页面动态添加一个`<script>`节点，相当于动态读取外域的JavaScript资源，最后就等着接收回调了

+ 

#### JSONP

```javascript
//以163的股票查询URL为例，对于URL:http://api.money.126.net/data/feed/0000001,1399001?callback=refreshPrice,会得到如下返回
// refreshPrice({"0000001":{"code":"0000001",...});

//需要首先再页面中准备好回调函数：
function refreshPrice(data) {
    var p = document.getElementById('test-jsonp');
    p.innerHTML = '当前价格: ' + 
        data['0000001'].name + ': ' + 
        data['0000001'].price + '; ' + 
        data['1399001'].name + ': ' + 
        data['1399001'].price;
}
//最后用getPrice()函数触发
function getPrice() {
    var 
    	js = document.createElement('script'),
        head = document.getElementsByTagName('head')[0];
    js.src = 'http://api.money.126.net/data/feed/0000001,1399001?callback=refreshPrice';
    head.appendChild(js);
}
//就完成了跨域加载数据
```

---

#### CORS

如果浏览器支持HTML5，就可以一劳永逸地使用新的跨域策略CORS了

CORS：Cross-Origin Resource Sharing

Origin表示本域，也就是浏览器当前页面的域，当JavaScript向外域发起请求后，浏览器收到响应后，首先检查`Access-Control-Allow-Origin`是否包含本域，如果是，则此次跨域请求成功，如果不是，则请求失败，JavaScript将无法获取到响应的任何数据

假设本域是`my.com`，外域是`sina.com`，只要响应头`Access-Control-Allow-Origin`为`http://my.com`或者是`*`，本次请求就可以成功

#### 可见，跨域能否成功，取决于对方服务器是否愿意给你设置一个正确的Access-Control-Allow-Origin，决定权始终在对方手中

这种跨域请求，称为“简单请求”，简单请求包括GET/HEAD和POST（POST的Content-Type类型仅限`application/x-www-form-urlencoded`、`multipart/form-data`和`text/plain`），并且不能出现任何自定义头，通常能满足90%的需求

最新的浏览器全面支持HTML5

**CDN** Content Delivery Network内容分发网络

在引用外部资源时，除了JavaScript和CSS外，都要验证CORS，例如当你引用了某个第三方CDN上的字体文件时：

```css
/*CSS*/
@font-face {
    font-family: 'FontAwesome';
    src: url('http://cdn.com/fonts/fontawesome.ttf') format('truetype');
}
```

如果该CDN服务商未正确设置`Access-Control-Allow-Origin`，那么浏览器无法加载字体资源

对于非简单请求PUT、DELETE以及其他类型的如`application/json`的POST请求，在发送AJAX请求之前，浏览器会先发送一个`OPTIONS`请求（称为preflighted请求）到这个URL上，询问目标服务器是否接受

```
OPTIONS /path/to/resource HTTP/1.1
Host: bar.com
Origin: http://my.com
Access-Control-Request-Method: POST
```

服务器必须响应并明确指出允许的Method

```
HTTP/1.1 200 OK
Access-Control-Allow-Origin: http://my.com
Access-Control-Allow-Methods: POST, GET, PUT, OPTIONS
Access-Control-Max-Age: 86400
```

浏览器确认服务器响应的`Access-Control-Allow-Methods`头确实包含将要发送的AJAX请求的Method，才会继续发送AJAX，否则，抛出一个错误

由于POST、PUT方式传送JSON格式的数据在REST中很常见，所以要跨域正确处理POST和PUT请求，服务器端必须正确响应OPTIONS请求

## Promise

浏览器事件，都必须是异步执行，异步执行可以用回调函数实现

```javascript
function callback() {
    console.log('Done');
}
console.log('before setTimeout()');
setTimeout(callback, 1000);
console.log('after setTimeout()');
//输出为
//before setTimeout()
//after setTimeout()
//Done
```

异步操作会在将来的某个时间点触发一个函数调用，AJAX就是典型的异步操作

```javascript
request.onreadystatechange = function () {
    if (request.readyState === 4) {
        if (request.status === 200) {
            return success(request.responseText);
        } else {
            return fail(request.status);
        }
    }
}
// 不好看，且不利于代码复用
// 希望能使用下面这样
//var ajax = ajaxGet('http://...');
//ajax.ifSuccess(success).ifFail(fail);
```

Promise对象，承诺将来会来执行的对象，Promise有各种实现，ES6中被统一规范，由浏览器直接支持。

可见Promise最大的好处是在异步执行的流程中，把执行代码和处理结果的代码清晰地分离了

```javascript
function test(resolve, reject) {
    var timeOut = Math.random() * 2;
    log('set timeout to: ' + timeOut + ' seconds.')
    setTimeout(function () {
        if (timeOut < 1) {
            log('call resolve()...');
            resolve('200 OK');
        }
        else {
            log('call reject()...');
            reject('timeout in ' + timeOut + ' seconds.')
        }
    }, timeOut * 1000);
}
```

有了执行函数，就可以用一个Promise对象来执行它，并在将来某个时刻获得成功或失败的结果

```javascript
var p1 = new Promise(test);
var p2 = p1.then(function (result) {
    console.log('成功: ' + result);
});
var p3 = p2.catch(function (reason) {
    console.log('失败: ' + reason);
});
//或者串联起来
new Promise(test).then(function (result) {
    console.log('成功: ' + result);
}).catch(function (reason) {
    console.log('失败: ' + reason);
});
```

Promise还可以做更多的事情，比如有若干个异步任务，需要先做任务1，如果成功再做任务2，任何失败则不再继续并执行错误处理函数

```javascript
job1.then(job2).then(job3).catch(handleError);
```

setTimeout可以看成一个模拟网络等异步执行的函数

用Promise简化Ajax异步处理

```javascript
'use strict';
//ajax函数将返回Promise对象
function ajax(method, url, data) {
    var request = new XMLHttpRequest();
    return new Promise(function (resolve, reject) {
        request.onreadystatechange = function () {
            if (request.readyState === 4) {
                if (request.status === 200) {
                    resolve(request.responseText);
                } else {
                    reject(request.status);
                }
            }
        };
        request.open(method, url);
        request.send(data);
    });
}
var log = document.getElementById('test-promise-ajax-result');
var p = ajax('GET', '/api/categories');
p.then(function (text) {
    log.innerText = text;
}).catch(function (status) {
    log.innerText = 'ERROR: ' + status;
});
```

除了串行执行若干异步任务外，Promise还可以并行执行异步任务

```javascript
var p1 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 500, 'P1');
});
var p2 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 600, 'P2');
});
Promise.all([p1, p2]).then(function (results) {
    console.log(results); // 获得一个Array: ['P1', 'P2']
});
// Promise.all([x,x,x,...])多个任务并行执行
```

```javascript
var p1 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 500, 'P1');
});
var p2 = new Promise(function (resolve, reject) {
    setTimeout(resolve, 600, 'P2');
});
Promise.race([p1, p2]).then(function (result) {
    console.log(result); // 'P1'
});
```

组合使用Promise，就可以把很多异步任务以并行和串行的方式组合起来执行

---

#### Canvas

是HTML5新增的组件，就像一块幕布，可以用JavaScript在上面绘制各种图表、动画等

没有Canvas的年代，只能借助Flash插件实现，页面也需要用JavaScript和Flash进行交互，有了Canvas，就不需要Flash了，直接使用JavaScript就可以完成绘制

一个Canvas定义了一个指定尺寸的矩形框，在这个范围内可以随意绘制

```html
<canvas id="test-canvas" width="300" height="200">
    <p>Current Price: 25.51</p>
</canvas>
```

由于浏览器对HTML5标准支持不一致，所以，通常在`<canvas>`内部添加一些说明性的HTML代码。如果浏览器支持Canvas，就忽略`<canvas>`内部的HTML，如果浏览器不支持Canvas，就显示`<canvas>`内部的HTML

```javascript
var ctx = canvas.getContext('2d');
//HTML5还有一个WebGL规范，允许在Canvas中绘制3D图形
gl = canvas.getContext('webgl');
```

`getContext('2d')`方法让我们拿到一个`CanvasRenderingContext2D`对象，所有的绘图操作都需要通过这个对象完成

Canvas的坐标以左上角为原点，水平向右为X轴，垂直向下为Y轴，以像素为单位，所以每个点都是非负整数

```javascript
'use strict';
var 
    canvas = document.getElementById('test-shape-canvas'),
    ctx = canvas.getContext('2d');
ctx.clearRect(0, 0, 200, 200);
ctx.fillStyle = '#dddddd';
ctx.fillRect(10, 10, 130, 130);
var path = new Path2D();
path.arc(75, 75, 50, 0, Math.PI*2, true);
ctx.strokeStyle = '#0000ff';
ctx.stroke(path);
//绘制文本
ctx.shadowOffsetX = 2;
ctx.shadowOffsetY = 2;
ctx.shadowBlur = 2;
ctx.shadowColor = '#666666';
ctx.font = '24px Arial';
ctx.fillStyle = '#333333';
ctx.fillText('带阴影的文字', 20, 40);
```

Canvas除了能绘制基本的形状和文本，还可以实现动画、缩放、各种滤镜和像素转换等高级操作，如果要实现非常复杂的操作，考虑以下优化方案

+ 通过创建一个不可见的Canvas来绘图，然后将最终绘制结果复制到页面的可见Canvas中；
+ 尽量使用整数坐标而不是浮点数；
+ 可以创建多个重叠的Canvas绘制不同的层，而不是在一个Canvas中绘制非常复杂的图
+ 背景图片如果不变可以直接用img标签并放到最底层

