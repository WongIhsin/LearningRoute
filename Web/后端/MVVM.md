# MVVM

Model-View-View-Model：关注Model的变化，让MVVM框架去自动更新DOM的状态，从而把开发者从操作DOM的繁琐步骤中解脱出来

常用的MVVM框架有哪些：Angular、Backbone.js、Ember都很困难

尤雨溪的MVVM框架**Vue.js**



使用Vue：直接像引用jQuery一样引用Vue

```html
<script src="https://unpkg.com/vue@2.0.1/dist/vue.js"></script>
```

或者把vue.js下载下来，放到项目的`/static/js`文件夹中，使用本地路径

`vue.js`是未压缩的用于开发的版本，它会在浏览器console中输出很多有用的信息，帮助调试代码，当开发完毕，需要真正发布到服务器时，应该使用压缩过的`vue.min.js`，它会移除所有调试信息，并且文件体积更小

#### 编写MVVM

用Vue将html和JavaScript两者关联起来，**需要特别注意的是**，在`<head>`内部编写的JavaScript代码，需要用jQuery把MVVM的初始化代码推迟到页面加载完毕后执行，否则直接在`<head>`内执行MVVM代码时，DOM节点尚未被浏览器加载，初始化将失败

```html
<html>
    <head>
        <script src="/static/js/jquery.min.js"></script>
        <script src="/static/js/vue.js"></script>
        <script>
            $(function () {
                var vm = new Vue({
                    el: '#vm',
                    data: {
                        name: 'Robot',
                        age: 15
                    }
                });
                window.vm = vm;
            });
        </script>
    </head>
    <body>
        <div id="vm">
            <p>Hello, {{ name }}!</p>
            <p>You are {{ age }} years old!</p>
        </div>
    </body>
</html>
var vm = new Vue({
    el: '#vm',
    data: {
        name: 'Robot',
        age: 15
    }
});
```

Vue作为MVVM框架会自动监听Model的任何变化，在Model数据变化时，更新View的显示。这种Model到View的绑定我们称为单向绑定

另一种单向绑定的方法是使用Vue指令的v-text

```html
<p>Hello, <span v-text="name"></span>!</p>
```

#### 单向绑定

把Model绑定到View，用JavaScript代码更新Model时，View就会自动更新

#### 双向绑定

用户如果更新了View，Model的数据也会自动被更新了，就是双向绑定

```javascript
$(function () {
    var vm = new Vue({
        el: '#vm',
        data: {
            email: '',
            name: ''
        },
        methods: {
            register: function () {
                alert(JSON.stringify(this.$data));
            }
        }
    });
    window.vm = vm;
});
```

```html
<form id="vm" action="#" v-on:submit.prevent="register">
    <p><input v-model="email"></p>
    <p><input v-model="name"></p>
</form>
```

双向绑定最大的好处是不再需要用jQuery去查询表单的状态，而是直接获得了用JavaScript对象表示的Model

#### 处理事件

`<form id="vm" v-on:submit.prevent="register">`其中的`v-on:submit="register"`指令就会自动监听表单的submit事件，并调用register方法处理该事件，使用.prevent表示阻止事件冒泡，这样浏览器不再处理`<form>`的`submit`事件

#### 同步DOM结构

MVVM还有一个重要的用途，让Model和DOM的结构保持同步

```html
<ol>
    <li v-for="t in todos">
        <dl>
            <dt>{{ t.name }}</dt>
            <dd>{{ t.description }}</dd>
        </dl>
    </li>
</ol>
```

```javascript
todos: [
    {
        name: 'A',
        description: 'aaaaaaaaaaa'
    },
    {
        name: 'B',
        description: 'bbbbbbbbbbb'
    },
    {
        name: 'C',
        description: 'ccccccccccc'
    }
]
```

`v-for`指令把数组和一组`<li>`元素绑定了

Vue之所以能够监听Model状态的变化，是因为JavaScript语言本身提供了Proxy或者Object.observe()机制来监听对象状态的变化，但是如果对数组直接赋值是没有办法更新View的，正确的方法是不要对数组元素赋值，而是更新。或者通过`splice()`方法，删除某个元素后，再添加一个元素

#### 同步到服务器端

`vue-resource`这个针对Vue的扩展，它可以给VM对象加上一个`$resource`属性，通过`$resource`来方便地操作API

