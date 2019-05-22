# Web开发

#### koa

koa是Express的下一代基于Node.js的web框架，目前有1.x和2.0两个版本

1. Express
   第一代最流行的web框架，对Node.js的http进行了封装

   ```javascript
   var express = require('express');
   var app = express();
   app.get('/', function (req, res) {
       res.send('Hello World!');
   });
   app.listen(3000, function () {
       console.log('Example app listening on port 3000!');
   });
   ```

   基于ES5的语法，要实现异步代码，只有一个方法：回调，如果异步嵌套层次过多，代码写起来就非常难看

   ```javascript
   app.get('/test', function (req, res) {
       fs.readFile('/file1', function (err, data) {
           if (err) {
               res.status(500).send('read file1 error');
           }
           fs.readFile('/file2', function (err, data) {
               if (err) {
                   res.status(500).send('read file2 error');
               }
               res.type('text/plain');
               res.send(data);
           });
       });
   });
   ```

   虽然可以用async这样的库来组织异步代码，但是用回调写异步实在太痛苦了

2. koa 1.0

   ```javascript
   //基于ES6的generator重新编写了下一代的web框架koa，使用generator实现异步
   var koa = require('koa');
   var app = koa();
   app.use('/test', function* () {
       yield doReadFile1();
       var data = yield doReadFile2();
       this.body = data;
   });
   app.listen(3000);
   ```

   简化异步代码，ES7的新的关键字async和await，可以轻松地把一个function变为异步模式

   ```javascript
   async function () {
       var data = await fs.read('/file1');
   }
   ```

3. koa2

   完全使用Promise并配合async来实现异步

   ```javascript
   app.use(async (ctx, next) => {
       await next();
       var data = await doReadFile();
       ctx.response.type = 'text/plain';
       ctx.response.body = data;
   });
   ```

#### koa入门

```javascript
const Koa = require('koa');
const app = new Koa();
app.use(async (ctx, next) => {
    await next();
    ctx.response.type = 'text/html';
    ctx.response.body = '<h1>Hello, koa2!</h1>';
});
app.listen(3000);
console.log('app started at port 3000...');
```

对应每一个http请求，koa将调用我们传入的异步函数来处理

参数`ctx`是由koa传入的封装了request和reponse的变量，我们可以通过它访问request和response，`next`是koa传入的将要处理的下一个异步函数

#### koa middleware

每收到一个http请求，koa就会调用通过`app.use()`注册的async函数，并传入ctx和next参数

koa把很多async函数组成一个处理链，每个async函数都可以做一些自己的事情，然后用`await next()`来调用下一个async函数，我们把每个async函数称为middleware，这些middleware可以组合起来，完成很多有用的功能

```javascript
app.use(async (ctx, next) => {
    console.log(`${ctx.request.method} ${ctx.request.url}`);
    await next();//调用下一个middleware
});
app.use(async (ctx, next) => {
    const start = new Date().getTime();
    await next();
    const ms = new Date().getTime() - start;
    console.log(`Time: ${ms}ms`);
});
app.use(async (ctx, next) => {
    await next();
    ctx.response.type = 'text/html';
    ctx.response.body = '<h1>Hello, koa2!</h1>';
});
```

middleware的顺序很重要，也就是调用`app.use()`的顺序决定了middleware的顺序

#### koa-router

`koa-router`这个middleware来让他负责处理URL映射

```javascript
const Koa = require('koa');
const router = require('koa-router')();
const app = new Koa();
const bodyParser = require('koa-bodyparser');
app.use(async (ctx, next) => {
    console.log(`Process ${ctx.request.method} ${ctx.request.url}...`);
    await next();
});
app.use(bodyParser());
router.get('/hello/:name', async (ctx, next) => {
    var name = ctx.params.name;
    ctx.response.body = `<h1>Hello, ${name}!</h1>`;
});
router.get('/', async (ctx, next) => {
    ctx.response.body = `<h1>Index</h1>
		<form action="/signin" method="post">
			<p>Name: <input name="name" value="koa"></p>
			<P>Password: <input name="password" type="password"></p>
			<p><input type="submit" value="Submit"></p>
		</form>`;
});
router.post('/signin', async (ctx, next) => {
    var
    	name = ctx.request.body.name || '',
        password = ctx.request.body.password || '';
    console.log(`signin with name: ${name}, password: ${password}`);
    if (name === 'koa' && password === '12345') {
        ctx.response.body = `<h1>Welcome, ${name}!</h1>`;
    } else {
        ctx.response.body = `<h1>Login failed!</h1>
		<p><a href="/">Try again</a></p>`;
    }
});
app.use(router.routes());
app.listen(3000);
console.log('app started at port 3000...');
```

对于POST方法，Node.js提供的原始request对象和koa提供的request对象，都不提供解析request的body的功能，需要使用`koa-bodyparser`

---

#### 使用Nunjucks（不懂）

一个模块引擎，就是基于模板数据构造出字符串输出的一个组件。模板引擎最常见的输出就是输出网页，也就是HTML文本，当然也可输出任意格式的文本

输出HTML有几个特别重要的问题要考虑

+ 转义：对特殊字符要转义，避免受到XSS攻击
+ 格式化：对不同类型的变量要格式化
+ 简单逻辑：模板还需要能执行一些简单逻辑，比如按条件if输出内容

Nunjucks是Mozilla开发的一个纯JavaScript编写的模板引擎，既可以用在Node环境下，又可以运行在浏览器端，主要还是运行在Node环境下，浏览器端又更好的模板解决方法，如MVVM框架等

#### Nunjucks就是用JavaScript重新实现了jinja2

本质上只需要构造一个函数：`function render(view, model) {...}`

`view`是模板的名称，又称为视图，因为可能存在多个模板，需要选择其中一个，`model`就是数据，在JavaScript中，就是一个简单的Object

`render`函数返回一个字符串，就是模板的输出

```javascript
//模板引擎
const nunjucks = require('nunjucks');
function createEnv(path, opts) {
    var 
    	autoescape = opts.autoescape === underfined ? true : opts.autoescape,
        noCache = opts.noCache || false,
        watch = opts.watch || false,
        throwOnUndefined = opts.throwOnUndefined || false,
        env = new nunjucks.Environment(
            new nunjucks.FileSystemLoader('views', {
                noCache: noCache,
                watch: watch,
            }), {
                autoescape: autoescape,
                throwOnUndefined: throwOnUndefined
            });
    if (opts.filters) {
        for (var f in opts.filters) {
            env.addFilter(f, opts.filters[f]);
        }
    }
    return env;
}

var env = createEnv('views', {
    watch: true,
    filters: {
        hex: function (n) {
            return '0x' + n.toString(16);
        }
    }
});

var s = env.render('hello.html', {name: '小明'});
console.log(s);
```

#### 性能

对于模板渲染本身来说，速度是非常快速的，因为就是拼接字符串，纯CPU操作，性能问题主要出现在从文件读取模板内容这一步。这是一个IO操作，在Node.js环境中，我们知道，单线程的JavaScript最不能忍受的就是同步IO

#### Nunjucks默认就使用同步IO读取模板文件

好消息是Nunjucks会缓存已读取的文件内容，也就是说，模板文件最多读取一次，就会放在内存中，后面的请求是不会再次读取文件的，只要我们指定了noCache: false这个参数

在开发环境下，可以关闭cache，这样每次重新加载模板，便于实时修改模板，在生产环境下，一定要打开cache，这样就不会有性能问题

---

#### mysql

访问数据库。Node的ORM框架Sequelize来操作数据库

Node.js程序如何访问MySQL数据库？**通过网络发送SQL命令**，然后MySQL服务器执行后返回结果

---

#### mocha

是JavaScript的一种单元测试框架

---

#### WebSocket

是HTML5新增的协议，它的目的是在浏览器和服务器之间建立一个不受限的双向通信的通道，任何一方都可以主动发消息给对方

首先，WebSocket连接必须由浏览器发起，因为请求协议是一个标准的HTTP请求。GET请求，地址不是类似`/path/`，而是以`ws://`开头的地址，请求头`Upgrade: websocket`和`Connection: Upgrade`表示这个连接将要被转换为WebSocket连接；`Sec-WebSocket-Key`是用于标识这个连接，并非用于加密数据；`Sec-WebSocket-Version`指定了WebSocket的协议版本

在Node.js中，使用最广泛的WebSocket模块是`ws`

---

# REST

#### 编写REST API

编写REST API，实际上就是编写处理HTTP请求的async函数，不过，REST请求和普通的HTTP请求有几个特殊的地方

+ REST请求仍然是标准的HTTP请求，但是除了GET请求外，POST、PUT等请求的body是JSON数据格式，请求的`Content-Type`为`application/json`
+ REST响应返回的结果是JSON数据格式，因此，响应的`Content-Type`也是`application/json`

REST规范定义了资源的通用访问格式，但不是一个强制要求

#### 开发REST API

使用REST和使用MVC是类似的，不同的是，提供REST的Controller处理函数最后不调用`render()`去渲染模板，而是把结果直接用JSON序列化返回给客户端

#### 错误处理

REST架构本身对错误处理没有统一的规定，实际应用时，各种各样的错误处理机制都有，有的设计得比较合理，有的设计的不合理，导致客户端尤其是手机客户端处理API简直就是噩梦

客户端会遇到两种类型的REST API错误

一是403、404、500等错误，这些错误实际上是HTTP请求可能发生的错误，REST请求只是一种请求类型和响应类型均为JSON的HTTP请求，因此，这些错误在REST请求中也会发生。此时客户端除了提示用户出现了网络错误稍后重试以外，并无法获得具体的错误信息

二是业务逻辑的错误，例如输入了不合法的Email地址，试图删除一个不存在的Product等，这种类型的错误完全可以通过JSON返回给客户端，这样客户端可以根据错误信息提示用户

如果一个REST异步函数想要返回错误，建议异步函数直接用throw语句抛出错误，让middleware去处理错误

