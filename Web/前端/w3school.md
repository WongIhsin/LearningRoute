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
<!-- align 为关于对齐方式的附加信息 -->
<body bgcolor="yellow"></body>
<!-- bgcolor 为关于背景颜色的附加信息 -->
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