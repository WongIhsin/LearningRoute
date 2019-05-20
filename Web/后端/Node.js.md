# Node.js

JavaScript是单线程执行

Ryan Dahl在2009年推出的，基于JavaScript语言和V8引擎的开源Web服务器项目，命名为Node.js。第一次把JavaScript带入到后端服务器开发

**优势**：JavaScript天生的事件驱动机制加V8高性能引擎

JavaScript语言本身是完善的函数式语言，在前端开发中，开发人员往往写得比较随意，在Node环境下，通过模块化的JavaScript代码，加上函数式编程，并且无需考虑浏览器兼容性问题，直接使用最新的ECMAScript 6标准，可以完全满足工程上的需求

`io.js`是尝鲜版，一些新的特性如果大家测试的比较好用，就把新特性加入到Node.js

---

#### Node.js环境和npm

Node.js平台是在后端运行JavaScript代码，所以必须首先在本机安装Node环境

#### npm

类似于mvn于Java，pip于Python，npm是Node.js的包管理工具

使用严格模式，如果有很多JavaScript文件，每个文件都写上`'use strict';`很麻烦，可以如下给Node.js传递一个参数，让Node直接为所有js文件开启严格模式

```bash
node --use_strict hello.js
```

---

#### 模块

在Node环境中，一个.js文件就称之为一个模块module，大大提高了代码的可维护性，其次，编写代码不必从零开始，当一个模块编写完毕，可以被其他地方引用，在编写程序的时候，也经常引用其他模块，包括Node内置的模块和其他第三方的模块

还可以避免函数名和变量名的冲突

```javascript
'use strict';
var s = 'Hello';
function great(name) {
    console.log(s + ', ' + name + '!');
}
module.exports = great;
```

`module.exports = great;`意思是把函数`great()`作为模块的输出暴露出去，这样其他模块就可以使用`great`函数了

其他模块如何使用其他模块的函数？

```javascript
'use strict';
var great = require('./hello');//引入hello模块用Node提供的require函数
var s = 'Michael';
great(s);
```

如果`require('hello')`函数只传入了模块名，Node会依次在内置模块、全局模块和当前模块下查找`hello.js`

这种模块加载机制称为**CommonJS规范**，在这个规范下，每个`.js`文件都是一个模块，他们内部各自使用的变量名和函数名都互不冲突

一个模块想要对外暴露变量（函数也是变量），可以用`module.exports = variable;`，一个模块要引用其他模块暴露的变量，用`var ref = require('module_name');`就拿到了引用模块的变量 

---

#### 模块原理

在浏览器中，大量使用全局变量可不好，在`a.js`中使用了全局变量`s`，在`b.js`中也是用全局变量`s`，将造成冲突，`b.js`中对`s`赋值会改变`a.js`的运行逻辑。JavaScript语言本身没有一种模块机制来保证不同模块可以使用相同的变量名

`Node.js`使用闭包来将一段JavaScript代码用一个函数包装起来，这段代码的所有全局变量也就变成了函数内部的局部变量

Node.js加载了hello.js之后，可以把代码包装一下

```javascript
(function () {
    //hello.js代码开始
    var s = 'Hello';
    var name = 'world';
    console.log(s + ' ' + name + '!');
    //hello.js代码结束
})();
```

模块的输出`module.exports`实现

```javascript
var module = {
    id: 'hello',
    exports: {}
};
var load = function (module) {
    //读取的hello.js代码
    function great(name) {
        console.log('Hello, ' + name + '!');
    }
    module.exports = great;
    //hello.js代码结束
    return module.exports;
};
var exported = load(module);
//通过把参数module传递给load()函数，hello.js就顺利地把一个变量传递给了Node执行环境，Node会把module变量保存到某个地方
save(module, exported);
```

Node保存了所有导入的module，当我们用`require()`获取module时，Node就找到对应的module，把这个module的exports变量返回，这样，另一个模块就顺利拿到了模块的输出

#### 两种Node环境中在一个模块中输出变量：

```javascript
function hello() {
    console.log('Hello, world!');
}
function greet(name) {
    console.log('Hello, ' + name + '!');
}
module.exports = {
    hello: hello,
    greet: greet
};
```

```javascript
function hello() {
    console.log('Hello, world!');
}
function greet(name) {
    console.log('Hello, ' + name + '!');
}
exports.hello = hello;
exports.greet = greet;
//不可以直接给exports赋值
exports = {
    hello: hello,
    greet: greet
};
```

#### 结论

如果要输出一个键值对象`{}`，可以利用`exports`这个已经存在的空对象`{}`，并继续在上面添加新的键值

如要输出一个函数或数组，必须直接对`module.exports`对象赋值

### 基本模块

Node.js是运行在服务器端的JavaScript环境，服务器程序和浏览器程序相比，最大的特点是没有浏览器的安全限制，而且，服务器程序必须能接受网络请求，读写文件，处理二进制内容，所以，Node.js内置的常用模块就是为了实现基本的服务器功能，这些模块在浏览器中是无法被执行的，因为他们的底层代码是用C/C++在Node.js运行环境中实现的

#### global

JavaScript有且仅有一个全局对象，在浏览器中，叫`window`对象，在Node.js环境中，也有唯一的全局对象，叫`global`，其对象的属性和方法也和浏览器环境的`window`不同

#### process

也是Node.js提供的一个对象，它代表当前Node.js进程，通过process对象可以拿到许多有用的信息，也是`process === global.process; // true`。JavaScript程序是由事件驱动执行的单线程模型，Node.js也不例外。Node.js不断执行响应事件的JavaScript函数，直到没有任何响应事件的函数可以执行时，Node.js就退出了

如果想要在下一次事件响应中执行代码，可以调用`process.nextTick()`

```javascript
//process.nextTick()将在下一轮事件循环中调用
process.nextTick(function () {
    console.log('nextTick callback!');
});
console.log('nextTick was set!');
//nextTick was set!
//nextTick callback!
```

`process.nextTick()`函数不是立刻执行，而是要等到下一次事件循环

Node.js进程本身的事件就由process对象来处理，如果我们响应exit事件，就可以在程序即将退出时执行某个回调函数

```javascript
process.on('exit', function (code) {
    console.log('about to exit with code: ' + code);
});
```

判断JavaScript执行环境，有时候程序本身需要判断自己到底是在什么环境下执行的，常用的方法就是根据浏览器和Node环境提供的全局变量名称来判断

```javascript
if (typeof(window) === 'undefined') {
    console.log('node.js');
} else {
    console.log('browser');
}
```

---

### 基本模块

#### fs

Node.js内置的`fs`模块就是文件系统模块，负责读写文件，fs模块同时提供了同步和异步的方法

**异步读文件**，安装JavaScript的标准，异步读取一个文本文件的代码如下

```javascript
'use strict';
var fs = require('fs');
fs.readFile('sample.txt', 'utf-8', function (err, data) {
    if (err) {
        console.log(err);
    } else {
        console.log(data);
    }
});
//当读取二进制文件时，不传入文件编码时，回调函数的data参数将返回一个Buffer对象，Node.js的Buffer对象就是一个包含零个或任意个字节的数组，注意和Array不同
fs.readFile('sample.png', function (err, data) {
    if (err) {
        console.log(err);
    } else {
        // Buffer -> String
        //var text = data.toString('utf-8');
        //console.log(text);
        // String -> Buffer
        //var buf = Buffer.from(text, 'utf-8');
        //console.log(buf);
        console.log(data);
        console.log(data.length + ' bytes');
    }
});
```

正常读取时，err参数为null，data参数为读取到的String；读取发生错误时，err参数代表一个错误对象，data为undefined。Node.js标准的回调函数：第一个参数代表错误信息，第二个参数代表结果

**同步读文件**

```javascript
'use strict';
var fs = require('fs');
var data = fs.readFileSync('sample.txt', 'utf-8');
console.log(data);
```

**写文件**

```javascript
'use strict';
var fs = require('fs');
var data = 'Hello, Node.js';
fs.writeFile('output.txt', data, function (err) {
    if (err) {
        console.log(err);
    } else {
        console.log('ok');
    }
});//传入数据是String，默认按UTF-8编码写入文本文件。如果传入数据是Buffer, 则写入的是二进制文件
//同步方法
'use strict';
var fs = require('fs');
var data = 'Hello, Node.js';
fs.writeFileSync('output.txt', data);
```

**stat**：如果我们要获取文件大小、创建时间等信息，可以使用`fs.stat()`，它返回一个`Stat`对象，能告诉我们文件或目录的详细信息

```javascript
'use strict';
var fs = require('fs');
fs.stat('sample.txt', function (err, stat) {
    if (err) {
        console.log(err);
    } else {
        console.log('isFile: ' + stat.isFile());
        console.log('isDirectory: ' + stat.isDirectory());
        if (stat.isFile()) {
            console.log('size: ' + stat.size);
            console.log('birth time: ' + stat.birthtime);
            console.log('modified time: ' + stat.mtime);
        }
    }
});
```

**异步还是同步**，在`fs`模块中，提供同步方法是为了方便使用，由于Node环境执行的JavaScript代码是服务器端代码，所有，绝大部分需要在服务器运行期反复执行业务逻辑代码，**必须使用异步代码**，否则，同步代码在执行时期，服务器将停止响应，因为JavaScript只有一个执行线程

服务器启动时如果需要读取配置文件，或者结束时需要写入到状态文件时，可以使用同步代码，因为这些代码只在启动和结束时执行一次，不影响服务器正常运行时的异步执行

---

#### stream

支持“流”这种数据结构。流是一种抽象的数据结构，可以把数据看成是数据流。比如敲键盘时，就可以把每个字符依次连接起来，看成字符流，这个流是从键盘输入到应用程序，实际上它还对应着一个名字：标准输入流`stdin`

如果应用程序把字符一个一个输出到显示器上，这也可以看成是一个流，这个流也有名字：标准输出流`stdout`。

流的特点是数据是有序的，而且必须依次读取，或者依次写入，不能像Array那样随机定位

Node.js中，流也是一个对象，我们只需要响应流的事件就可以了：data事件表示流的数据已经可以读取了，end事件表示这个流已经到末尾了，没有数据可以读取了，error事件表示出错了

```javascript
'use strict';
var fs = require('fs');
var rs = fs.createReadStream('sample.txt', 'utf-8');
rs.on('data', function (chunk) {
    console.log('DATA');
    console.log(chunk);
});
//data事件可能会有多次，每次传递的chunk是流的一部分数据
rs.on('end', function () {
    console.log('END');
});
rs.on('error', function (err) {
    console.log('ERROR: ' + err);
});
```

data事件可能会有多次，每次传递的chunk是流的一部分数据

要以流的形式写入文件，只需要不断调用write()方法，最后以end()结束

```javascript
'use strict';
var fs = require('fs');
var ws1 = fs.createWriteStream('output1.txt', 'utf-8');
ws1.write('使用Stream写入文本数据...\n');
ws1.write('END.');
ws1.end();
var ws2 = fs.createWriteStream('output2.txt');
ws2.write(new Buffer('使用Stream写入二进制数据...\n', 'utf-8'));
ws2.write(new Buffer('END.', 'utf-8'));
ws2.end();
```

所有可以读取数据的流都继承自`stream.Readable`，所有可以写入的流都继承自`stream.Writable`

#### pipe

两个流可以连接串起来，像两个水管串成一个更长的水管一样，一个`Readable`和一个`Writeable`流串起来后，所有的数据自动从`Readable`流进入`Writeable`流，这种操作叫`pipe`

`Readable`流的`pipe()`方法

```javascript
'use strict';
var fs = require('fs');
var rs = fs.createReadStream('sample.txt');
var ws = fs.createWriteStream('copied.txt');
rs.pipe(ws);
```

默认情况下，Readable流的数据读取完毕，end事件触发后，将自动关闭Writeable流，如果我们不希望自动关闭Writeable流，需要传入参数

```javascript
readable.pipe(writable, {end: false});
```

---

#### http

HTTP服务器：应用程序直接操作http模块提供的`request`和`response`对象

```javascript
'use strict';
var http = require('http');
var server = http.createServer(function (request, response) {
    console.log(request.method + ': ' + request.url);
    response.writeHead(200, {'Content-Type': 'text/html'});
    response.end('<h1>Hello world!</h1>');
});
server.listen(8080);
console.log('Server is running at http://127.0.0.1:8080/');
```

**文件服务器**：解析`request.url`中的路径，解析URL需要用到Node.js提供的url模块

```javascript
'use strict';
var url = require('url');
console.log(url.parse('http://user:pass@host.com:8080/path/to/file?query=string#hash'));
```

处理文档文件目录需要使用Node.js提供的path模块，可以方便地构造目录

```javascript
'use strict';
var path = require('path');
var workDir = path.resolve('.');//解析当前目录
var filePath = path.join(workDir, 'pub', 'index.html');
```

实现一个文件服务器

```javascript
'use strict';
var fs = require('fs'),
    url = require('url'),
    path = require('path'),
    http = require('http');
//从命令行参数获取root目录，默认是当前目录
var root = path.resolve(process.argv[2] || '.');
console.log('Static root dir: ' + root);
var server = http.createServer(function (request, response) {
    var pathname = url.parse(request.url).pathname;
    var filepath = path.join(root, pathname);
    fs.stat(filepath, function (err, stats) {
        if (!err && stats.isFile()) {
            console.log('200 ' + request.url);
            response.writeHead(200);
            fs.createReadStream(filepath).pipe(response);
        } else {
            console.log('404 ' + request.url);
            response.writeHead(404);
            response.end('404 Not Found');
        }
    });
});
server.listen(8080);
console.log('Server is running at http://127.0.0.1:8080/');
```

`response`对象本身是一个`Writable Stream`，直接用`pipe()`方法就实现了自动读取文件内容并输出到HTTP响应

---

#### Web开发

静态Web页面：由文本编辑器直接编辑并生成静态的HTML页面，如果要修改Web页面的内容，就需要再次编辑HTML源文件

CGI：要处理用户发送的动态数据，出现了Common Gateway Interface，用C/C++编写

ASP/JSP/PHP：由于Web应用的特点是修改频繁，用C/C++这样的低价语言非常不适合Web开发，而脚本语言由于开发效率高，与HTML结合密切，因此，迅速取代了CGI模式。ASP是微软推出的VBScript脚本编程的Web开发技术，JSP用Java来编写脚本，PHP则本身就是开源的脚本语言

MVC：为了解决直接用脚本语言嵌入HTML导致的可维护性差的问题，Web应用也引入了Model-View-Controller的模式来简化Web开发。ASP发展成为ASP.Net，JSP和PHP也有一大堆MVC框架

目前，Web技术仍在快速发展中，异步开发、新的MVVM前端技术层出不穷

#### 显著优势

前端JavaScript开发人员现在同时可以编写后端代码；
前后端统一使用JavaScript，就没有切换语言的障碍了；
速度快、非常快，得益于Node.js天生是异步的

目前，在npm上已发布的开源Node.js模块数量超过了30万个

---

# 不懂，再看crypto

#### crypto

该模块的目的是为了提供通用的加密和哈希算法。用纯JavaScript代码实现这些功能不是不可能，但速度会非常慢，Node.js用C/C++实现这些算法后，通过crypto这个模块暴露为JavaScript接口，这样用起来方便，运行速度也快

**MD5和SHA1**：

```javascript
const crypto = require('crypto');
const hash = crypto.createHash('md5');
hash.update('Hello, world!');
hash.update('Hello, nodejs!');
console.log(hash.digest('hex'));//'ef939a842ea8b4551ec17ae038090f10' 
//update()方法默认为utf-8，也可传入Buffer
```

**Hmac**：可以利用MD5或SHA1等哈希算法，还需要一个密钥，只要密钥发生了变化，那么同样的输入数据也会得到不同的签名，Hmac可以理解为用随机数**增强**的哈希算法

```javascript
const crypto = require('crypto');
const hmac = crypto.createHmac('sha256', 'secret-key');
hmac.update('Hello, world!');
hmac.update('Hello, nodejs');
console.log(hmac.digest('hex'));
```

**AES**：常用的对称加密算法，加解密都用同一个密钥，crypto模块提供了AES支持，但是需要自己封装好函数，便于使用

```javascript
const crypto = require('crypto');
function aesEncrypt(data, key) {
    const cipher = crypto.createCipher('aes192', key);
    var crypted = cipher.update(data, 'utf8', 'hex');
    crypted += cipher.final('hex');
    return crypted;
}
function aesDecrypt(encrypted, key) {
    const decipher = crypto.createDecipher('aes192', key);
    var decrypted = decipher.update(encrypted, 'hex', 'utf8');
    decrypted += decipher.final('utf8');
    return decrypted;
}
var data = 'Hello, this is a secret message!';
var key = 'Password';
var encrypted = aesEncrypt(data, key);
var decrypted = aesDecrypt(encrypted, key);
console.log('Plain text: ' + data);
console.log('Encrypted text: ' + encrypted);
console.log('Decrypted text: ' + decrypted);
```

AES有很多不同的算法，如aes192，aes-128-ecb，aes-256-cbc等，加密结构通常有两种表示方法：hex和base64

**Diffie-Hellman**：DH算法是一种密钥交换协议，可以让双方在不泄露密钥的情况下协商出一个密钥来

```javascript
const crypto = require('crypto');
var ming = crypto.createDiffieHellman(512);
var ming_keys = ming.generateKeys();
var prime = ming.getPrime();
var gemerator = ming.getGenerator();
console.log('Prime: ' + prime.toString('hex'));
console.log('Generator: ' + generator.toString('hex'));
var hong = crypto.createDiffieHellman(prime, generator);
var hong_keys = hong.generateKeys();
var ming_secret = ming.computeSecret(hong_keys);
var hong_secret = hong.computeSecret(ming_keys);
console.log('Secret of Xiao Ming: ' + ming_secret.toString('hex'));
console.log('Secret of Xiao Hone: ' + hong_secret.toString('hex'));
```

**RSA**：RSA算法是一种非对称加密算法，即由一个私钥和一个公钥构成的密钥对，通过私钥加密，公钥解密，或者通过公钥加密，私钥解密，其中，公钥可以公开，私钥必须保密

```javascript
const fs = require('fs'),
      crypto = require('cryptoo');
function loadKey(file) {
    return fs.readFileSync(file, 'utf8');
}
let prvKey = loadKey('./rsa-prv.pem'),
    pubKey = loadKey('./rsa-pub.pem'),
    message = 'Hello, world!';
let enc_by_prv = crypto.privateEncrypt(prvKey, Buffer.from(message, 'utf8'));
console.log('encrypted by private key: ' + enc_by_prv.toString('hex'));
let dec_by_pub = crypto.publicDecrypt(pubKey, enc_by_prv);
console.log('decrypted by public key: ' + dec_by_pub.toString('utf8'));
```

如果把message字符串的长度加到很长，这是，执行RSA加密会得到一个类似这样的错误：`data too large fot key size`，这是因为RSA加密的原始信息必须小于Key的长度

很长的消息时，RSA并不适合加密大数据，而是先生成一个随机的AES密码，用AES加密原始信息，然后用RSA加密AES口令，这样，实际使用RSA时，给对方传的密文分两部分，一部分是AES加密的密文，另一部分是RSA加密的AES口令，对方用RSA先解密出AES口令，再用AES解密密文，即可获得明文

**证书**：crypto模块也可以处理数字证书，数字证书通常用在SSL连接，也就是Web的https连接，一般情况下，https连接只需要处理服务器端的单向认证，如无特殊需求，建立用反向代理服务器如Nginx等Web服务器去处理证书

