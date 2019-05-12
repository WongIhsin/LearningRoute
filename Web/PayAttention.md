#### HTML基础

---

1. 在开始标签中添加斜杆，比如换行`<br/>`是关闭空元素的正确方法，HTML、XHTML、XML都接受这种方式

2. HTML**标签**、**属性**、**属性值**对大小写不敏感，推荐小写

3. `href`属性为**hypertext reference**的缩写

4. 默认情况下，HTML会自动地在**块级**元素前后添加一个额外的空行，比如段落、标题元素前后

5. **标题很重要**，搜索引擎使用标题为您的网页的结构和内容编制索引

6. style属性提供了一种改变所有HTML元素的样式的通用方法，可以直接使用style属性直接添加样式，也可以间接地在独立的样式表中CSS文件中进行定义

7. 浏览器通常会为`<q>`元素包围引号，**引用**Quotation；
   通常会对`<blockquote>`元素进行**缩进**处理，长引用；
   用于缩略词的HTML`<abbr>`，对缩写进行标记能够为浏览器、翻译系统以及搜索引擎提供有用的信息；
   用于著作标题的HTML`<cite>`；
   用于双向重写的HTML`<bdo>`定义双流向覆盖bi-directional override`<bdo>`元素用于覆盖当前文本方向

8. 键盘格式`<kbd>`定义键盘输入
   样本格式`<samp>`定义计算机输出示例
   代码格式`<code>`定义编程代码示例，`<code>`元素**不保留**多余的**空格**和**折行**，如需保留，则需要使用`<pre>`元素包围代码
   变量格式化`<var>`定义数学变量

9. 当浏览器读到一个样式表，它就会按照这个样式表来对文档进行格式化，有三种方式来插入样式表：

   - 外部样式表
     当样式需要被应用到很多页面的时候，外部样式表将是理想的选择，使用外部样式表，可以通过更改一个文件来改变整个站点的外观

     ```html
     <head>
         <!-- link 定义资源引用 —->
         <link rel="stylesheet" type="text/css" href="mystyle.css">
     </head>
     ```

   - 内部样式表
     当单个文件需要特别样式时，就可以使用内部样式表。可以在head部分通过`<style>`标签定义内部样式表

     ```html
     <head>
         <style type="text/css">
             body {background-color: red}
             p {margin-left: 20px}
         </style>
     </head>
     ```

   - 内联样式
     当特殊的样式需要应用到个别元素时，就可以使用内联样式。使用内联样式的方法是在相关的标签中使用样式属性。样式属性可以包含任何CSS属性

     ```html
     <p style="color: red; margin-left: 20px">
         This is paragraph
     </p>
     ```

10. HTML超链接可以是一个字、一个词、一组词、也可以是一幅画像，可以点击这些内容来跳转到新的文档或者当前文档中的某个部分

    + 通过使用href属性，创建指向另一个文档的链接
    + 通过使用name属性，创建文档内的书签

    超链接Hyper Text，标准叫法为**锚**anchor，使用`<a>`标签标记

11. 使用Target属性，可以定义被链接的文档在何处显示，`target="_blank"`会在新的窗口打开文档

12. name属性规定**锚**anchor的名称，当使用**命名锚**的时候，可以创建直接跳至该命名锚（比如页面中的某个小节）的链接，这样使用者就无需不停地滚动页面来寻找他们需要的信息了；
    可以使用id属性来替代name属性，命名锚同样有效；

    ```html
    <a name="tips">基本的注意事项</a>
    <!-- 使用# -->
    <a href="#tips">有用的提示</a>
    <a href="http://www.xxx.com.cn/html/html_links.asp#tips">有用的提示</a>
    <!-- 也可以链接邮箱 -->
    ```

13. 如果浏览器找不到已定义的命名锚，那么就会定位到文档的顶端，不会有错误发生

14. 请始终将正斜杠添加到**子文件夹**，假如这样写`href="http://www.xxx.com.cn/html"`，就会向服务器产生两次HTTP请求，服务器会添加正斜杠到这个地址，然后创建一个新的请求`href="http://www.xxx.com.cn/html/"`

15. alt替换文本属性，用来为图像定义一串预备的可替换的文本，替换文本属性的值是用户定义的。在浏览器无法载入图片时，替换文本属性告诉读者他们失去的信息，此时，浏览器将显示这个替代性的文本而不是图像

16. HTML的body可以添加图片`<body background="/i/background.jpg">`，但是图片小于页面时，图片会进行重复

17. id和name有时候差不多

18. `<map>`定义图像地图；`<area>`定义图像地图中的可点击区域

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

19. 空单元格如果什么都没有，浏览器可能无法显示出这个单元格的边框。可以在空单元格中添加一个空格占位符，就可以将边框显示出来。空格占位符为`&nbsp;`

20. 可以通过`<div>`和`<span>`将HTML元素组合起来。大多数HTML元素被定义为块级元素或内联元素，而块级元素在浏览器显示时，通常会以新行来开始和结束

21. HTML的`<div>`元素是块级元素，是可用于组合其他HTML元素的容器。`<div>`可用于对大的内容块设置样式属性，另一个常见的用途是文档布局。div是division

22. HTML的`<span>`元素是内联元素，可用作文本的容器，可用于为部分文本设置样式属性

---

#### 响应式Web设计

Responsive Web Design响应式Web设计RWD
RWD能够以可变尺寸传递网页
RWD对于平板和移动设备是必须的

---

#### Bootstrap

是最流行的开发响应式Web的HTML，CSS和JS框架，Bootstrap帮助您开发在任何尺寸都外观出众的站点：显示器、笔记本电脑、平板电脑和手机等

---

# HTML框架

通过使用框架，可以在同一个浏览器窗口中显示不止一个页面

**不能将`<body>`标签和`<frameset>`标签同时使用**

---

#### 导航框架~~（没理解）~~用到了`target="showframe"`

---

1. 图像文件不应超过10k

2. script标签既可以包含脚本语言，又可以通过src属性指向外部脚本文件

3. 必需的type属性规定脚本的MIME类型

4. JavaScript最常用于图片操作、表单验证以及内容动态更新

5. `<noscript>`标签提供无法使用脚本时的替代内容

6. 脚本应该写在注释标签当中，以应对老式标签不认识`<script>`而将脚本以文本的方式显示出来，而新的浏览器将读懂这些脚本并执行它们，即使代码被嵌套在注释标签内

   ```html
   <script type="text/javascript">
       <!--
       document.write("Hello World!")
       //-->
   </script>
   ```

7. 