# CSS3

#### CSS3边框

```css
div {
    border:2px solid;
    border-radius:25px;
    -moz-border-radius:25px; /*Old Firefox*/
}/*圆角边框*/
div {
    box-shadow: 10px 10px 5px #888888;
}/*边框阴影*/
div {
    border-image:url(border.png) 30 30 round;
}/*边框图片*/
```

#### CSS3背景

```css
div {
    background: url(bg_flower.gif);
    background-size: 63px 100px;
    /*background-size: 63px 100px;*/
    background-repeat: no-repeat;
}/*规定图片尺寸*/
div {
    background: url(bg_flower.gif),url(bg_flower_2.gif);/*多重背景图片*/
    background-repeat: no-repeat;
    background-size: 100% 100%;
    background-origin: content-box;
}/*在content-box中定位背景图片*/
```

#### CSS3文本效果

```css
h1 {
    text-shadow: 5px 5px 5px #FF0000;
}/*标题添加阴影*/
p {
    word-wrap:break-word;
}/*允许对长单词进行拆分，并换行到下一行*/
```

#### CSS3字体

```html
<style>
    @font-face
    {
        font-family: myFirstFont;
        src: url('Sansation_Light.ttf'),
             url('Sansation_Light.eot');
    }
    @font-face
    {
        font-family: myFirstFont;
        src: url('Sansation_Bold.ttf'),
             url('Sansation_Bold.eot');
        font-weight: bold;
    }
    div {
        font-family: myFirstFont;
    }
</style>
```

@font-face规则

#### CSS3 2D转换、3D转换

translate()、rotate()、scale()、skew()、matrix()

rotateX()、rotateY()

### CSS3过渡

```css
div {
    transition: width 2s;
}
div:hover {
    width: 300px;
}
```

#### CSS3动画

```css
@keyframes myfirst
{
    from {background: red;}
    to {background: yellow;}
}
div {
    animation: myfirst 5s;
}
```

@keyframes规则用于创建动画，在@keyframes中规定某项CSS样式，就能创建由当前样式逐渐改为新样式的动画效果

#### CSS3多列

column-count属性规定元素应该被分隔的列数

```css
div {
    column-count:3;
    column-gap:40px;
    column-rule:3px outset #ff0000;
}
```

#### CSS用户界面

CSS3 Resizing规定是否可由用户调整元素尺寸

```css
div {
    resize:both;
    overflow:auto;
}
```

CSS3 Box Sizing以确切的方式定义适应某个区域的具体内容

```css
div {
    box-sizing:border-box;
    width:50%;
    float:left;
}/*规定两个并排的带边框方框*/
```

CSS3 Outline Offset属性对轮廓进行偏移，并在超出边框边缘的位置绘制轮廓

```css
div {
    border:2px solid black;
    outline:2px solid red;
    outline-offset:15px;
}
```

