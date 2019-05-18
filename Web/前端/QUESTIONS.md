# 个人理解

一个浏览器，发一个请求，拿到服务器的HTML文档，HTML能够读懂JavaScript脚本，JavaScript脚本运行，出现动画之类的什么效果

用户操作后，对应JavaScript脚本运行，修改HTML页面立即被更新

用户操作会产生很多的request，一部分是HTML文档发出去的，这时候就会返回新的页面，另一部分是JavaScript脚本发出的，脚本会接收到数据，然后更新，发生改变，HTML文档被立马更新

#### 同源策略

Same Origin Policy是一种约定，Web是建构在同源策略基础之上的，浏览器只是针对同源策略的一种实现

核心在于它认为自任何站点装载的信赖内容都是不安全的

浏览器不可以跨域访问。但是JavaScript资源可以跨域引用，即JSONP

还有CORS，原理没理解

为什么是服务器端控制，那黑客可以随便操作了

# JSONP

因为可以跨域读取JavaScript资源，可以事先在本地写好函数，然后引用其他资源的JavaScript资源，返回一个JavaScript内容，会去调用本地的function去执行，从而实现跨域



#### JavaScript脚本修改的值貌似会立即显示到页面上

# JavaScript的安全性问题怎么解决的：

- JavaScript可以读取到用户输入的密码
- 表单提交的Request的封装问题
- file文件的路径处理问题，有了**File**和**FileReader**还是只能读到假的路径吗

**猜测可能是浏览器的同源策略**使得不能请求不同的域名，这个或许有一定的道理

# 浏览器的具体逻辑是什么