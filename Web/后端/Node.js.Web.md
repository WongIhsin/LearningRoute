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
   

3. 