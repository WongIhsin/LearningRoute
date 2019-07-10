# Bootstrap教程

官网：https://v4.bootcss.com/docs/4.0/getting-started/introduction/

---

#### 范例如下

```html
<!doctype html>
<html lang="en">
  <head>
    <!-- Required meta tags -->
    <meta charset="utf-8">
    <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">

    <!-- Bootstrap CSS -->
    <link rel="stylesheet" href="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/css/bootstrap.min.css" integrity="sha384-ggOyR0iXCbMQv3Xipma34MD+dH/1fQ784/j6cY/iJTQUOhcWr7x9JvoRxT2MZw1T" crossorigin="anonymous">

    <title>Hello, world!</title>
  </head>
  <body>
    <h1>Hello, world!</h1>

    <!-- Optional JavaScript -->
    <!-- jQuery first, then Popper.js, then Bootstrap JS -->
    <script src="https://code.jquery.com/jquery-3.3.1.slim.min.js" integrity="sha384-q8i/X+965DzO0rT7abK41JStQIAqVgRVzpbzo5smXKp4YfRvH+8abtTE1Pi6jizo" crossorigin="anonymous"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/popper.js/1.14.7/umd/popper.min.js" integrity="sha384-UO2eT0CpHqdSJQ6hJty5KVphtPhzWj9WO1clHTMGa3JDZwrnQq4sF86dIHNDz0W1" crossorigin="anonymous"></script>
    <script src="https://stackpath.bootstrapcdn.com/bootstrap/4.3.1/js/bootstrap.min.js" integrity="sha384-JjSmVgyd0p3pXB1rRibZUAYoIIy6OrQ6VrjIEaFf/nJGzIxFDsf4x0xIM+B07jRM" crossorigin="anonymous"></script>
  </body>
</html>
```

#### 说明

+ **HTML5 doctype**
  `<!DOCTYPE html>`

+ **响应式meta标签**：初始化移动浏览显示：视口宽度为设备宽度`width=device-width`，初始缩放比例为1，不做缩放

  ```html
  <meta name="viewport" content="width=device-width, initial-scale=1, shrink-to-fit=no">
  ```

#### 注意

将引入 Bootstrap 样式表的 `<link>` 标签复制并粘贴到 `<head>` 中，并放在所有其他样式表之前。

使用，jQuery需要在bootstrape之前引入

css文件放置在head中，js文件放置在body最下面

#### 优势

移动设备优先；响应式设计，自适应于台式机、平板电脑和手机

#### Bootstrap网格系统

提供了一套响应式、移动设备优先的流式网格系统，随着屏幕或视口viewport尺寸的增加，系统会自动分为最多12列。响应式的，列会根据屏幕大小自动重新排列

网格系统分为以下5类：

+ .col：针对所有设备
+ .col-sm：平板（屏幕宽度等于或大于576px）
+ .col-md：桌面显示器（屏幕宽度等于或大于768px）
+ .col-lg：大桌面显示器（屏幕宽度等于或大于992px）
+ .col-xl：超大桌面显示器（屏幕宽度等于或大于1200px）

#### 网格系统规则

+ 网格每一行需要放在设置了.container（固定宽度）或.container-fluid（全屏宽度）类的容器中，这样就可以自动设置一些外边距和内边距
+ 使用行来创建水平的列组
+ 内容需要放置在列中，并且只有列可以是行的直接子节点
+ 预定义的类如.row和.col-sm-4可以用于快速制作网格布局
+ 列通过填充创建列内容之间的间隙，这个间隙是通过.rows类上的负边距设置第一行和最后一列的偏移
+ 网格列是通过跨越指定的12个可用列来创建
+ 使用弹性盒子flexbox而不再使用浮动了，Flexbox的一大优势是，没有指定宽度的网格列将自动设置为等宽与等高列

```html
<!-- 控制列的宽度及在不同的设备上如何显示 -->
<div class="row">
    <div class="col-*-*"></div>
    <div class="col-*-*"></div>
    <div class="col-*-*"></div>
    <div class="col-*-*"></div>
</div>
```

创建一行class="row"，然后添加是需要的列`(.col-*-*类中设置)`，第一个星号`(*)`表示响应的设备：`sm/md/lg/xl`，第二个星号`(*)`表示一个数字，同一行的数字相加为12