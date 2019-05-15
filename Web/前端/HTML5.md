# HTML5

---

#### HTML5视频

视频格式：
Ogg：带有Theora视频编码和Vorbis音频编码的Ogg文件
MPEG4：带有H.264视频编码和AAC音频编码的MPEG4文件
WebM：带有VP8视频编码和Vorbis音频编码的WebM文件

`<video src="movie.ogg" controls="controls">`的controls属性供添加播放、暂停和音量控件，不设置的话将会没有这些控件

**video元素允许多个source元素，可以链接不同的视频文件，浏览器将使用第一个可识别的格式**

---

#### HTML5表单元素的keygen元素

keygen元素的作用是提供一种验证用户的可靠方法

keygen元素是密钥对生成器key-pair generator，当提交表单时，生成两个键，一个是私钥，一个是公钥

私钥private key存储于客户端，公钥public key则发送到服务器，公钥可用于之后验证用户的客户端证书client certificate

目前支持度比较糟糕

#### HTML5的output元素

```html
<output id="result" onforminput="resCalc()"></output>
```

实现简单计算输出(不是很清楚)

---

#### HTML5 Canvas

用于在网页上绘制图形，canvas元素使用JavaScript在网页上绘制图像

画布是一个矩形区域，可以控制其每一像素，canvas拥有多种绘制路径、矩形、圆形、字符以及添加图像的方法

向HTML5页面添加canvas元素，规定元素的id、宽度和高度：
`<canvas id="myCanvas" width="200" height="100"></canvas>`

#### 通过JavaScript来绘制

canvas元素本身是没有绘图能力的，所有的绘制工作必须在JavaScript内部完成

```html
<script type="text/javascript">
    var c = documnet.getElementById("myCanvas");
    var cxt = c.getContext("2d");
    cxt.fillStyle = "#FF0000";
    cxt.fillRect(0,0,150,75);
</script>
```

---

#### HTML5 内联SVG

SVG指**可伸缩矢量图像**Scalable Vector Graphics，用于定义用于网络的基于矢量的图形，使用XML格式定义图形，在放大或者改变尺寸的情况下其图形质量不会有损失，是万维网联盟的标准

#### SVG的优势

+ SVG可以通过文本编辑器来创建和修改
+ SVG图像可被搜索、索引、脚本化或压缩
+ SVG是可伸缩的
+ SVG图像可在任何的分辨率下被高质量地打印
+ SVG可在图像质量不下降的情况下被放大

在HTML5中，能够将SVG元素直接嵌入HTML页面中：

```html
<!DOCTYPE html>
<html>
    <body>
        <svg xmlns="http://www.w3.org/2000/svg" version="1.1" height="190">
            <polygon points="100,10 40,180 190,60 10,60 160,180" style="fill:lime;stroke:purple;stroke-width:5;fill-rule:evenodd;" />
        </svg>
    </body>
</html>
```

---

#### SVG和Canvas

都允许在浏览器中创建图形，但本质上是不同的

+ SVG是一种使用XML描述2D图形的语言，SVG基于XML，意味着SVG DOM中的每个元素都是可用的，可以为某个元素附加JavaScript事件处理器。SVG中每个被绘制的图形均被视为是对象，如果SVG对象的属性发生变化，那么浏览器能够自动重现图形
+ Canvas通过JavaScript来绘制2D图形，是逐像素进行渲染的。一旦图形被绘制完成，他就不会继续得到浏览器的关注，如果其位置发生变化，那么整个场景也需要重新绘制，包括任何或许已被图形覆盖的对象

#### Canvas

依赖分辨率，不支持事件处理器，弱的文本渲染能力，能够以.png或.jpg格式保存结果图像。最适合图像密集型的游戏，其中的许多对象会被频繁重绘

#### SVG

不依赖分辨率，支持事件处理器，最适合带有大型渲染区域的应用程序(比如谷歌地图)，复杂度高会减慢渲染速度(任何过度使用DOM的应用都不快)，不适合游戏应用

---

#### HTML多媒体

Web上的多媒体指的是音效、音乐、视频和动画，现代网络浏览器已支持很多多媒体格式

#### 视频格式

**AVI**	.avi 	Audio Video Interleave格式，微软，所有Windows的计算机都支持AVI格式
**WMV** 	.wmv 	Windows Media格式，微软，需要安装额外的(免费)组件
**MPEG**	.mpg/.mpeg 	Moving Pictures Expect Group格式是因特网上最流行的格式
**QuickTime** 	.mov	苹果公司
**ReadVideo** 	.rm/.ram 	RealVideo格式是由Read Media针对因特网开发的
**Flash** 	.swf/.flv 	Flash（Shockwave）格式是由Macromedia开发的，需要额外组件
**Mpeg-4**	.mp4	Mpeg-4(with H.264 video compression)一种针对因特网的新格式

#### 声音格式

**MIDI** 	.mid/.midi	MIDI(Musical Instrument Digital Interface)是一种针对电子音乐设备的格式，不含声音，但包含可被电子产品(比如声卡)播放的数字音乐指令，例如一个几十KB大小的文件可以播放几分钟，得到了广泛的平台上的大量软件的支持
**RealAudio**	.rm/.ram	Real Media针对因特网开发的，也支持视频，该格式允许低带宽条件下的音频流(在线音乐、网络音乐)
**Wave** 	.wav	Wave(waveform)格式是由IBM和微软开发的
**WMA**	.wma	Windows Media Audio，质量优于MP3，兼容大多数播放器，WMA文件可作为连续的数据流来传输，这使它对于网络电台或在线音乐很实用
**MP3**	.mp3/.mpga	MP3文件实际上是MPEG文件的声音部分，MPEG格式最初是由运动图像专家组开发的，MP3是其中最受欢迎的针对音乐的声音格式

WAVE是因特网上最受欢迎的**无压缩**声音格式，所有流行的浏览器都支持。如果您需要未经压缩的声音(音乐或演讲)，那么您应该使用WAVE格式

MP3是最新的**压缩**录制音乐格式，MP3这个术语已经成为数字音乐的代名词

---

#### HTML Object元素

`<object>`作用是支持HTML助手(插件)

辅助应用程序helper application是可由浏览器启动的程序，辅助应用程序也称为插件

辅助程序可用于播放音频和视频以及其他，辅助程序是使用`<object>`标签来加载的。

使用QuickTime来播放Wave音频

```html
<object width="420" height="360" classid="clsid:02BF25D5-8C17-4B23-BC80-D3499ABDDC5J" codebase="http://www.apple.com/qtactivex/qtplugin.cab">
<param name="src" value="bird.wav" />
<param name="controller" value="true" />
</object>
```

...

---

#### 播放音频

+ 使用插件：浏览器插件是一种扩展浏览器标准功能的小型计算机程序，可以使用`<object>`或`<embed>`标签来将插件添加到HTML页面，这些标签定义资源(通常非HTML资源)的容器，根据类型，它们即会由浏览器显示，也会由外部插件显示

+ 使用`<embed>`标签定义外部（非HTML）内容的容器
  `<embed height="100" width="100" src="song.mp3" />`

+ 使用`<object>`标签也可以定义外部（非HTML）内容的容器
  `<object height="100" width="100" data="song.mp3"></object>`

+ 使用HTML5`<audio>`元素

  ```html
  <audio controls="controls">
      <source src="song.mp3" type="audio/mp3" />
      <source src="song.ogg" type="audio/ogg" />
      Your browser does not support this audio format.
  </audio>
  ```

最好的HTML解决方法

```html
<audio controls="controls" height="100" width="100">
    <source src="song.mp3" type="audio/mp3" />
    <source src="song.ogg" type="audio/ogg" />
    <embed height="100" width="100" src="song.mp3" />
</audio>
```

使用两种不同的音频格式，HTML5`<audio>`元素会尝试以mp3或ogg来播放音频，如果失败，代码将**回退尝试**`<embed>`元素

---

#### 使用雅虎媒体播放器

```html
<a href="song.mp3">Play Sound</a>

<script type="text/javascript" src="http://mediaplayer.yahoo.com/js"></script>
```

免费的，如需使用它，需要把这段JavaScript插入网页底部

如果网页包含指向媒体文件的超链接，大多数浏览器会使用“辅助应用程序”来播放文件

#### 内联的声音

当在网页中包含声音，或者作为网页的组成部分时，它被称为内联声音

---

#### HTML视频

+ 使用`<embed>`标签：src属性
+ 使用`<object>`标签：data属性
+ 使用`<video>`标签：source子标签

---

#### HTML5地理定位

HTML5 Geolocation API用于获得用户的地理位置

```html
<script>
    var x = document.getElementById("demo");
    function getLocation() {
        if (navigator.geolocation) {
            navigator.geolocation.getCurrentPosition(showPosition);
        } else {
            x.innerHTML = "Geolocation is not supported by this browser.";
        }
    }
    function showPostion(position) {
        x.innerHTML = "Latitude:" + position.coords.latitude + "<br />Longitude: " + position.coords.longitude;
    }
</script>
```

#### `getCurrentPosition()`方法——返回数据

```html
<script>
    var x = document.getElementById("demo");
    function getLocation() {
        if (navigator.geolocation) {
            navigator.geolocation.watchPosition(showPosition);
        } else {
            x.innerHTML = "Geolocation is not supported by this browser.";
        }
    }
    function showPosition(position) {
        x.innerHTML = "Latitude: " + position.coords.latitude + "<br />Longitude: " + position.coords.longtitude;
    }
</script>
```

---

#### HTML5拖放

拖放是HTML5标准的组成部分，任何元素都是可拖放的，Drag和Drop

```html
<!DOCTYPE html>
<html>
    <head>
        <script>
            function allowDrop(ev) {
                ev.preventDefault();
            }
            function drag(ev) {
                ev.dataTransfer.setData("text", ev.target.id);  //规定被拖动数据的数据类型和值
            }
            function drop(ev) {
                ev.preventDefault();
                var data = ev.dataTransfer.getData("text");
                ev.target.appendChild(document.getElementById(data));
            }
        </script>
    </head>
    <body>
        <div id="div1" ondrop="drop(event)" ondragover="allowDrop(event)"></div>
        <img id="drag1" src="img_logo.gif" draggable="true" ondragstart="drag(event)" width="336" height="69">
    </body>
</html>
```

把元素设置为可拖放`<img draggable="true">`

规定元素被拖动时发生的事情，ondragstart属性调用了一个drag(event)函数，规定拖动什么数据；
dataTransfer.setData()方法设置被拖动数据的数据类型和值。
本例中数据类型为text，值为可拖动元素的id，即drag1

拖到何处ondragover，ondragover事件规定被拖动的数据能够被放置在何处。默认数据元素无法被放置到其他元素中，为了实现拖放，我们必须阻止元素的这种默认的处理方式，由ondragover事件的event.preventDefault()方法完成

进行放置ondrop，当放开被拖数据时，会发生drop事件。调用preventDefault()来阻止数据的浏览器默认处理方式。drop事件的默认行为是以链接形式打开。通过dataTransfer.getData()方法获得被拖的数据，该方法将返回在setData()方法中设置为相同类型的任何数据，即id("drag1")，然后把被拖元素追加到放置元素中

---

#### HTML本地存储

本地存储优于cookies。Local Storage本地存储，web应用程序能够在用户浏览器中对数据进行本地的存储

HTML5之前，应用程序数据只能存储在cookie中，包括每个服务器请求。本地存储则更安全，并且可在不影响网站性能的前提下将大量数据存储于本地。

存储限制要大得多，至少5MB，并且信息不会被传输到服务器

#### HTML本地存储对象

HTML本地存储提供了两个在客户端存储数据的对象

+ window.localStorage——存储没有截至日期的数据
+ window.sessionStorage——针对一个session来存储数据(当关闭浏览器标签页时数据会丢失)

localStorage的名称/值对始终存储为字符串，如果需要要记得转换为其他格式

#### SessionStorage对象

等同于LocalStorage对象，不同之处在于只对一个Session存储数据，如果用户关闭具体的浏览器标签页，数据也会被删除

---

#### HTML5应用程序缓存

通过创建cache manifest文件，可轻松创建web应用的离线版本

HTML5引入了应用程序缓存Application Cache，这意味着可对web应用进行缓存，并可在没有因特网连接时进行访问

如需启用应用程序缓存，请在文档的`<html>`标签中包含manifest属性

```html
<!DOCTYPE html>
<html manifest="demo.appcache">
<body>
    文档内容.....
</body>
</html>
```

每个指定了manifest的页面在用户对其访问时都会被缓存，如果未指定manifest属性，则页面不会被缓存（除非在manifest文件中直接指定了该页面）

manifest文件的建议文件扩展名为 **.appcache**

manifest文件需要设置正确的MIME-type，即**text/cache-manifest**，必须在web服务器上进行配置

Manifest文件是简单的文本文件，它告知浏览器被缓存的内容(以及不缓存的内容)

+ CACHE MANIFEST——在此标题下列出的文件将在首次下载后进行缓存
+ NETWORK——在此标题下列出的文件需要与服务器的连接，且不会被缓存
+ FALLBACK——再此标题下列出的文件规定当前页面无法访问时的回退页面(比如404页面)

```
CACHE MANIFEST
# 2012-02-21 v1.0.0
/theme.css
/logo.gif
/main.js

NETWORK:
# 可以使用星号*来表示所有的资源/文件都需要因特网连接
login.asp

FALLBACK:
# 无法建立网络连接的时候使用"offline.html"代替/html/目录中的所有文件
/html/ /offline.html
```

应用的缓存只会在manifest文件改变时被更新，如果编辑了一幅图像，或者修改了一个JavaScript函数，这些改变都不会被重新缓存，更新注释中的日期和版本号是一种使浏览器重新缓存文件的办法

---

#### HTML5 Web Workers

Web worker是运行在后台的JavaScript，独立于其他脚本，不会影响页面的性能，可以继续做任何愿意做的事情

当在HTML页面中执行脚本时，页面是不可响应的，直到脚本已完成

在一个外部JavaScript文件中创建我们的web worker，即如下demo_workers.js文件:

```html
var i = 0;
function timedCount() {
	i = i + 1;
	postMessage(i);  // 向html页面传回一段消息
	setTimeout("timedCount()", 500);  // 延迟500毫秒后再次进入该函数
}
timedCount();
```

需要从HTML页面调用它，此时web worker位于外部文件中，无法访问下列JavaScript对象：window对象、document对象、parent对象

```html
<!DOCTYPE html>
<html>
    <body>
        <p>Count numbers: <output id="result"></output></p>
        <button onclick="startWorker()">Start Worker</button>
        <button onclick="stopWorker()">Stop Worker</button>
        <br />
       	<br />
<script>
var w;
    
function startWorker() {
    if(typeof(Worker) !== "undefined") {
        if(typeof(w) == "undefined") {
            w = new Worker("demo_workers.js");
        }
        w.onmessage = function(event) {
            document.getElementById("result").innerHTML = event.data;
        };
    } else {
        document.getElementById("result").innerHTML = "Sorry! No Web Worker support.";
    }
}

function stopWorker() { 
    w.terminate();
    w = undefined;
}            
</script>
    </body>
</html>
if (typeof(w) == "undefined") {
	w = new Worker("demo_workers.js")
}
//向web worker添加一个onmessage事件监听器
w.onmessage = function (event) {
	document.getElementById("result").innerHTML = event.data;
};
w.terminate();
w = undefined;
```

当web worker传送消息时，会执行事件监听器中的代码，来自web worker的数据会存储于event.data中

#### 终止Web Worker

创建web worker后，他会继续监听消息，即使在外部脚本完成之后，直到其被终止为止

使用terminate()方法终止web worker并释放浏览器/计算机资源

#### 复用Web Worker

把worker变量设置为undefined，在其被终止后，可以重复使用该代码

---

#### HTML Server-Sent事件

Server-Sent事件允许网页自动从服务器获得更新，以前是网页询问是否有可用的更新，而server-sent事件使得更新能够自动达到

**接收Server-Sent事件通知**

EventSource对象用于接收服务器发送事件通知

```javascript
var source = new EventSource("demo_sse.php")
source.onmessage = function(event) {
    documnet.getElementById("result").innerHTML += event.data + "<br>";
};
```

服务器端代码实例，服务器端事件流的语法非常简单，把Content-Type报头设置为text/event-stream，现在，就可以开始发送事件流了

```php
PHP 中的代码 (demo_sse.php)：

<?php
header('Content-Type: text/event-stream');
header('Cache-Control: no-cache');

$time = date('r');
echo "data: The server time is: {$time}\n\n";
flush();
?>

ASP 中的代码 (VB) (demo_sse.asp)：

<%
Response.ContentType = "text/event-stream"
Response.Expires = -1
Response.Write("data: The server time is: " & now())
Response.Flush()
%>
```

onopen（当通往服务器的连接被打开）

onmessage（当接收到消息）

onerror（当发生错误）