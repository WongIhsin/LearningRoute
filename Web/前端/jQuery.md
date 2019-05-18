# jQuery

JavaScript世界中使用最广泛的一个库

大约80%-90%的网站直接或间接的使用了jQuery，解决了一些很重要的问题

+ 消除浏览器差异：不需要自己写冗长的代码来针对不同的浏览器来绑定事件，编写AJAX等代码
+ 简洁的操作DOM的方法：写`$('#test')`肯定比`document.getElementById('test')`来得简洁
+ 轻松实现动画，修改CSS等各种操作

目前jQuery由1.x和2.x两个主要版本，区别在于2.x移除了对古老的IE6/7/8的支持，因此2.x的代码更精简，选择哪个版本主要取决于你是否想支持IE6-8

jQuery只是一个`jquery-xxx.js`文件

使用jQuery只需在页面的`<head>`引入jQuery文件即可：

```html
<html>
    <head>
        <script src="//code.jquery.com/jquery-1.11.3.min.js"></script>
        ...
    </head>
    <body>
        ...
    </body>
</html>
```

# $符号

`$`是著名的jQuery符号，实际上，jQuery把所有功能全部分装在一个全局变量jQuery中，而`$`也是一个合法的变量名，它是变量jQuery的别名

```javascript
window.jQuery; // jQuery(selector, context)
window.$; // jQuery(selector, context)
$ === jQuery; // true
typeof($); // 'function'
jQuery.noConflict(); // 当$被占用了，只能让jQuery把$变量交出来，然后就只能使用jQuery
$; // underfined
//在jQuery占用$之前，先内部把原来的$保存，调用jQuery.noConflict()时会把原来保存的变量还原
```

很多JavaScript压缩工具可以对函数名和参数改名，所以压缩过的jQuery源码`$`函数可能变成`a(b,c)`

---

#### 选择器

```javascript
// 查找<div id="abc">;
var div = $('#abc'); // id
var ps = $('p'); // tag
ps.length;
var a = $('.red'); // class="red"
var a = $('.red.green'); // .red.green没有空格 class="red green"
var email = $('[name=email]'); // <?? name="email">
var a = $('[items="A B"]'); // <?? items="A B">
var icons = $('[name^=icon]');
var names = $('[name$=with]');
//返回的对象是jQuery对象
```

jQuery对象，类似数组，每个元素都是一个引用了DOM节点的对象：

`[<div id="abc">...</div>]`存在，`[]`不存在某个节点

不会返回underfined或者null，不必在下一行判断`if (div === underfined)`

```javascript
var div = $('#abc');
var divDom = div.get(0); //假设存在div，获取第一个DOM元素
var another = $(divDom); //重新把DOM包装为jQuery对象
```

#### 层级选择器Descendant Selector

```javascript
$('ancestor descendant');
$('ul.lang li');
$('form[name=upload] input');//可以是子元素的子元素
```

#### 子选择器Child Selector

```javascript
$('parent>child');//必须是子元素
```

#### 过滤器Filter

```javascript
$('ul.lang li');//选出所有节点
$('ul.lang li:first-child');//第一个节点
$('ul.lang li:last-child');//最后一个节点
$('ul.lang li:nth-child(2)');//第二个节点，从1开始
$('ul.lang li:nth-child(even)');//序号为偶数的节点
$('ul.lang li:nth-child(odd)');//序号为奇数的节点
```

jQuery还有一组特殊的选择器

`:input`：可以选择`<input>`，`<textarea>`，`<select>`，`<button>`
`:file`：可以选择`<input type="file">`
`:checkbox/:raido/:focus`
`:checked`：选择当前勾上的单选框和复选框，用这个选择器可以立即获得用户选择的项目，如`$('input[type=radio]:checked')`
`:visible/:hidden/:enabled/:disabled`

---

#### 查找和过滤

通常情况下，选择器可以直接定位到我们想要的元素，但是，当我们拿到一个jQuery对象后，还可以以这个对象为基准，进行查找和过滤

```javascript
var ul = $('ul.lang');
var dy = ul.find('.dy');
var swf = $('#swift');
var parent = swf.parent();
var a = swf.parent('.red');//获得swift的上层节点<ul>，同时传入过滤条件，如果ul不符合条件，返回空jQuery对象
var swift = $('#swift');
swift.next(); //swift的下一个元素
swift.next('[name=haskell]'); // 如果swift的下一个元素不符合条件，就返回空的jQuery对象
swift.prev();
swift.prev('.dy'); //同上，是指前一个元素
```

`filter()`方法可以过滤掉不符合选择器条件的节点

```javascript
var langs = $('ul.lang li');
var a = langs.filter('.dy');
//或者传入一个函数
var langs = $('ul.lang li');
langs.filter(function () {
    return this.innerHTML.indexOf('S') === 0; //返回S开头的节点
}); //注意这里的this为DOM对象而不是jQuery对象
```

`map()`方法把一个jQuery对象包含的若干DOM节点转化为其他对象

```javascript
var langs = $('ul.lang li');
var arr = langs.map(function () {
    return this.innerHTML;
}).get(); // 用get()拿到包含string的Array: ['..','..',...]
```

`first()/last()/slice()`方法可以返回一个新的jQuery对象，把不需要的DOM节点去掉

```javascript
var langs = $('ul.lang li');
var js = langs.first(); //相当于$('ul.lang li:first-child')
var haskell = langs.last(); //相当于$('ul.lang li:last-child')
var sub = langs.slice(2, 4); //和数字的slice()方法一致
```

---

#### 操作DOM

```javascript
//修改Text和HTML
$('#test-ul li[name=book]').text(); // Java & JavaScript
$('#test-ul li[name=book]').html(); // Java &amp; JavaScript
$('#test-ul li.js').html('<span style="color: red">JavaScript</span>');
$('#test-ul li[name=book]').text('JavaScript & ECMAScript');
```

一个jQuery对象可以包含0个或任意个DOM对象，它的方法实际上会作用在对应的每个DOM节点上。即使选择器没有返回任何DOM节点，调用jQuery对象的方法仍然不会报错

#### 修改CSS

```javascript
$('#test-css li.dy>span').css('background-color', '#ffd351').css('color', 'red');
var div = $('#test-div');
div.css('color');//获取CSS属性
div.css('color', '#336699');//设置CSS属性
div.css('color', '');//清楚CSS属性
//可以使用`background-color`和`backgroundColor`两种格式
```

`css()`方法将作用于DOM节点的`style`属性，具有最高优先级

**jQuery对象的所有方法都返回一个jQuery对象(可能是新的也可能是自身)，这样就可进行链式调用**

如果要修改class属性，可以使用jQuery提供的下列方法

```javascript
var div = $('#test-div');
div.hasClass('highlight'); //false, class是否还有highlight
div.addClass('highlight'); //添加highlight这个class
div.removeClass('highlight'); //删除highlight这个class
```

#### 显示和隐藏DOM

隐藏一个DOM，可以设置CSS的display属性为none。恢复显示就需要记下原来的display属性是block还是inline，考虑到显示和隐藏DOM元素使用非常普遍，jQuery直接提供了show()和hide()方法

```javascript
var a = $('a[target=_blank]');
a.hide(); //隐藏
a.show(); //显示
//隐藏DOM节点并未改变DOM树的结构，只影响DOM节点的显示，这和删除DOM节点是不同的
```

#### 获取DOM信息

利用jQuery对象的若干方法，可直接获得DOM的宽高等信息，而无需针对不同的浏览器编写特定代码

```javascript
$(window).width();
$(window).height();
$(document).width();
$(document).height();
var div = $('#test-div');
div.width();
div.height();
div.width(400);//设置CSS属性width:400px，是否生效要看CSS是否有效
div.height('200px');

var div = $('#test-div');
div.attr('data');
div.attr('name');
div.attr('name', 'Hello');//div的name属性变为Hello
div.removeAttr('name');//删除name属性
div.attr('name');//undefined
```

HTML5规定有一种属性在DOM节点中可以没有值，只有出现于不出现两种

```html
<input id="test-radio" type="radio" name="test" checked value="1">
```

此时使用prop合理一点

```javascript
var radio = $('#test-radio');
radio.attr('checked'); // 'checked'
radio.prop('checked'); // true
//判断的时候
radio.is(':checked'); // true
//用is()方法判断比较好
//selected.is(':selected')
```

#### 操作表单

```javascript
var input = $('#test-input');
input.val();
input.val('abc@example.com');
//可以处理select和textarea
```

#### 修改DOM结构

**添加DOM**：要添加新的DOM节点，除了通过jQuery的`html()`这种暴力方法外，还可以用`append()`方法

```javascript
$('#test-ul li.js').html('<span style="color: red">JavaScript</span>');
$('#test-div>ul').append('<li><span>Haskell</span></li>');
```

除了接收字符串，`append()`还可以接收原始的DOM对象，jQuery对象和函数对象

```javascript
var ps = document.createElement('li');
ps.innerHTML = '<span>Pascal</span>';
ul.append(ps); // DOM对象
ul.append($('#scheme')); // jQuery对象
ul.append(function (index, html) {
    return '<li><span>Language - ' + index + '</span></li>';
}); // 函数对象
```

传入函数时，要求返回一个字符串/DOM对象或者是jQuery对象

**`append()`把DOM添加到最后，`prepend()`则把DOM添加到最前面**

注意：如果要添加的DOM节点已经存在于HTML文档中，会首先从文档移除，然后再添加，用`append()`可以移动一个DOM节点

```javascript
var js = $('#test-div>ul>li:first-child');
js.after('<li><span>Lua</span></li>');
//同级节点可以用after()或者before()
```

#### 删除节点

```javascript
var li = $('#test-div>ul>li');
li.remove(); //所有<li>全部删除
```

---

#### 事件

因为JavaScript在浏览器中是以单线程模式运行，页面加载后，一旦页面上所有的JavaScript代码被执行完成后，就只能依赖触发事件来执行JavaScript代码

浏览器在接收到用户的鼠标或键盘输入后，会自动在对应的DOM节点上触发相应的事件，如果该节点已经绑定了对应的JavaScript处理函数，该函数就会自动调用

由于不同的浏览器绑定事件的代码都不太一样，所以用jQuery来写代码，就屏蔽了不同浏览器的差异，我们总是在写编写相同的代码

```javascript
var a = $('#test-link');
a.on('click', function () {
    alert('Hello!');
});
a.click(function () {
    alert('Hello!');
});
```

jQuery能够绑定的事件主要有：

#### 鼠标事件

click：鼠标单击时触发，dblclick：鼠标双击时触发；mouseenter：鼠标进入时触发；mouseleave：鼠标移出时触发；mousemove：鼠标在DOM内部移动时触发；hover：鼠标进入和退出时触发两个函数，相当于mouseenter加上mouseleave

#### 键盘事件

键盘事件仅作用在当前焦点的DOM上，通常是`<input>`和`<textarea>`
keydown：键盘按下时触发；keyup：键盘松开时触发；keypress：按一次键后触发

#### 其他事件

focus：当DOM获得焦点时触发；blur：当DOM失去焦点时触发；change：当`<input>`、`<select>`或`<textarea>`的内容改变时触发；submit：当`<form>`提交时触发；ready：当页面被载入并且DOM树完成初始化后触发

其中ready仅作用于document对象，由于ready事件在DOM完成初始化后触发，且只触发一次，所以非常适合用来写其他的初始化代码

由于ready事件使用非常普遍，所以可以这样简化

```javascript
$(document).ready(function () {
    //on('submit', function)也可以简化
    $('#testForm').submit(function () {
        alert('submit!');
    });
});
//甚至简化为如下，最为常见
$(function () {
    //init...
});
//可以反复绑定事件处理函数，它们会依次执行
```

#### 事件参数

mousemove和keypress，需要获取鼠标位置和按键的值，否则监听就没有什么意义了，所有事件都会传入Event对象作为参数，可以从Event对象上获取到更多的信息

```javascript
$(function () {
    $('#testMouseMoveDiv').mousemove(function (e) {
        $('#testMouseMoveSpan').text('pageX = ' + e.pageX + ', pageY = ' + e.pageY);
    });
});
```

#### 取消绑定

一个已经被绑定的事件可以接触绑定，通过`off('click', function)`实现

```javascript
function hello() {
    alert('hello!');
}
a.click(hello);

setTimeout(function () {
    a.off('click', hello);
}, 10000);
//也可以用off('click')一次性移除已绑定click事件的所有处理函数
//off()一次性移除已绑定的所有类型的事件处理函数
```

#### 事件触发条件

一个需要注意的问题是，事件的触发总是由用户操作引发的，用JavaScript代码去改动，**将不会触发事件**

当希望代码能够触发一些事件时，直接调用无参数的change()方法来触发该事件

```javascript
var input = $('#test-input');
input.change(function () {
    console.log('changed...');
});
input.val('change it!'); //无法触发change事件
input.change(); // 触发change事件
input.trigger('change'); //input.change()是trigger()方法的简写
```

#### 浏览器安全限制

在浏览器中，有些JavaScript代码只有在用户触发下才能执行，例如`window.open()`函数

#### 事件触发顺序

#### 事件冒泡

# 例如一个元素绑定多个click，如何执行。倒着执行的？不确定

---

# 动画

用JavaScript实现动画，原理非常简单：只需要在固定的时间间隔，每次把DOM元素的CSS样式修改一点，看起来就像动画了

使用jQuery实现动画

#### show/hide

```javascript
var div = $('#test-show-hide');
div.hide(3000); // 在3秒内逐渐消失
div.show('slow'); //在0.6秒内逐渐显示
div.toggle('slow'); //根据当前状态决定是show()还是hide()
//这些是从左上角逐渐展开或收缩的
```

#### slideUp/slideDown

```javascript
//在垂直方向逐渐展开或收缩
var div = $('#test-slide');
div.slideUp(3000); // 在3秒内逐渐向上消失
div.slideDown('slow'); // 相反
div.slideToggle(3000); //同上，根据元素是否可见来决定下一步的动作
```

#### fadeIn/fadeOut

```javascript
var div = $('#test-fade');
div.fadeOut('slow'); // 在0.6秒内淡出
div.fadeIn('slow');
div.fadeToggle('slow');
```

#### 自定义动画

`animate()`，可以实现任意动画效果，需要传入的参数就是DOM元素最终的CSS状态和时间，jQuery在时间段内不断调整CSS直到达到我们设定的值

```javascript
var div = $('#test-animate');
div.animate({
    opacity: 0.25,
    width:'256px',
    height:'256px'
}, 3000, function () {
    console.log('动画已结束');
    $(this).css('opacity', '1.0').css('width', '128px').css('height', '128px');
}); //3秒内CSS过渡到设定值
//最后的函数可以不传入，传入的话就在动画执行结束后执行该函数
//最后的函数对于普通动画也是适用的
```

#### 串行动画

```javascript
//delay()方法可以实现暂停
var div = $('#test-animates');
div.slideDown(2000).delay(1000).animate({
    width:'256px',
    height:'256px'
}, 2000).delay(1000).animate({
    width:'128px',
    height:'128px'
}, 2000);
```

动画是异步的

---

# AJAX

jQuery在全局对象jQuery（也就是$）绑定了`ajax()`函数，可以处理AJAX请求。`ajax(url, settings)`函数需要接收一个URL和一个可选的settings对象

settings对象常用的选项如下

+ async：是否异步执行AJAX请求，默认为true
+ method：发送的Method，缺省为GET，可以指定为POST和PUT
+ contentType：发送POST请求的格式，默认值为`application/x-www-form-urlencoded;charset=UTF-8`，也可以指定为`text/plain`，`application/json`
+ data：发送的数据，可以是字符串、数组或object
+ headers：发送的额外的HTTP头，必须是一个object
+ dataType：接收的数据格式

```javascript
var jqxhr = $.ajax('/api/categories', {
    dataType: 'json'
});
```

不过，如果用回调函数处理返回的数据和出错时的响应呢

jQuery的jqXHR对象类似一个Promise对象，可以用链式写法来处理各种回调

```javascript
'use strict';
function ajaxLog(s) {
    var txt = $('#test-response-text');
    txt.val(txt.val() + '\n' + s);
}
$('#test-response-text').val('');
var jqxhr = $.ajax('/api/categories', {
    dataType:'json'
}).done(function (data) {
    ajaxLog('成功，收到的数据：' + JSON.stringify(data));
}).fail(function (xhr, status) {
    ajaxLog('失败：' + xhr.status + ', 原因: ' + status);
}).always(function () {
    ajaxLog('请求完成：无论成功还是失败都会调用');
};
```

---

#### GET

对常用的Ajax操作，jQuery提供了一些辅助方法，由于GET请求最常见，所有jQuery提供了get()方法

```javascript
var jqxhr = $.get('/path/to/resource', {
    name:'Bob Lee',
    check: 1
});
//实际的URL是/path/to/resource?name=Bob%20Lee&check=1
```

#### POST

传入的第二个参数默认被序列化为`application/x-www-form-urlencoded`

```javascript
var jqxhr =$.post('/path/to/resource', {
    name: 'Bob Lee',
    check: 1
});//name=Bob Lee, check=1将作为body传输
```

#### getJSON

```javascript
var jqxhr = $.getJSON('/path/to/resoure', {//GET方式
    name: 'Bob Lee',
    check: 1
}).done(function (data) {
    //data已经被解析为JSON对象了
});
```

#### 安全限制

jQuery的AJAX完全封装的是JavaScript的AJAX操作，所以它的安全限制和前面讲的用JavaScript写AJAX完全一样

如需使用JSONP，可以在ajax()中设置`jsonp: 'callback'`，让jQuery实现JSONP跨域加载数据

---

#### 扩展jQuery来实现自定义方法，也称为编写jQuery插件

编写jQuery插件，给jQuery对象绑定一个新方法是通过扩展`$.fn`对象实现的

```javascript
$.fn.highlight1 = function () {
    //this已经绑定了当前jQuery对象
    this.css('backgroundColor', '#fffceb').css('color', '#d85030');
    return this;//因为jQuery对象支持链式操作，自己写的扩展方法也要能继续链式下去
}
$('span.h1').highlight1().slideDown();
```

注意到函数内部的this在调用时被绑定为jQuery对象，所以函数内部代码可以正常调用所有jQuery对象的方法

```javascript
$.fn.highlight2 = function (options) {
    var bgcolor = options && options.backgroundColor || '#fffceb';
    var color = options && options.color || '#d85030';
    this.css('backgroundColor', bgcolor).css('color', color);
    return this;
}
```

另一种方法是使用jQuery提供的辅助方法`$.extend(target, obj1, obj2, ...)`，它把多个object对象的属性合并到第一个target对象中，遇到同名属性，总是使用靠后的对象的值，也就是越往后，优先级越高

```javascript
var opts = $.extend({}, {
    backgroundColor:'#00a8e6',
    color:'#ffffff'
}, options);
```

```javascript
$.fn.highlight = function (options) {
    var opts = $.extend({}, $.fn.highlight.defaults, options);
    this.css('backgroundColor', opts.backgroundColor).css('color', opts.color);
    return this;
}
//设定默认值
$.fn.highlight.defaults = {
    color: '#d85030',
    backgroundColor: '#fff8de'
}

//使用时，只需设定一次默认值，就可以非常简单地使用highlight()了
$.fn.highlight.defaults.color = '#fff';
$.fn.highlight.defaults.backgroundColor = '#000';
```

#### 针对特定元素的扩展

编写的扩展像`submit()`方法只能针对`form`一样，只能针对某些类型的DOM元素。借助`filter()`

```javascript
$.fn.external = function () {
    return this.filter('a').each(function () {
        //each()内部的回调函数的this绑定为DOM本身
        var a = $(this);
        var url = a.attr('href');
        if (url && (url.indexOf('http://') === 0 || url.indexOf('https://') === 0)) {
            a.attr('href', '#0').removeAttr('target').append('<i class="uk-icon-external-link"></i>').click(function () {//<i>为斜体
                if (confirm('你确定要前往' + url + '?')) {
                    window.open(url);
                }
            });
        }
    });
}
```

