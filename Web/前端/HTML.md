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

`<title>`标题定义文档的标题
`<base target="_blank" />`使得页面中的所有标签在新窗口中打开

`<meta name="author" content="xx.com.cn">`定义了创作者
`<meta name="generator" content="Dreamweaver 8.0en">`定义了编辑软件
也可以定义关键词

`<meta http-equiv="Refresh" content="5;url=http://www.xx.com.cn" />`把用户重定向到新的网址

Head元素是所有头部元素的容器，`<head>`内的元素可包含脚本，指示浏览器在何处可以找到样式表，提供元信息，等

---

#### HTML `<title>`元素

title元素在所有HTML/XHTML文档中都是必须的

+ title元素定义浏览器工具栏中的标题
+ 提供页面被添加到收藏夹时显示的标题
+ 显示在搜索引擎结果中的页面标题

#### HTML `<base>` 元素

base标签为页面上的所有链接规定默认地址，页面中的所有链接如果使用了相对路径，那么就会以base标签中规定的地址作为基础地址，如果要使用base标签的href属性，那么这个路径必须是以`/`结尾的，否则这个路径是无效果的
或默认目标target

```html
<head>
    <base href="http://www.baidu.com" />
    <base target="_blank" />
</head>
```

#### HTML`<link>`元素

link标签定义文档与外部资源之间的关系，最常用于连接样式表

```html
<head>
    <link rel="stylesheet" type="text/css" href="mystyle.css" />
</head>
```

#### HTML`<style>`元素

用于为HTML文档定义样式信息

#### HTML `<meta>`元素

元数据metadata是关于数据的信息，提供关于HTML文档的元数据。元数据不会显示在页面上，但是对于机器是可读的

典型的情况是，meta元素被用于规定页面的描述、关键词、文档的作者、最后修改时间以及其他元数据等，元数据可用于浏览器（如何显示内容或重新加载页面），搜索引擎（关键词），或其他web服务

一些搜索引擎会利用meta元素的name和content属性来索引您的页面，name和content属性的作用是描述页面的内容

#### HTML `<script>`元素

---

# HTML字符实体

HTML中的预留字符(如<和>)必须被替换为**字符实体**character entities

字符实体`&entity_name;`或者`&#entity_number;`
如需要显示小于号，必须要这样写：`&lt;`或`&#60;`

浏览器不一定支持所有的实体名称，对实体数字的支持却很好

不间断空格(non-breaking space)，(`&nbsp;`)

浏览器总是会截短HTML页面中的空格，如需要在页面中增加空格的数量，需要使用`&nbsp;`字符实体

---

# HTML URL

HTML URL，统一资源定位符，Uniform Resource Locator，用于定位万维网上的文档或其他数据

`scheme://host.domain:port/path/filename`

host：定义域主机，http的默认主机是www
domain：定义因特网域名
path：定义服务器上的路径（如果省略，则文档必须位于网站的根目录中）

当scheme为file时，表示本地计算机上的文件

---

#### HTML URL字符编码

URL编码会将字符转换为可通过因特网传输的格式，URL只能使用ASCII字符集来通过因特网进行发送

URL常常会含有ASCII集合之外的字符，URL必须转换为有效的ASCII格式，使用%其后跟随两位十六进制数来替换非ASCII字符，URL不能包含空格，使用+来替换空格，+号为%2B

---

#### HTTP Web Server

---

#### HTML颜色

颜色由红、绿、蓝混合而成，`#000000`=`rgb(0,0,0)`

大多数的浏览器都支持颜色名集合。仅有16种颜色名被W3C的HTML4.0标准所支持。如果需要使用其他颜色，需要使用十六进制的颜色值。

#### Web安全色

数年以前，大多数计算机仅支持256种颜色，一系列216种Web安全色作为Web标准被建议使用，其中的原因是，微软和Mac操作系统使用了40种不同的保留的固定系统颜色（双方大约各使用20种）

216跨平台色

最初，216跨平台web安全色被用来确保：当计算机使用256色调色板时，所有的计算机能够正确地显示所有的颜色

---

#### HTML颜色名

16种被HTML4.0标准支持的颜色名：
aqua、black、blue、fuchsia、gray、green、lime、maroon、navy、olive、purple、red、silver、teal、white、yellow

---

# HTML `<!DOCTYPE>`

`<!DOCTYPE>`声明帮助浏览器正确地显示网页，为浏览器提供一项信息(声明)，即HTML是用什么版本编写的

```html
<!-- HTML5 -->
<!DOCTYPE html>
<!-- HTML4.01 -->
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01 Transitional//EN" "http://www.w3.org/TR/html4/loose.dtd">
<!-- XHTML1.0 -->
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

---

# HTML表单

HTML表单用于搜集不同类型的用户输入

`<form>`元素定义HTML表单，HTML表单包含**表单元素**，包括不同类型的input元素、复选框、单选按钮、提交按钮等

```html
<form action="action_page.php" method="GET">
    First name:<br/>
    <input type="text" name="firstname">
    <br/>
    Last name:<br/>
    <input type="text" name="lastname">
    <br/>
    <input type="radio" name="sex" value="male" checked>Male
    <br/>
    <input type="radio" name="sex" value="female">Female
    <br/>
    <input type="submit" value="Submit">
</form>
```

#### `<input>`元素

最重要的表单元素

+ 文本输入`<input type="text">`用于**文本输入**的单行输入字段，文本字段的默认宽度是20个字符
+ 单选按钮输入`<input type="radio">`定义单选按钮，允许用户在有限数量的选项中选择其中之一
+ 提交按钮`<input type="submit">`定义用于向表单处理程序form-handler提交表单的按钮，表单处理程序在表单form的action属性中指定

#### Action属性

action属性定义在提交表单时执行的动作，如果省略action属性，则action会被设置为当前页面

#### Method属性

method属性规定在提交表单时所用的HTTP方法GET或POST。GET为默认方法

当表单提交是被动的，比如搜索引擎查询，并且没有敏感信息，使用GET，表单数据在页面地址栏中是可见的

GET最适合少量数据的提交，浏览器会设定容量限制

#### Name属性

如果要正确地被提交，每个输入字段必须设置一个name属性

用`<fieldset>`组合表单中的相关数据，`<legend>`元素为`<fieldset>`元素定义标题

```html
<form action="action_page.php">
    <fieldset>
        <legend>Personal information</legend>
        First name:<br>
        <input type="text" name="firstname" value="Mickey">
        <br>
        Last name:<br>
        <input type="text" name="lastname" value="Mouse">
        <br>
        <input type="submit" value="Submit">
    </fieldset>
</form>
```

#### HTML Form属性

| 属性           | 描述                                                 |
| -------------- | ---------------------------------------------------- |
| accept-charset | 规定在被提交表单中使用的字符集(默认：页面字符集)     |
| action         | 规定向何处提交表单的地址URL(提交页面)                |
| autocomplete   | 规定浏览器应该自动完成表单(默认开启)                 |
| enctype        | 规定被提交数据的编码(默认：url-encoded)              |
| method         | 规定在提交表单时所用的HTTP方法(默认：GET)            |
| name           | 规定识别表单的名称(对于DOM使用：document.forms.name) |
| novalidate     | 规定浏览器不验证表单                                 |
| target         | 规定action属性中地址的目标(默认：_self)              |

#### HTML表单元素

+ `<input>`元素

+ `<select>`元素定义下拉列表，`<option>`元素定义待选择的选项，列表通常会把首个选项显示为被选选项，能够通过添加selected属性来定义预定义选项

  ```html
  <select name="cars">
      <option value="volvo">Volvo</option>
      <option value="saab">Saab</option>
      <option value="fiat" selected>Fiat</option>
  </select>
  ```

+ `<textarea>`元素定义多行输入字段(**文本域**)

  ```html
  <textarea name="message" rows="10" cols="30">
      The cat was playing in the garden.
  </textarea>
  ```

+ `<button>`元素定义可点击的**按钮**

  ```html
  <button type="button" onclick="alert('Hello World!')">Click Me!</button>
  ```

#### HTML5`<datalist>`元素

`<datalist>`元素为`<input>`元素规定预定义选项列表，用户会在它们输入数据时看到预定义选项的下拉列表

```html
<form action="action_page.php">
    <input list="browsers">
    <datalist id="browsers">
        <option value="Internet Explorer">
        <option value="Firefox">
        <option value="Chrome">
        <option value="Opera">
        <option value="Safari">
    </datalist>
</form>
```

#### HTML输入类型

`<input type="text">`定义供文本输入的单行输入字段

`<input type="password">`定义密码字段，password字段中的字符会被做掩码处理(显示为星号或者实心圆)

`<input type="submit">`定义**提交**表单数据至**表单处理程序**的**按钮**

如果省略了提交按钮的value属性，那么该按钮将获得默认文本，即按钮上会显示"提交"两个字

---

#### Input Type: radio

#### Input Type: checkbox

`<input type="checkbox">`定义复选框

```html
<form>
    <input type="checkbox" name="vehicle" value="Bike">I have a bike
    <br>
    <input type="checkbox" name="vehicle" value="Car">I have a car
</form>
```

HTML5增加了多个新的输入类型，老式Web浏览器不支持的输入类型，会被视为输入类型text

#### 输入类型：number

`<input type="number">`用于应该包含数字值的输入字段

```html
<form>
    Quantity (between 1 and 5):
    <input type="number" name="quantity" min="1" max="5">
</form>
```

#### 输入限制

input标签的value为输入字段的默认值

输入类型：date：`<input type="date">`用于应该包含日期的输入字段

输入类型：color：`<input type="color">`用于应该包含颜色的输入字段

输入类型：range：`<input type="range">`用于应该包含一定范围内的值的输入字段

输入类型：month：`<input type="month">`允许用户选择月份和年份

输入类型：week：`<input type="week">`允许用户选择周和年

输入类型：time：`<input type="time">`允许用户选择时间(无时区)

输入类型：datetime/datetime-local：`<input type="datetime"><input type="datetime-local">`允许用户选择日期和时间(有时区/无时区)

输入类型：email：`<input type="email">`用于应该包含电子邮件地址的输入字段

输入类型：search：`<input type="search">`用于搜索字段(搜索字段的表现类似常规文本字段)

输入类型：tel：`<input type="tel">`用于应该包含电话号码的输入字段

输入字段：url：`<input type="url">`用于应该包含URL地址的输入字段

#### HTML Input属性

**value属性**规定输入字段的初始值

**readonly属性**规定输入字段为只读(不能修改)。readonly属性不需要值，等同于readonly="readonly"

```html
<form action="">
    <input type="text" name="firstname" value="John" readonly>
</form>
```

**disabled属性**规定输入字段是禁用的。被禁用的元素是不可用和不可点击的，被禁用的元素不会被提交，disabled属性不需要值，等同于disabled="disabled"

**size属性**规定输入字段的尺寸(以字符计)，即显示的框的长度

**maxlength属性**规定输入字段允许的最大长度

---

#### HTML5的input和form属性

autocomplete属性规定表单或输入字段是否应该自动完成，on和off，当自动完成开启，浏览器会基于用户之前的输入值自动填写值，适用于form和input类型

novalidate属性，用于form属性。如果设置，则novalidate规定在提交表单时不对表单数据进行验证

autofocus属性是布尔属性，如果设置，则规定当前页面加载时input元素应该自动获得焦点

form属性规定input元素所属的一个或多个表单，如需引用一个以上的表单，请使用空格分隔的表单id列表

```html
<form action="action_page.php" id="form1">
    First name: <input type="text" name="fname"><br>
    <input type="submit" value="Submit">
</form>

Last name: <input type="text" name="lname" form="form1">
```

formaction属性规定当提交表单时处理该输入控件的文件的URL，formaction属性覆盖form的action属性，适用于type="submit"以及type="image"

formenctype属性规定当把表单数据form-data提交至服务器时如何对其进行编码，仅针对method="post"的表单，覆盖form的enctype属性，适用于type="submit"以及type="image"

formmethod属性定义用以向action URL发送表单数据form-data的HTTP方法，覆盖form的method属性，适用于type="submit"以及type="image"

formnovalidate属性是布尔属性，如果设置，则规定在提交表单时不对input元素进行验证，覆盖form的novalidate属性，可用于type="submit"

formtarget属性规定的名称或关键词指示提交表单后再何处显示接受到的响应，覆盖form的target属性，适用于type="submit"以及type="image"

height和width属性规定input元素的高度和宽度，仅用于`<input type="image">`。请始终规定图像的尺寸，如果浏览器不清楚图像尺寸，则页面会在图像加载时闪烁

list属性引用的`<datalist>`元素中包含了`<input>`元素的预定义选项

```html
<input list="browsers">
<datalist id="browsers">
    <option value="Internet Explorer">
    <option value="Firefox">
    <option value="Chrome">
    <option value="Opera">
</datalist>
```

min和max属性规定input元素的最小值和最大值

multiple属性是布尔属性，如果设置，则规定允许用户在input元素中插入一个以上的值，使用于以下输入类型：email和file

pattern属性规定用于检查input元素值的正则表达式，使用全局的title属性对模式进行描述以帮助用户
`<input type="text" name="country_code" pattern="[A-Za-z]{3}" title="Three letter country code">`鼠标移动到input上会显示title内容

placeholder属性规定用以描述输入字段预期值的提示(样本值或有关格式的简短描述)，该提示会在用户输入值之前显示在输入字段中

required属性是布尔属性，如果设置，则规定在提交表单之前必须填写输入字段

step属性规定input元素的合法数字间隔，step属性可于max和min属性一起使用，来创建合法值的范围

---

# HTML5简介

HTML5是最新的HTML标准，是专门为承载丰富的web内容而设计的，并且无需额外插件，拥有新的语义、图形、以及多媒体元素，提供的新元素和新的API简化了web应用程序的搭建

HTML5是跨平台的，被设计为在不同类型的硬件(PC、平板、手机、电视机等等)之上运行

新的属性语法：
Empty `<input type="text" value="John Doe" disabled>`
Unquoted `<input type="text" value=John Doe>`
Double-quoted `<input type="text" value="John Doe">`
Single-quoted `<input type="text" value='John Doe'>`

根据对属性的需求，可能会用到所有4种语法

---

HTML5新特性：强大的图像支持（藉由`<canvas>`和`<svg>`）；强大的多媒体支持（藉由`<video>`和`<audio>`）；强大的新API，**比如用本地存储取代cookie（不懂）**

---

所有浏览器，不论新旧，都会自动把未识别元素当作行内元素来处理

---

`<canvas>`定义使用JavaScript的图像绘制

`<svg>`定义使用SVG的图像绘制

---

#### shiv：为Internet Explorer支持而添加的shiv

```html
<!--[if lt IE 9]>
	<script src="http://html5shiv.googlecode.com/svn/trunk/html5.js"></script>
<![endif] -->
```

请始终定义图像尺寸，这样做会减少闪烁，因为浏览器会在图像加载之前为图像预留空间