#### JavaScript使用

HTML中的脚本必须位于`<script>`和`</script>`标签之间，一些老旧的实例可能会在`<script>`标签中使用`type="text/javascript"`，现在已经不必这样做了，JavaScript是所有现代浏览器以及HTML5中的默认脚本语言

通常的做法是把函数放入到`<head>`部分中，或者放在页面底部，这样就可以把它们安置到同一处位置，不会干扰页面的内容

在`<head>`或`<body>`中引用脚本文件都是可以的，实际运行效果与在`<script>`标签中编写脚本完全一致

#### JavaScript输出（注意）

`document.write()`仅仅向文档输出写内容，如果在文档已完成加载后执行`document.write()`，整个HTML页面将被覆盖

#### JavaScript语句

JavaScript对大小写是敏感的；可以用反斜杠**`\`**对代码进行换行

#### JavaScript变量

变量可以使用短名称（如x和y），也可以使用描述性更好的名称（如age、sum等）

+ 变量必须以字母开头
+ 变量也能以`$`和`_`符号开头（不推荐）
+ 变量名称对大小写敏感

可以在一条语句中声明很多变量，该语句以`var`开头，使用逗号分隔变量；也可以横跨多行

```javascript
var name="Gates", age=56, job="CEO";
//或者
var name="Gates",
    age=56,
    job="CEO";
```

重新声明JavaScript变量，该变量的值不会丢失

```javascript
var carname = "Volvo";
var carname; // "Volvo"
```

JavaScript中的极大或极小数字可以通过科学(指数)计数法来书写

```javascript
var y = 123e5; // 12300000
var z = 123e-5; // 0.00123
```

#### JavaScript数组

```javascript
var cars = new Array();
cars[0] = "Audi";
cars[1] = "BMW";
cars[2] = "Volvo";
//或者
var cars = new Array("Audi", "BMW", "Volvo");
//或者
var cars = ["Audi", "BMW", "Volvo"]
```

#### JavaScript对象

对象由花括号分隔，在括号内部，对象的属性以名称和值对的形式`(name: value)`来定义。属性由逗号分割

```javascript
var person = {
    firstname: "Bill",
    lastname: "Gates",
    id: 5566
};
//对象属性有两种寻址方式
name = person.lastname;
name = person["lastname"];
```

JavaScript中的所有事物都是对象：字符串、数字、数组、日期等，对象是拥有属性和方法的数据

#### JavaScript函数

函数是由事件驱动的或者当它被调用时执行的可重复使用的代码块

在JavaScript函数内部声明的变量(使用var)是**局部**变量，所以只能在函数内部访问它；

函数外声明的变量是**全局**变量，网页上的所有脚本和函数都能访问它

JavaScript变量的生存期是从它们被声明的时间开始，局部变量会在函数运行以后被删除，**全局变量会在页面关闭后被删除**

把值赋给尚未声明的变量，该变量将被自动作为全局变量声明，即使在函数中

#### JavaScript运算符

数字和字符串相加，将变为字符串

**条件运算符**

```javascript
//条件运算符
variablename = (condition)?value1:value2
```

#### JavaScript Switch语句

```javascript
//使用switch语句来选择要执行的多个代码块之一
var day = new Date().getDay();
switch (day) {
    case 0:
        x="Today it's Sunday";
        break;
    case 1:
        ...
    default:
        x = "Looking forward to the Weekend";
}
```

#### JavaScript标签

可以对JavaScript语句进行标记

```javascript
cars = ["BMW", "Volvo", "Saab", "Ford"];
list: {
    document.write(cars[0] + "<br>");
    break list;
    document.write(cars[1] + "<br>");
}
```

#### JavaScript表单验证

JavaScript可用来在数据被送往服务器前对HTML表单中的这些输入数据进行验证

#### JavaScript HTML DOM

HTML DOM (文档对象模型)，通过HTML DOM，可访问JavaScript HTML文档的所有元素。当网页被加载时，浏览器会创建页面的文档对象模型（Document Object Model）HTML DOM模型被构造为对象的树

```javascript
var x = document.getElementById("intro");//根据id查找元素
var y = x.getElementsByTagName("p");//通过标签名查找元素
```

#### JavaScript HTML DOM

HTML DOM允许JavaScript改变HTML元素的内容

```javascript
document.getElementById("p1").innerHTML = "New text!";
document.getElementById("image").src = "landscape.jpg";//改变HTML属性
document.getElementById("p2").style.color = "blue";
```

#### JavaScript HTML DOM 事件

HTML DOM使JavaScript有能力对HTML事件做出反应

```html
<h1 onclick="this.innerHTML='谢谢!'">请点击该文本</h1>
```

使用HTML DOM来分配事件

```html
<script>
document.getElementById("myBtn").onclick = function () {
    displayDate()
};//displayDate()将分配给`myBtn`的HTML元素，点击时将执行该函数，而不是function () {displayDate()};
</script>
```

#### onload()和onunload()事件

会在用户进入或离开页面时被触发。onload事件可用于检测访问者的浏览器类型和浏览器版本，并基于这些信息来加载网页的正确版本。onload和onunload事件可用于处理cookie

# JavaScript对象

+ 定义并创建对象的实例
+ 使用函数来定义对象，然后创建新的对象实例

```javascript
person = new Object();
person.firstname = "Bill";
person.lastname = "Gates";
person.age = 56;
person.eyecolor = "blue";
//或者
person = {firstname: "John", lastname:"Doe", age:50, eyecolor:"blue"};
```

```javascript
function Person(firstname, lastname, age, eyecolor) {
    this.firstname = firstname;
    this.lastname = lastname;
    this.age = age;
    this.eyecolor = eyecolor;
}
var myFather = new Person("Bill", "Gates", 56, "blue");
```

JavaScript基于prototype，而不是基于类的

`for ... in`循环遍历对象的属性，将针对每个属性执行一次代码块

```javascript
for (x in myFather) {
    txt = txt + myFather[x];
}
```

---

#### JavaScript Number对象

JavaScript只有一种数字类型，**所有JavaScript数字均为64位**，JavaScript中的所有数字都存储为64位(8bit)浮点数

整数最多为15位，小数的最大位数是17，浮点数运算不总是100%准确

如果前缀为0，则JavaScript会把数值常量解释为八进制数，0x则解释为十六进制

#### JavaScript Window浏览器对象模型BOM

Browser Object Model浏览器对象模型

所有浏览器都支持**window**对象，它表示浏览器窗口，所有JavaScript全局对象、函数以及变量均自动成为window对象的成员

```javascript
window.document.getElementById("header");
document.getElementById("header");//两者相同
```

#### Window Navigator

window.navigator对象在编写时可不使用window这个前缀

来自navigator对象的信息具有误导性，不应该被用于检测浏览器版本，因为navigator数据可被浏览器使用者更改，浏览器无法报告晚于浏览器发布的新操作系统

#### JavaScript消息框

警告框`alert("文本")`；确认框`confirm("文本")`；提示框`prompt("文本","默认值")`

#### JavaScript计时事件

`setTimeout()`未来的某时执行代码；
`clearTimeout()`取消`setTimeout()`

```javascript
var t = setTimeout("alert('5 seconds!')", 5000);
//无穷循环
var c=0;
var t;
function timedCount() {
    document.getElementById('txt').value = c;
    c += 1;
    t = setTimeout("timedCount()", 1000)
}
function stopCount() {
    clearTimeout(t);
}
```

#### JavaScript Cookies

cookie是存储于访问者的计算机中的变量，每当同一台计算机通过浏览器请求某个页面时，就会发送这个cookie

名字cookie：当访问者首次访问页面时，他也许会填写他的名字，名字会存储在cookie中，当访问者再次访问网络时，他们会收到类似”Welcome John Doe!“的欢迎词，名字则是从cookie中取回的

密码cookie：当访问者首次访问页面时，他或许会填写他的密码，密码也可被存储于cookie中，当他们再次访问网站时，密码就会从cookie中取回

日期cookie：当访问者首次访问你的网站时，当前的日期可存储于cookie，当他们再次访问网站时，他们会收到类似这样的一条消息”Your last visit was on Tuesday August 11, 2005“。日期也是从cookie中取回的

---

# CDN-内容分发网络

总是希望网页可以尽可能地快，希望页面的容量尽可能地小，同时您希望浏览器尽可能多地进行缓存

如果许多不同的网站使用相同的JavaScript框架，那么把框架库放在一个通用的位置供每个网页分享就变得很有意义

CDN Content Delivery Network 解决了这个问题，CDN是包含可分享代码库的服务器网络

Google为一系列JavaScript库提供了免费的CDN，包括jQuery/Prototype/MooTools/Dojo/Yahoo!YUI等

引用jQuery

```html
<script src="https://ajax.googleapis.com/ajax/libs/jquery/1.8.3/jquery.min.js"></script>
```

---

#### jQuery描述

主要的jQuery函数是`$()`函数（jQuery函数），如果您向该函数传递DOM对象，它会返回jQuery对象，带有向其添加的jQuery功能

---

**jQuery**是一个JavaScript库
**jQuery Mobile**是一个为触控优化的框架，用于创建移动web应用程序
**HTML DOM**定义了访问和操作HTML文档的标准方法

**AJAX**：是异步JavaScript和XML，不是一种新的编程语言，而是一种使用现有标准的新方法，通过于服务器进行数据交换，可以在不重新加载整个页面的情况下，对网页的某部分进行更新

**JSON**：JavaScript对象表示法JavaScript Object Notation，是存储和交换文本信息的语法，类似XML，JSON比XML更小、更快、更易解析

**ASP/PHP**：和HTML文档中的脚本运行在客户端浏览器不同，ASP、PHP文件中的脚本在服务器上运行

---

# JavaScript实现

JavaScript的核心**ECMAScript**描述了该语言的语法和基本对象
**DOM**（文档对象模型）描述了处理网页内容的方法和接口
**BOM**（浏览器对象模型）描述了与浏览器进行交互的方法和接口

---

#### ECMAScript

并不与任何具体浏览器相绑定，实际上，它也没有提到用于任何用户输入输出的方法。ECMAScript可以为不同种类的宿主环境提供核心的脚本编程能力，因此核心的脚本语言是与任何特定的宿主环境分开进行规定的

Web浏览器对于ECMAScript来说是一个宿主环境，但它并不是唯一的宿主环境，事实上，还有不计其数的其他各种环境可以容纳ECMAScript实现

---

#### 变量

ECMAScript的解释程序遇到未声明过的标识符时，用该变量名创建一个全局变量，并将其初始化为指定的值

#### 原始类型与引用类型

**原始值**：存储在栈stack中的简单数据段，也就是说，他们的值直接存储在变量访问的位置
**引用值**：存储在堆heap中的对象，也就是说，存储在变量处的值是一个指针point，指向存储对象的内存处

在为变量赋值时，ECMAScript的解释程序必须判断该值是原始类型，还是引用类型。要实现这一点，解释程序则需尝试判断该值是否为ECMAScript的原始类型之一，即Undefined、Null、Boolean、Number、String型，由于这些原始类型占据的空间是固定的，所以可将他们存储在较小的内存区域——**栈**中。这样存储便于迅速查寻变量的值

#### 原始类型

ECMAScript有5中原始类型primitive type，即Undefined、Null、Boolean、Number和String

**Undefined类型**只有一个值，即undefined，当声明的变量未初始化时，该变量的默认值是undefined

当函数无明确返回值时，返回的也是值undefined

**Null类型**也只有一个值，即null，undefined实际上是从值null派生来的

```javascript
null == undefined; // true
```

undefined是声明了变量但未对其初始化时赋予该变量的值，null则用于表示尚未存在的对象。如果函数或方法要返回的是对象，找不到该对象的时候，返回的通常是null

对于浮点字面量用它进行计算前，真正存储的是字符串

#### 特殊数字Number值

`Infinity`无穷大；`-Infinity`负无穷大；`isFinite()`判断该数是不是无穷大；无穷大和负无穷大不可以用于算术计算

`NaN`不可以用于算术计算。它与自身不相等

```javascript
NaN == NaN; // false
isNaN("blue"); // true
isNaN("555"); // false
```

#### String类型

是唯一没有固定大小的原始类型

#### 整数的原码/反码/补码

18  --->  0000 0000 0000 0000 0000 0000 0001 0010
-18 ---> 反码 1111 1111 1111 1111 1111 1111 1110 1101
             补码 1111 1111 1111 1111 1111 1111 1110 1110

以补码表示

#### 等性运算符

**等号/非等号**：在检查相等性前，会执行类型转换
**全等号/非全等号**：在检查相等性前，不会执行类型转换

---

#### ECMAScript函数概述

函数是ECMAScript的核心

尽管可以使用Function构造函数创建函数，但最好不要使用它，因为用它定义函数比用传统方式要慢得多。不过，所有函数都应该看作Function类的实例

```javascript
var doAdd = new Function("iNum", "alert(iNum + 20)");
doAdd(10);
```

#### ECMAScript闭包（closure）

闭包，指的是词法表示包括不被计算的变量的函数，也就是说，函数可以使用函数之外定义的变量

#### 对象废除

ECMAScript拥有无用存储单元收集程序garbage collection routine。意味着不必专门销毁对象来释放内存，当再没有对对象的引用时，称该对象被废除了dereference了。运行无用存储单元收集程序时，所有废除的对象都被销毁，每当函数执行完它的代码，无用存储单元收集程序都会运行，释放所有的局部变量，还有在一些其他不可预知的情况下，无用存储单元收集程序也会运行

把对象的所有引用都设置为null，可以强制性地废除对象，下次运行无用存储单元收集程序时，该对象将被销毁

#### 早绑定和晚绑定

所谓绑定binding，即把对象的接口与对象实例结合在一起的方法

早绑定early binding是指在实例化对象之前定义它的属性和方法，这样编译器或解释程序就能够提前转换机器代码。在Java和Visual Basic这样的语言中，有了早绑定，就可以在开发环境中使用IntelliSense（即给开发者提供对象中属性和方法列表的功能）ECMAScript不是强类型语言，不支持早绑定

晚绑定late binding指的是编辑器或解释程序在运行前，不知道对象的类型，使用晚绑定，无需检查对象的类型，只需检查对象是否支持属性和方法即可。ECMAScript中的所有变量都采用晚绑定的方法，就允许执行大量的对象操作，而无任何惩罚

#### ECMAScript对象类型

**本地对象native object**：独立于宿主环境的ECMAScript实现提供的对象
**内置对象build-in object**：在ECMAScript程序开始执行时出现：只定义了两个内置对象：Global和Math，每个内置对象也都是本地对象
**宿主对象host object**：所有非本地对象都是宿主对象，即由ECMAScript实现的宿主环境提供的对象，BOM和DOM

#### ECMAScript的字符串连接的性能

ECMAScript的字符串是不可变的

---

# ECMAScript继承机制实现

所有开发者定义的类都可以作为基类，出于安全原因，本地类和宿主类不能作为基类，这样可以防止公用访问编辑过的浏览器级的代码

ECMAScript中的继承机制并不是明确规定的，而是通过模仿实现的，意味着所有的继承细节并非完全由解释程序处理

#### 对象冒充object masquerading

并非是构想原始的ECMAScript时的设计，是在开发者理解函数的工作方式后才发展出来的

关键字this引用的是构造函数当前创建的对象，在`ClassB`中把`ClassA`作为常规函数来建立继承机制，而不是作为构造函数

**对象冒充可以实现多重继承**

#### call()方法/apply()方法

与继承机制的对象冒充方法一起使用在`ClassB()`中调用`ClassA.call(this, sColor);`或者`ClassA.apply(this, new Array(sColor));`或者`ClassA.apply(this, arguments);`

#### 原型链(prototype chaining)

```javascript
function ClassB() {
}
ClassB.prototype = new ClassA();
```

调用`ClassA`的构造函数，没有给它传递参数，这在原型链中是标准做法，要确保构造函数没有任何参数

原型链不支持多重继承

---

# HTML DOM

`<title>DOM 教程</title>`的元素节点`<title>`，包含值为`DOM 教程`的**文本节点**

HTML文档中的所有内容都是节点：整个文档是一个**文档节点**；每个HTML元素都是**元素节点**；HTML元素内的文本是**文本节点**；每个HTML属性是**属性节点**；注释是**注释节点**

---

# JSON

`eval()`函数可编译并执行任何JavaScript代码，隐藏了一个潜在的安全问题

使用JSON解析器将JSON转换为JavaScript对象是更安全的做法，JSON解析器只能识别JSON文本，而不会编译脚本。

```javascript
var txt = '{"name":"Bill Gates","age":63,"city":"Seattle"}';
var obj = JSON.parse(txt);
document.getElementById("demo").innerHTML = obj.name + ", " + obj.age;
```

---

# jQuery

```html
<html>
    <head>
        <script type="text/javascript" src="jquery.js"></script>
        <script type="text/javascript">
            $(document).ready(function () {
                $("button").click(function () {
                    $("p").hide();
                });
            });
        </script>
    </head>
    <body>
        ...
    </body>
</html>
```

Google和Microsoft对jQuery的支持都很好，如果不愿意在自己的计算机上存放jQuery库，那么可以从Google和Microsoft加载CDN jQuery核心文件

```html
<head>
    <script type="text/javascript" src="http://ajax.googleapis.com/ajax/libs/jquery/1.4.0/jquery.min.js"></script>
</head>
或者
<head>
    <script type="text/javascript" src="http://ajax.microsoft.com/ajax/jquery/jquery-1.4.min.js"></script>
</head>
```

使用谷歌或微软的jQuery有一个很大的优势就是：许多用户在访问其他站点时，已经从谷歌或微软加载过jQuery，所以结果是当它们访问你的站点时，会从缓存种加载jQuery，这样可以减少加载时间，同时，大多数CDN都可以确保当用户向其请求文件时，会从离用户最近的服务器上返回响应，这样也可以提高加载速度

#### jQuery语法

`$(selector).action()`

#### 文档就绪函数

```javascript
$(document).ready(function () {
    ----jQuery funcitons go here -----
});
```

这是为了防止文档在完全加载（就绪）之前运行jQuery代码

#### 单独文件中的函数

```html
<head>
    <script type="text/javascript" src="jquery.js"></script>
    <script type="text/javascript" src="my_jquery_functions.js"></script>
</head>
```

jQuery名称冲突：使用`$`符号作为jQuery的简介方式，某些其他JavaScript库中的函数，比如Prototype，同样使用`$`符号，jQuery使用名为`noConflict()`的方法来解决该问题，如下使用`jq`来代替`$`符合

`var jq = jQuery.noConflict();`

---

# AJAX

AJAX是一种无需重新加载整个网页的情况下，能够更新部分网页的技术

Asynchronous JavaScript And XML

是一种用于创建快速动态网页的技术，通过在后台与服务器进行少量数据交换，AJAX可以使网页实现异步更新，这意味着可以在不重新加载整个网页的情况下，对网页的某部分进行更新。传统的网页（不使用AJAX）如果需要更新内容，必须重载整个网页面

#### AJAX-创建 XMLHttpRequest对象

XMLHttpRequest是AJAX的基础，所有现代浏览器都支持XMLHttpRequest对象，XMLHttpRequest用于在后台与服务器交换数据，这意味着可在不重新加载整个网页的情况下，对网页的某部分进行更新

```javascript
variable = new XMLHttpRequest();
//老版本的Internet Explorer使用ActiveX对象
variable = new ActiveObject("Microsoft.XMLHTTP");
```

#### 向服务器发送请求

```javascript
xmlhttp.open("GET", "test1.txt", true);
xmlhttp.send();
```

`open(method, url, async)`规定请求的类型、URL以及是否异步处理请求：`method`请求的类型；`url`文件在服务器上的位置；`async`异步true或同步false；

`send(string)`将请求发送到服务器：`string`：仅用于POST请求

如果需要像HTML表单那样POST数据，请使用`setRequestHeader()`来添加HTTP头，然后在`send()`方法中规定希望发送的数据

```javascript
xmlhttp.open("POST", "ajax_test.asp", true);
xmlhttp.setRequestHeader("Content-type", "application/x-www-form-urlencoded");
xmlhttp.send("fname=Bill&name=Gates");
```

当使用`async=true`时，请规定在响应处于`onreadystatechange`事件中的就绪状态时执行的函数

```javascript
xmlhttp.onreadystatechange = function () {
    if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
        document.getElementById("myDiv").innerHTML = xmlhttp.responseText;
    }
}
xmlhttp.open("GET", "test1.txt", true);
xmlhttp.send();
```

使用`async=flase`时，请不要编写`onreadystatechange`函数，把代码放到`send()`语句后面即可

如需获得来着服务器的响应，请使用XMLHttpRequest对象的responseText或responseXML属性

responseText属性返回字符串形式的响应，responseXML属性用在来自服务器的响应是XML

#### onreadystatechange事件

当请求被发送到服务器时，我们需要执行一些基于响应的任务，每当readyState改变时，就会触发onreadstatechange事件，readyState属性存有XMLHttpRequest的状态信息

#### XMLHttpRequest对象的三个重要的属性

+ onreadystatechange：存储函数（或函数名），每当readyState属性改变时，就会调用该函数
+ readyState：存有XMLHttpRequest的状态，从0到4发生变化：0：请求未初始化；1：服务器连接已建立；2：请求已接收；3：请求处理中；4：请求已完成，且响应已就绪
+ status：200：OK；404：未找到页面

#### 使用Callback函数

callback函数是一种以参数形式传递给另一个函数的函数。如果网站上存在多个AJAX任务，那么应该为创建XMLHttpRequest对象编写一个标准的函数，并为每个AJAX任务调用该函数

```javascript
function myFunction() {
    loadXMLDoc('ajax_info.txt', function() {
        if (xmlhttp.readyState == 4 && xmlhttp.status == 200) {
            document.getElementById("myDiv").innerHTML = xmlhttp.responseText;
        }
    });
}
```

---

#### jQuery AJAX load()方法

jQuery load()方法是简单但强大的AJAX方法。`load()`方法从服务器加载数据，并把返回的数据放入被选元素中

```javascript
$(selector).load(URL, data, callback);
```

URL：规定您希望加载的URL；data：规定与请求一同发送的查询字符串键值对集合；callback是load()方法完成后所执行的函数名称

callback参数规定load()方法完成后所要允许的回调函数：

+ responseTxt：包含调用成功时的结果内容
+ statusTxt：包含调用的状态
+ xhr：包含XMLHttpRequest对象

```javascript
$("button").click(function () {
    $("#div1").load("demo_test.txt", function (responseTxt, statusTxt, xhr) {
        if (statusTxt == "success") alert("外部内容加载成功");
        if (statusTxt == "error") alert("Error: " + xhr.status + ": " + xhr.statusText);
    });
});
```

POST方法不会缓存数据

`$.get(URL, callback);`方法：从服务器上的一个文件中取回数据

```javascript
//$.get(URL, callback);
$("button").click(function () {
    $.get("demo_test.asp", function (data, status) {
        alert("Data: " + data + "\nStatus: " + status)
    });
});
```

`$.post(URL, data, callback);`方法：通过HTTP POST请求从服务器上请求数据

```javascript
$("button").click(function () {
    $.post("demo_test_post.asp", {
        name: "Donald Duck",
        city: "Duckbury"
    }, function (data, status) {
        alert("Data: " + data + "\nStatus: " + status);
    });
});
```

#### jQuery ajax()方法

```javascript
$(document).ready(function () {
    $("#b01").click(function () {
        htmlobj = $.ajax({url:"/jquery/test1.txt",async:false});
        $("#myDiv").html(htmlobj.responseText);
    });
});
```



---

#### noConflict()方法

noConflict()方法只会释放`$`标识符的控制，仍然可以通过全名替代简写的方式来使用jQuery：

```javascript
$.noConflict();
jQuery(document).ready(function () {
    jQuery("button").click(function () {
        jQuery("p").text("jQuery 仍在运行！");
    });
});
```

也可以创建自己的简写，`noConflict()`可返回对jQuery的引用，可以把它存入变量，以供稍后使用

```javascript
var jq = $.noConflict();
jq(document).ready(function () {
    jq("button").click(function () {
        jq("p").text("jQuery 仍在运行！");
    });
});
```

如果你的jQuery代码块使用`$`简写，并且你不愿意改变这个快捷方式，那么**可以把`$`符号作为变量传递给ready方法**，这样就可以在函数内使用`$`符号了，而在函数外，依旧不得不使用`jQuery`

```javascript
$.noConflict();
jQuery(document).ready(function ($) {
    $("button").click(function () {
        $("p").text("jQuery 仍在运行！");
    });
});
```

