# CSS

层叠样式表Cascading Style Sheets，样式定义如何显示HTML元素，样式通常存储在样式表中

解决内容与表现分离的问题，外部样式表可以极大地提高工作效率，外部样式表通常存储在CSS文件中，多个样式定义可层叠为一

由于允许同时控制多重页面的样式和布局，CSS可以称得上WEB设计领域的一个突破，如需进行全局的更新，只需要简单地更改样式，然后网站中的所有元素均会自动地更新

样式表允许以多种方式规定样式信息。样式可以规定在单个的HTML元素中，在HTML页的头元素中，或者在一个外部的CSS文件中。甚至在同一个HTML文档内部引用多个外部样式表

---

#### 层叠次序

当同一个HTML元素被不止一个样式定义时，会使用哪个样式？
一般而言，所有的样式会根据下面的规则层叠于一个新的虚拟样式表中，其中数字4拥有最高的优先权

1. 浏览器缺省设置
2. 外部样式表
3. 内部样式表（位于`<head>`标签内部）
4. 内联样式（在HTML元素内部）

内联样式拥有最高的优先权，意味着它将优先于以下的样式声明：`<head>`标签中的样式声明、外部样式表中的样式声明、或者浏览器中的样式声明(缺省值)

---

# CSS基础语法

CSS规则由两个主要的部分构成：选择器、以及一条或多条声明

```css
selector {declaration1; declaration2; ... declarationN;}
selector {property1: value1; property2: value2;}
```

选择器通常是您需要改变样式的HTML元素
每条声明由一个属性和一个值组成

属性property是您希望设置的样式属性style attribute，每个属性有一个值，属性和值被冒号分开

**使用花括号来包围声明**

#### 值的不同写法和单位

除了使用英文单词red，还可以使用十六进制的颜色值#ff0000，为了节约字节，可以使用CSS的缩写形式`p { color: #f00; }`，还可以通过两种方法使用RGB值`p { color: rgb(255,0,0); }`和`p { color: rgb(100%,0%,0%); }`

当使用RGB百分比时，即使当值为0时也要使用百分比符号

如果值为若干单词，要给值加引号`p {font-family: "sans serif";}`

---

#### 多重声明

如果要定义不止一个声明，则需要用分号将每个声明分开，最后一条规则是不需要加分号的。然而大多数有经验的设计师会在每条声明的末尾都加上分号，这么做的好处是，当从现有的规则中增减声明时，会尽可能地减少出错的可能性

应该在每行只描述一个属性，这样可以增强样式定义的可读性。大多数样式表包含不止一条规则，而大多数规则包含不止一个声明。

```css
p {
    text-align: center;
    color: black;
    font-family: arial;
}

body {
    color: #000;
    background: #fff;
    margin: 0;
    padding: 0;
    font-family: Georgia, Palatino, serif;
}
```

是否包含空格不会影响CSS在浏览器的工作效果，同样，与XHTML不同，CSS对大小写不敏感，如果涉及到与HTML文档一起工作的话，class和id名称对大小写是敏感的

---

#### 选择器的分组

被分组的选择器可以分享相同的声明，用逗号将需要的分组的选择器分开

```css
h1, h2, h3, h4, h5, h6 {
    color: green;
}
```

子元素从父元素继承属性，但是它并不总是按此方法工作。某些浏览器无法理解继承

如果不希望某个属性被所有的子元素继承，那么就创建一个新的针对子元素的特殊规则，这样它就会摆脱父元素的规则

---

#### CSS派生选择器

通过依据元素在其位置的上下文关系来定义样式，可以使标记更加简洁

CSS1中叫做上下文选择器Contextual Selectors，CSS2中叫做派生选择器

```css
li strong {
    font-style: italic;
    font-weight: normal;
}
```

上面的例子中，只有li元素中的strong元素的样式为斜体字，而其他的strong元素的样式为正常的加粗，无需为strong元素定义特别的class或id，代码更加简洁。

---

#### CSS id 选择器

id选择器可以为标有特定id的HTML元素指定特定的样式，以**#**来定义

```css
#red {color: red;}
#green {color: green;}
```

id属性只能在每个HTML文档中出现一次，不然js识别不到

---

#### id选择器和派生选择器

在现代布局中，id选择器常常用于建立派生选择器

```css
#sidebar p {
    font-style: italic;
    text-align: right;
    margin-top: 0.5em;
}
```

span是文本的容器，div是元素的容器

即使被标注为sidebar的元素只能在文档中出现一次，这个id选择器作为派生选择器也可以被使用很多次

**单独的选择器**：id选择器即使不被用来创建派生选择器，它也可以独立发挥作用

```css
#sidebar {
    border: 1px dotted #000;
    padding: 10px;
}
```

老式的浏览器需要特别地定义这个选择器所属的元素，如`div#sidebar`

---

#### CSS类选择器

在CSS中，类选择器以一个点号显示。和id一样，class也可被用作派生选择器

```css
.center {text-align: center}
.fancy td {
    color: #f60;
    background: #666;
}
```

所有center类的HTML元素均为居中，类名为fancy的更大的元素内部的表格单元都会以灰色背景显示橙色文字

**类名的第一个字符不能使用数字**

元素也可以基于它们的类而被选择：

```css
td.fancy {
    color: #f60;
    background: #666;
}
```

---

#### CSS属性选择器

可以为拥有指定属性的HTML元素设置样式，而不仅限于class和id属性

```css
/*为带有title属性的所有元素设置样式*/
[title]
{
    color: red;
}
/*为title="W3School"的所有元素设置样式*/
[title=W3School]
{
    border: 5px solid blue;
}
/*为包含指定值的title属性的所有元素设置样式，适用于由空格分隔的属性值*/
[title~=hello] { color:red; }
/*为包含指定值的lang属性的所有元素设置样式，适用于由连字符分隔的属性值*/
[lang|=en] { color:red; }
/*属性选择器在为不带有class或id的表单设置样式时特别有用*/
input[type="text"]
{
    width: 150px;
    display: block;
    margin-bottom: 10px;
    background-color: yellow;
    font-family: Verdana, Arial;
}
input[type="button"]
{
    width: 120px;
    margin-left: 35px;
    display: block;
    font-family: Verdana, Arial;
}
```

---

#### 如何插入样式表

当读到一个样式表时，浏览器会根据它来格式化HTML文档，插入样式表的三种方法：

+ 外部样式表
  当样式需要应用于很多页面时，外部样式表将是理想的选择，在使用外部样式表的情况下，可以通过改变一个文件来改变整个站点的外观。每个页面使用`<link>`标签链接到样式表，`<link>`标签在文档的头部

  ```html
  <head>
      <link rel="stylesheet" type="text/css" href="mystyle.css" />
  </head>
  ```

  **不要再属性值与单位之间留有空格**

+ 内部样式表
  当单个文件需要特殊的样式时，就应该使用内部样式表，可以使用`<style>`标签在文档头部定义内部样式表

  ```html
  <head>
  <style type="text/css">
      hr {color: sienna;}
      p {margin-left: 20px;}
      body {background-image: url("image/back40.gif");}
  </style>
  </head>
  ```

+ 内联样式
  由于要将表现和内容混杂在一起，内联样式会损失掉样式表的许多优势，慎用。例如当样式仅需要在一个元素上应用一次时

  ```html
  <p style="color: sienna; margin-left: 20px">
  This is a paragraph
  </p>
  ```

#### 多重样式

内联样式 > 内部样式表 > 外部样式表

---

# CSS样式

#### CSS背景

CSS允许应用纯色作为背景，也允许使用背景图形创建相当复杂的效果，CSS在这方面的能力远远在HTML之上

可以使用background-color属性为元素设置背景色，接受任何合法的颜色值

**background-color**不能继承，其默认值是transparent，透明之意，如果一个元素没有指定背景色，那么背景色就是透明的，这样其祖先元素的背景才能可见

**background-image**属性。默认值为none，表示背景上没有放置任何图像。如需要设置一个背景图像，必须为这个属性设置一个URL值。可以为p.flower一个段落应用一个背景，也可以对行内元素设置背景。无法继承

```css
body {background-image: url(/i/eg_bg_04.gif);}
```

事实上，所有背景属性都不能继承

**background-repeat**属性，背景重复，在页面上对背景图像进行平铺

```css
body {
    background-image: url(/i/eg_bg_03.gif);
    background-repeat: repeat-y;
    /*使用关键字*/
    background-position: center;
    /*也可以使用长度值，百分数值*/
    /*百分数值*/
    background-position: 50% 50%;
    /*长度值*/
    background-position: 50px 100px;
    background-attachment: fixed;
}
```

**background-position**属性改变图像在背景中的位置

**关键字**
图像放置关键字位置top right使图像放置在元素内边距区的右上角。根据规范，位置关键字可以按任何顺序出现，只要保证不超过两个关键字：一个对应水平方向，一个对应垂直方向。如果只出现一个关键字，则认为另一个关键字是center

**百分比数值**
百分比数值同时应用于元素和图像，也就是图像中描述为50% 50%的点（中心点）与元素中描述为50% 50%的点（中心点）对齐
如果只提供一个百分数值，所提供这个值将用作水平值，垂直值将假设为50%，和关键字类似
background-position的默认值为0% 0%，功能上相当于top left

**长度值**
长度值的解释是元素内边距区左上角的偏移，偏移点是图像的左上角

#### 背景关联

如果文档比较长，那么当文档向下滚动时，背景图像也会随之滚动，当文档滚动到超过图像的位置时，图像就会消失，可以通过使用**background-attachment**属性防止这种滚动，通过这个属性，可声明图像相对于可视区是固定的fixed，因此不会受到滚动的影响。
默认值是scroll，也就是说，默认的情况下，背景会随文档滚动

可以将所有背景属性设置在一个声明之中

```css
body {
    background: #ff0000 url(/i/eg_bg_03.gif) no-repeat fixed center;
}
```

---

## CSS文本

#### 缩进文本

可定义文本的外观，改变文本的颜色、字符间距、对齐文本、装饰文本、对文本进行缩进等

```css
/*段落首行缩进5em*/
p {text-indent: 5em;}
/*使用负值实现悬挂缩进*/
p {text-indent: -5em; padding-left: 5em;}
/*使用百分比数值20%将会缩进元素父元素的宽度的20%*/
```

text-indent属性可以继承

#### em：一个em等于任何字体中的字母所需要的垂直空间

#### 水平对齐

text-align是一个基本的属性，会影响一个元素中的文本行互相之间的对齐方式，left、right、center、justify（两端对齐）

需要注意的是，**要由用户代理而不是CSS来确定两端对齐文本如何拉伸**，以填满父元素左右边界之间的空间**（不懂）**

#### 字间隔

**word-spacing**属性可以改变字（单词）之间的标准间隔，默认值normal与设置值为0是一样的

#### 字母间隔

**letter-spacing**属性与word-spacing的区别在于字母间隔修改的是字符或字母之间的间隔

#### 字符转换

**text-transform**属性处理文本的大小写。none/uppercase/lowercase/capitalize

默认值none对文本不做任何改动，capitalize只对每个单词的首字母大写

```css
h1 {text-transform: uppercase}
```

#### 文本装饰

**text-decoration**属性，none/underline/overline/line-through/blink

```css
/*去掉超链接下面的下划线*/
a {text-decoration: none;}
```

文本装饰会替代而不是叠加

#### 处理空白符

**white-space**属性会影响到用户代理对源文档中的空格、换行、Tab字符的处理

```css
p {white-space: normal;}
p {white-space: pre;}
p {white-space: nowrap;} /*防止元素中的文本换行，除非使用了一个br元素*/
p {white-space: pre-wrap;} /*会保留空白符和换行符，还会自动换行*/
p {white-space: pre-line;} /*会保留换行符，合并空白符，会自动换行*/
```

#### 文本方向

**direction**属性影响块级元素中文本的书写方向、表中列布局的方向、内容水平填充其元素框的方向、以及两端对齐元素中最后一行的位置

对于行内元素，只有当unicode-bidi属性设置为embed或bidi-override时才会应用direction属性

ltr和rtl

#### 行间距

**line-height**属性设置行高

---

#### CSS字体

定义文本的字体系列、大小、加粗、风格、变形

**font-family**属性定义文本的字体系列

```css
body {font-family: sans-serif;}
```

这样用户代理就会从sans-serif字体系列中选择一个字体（如Helvetica），并将其应用到body元素。因为有继承，这种字体选择还将应用到body元素中包含的所有元素，除非有一种更特定的选择器将其覆盖

如果用户代理上没有安装某一个指定字体系列，就只能使用用户代理的默认字体来显示

可以通过结合特定字体名和通用字体系列来解决这个问题。CSS定义了5种通用字体系列Serif/Sans-serif/Monospace/Cursive/Fantasy

```css
h1 {font-family: Georgia, serif;}
```

如果对字体非常熟悉，也可以为给定的元素指定一系列类似的字体，把这些字体按照优先顺序排列，然后用逗号进行连接

```css
p {font-family: Times, TimesNR, 'New Century Schooolbook', Georgia, 'New York', serif;}
```

#### 字体风格

**font-style**属性最常用于规定斜体文本，normal/italic斜体/oblique倾斜显示

italic斜体和oblique倾斜显示，通常情况下在web浏览器中看上去完全一样

#### 字体变形

**font-variant**属性设定小型大写字母`p {font-variant:small-caps;}`

#### 字体加粗

**font-weight**属性设置文本的粗细`p {font-weight:normal;}`

#### 字体大小

**font-size**属性设置文本的大小，可以是绝对或相对值。默认大小是16px=1em

W3C推荐使用em尺寸单位，em的值会相对于父元素的字体大小改变

**结合使用百分比和EM**，在所有浏览器中均有效的方案是body元素(父元素)以百分比设置默认的font-size值：

```css
body {font-size:100%;}
h1 {font-size:3.75em;}
h2 {font-size:2.5em;}
p {font-size:0.875em;}

/*在一个声明之内*/
p {font: italic bold 12px/30px arial,sans-serif;}
```

---

#### CSS链接

链接的特殊性在于能够根据它们所处的状态来设置它们的样式

链接的四种状态：

+ link——普通的、未被访问的链接
+ visited——用户已访问的链接
+ hover——鼠标指针位于链接的上方
+ active——链接被点击的时刻

```css
a:link {color:#FF0000;}
a:visited {color:#00FF00;}
a:hover {color:#FF00FF;}  /*必须位于link和visited之后*/
a:active {color:#0000FF;}  /*必须位于hover之后*/
```

---

#### CSS列表

列表属性允许您放置、改变列表项标志、或者将图像作为列表项标志

**列表类型**：要修改用于列表项的标志类型，可以使用属性list-style-type

```css
ul {list-style-type: square}
ul li {lsit-style-image: url(xxx.gif)}
li {list-style-position: inside}
li {list-style: url(example.gif) square inside}
```

**列表项图像**：

**列表标志位置**：

---

#### CSS表格

**表格边框**：border

**折叠边框**：border-collapse属性设置是否将表格边框折叠为单一边框

表格文本对齐：**text-align**属性设置水平对齐方式，**vertical-align**属性设置垂直对齐方式

表格颜色：

```css
table {
    border-collapse: collapse;
    width: 100%;
    height: 50px;
}
table, th, td {
    border: 1px solid blue;
}
th {
    background-color: green;
    color: white;
}
td {
    text-align: right;
    height: 50px;
    vertical-align: bottom;
    padding: 15px;
}
```

---

#### CSS轮廓

轮廓outline是绘制于元素周围的一条线，位于边框边缘的外围，可起到突出元素的作用，CSS outline属性规定元素轮廓的样式、颜色和宽度

outline、outline-color、outline-style、outline-width

---

#### CSS框模型概述

Box Model规定了元素框处理元素的内容、内边距、边框、和外边距的方式

border边框、margin外边距、padding内边距、element元素

内边距、边框、外边距都是可选的，默认值都是零

增加内边距、边框和外边距不会影响内容区域的尺寸，但是会增加元素框的总尺寸

内边距、边框、外边距可以应用于一个元素的所有边，也可以应用于单独的边。外边距可以是负值，而且在很多情况下都要使用负值的外边距

---

#### CSS内边距

padding属性，不允许使用负值，接收长度值或百分比值

按照上、右、下、左的顺序分别设置各边的内边距，也可以使用上、右、下、左内边距padding-top、padding-right、padding-bottom、padding-left

```css
h1 {padding: 10px 0.25em 2ex 20%;}
h2 {
    padding-top: 10px;
    padding-right: 0.25em; /*em是字体高度*/
    padding-bottom: 2ex; /*ex是字母x的高度*/
    padding-left: 20%;
}
```

---

#### CSS边框

元素的边框border是围绕元素内容和内边距的一条或多条线，规定元素边框的样式、宽度、颜色

**border-style**属性：border-top-style、border-right-style、border-bottom-style、border-left-style

**border-width**属性：

由于border-style的默认值是none，如果没有声明样式，就相当于border-style: none。因此，**如果希望边框出现，就必须声明一个边框样式**

**border-color**属性：可以是任何类型的颜色值

**值复制**，只写上右的属性会把下左的属性复制为一样的

默认边框颜色是元素本身的前景色，如果没有为边框声明颜色，他将与元素的文本颜色相同。如果没有任何文本，那么该表的边框颜色就是其父元素的文本颜色（因为color可以继承）

`border-color: transparent;`使用边框就像是额外的内边距一样，元素的背景会延伸到边框区域

---

#### CSS外边距

margin属性，接收任何长度单位、百分数值甚至负值

margin可以设置为auto

margin的默认值是0，所以如果没有为margin声明一个值，就不会出现外边距

---

#### 值复制

如果少左，使用右的值；少下，使用上的值；少右，使用上的值

---

#### CSS外边距合并

当两个垂直外边距相遇时，他们将形成一个外边距，合并后的外边距的高度等于两个发生合并的外边距的高度中的较大者

当一个元素包含在另一个元素中时，假设没有内边距或边框把外边距分隔开，它的上和/或下外边距也会发生合并

---

# CSS定位Positioning

Positioning属性允许你对元素进行定位

#### CSS定位和浮动

CSS为定位和浮动提供了一些属性，利用这些属性，可以建立列式布局，将布局的一部分与另一部分重叠，还可以完成多年来通常需要使用多个表格才能完成的任务

定位的基本思想很简单，允许你定义元素框相对于其正常位置应该出现的位置，或相对于父元素、另一个元素甚至浏览器窗口本身的位置。

浮动不完全是定位

---

#### 一切皆为框

可使用display属性改变生成的框的类型，这意味着通过将display属性设置为block，可以让行内元素，比如`<a>`表现得像块级元素一样。还可以将display设置为none，让生成的元素根本没有框

一些情况下，即使没进行显示定义，也会创建块级元素：把一些文本添加到一个块级元素（比如div）的开头，即使没有把这些文本定义为段落，它也会被当作段落来对待，此时，这个框称为无名块框

---

#### CSS定位机制

三种基本的定位机制：普通流、浮动、绝对定位

除非专门指定，否则所有框都在普通流中定位。也就是说，普通流中的元素的位置由元素在HTML中的位置决定

块级框从上到下一个接一个地排列，框之间的垂直距离是由框的垂直外边距计算出来

行内框在一行中水平布置，可以使用水平内边距、边框和外边距调整他们的间距。但是垂直内边距、边框和外边距不影响行内框的高度。由一行形成的水平框称为**行框**Line Box，行框的高度总是足以容纳它包含的所有行内框。设置行高可以增加这个框的高度

---

#### CSS position属性

position属性：
**static**：元素框正常生成，块级元素生成一个矩形框，作为文档流的一部分，行内元素则会创建一个或多个行框，置于其父元素中
**relative**：元素框偏移某个距离，元素仍保持其未定义前的形状，它原本所占的空间仍保留
**absolute**：元素框从文档流完全删除，并相对于其他包含块定位，包含块可能是文档中的另一个元素或者是初始包含块。元素原先在正常文档流中所占的空间会关闭，就好像元素原来不存在一样，元素定位后生成一个块级框，而不论原来它在正常流中生成何种类型的框
**fixed**：元素框的表现类似于将position设置为absolute，不过其包含块是视窗本身

---

#### CSS相对定位

设置为相对定位的元素框会偏移某个距离。元素仍然保持其未定位前的形状，它原本所占的空间仍保留

```css
#box_relative {
    position: relative;
    left: 30px;
    top: 20px;
}
```

---

#### CSS绝对定位

设置为绝对定位的元素框从文档流完全删除，并相对于其他包含块定位，包含块可能是文档中的另一个元素或者是初始包含块，元素原先在正常文档流中所占的空间会关闭，就好像该元素原来不存在一样，元素定位后生成一个块级框，而不论原来他在正常流中生成何种类型的框

```css
#box_absolute {
    position: absolute;
    left: 30px;
    top: 20px;
}
```

绝对定位的元素的位置相对于**最近的已定位祖先元素**，如果元素没有已定位的祖先元素，那么它的位置相对于**最初的包含块**

可以通过设置z-index属性来控制这些框的堆放次序

---

#### CSS浮动

浮动的框可以向左或向右移动，直到它的外边缘碰到包含框或另一个浮动框为止

通过float属性设置浮动，浮动可能会出现卡住的现象：水平排列的三个浮动元素无法容纳时，其他块会向下移动，直到有足够的空间，如果浮动元素的高度不同，那么当它们向下移动时可能被其他浮动元素卡住

---

#### 行框和清理

浮动框旁边的行框被缩短，从而给浮动框留出空间，行框围绕浮动框

如果想阻止行框围绕浮动框，需要对该框应用clear属性，clear属性的值可以是left、right、both或none，表示框的哪些边不应该挨着浮动框

---

#### CSS元素选择器

最常见的CSS选择器是元素选择器，文档的元素就是最基本的选择器

元素选择器又称为类型选择器type selector

```css
html {color: black;}
h1 {color: blue;}
h2 {color: silver;}
```

也可以为XML文档中的元素设置样式

---

#### CSS分组

选择器分组

```css
body, h2, p, table, th, td, pre, strong, em {color: gray;}
```

通配符选择器

universal selector，貌似就只能使用*

```css
* {color:red;}
```

声明分组

```css
h1 {
    font: 28px Verdana;
    color: blue;
    background: red;
}
```

---

#### CSS类选择器详解

类选择器允许以一种独立于文档元素的方式来指定样式

```css
*.important {color: red;}
.important {color: red;}
p.important {color: red;}
```

在HTML中，一个class值中可能包含一个词列表，各个词之间用空格分隔

```html
<p class="important warning">
    This paragraph is a very important warning.
</p>
```

```css
.important {font-weight:bold;}
.warning {font-style:italic;}
/*class中同时含有important和warning的所有元素*/
.important.warning {background:silver;}
```

通过把两个类选择器链接在一起，仅可以选择同时包含这些类名的元素

---

#### CSS ID 选择器

允许一种独立于文档元素的方式来指定样式

```css
*#intro {font-weight: bold;}
#intro {font-weiht: bold;}
/*两者效果一样，*可忽略*/
```

---

#### CSS属性选择器

属性选择器可以根据元素的属性及属性值来选择元素，还可以根据多个属性进行选择，只需将属性选择器链接在一起即可

```css
*[title] {color:red;}
a[href] {color:red;} /*只对有href属性的锚应用样式*/
a[href][title] {color:red;} /*同时有href和title属性的HTML超链接*/
a[href="http://www.w3school.com.cn/about_us.asp"] {color: red;}
a[href="http://www.w3school.com.cn/"][title="W3School"] {color: red;}
```

根据部分属性值选择

```css
p[class~="important"] {color: red;} /*必须是一个完整的词*/
```

`p.important`和`p[class="important"]`是等价的

#### 子串匹配属性选择器

| 类型         | 描述                                     |
| ------------ | ---------------------------------------- |
| [abc^="def"] | 选择abc属性值以def开头的所有元素         |
| [abc$="def"] | 选择abc属性值以def结尾的所有元素         |
| [abc*="def"] | 选择abc属性值中包含**子串**def的所有元素 |

特定属性选择类型

```css
*[lang|="en"] {color: red;}
```

`[att|="val"]`会选择att属性等于`val`或以`val`开头的所有元素，必须是完整的词

`[att~="val"]`包含一个词的所有元素，必须是完整的词

---

#### CSS后代选择器

descendant selector包含选择器

两个元素之间的层次间隔可以是无限的，`ul em`会选择所有从`ul`元素继承的`em`元素，不论`em`的嵌套层次多深

#### CSS子元素选择器

```css
h1 > strong {color: red;}
table.company td > p {color: red;}
```

只选取子元素，多层嵌套不选取。可以结合后代选择器和子选择器

#### CSS相邻兄弟选择器

Adjacent sibling selector可选择紧接在另一元素后的元素，且二者有相同的父元素

选择紧接在h1元素后出现的段落，可以这样写

```css
h1 + p {margin-top: 50px;}
```

#### CSS伪类Pseudo-class

CSS伪类用于向某些选择器添加特殊的效果

`selector: pseudo-class {property: value}`

#### 锚伪类

```css
a:link {color: #FF0000}
a:visited {color: #00FF00}
a:hover {color: #FF00FF} /*必须放置在link和visited之后*/
a:active {color: #0000FF} /*必须放置在hover之后*/
```

####  CSS2 - :first-child伪类

使用first-child伪类来选择元素的第一个子元素

```css
p:first-child {font-weight: bold;}
li:first-child {text-transform: uppercase;}
```

#### CSS2 - :lang伪类

为不同的语言定义特殊的规则

```html
<html>
    <head>
        <style type="text/css">
            q:lang(no) {
                quotes: "~" "~" /*不懂*/
            }
        </style>
    </head>
    <body>
        <p>文字<q lang="no">段落中的引用的文字</q>文字</p>
    </body>
</html>
```

---

#### CSS伪元素Pseudo-elements

用于向某些选择器设置特殊效果

#### :first-line伪元素

用于向文本的首行设置特殊样式

```css
p:first-line { /*对p元素的第一行文本进行格式化*/
    color: #ff0000;
    font-variant: small-caps;
}
```

#### :first-letter伪元素

用于向文本的首字母设置特殊样式

```css
p:first-letter {
    color: #ff0000;
    font-size: xx-large;
}
```

#### CSS2 - :before伪元素/after伪元素

可以在元素的内容前面插入新内容/之后插入新内容

```css
h1:before {
    content:url(logo.gif);
}
h1:after {
    content:url(logo.gif);
}
```

---

#### CSS水平对齐

margin:auto属性来水平对齐

```css
.center {
    margin-left: auto;
    margin-right: auto;
    width: 70%;
    background-color: #b0e0e6;
}
```

position属性进行左和右对齐

```css
.right {
    position: absolute;
    right: 0px;
    width: 300px;
    background-color: #b0e0e6;
}
```

使用float属性来进行左和右对齐

```css
.right {
    float: right;
    width: 300px;
    background-color: #b0e0e6;
}
```

#### CSS尺寸

Dimension属性允许你控制元素的高度和宽度。同样，允许你增加行间距

#### CSS分类Classification

规定如何以及在何处显示元素