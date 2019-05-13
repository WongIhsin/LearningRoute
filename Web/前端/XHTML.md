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

