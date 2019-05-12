# HTML系列教程

---

#### HTML Hyper Text Markup Language超文本标记语言

#### XHTML eXtensible Hyper Text Markup Language可扩展超文本标记语言

更严谨更纯净的HTML版本

#### HTML 5 第五次重大修改HTML

---

#### CSS Cascading Style Sheets 层叠样式表

#### CSS3 最新的CSS标准

---

# HTML

---

#### 一些废话

HTML是**标记语言**，非编程语言，由一套**标记标签**markup tag组成，使用标记标签来描述网页。标签是**尖括号**包围的关键词，**成对**出现，称为**开始标签**和**结束标签**，或**开放标签**和**闭合标签**

**Start tag/End tag**：开始标签/结束标签
**Opening tag/Closing tag**：开放标签/闭合标签

HTML文档也被称为**网页**

```html
<html>和</html>之间的文本描述网页
<body>和</body>之间的文本是可见的页面内容
```

HTML标题由`<h1>`到`<h6>`标签定义的
HTML链接由`<a>`标签进行定义的
HTML图像由`<img>`标签进行定义的

```html
<h1>This is a heading</h1>
<a herf="http://www.w3school.com.cn">This is a link</a>
<img src="w3school.jpg" width="104" height="142"/>
```

---

#### HTML元素

HTML元素指的是从开始标签start tag到结束标签end tag的所有代码
某些HTML元素具有空内容**Empty Content**
空元素**在开始标签中进行关闭**，如`<img src="xx.jpg"/>`
大多数HTML元素可拥有**属性**
大多数HTML元素可以嵌套，即可以包含其他HTML元素

**在开始标签中添加斜杆，比如换行`<br/>`是关闭空元素的正确方法，HTML、XHTML、XML都接受这种方式**

**HTML标签对大小写不敏感，推荐小写**

---

#### HTML属性

属性总是在HTML元素的**开始标签**中规定，且**属性**和**属性值**对大小写不敏感

```html
<a href="http://www.w3school.com.cn">This is a link</a>
<!-- href 为 Hypertext Reference的缩写 -->
<h1 align="center">This is a heading</h1>
<!-- align 为关于对齐方式的附加信息 建议使用样式替代 -->
<body bgcolor="yellow"></body>
<!-- bgcolor 为关于背景颜色的附加信息 建议使用样式替代 
使用<body style="background-color:yellow">代替
-->
<table border="1"></table>
<!-- 关于表格边框的附加信息 -->
```

属性值必须被**引号**包括，单双引号均可

---

#### HTML标题

浏览器会自动的在标题`<h1>heading</h1>`前后添加空行
默认情况下，HTML会自动地在块级元素前后添加一个额外的空行，比如段落、标题元素前后

**标题很重要**，搜索引擎使用标题为您的网页的结构和内容编制索引

HTML水平线为`<hr />`可用于分隔内容，`<br />`换行

---

#### HTML段落

浏览器会自动地在段落的前后添加空行

无法通过在HTML代码中添加额外的空格或换行来改变输出的效果，显示页面时，浏览器会移除源代码中多余的空格和空行，所有连续的空格或空行（换行）都会被算作是一个空格。且调节浏览器的大小会改变段落的行数

```html
<p>      
		a
    	b
</p>
<!-- 输出为`a b` -->
```

---

#### HTML样式

style属性用于改变HTML元素的样式，提供了一种改变所有HTML元素的样式的通用方法，可以通过style属性直接将样式添加到HTML元素，或者间接地在独立的样式表中CSS文件中进行定义

`align/bgcolor/color`属性请使用样式代替`style="text-align:center;background-color:yellow;color:red;font-family:verdana"`

---

#### HTML文本格式化

HTML可定义很多供格式化输出的元素，比如粗体和斜体字

---

#### HTML引用

引用Quotation即`<q>`用于短的引用，浏览器通常会为`<q>`元素包围引号
`<blockquote>`用于长引用，通常会对`<blockquote>`元素进行**缩进**处理
用于缩略词的HTML`<abbr>`，对缩写进行标记能够为浏览器、翻译系统以及搜索引擎提供有用的信息
用于定义的HTML`<dfn>`定义项目或缩写的**定义**
用于联系信息的HTML`<address>`
用于著作标题的HTML`<cite>`

用于双向重写的HTML`<bdo>`定义双流向覆盖bi-directional override`<bdo>`元素用于覆盖当前文本方向

---

#### HTML计算机代码元素

键盘格式`<kbd>`定义键盘输入
样本格式`<samp>`定义计算机输出示例
代码格式`<code>`定义编程代码示例，`<code>`元素**不保留**多余的**空格**和**折行**，如需保留，则需要使用`<pre>`元素包围代码
变量格式化`<var>`定义数学变量

---

#### HTML注释

条件注释定义只有Internet Explorer执行的HTML标签

```html
<!--[if IE 8]>
	.... some HTML here ....
<![endif]-->
```

---

#### HTML CSS

通过使用HTML4.0，所有的格式化代码均可移出HTML文档，然后移入一个独立的样式表

当浏览器读到一个样式表，它就会按照这个样式表来对文档进行格式化，有三种方式来插入样式表：

+ 外部样式表
  当样式需要被应用到很多页面的时候，外部样式表将是理想的选择，使用外部样式表，可以通过更改一个文件来改变整个站点的外观

  ```html
  <head>
      <!-- link 定义资源引用 -->
      <link rel="stylesheet" type="text/css" href="mystyle.css">
  </head>
  ```

+ 内部样式表
  当单个文件需要特别样式时，就可以使用内部样式表。可以在head部分通过`<style>`标签定义内部样式表

  ```html
  <head>
      <style type="text/css">
          body {background-color: red}
          p {margin-left: 20px}
      </style>
  </head>
  ```

+ 内联样式
  当特殊的样式需要应用到个别元素时，就可以使用内联样式。使用内联样式的方法是在相关的标签中使用样式属性。样式属性可以包含任何CSS属性

  ```html
  <p style="color: red; margin-left: 20px">
      This is paragraph
  </p>
  ```

---

#### HTML链接

HTML超链接可以是一个字、一个词、一组词、也可以是一幅画像，可以点击这些内容来跳转到新的文档或者当前文档中的某个部分

使用`<a>`标签的方式：

+ 通过使用href属性，创建指向另一个文档的链接
+ 通过使用name属性，创建文档内的书签

不必一定是文本，图片或其他HTML元素都可以成为链接

使用Target属性，可以定义被链接的文档在何处显示，`target="_blank"`会在新的窗口打开文档

name属性规定**锚**anchor的名称，当使用**命名锚**的时候，可以创建直接跳至该命名锚（比如页面中的某个小节）的链接，这样使用者就无需不停地滚动页面来寻找他们需要的信息了

可以使用id属性来替代name属性，命名锚同样有效

```html
<a name="tips">基本的注意事项</a>
<!-- 使用# -->
<a href="#tips">有用的提示</a>
<a href="http://www.xxx.com.cn/html/html_links.asp#tips">有用的提示</a>
<!-- 也可以链接邮箱 -->
```

如果浏览器找不到已定义的命名锚，那么就会定位到文档的顶端，不会有错误发生

请始终将正斜杠添加到**子文件夹**，假如这样写`href="http://www.xxx.com.cn/html"`，就会向服务器产生两次HTTP请求，服务器会添加正斜杠到这个地址，然后创建一个新的请求`href="http://www.xxx.com.cn/html/"`

---

#### HTML图像

`<img>`空标签，只有属性，没有闭合标签。使用源属性src(Source)，源属性的值是图像的URL地址，浏览器将图像显示在文档中图像标签出现的地方

alt替换文本属性，用来为图像定义一串预备的可替换的文本，替换文本属性的值是用户定义的。在浏览器无法载入图片时，替换文本属性告诉读者他们失去的信息，此时，浏览器将显示这个替代性的文本而不是图像

HTML的body可以添加图片`<body background="/i/background.jpg">`，但是图片小于页面时，图片会进行重复

id和name有时候差不多

`<map>`定义图像地图；`<area>`定义图像地图中的可点击区域

```html
<img src="/xx.jpg" border="0" usemap="#planetmap" alt="Planets" />
<map name="planetmap" id="planetmap">
    <area 
    shape="circle"
    coords="180,139,14"
    href="/example/html/venus.html"
    target="_blank"
    alt="Venus"/>
    ...
</map>
```

---

#### HTML表格

表格由`<table>`标签来定义，每个表格均有若干行，由`<tr>`标签定义，每行被分割为若干个单元格，由`<td>`标签定义，td表示Table Data，即数据单元格的内容。数据单元格可以包含文本、图片、列表、段落、表单、水平线、表格等。

表格的表头使用`<th>`标签进行定义，大多数浏览器会把表头显示为粗体居中的文本

空单元格如果什么都没有，浏览器可能无法显示出这个单元格的边框。可以在空单元格中添加一个空格占位符，就可以将边框显示出来。空格占位符为`&nbsp;`

表格的标题由标签`<caption>我的标题</caption>`表示

横跨两行的单元格用`<th colspan="2">`表示

`<table border="1" cellpadding="10">`表示单元格内容与其边框之间的空白
`<table border="1" cellspacing="10">`表示增加单元格之间的距离
`<table border="1" background="/x/xx.gif">`表示向表格添加背景
`<td border="1" background="/x/xx.gif">`表示向单元格添加背景
`<td align="left">`排列单元格内容
`<table frame="below">`控制围绕表格的边框

---

#### HTML列表

无序列表`<ul>`每个列表项`<li>`，列表项内部可以使用段落、换行符、图片、链接以及其他列表等

有序列表`<ol>`每个列表项`<li>`，列表项内部可以使用段落、换行符、图片、链接以及其他列表等

定义列表
不仅仅是一列项目，而是项目及其注释和组合，以`<dl>`开始，每个自定义列表项`<dt>`开始，每个自定义列表项的定义以`<dd>`开始，定义列表的列表项内部可以使用段落、换行符、图片、链接以及其他列表等

---

#### HTML `<div>`和`<span>`

可以通过`<div>`和`<span>`将HTML元素组合起来

大多数HTML元素被定义为块级元素或内联元素，而块级元素在浏览器显示时，通常会以新行来开始和结束

HTML的`<div>`元素是块级元素，是可用于组合其他HTML元素的容器

`<div>`可用于对大的内容块设置样式属性，另一个常见的用途是文档布局

HTML的`<span>`元素是内联元素，可用作文本的容器，可用于为部分文本设置样式属性

---

# HTML类

对HTML进行分类(设置类)，使我们能够为元素的类定义CSS样式
为相同的类设置相同的样式，或者为不同的类设置不同的样式

分类块级元素

```html
<!DOCTYPE html>
<html>
    <head>
        <style>
            .cities {
                background-color:black;
                color:white;
                margin:20px;
                padding:20px;
            }
        </style>
    </head>
    
    <body>
        <div class="cities">
            <h2>London</h2>
            <p>London is the capital city of England. It is the most populous city in the United Kingdom, with a metropolitan area of over 13 million inhabitants.</p>
        </div>
        
        <div class="cities">
            <h2>Paris</h2>
            <p>Paris is the capital and most populous city of France.</p>
        </div>
    </body>
</html>
```

分类行内元素

`<span>`元素能够用作文本的容器，设置`<span>`元素的类，能够为相同的`<span>`元素设置相同的样式

```html
<!DOCTYPE html>
<html>
    <head>
        <style>
            span.red {color:red;}
        </style>
    </head>
    
    <body>
        <h1> My <span class="red">Important</span> Heading</h1>
    </body>
</html>
```

---

# HTML布局

使用`<div>`元素的HTML布局，`<div>`元素常用作布局工具，因为能够轻松地通过CSS对其进行定位

```html
<!DOCTYPE html>
<html>
    <head>
        <style>
            #header {
                background-color:black;
                color:white;
                text-align:center;
                padding:5px;
            }
            #nav {
                line-height:30px;
                background-color:#eeeeee;
                height:300px;
                width:100px;
                float:left;
                padding:5px;
            }
            #section {
                width:350px;
                float:left;
                padding:10px;
            }
            #footer {
                background-color:black;
                color:white;
                clear:both;
                text-align:center;
                padding:5px;
            }
        </style>
    </head>
    <body>
        <div id="header">
        	<h1>City Gallery</h1>
    	</div>
    	<div id="nav">
        	London<br>
        	Paris<br>
        	Tokyo<br>
    	</div>
   	 	<div id="section">
        	<h1>London</h1>
        	<p>London is the capital city of England. It is the most populous city in the United Kingdom, with a metropolitan area of over 13 million inhabitants.</p>
        	<p>Standing on the River Thames, London has been a major settlement for two millennia, its history going back to its founding by the Romans, who named it Londinium.</p>
    	</div>
    	<div id="footer">
        	Copyright xxx.com.cn
    	</div>
    </body>
</html>
```

HTML5提供的新语义元素定义了网页的不同部分

header定义文档或节的页眉；nav定义导航链接的容器；section定义文档中的节；article定义独立的自包含文章；aside定义内容之外的内容(比如侧栏)；footer定义文档或节的页脚；details定义额外的细节；summary定义details元素的标题

即使用

```html
<head>
    <style>
        header {
            background-color:black;
            color:white;
            text-align:center;
            padding:5px;
        }
        nav {
            line-height:30px;
            background-color:#eeeeee;
            height:300px;
            ...
        }
    </style>
</head>
<body>
    <header>
        <h1>City Gallery</h1>
    </header>
    <nav>
        ...
    </nav>
</body>
```

---

# HTML响应式Web设计

Responsive Web Design响应式Web设计RWD
RWD能够以可变尺寸传递网页
RWD对于平板和移动设备是必须的

一是直接自己创建响应式设计

二是使用Bootstrap，即使用现成的CSS框架

----

#### Bootstrap

是最流行的开发响应式Web的HTML，CSS和JS框架，Bootstrap帮助您开发在任何尺寸都外观出众的站点：显示器、笔记本电脑、平板电脑和手机等

```html
<!DOCTYPE html>
<html>
    <head>
        <meta charset="utf-8">
        <meta name="viewport" content="width=device-width, initial-scale=1">
        <link rel="stylesheet" href="http://maxcdn.bootstrapcdn.com/bootstrap/3.2.0/css/bootstrap.min.css">
    </head>
    <body>
        ...
        <!-- Bootstrap教程 -->
    </body>
</html>
```

---

# HTML框架

通过使用框架，可以在同一个浏览器窗口中显示不止一个页面

每份HTML文档称为一个框架，并且每个框架都独立于其他的框架

**框架结构标签** `<frameset>`

+ 框架结构标签`<frameset>`定义如何将窗口分割为框架
+ 每个frameset定义了一系列行或列
+ rows/columns的值规定了每行或每列占据屏幕的面积

Frame标签定义了放置在每个框架中的HTML文档

```html
<frameset cols="25%,75%">
    <frame src="frame_a.html" />
    <frame src="frame_b.html" />
</frameset>
<!-- 可在<frame>标签中加入noresize="noresize"防止用户拖拽其大小 -->
```

**不能将`<body>`标签和`<frameset>`标签同时使用**

对不支持框架的浏览器添加`<noframes>`标签，如果想添加包含一段文本的`<noframes>`标签，就必须要将这段文字嵌套于`<body></body>`标签中

#### 导航框架

```html
<!-- 导航框架 -->
<!--  -->
<frameset cols="30%,70%">
    <frame src="/example/html/html_contents.html" />
    <frame src="/example/html/frame_a.html" name="showframe" />
</frameset>
```

在`html_contents.html`中写入

```html
<p> <a href="fram_b.html" target="showframe">Frame A</a> </p>
```

---

# HTML Iframe

用于在网页内显示网页，内联框架

```html
<iframe src="demo_iframe.html" width="200" height="200" frameborder="0"></iframe>
```

某些老式的浏览器不支持内联框架，如果不支持，则iframe是不可见的

src指向隔离页面的位置；height和width属性用于规定iframe的高度和宽度，默认值为像素，也可使用百分比来设定（比如80%）；frameborder属性规定是否显示iframe周围的边框，0为移除边框

iframe可以作为链接的目标target，链接的target属性必须引用iframe的name属性

```html
<iframe src="demo_iframe.html" name="iframe_a"></iframe>
<p><a herf="http://www.baidu.com" target="iframe_a">XXX.com.cn</a></p>
```

---

#### ~~HTML背景~~应该使用CSS来定义HTML元素的布局和显示属性

好的背景使站点看上去特别棒

`<body>`拥有两个配置背景的标签

```html
<body bgcolor="#000000" text="yellow"></body>
<body bgcolor="rgb(0,0,0)"></body>
<body bgcolor="black"></body>

<body background="clouds.gif"></body>
<body background="http://www.xx.com.cn/clouds.gif">
```

**图像文件不应超过10k**

---

#### HTML脚本

JavaScript使HTML页面具有更强的动态和交互性

应对不支持或禁用脚本的浏览器使用`<noscript>Sorry, your browser does not support JavaScript!</noscript>`

`<script>`标签用于定义客户端脚本，既可以包含脚本语言，也可以通过src属性指向外部脚本文件

必须的type属性规定脚本的MIME类型

JavaScript最常用于图片操作、表单验证以及内容动态更新

`<noscript>`标签提供无法使用脚本时的替代内容，只有在浏览器不支持脚本或者禁用脚本时，才会显示noscript元素中的内容

#### 如何应付老式的浏览器

老式浏览器不认识`<script>`标签，所以会将标签所包含的内容以文本方式显示在页面上，为了避免这种情况，应该将脚本隐藏在注释标签当中，新的浏览器会读懂这些脚本，并执行他们，即使代码被嵌套在注释标签内

```html
<script type="text/javascript">
    <!--
    document.write("Hello World!")
    //-->
</script>
```

---

#### HTML头部元素



#### 