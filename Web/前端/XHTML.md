# XHTML

XHTML指可扩展超文本标签语言EXtensible HyperText Markup Language

XHTML是HTML与XML(扩展标记语言)的结合物

---

某些浏览器运行在计算机中，某些浏览器则运行在移动电话和手持设备上，后者没有能力和手段来解释糟糕的标记语言

属性名称必须小写、属性值必须加引号、属性不能简写、用Id属性代替name属性、XHTML DTD定义了强制使用的HTML元素

在"/"符号前添加一个额外的空格，以使你的XHTML与当今的浏览器相兼容

---

#### 语言属性lang

lang属性应用于几乎所有的XHTML元素，它定义元素内部的内容的所用语言的类型，如果在某元素内使用lang属性，就必须添加额外的xml:lang

**no为无语言声明**

```html
<div lang="no" xml:lang="no">Heia Norge!</div>
```

---

# 强制使用的XHTML元素

所有的XHTML文档必须进行文件类型声明(DOCTYPE declaration)

XHTML文档必须存在html/head/body，而且title元素必须位于head元素中

html标签内的xmlns属性是必须的，然后即使没有这个属性时也不会报错，因为`"xmlns=http://www.w3.org/1999/xhtml"`是一个固定的值，即使没有包含在代码中，这个值也会被添加到html标签中

---

#### XHTML DTD

`<!DOCTYPE>`是强制使用的

#### 3种文档类型声明

DTD规定了使用通用标记语言SGML的网页的语法。在通用标记语言SGML的文档类型声明或DTD中，XHTML被详细地进行了描述。XHTML DTD使用精确的可被计算机读取的语言来描述合法的XHTML标记的语法和句法(不懂)

+ STRICT——严格类型
+ TRANSITIONAL——过渡类型
+ FRAMESET——框架类型

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
```

---

#### XHTML模块

---

#### XHTML标准属性

**核心属性**Core Attributes
class：元素的类；
id：元素的某个特定id；
style：内联样式定义；
title：显示于提示工具中的文本

**语言属性**Language Attributes
dir：设置文本的方向
lang：设置语言代码

**键盘属性**Keyboard Attributes
accesskey：设置访问某元素的键盘快捷键
tabindex：设置某元素的Tab次序

---

#### XHTML事件属性

HTML事件触发浏览器中的行为，比如用户点击一个HTML元素时启动一段JavaScript。可插入HTML标签以定义事件行为的一系列属性

**窗口事件**Window Events：
onload：当文档被载入时执行脚本
onunload：当文档被卸下时执行脚本

**表单元素事件**Form Element Events：
onchange：当元素改变时执行脚本
onsubmit：当表单被提交时执行脚本
onreset：当表单被重置时执行脚本
onselect：当元素被选取时执行脚本
onblur：当元素失去焦点时执行脚本
onfocus：当元素获得焦点时执行脚本

**键盘事件**Keyboard Events：
onkeydown：当键盘被按下时执行脚本
onkeypress：当键盘被按下后又松开时执行脚本
onkeyup：当键盘被松开时执行脚本

**鼠标事件**Mouse Events：
onclick：当鼠标被单机时执行脚本
ondblclick：当鼠标被双击时执行脚本
onmousedown：当鼠标按钮被按下时执行脚本
onmousemove：当鼠标指针移动时执行脚本
onmouseout：当鼠标移出某元素时执行脚本
onmouseover：当鼠标指针悬停于某元素之上时执行脚本
onmouseup：当鼠标按钮被松开时执行脚本



---

# XHTML简介

XHTML是以XML格式编写的HTML

1. 文档结构：
   + XHTML DOCTYPE是强制性的
   + `<html>`中的XML namespace属性是强制性的
   + `<html>`/`<head>`/`<title>`/`<body>`也是强制性的
2. 元素语法：
   + XHTML元素必须正确嵌套
   + 元素必须始终关闭
   + 元素必须小写
   + 文档必须有一个根元素
3. 属性语法：
   + XHTML属性必须使用小写
   + 属性值必须使用引号包围
   + 属性最小化也是禁止的(**不懂**)

#### `<!DOCTYPE ...>`是强制性的

`<html>/<head>/<title>/<body>`元素也必须存在，并且必须使用`<html>`中的xmlns属性为文档规定命名空间

带有最少的必需标签的XHTML文档

```html
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML 1.0 Transitional//EN" "http://www.w3.org/TR/xhtml1/DTD/xhtml1-transitional.dtd">
<html xmlns="http://www.w3.org/1999/xhtml">
    <head>
        <title>Title of document</title>
    </head>
    <body>
        ......
    </body>
</html>
```

---

#### XHTML元素

XHTML元素是以XML格式编写的HTML元素

XHTML文档必须有一个根元素

#### XHTML属性

XHTML属性是以XML格式编写的HTML属性

禁止属性简写(属性最小化是禁止的)

```html
<!-- 错误示范 -->
<input checked>
<option selected>
<!-- 正确方式 -->
<input checked="checked" />
<option selected="selected" />
```

