2019.5.2 + 2019.5.4

### HTTP

#### 基本概念

1. HTTP：Hyper Text Transfer Protocol 超文本传输协议
2. 用于从万维网 WWW：World Wide Web 服务器传输超文本到本地浏览器的传送协议
   		**互联网包含因特网，因特网包含万维网**
      		**只要应用层使用了HTTP协议，就可以称为万维网**
3. HTTP 应用层协议，面向对象的协议，适用于分布式超媒体信息系统
4. 基于TCP/IP通信协议来传递数据

#### 特点

1. 无连接
每次连接只处理一个请求，服务器处理完客户的请求，并收到客户的应答后，即断开连接，节省传输时间
   
2. 无状态
对于事物处理没有记忆能力，如果后续处理需要前面的信息，必须重传

***无状态无连接不代表HTTP不能保持TCP连接，更不能代表HTTP使用的是UDP协议***

***Keep-Alive不会永久保持连接，它有一个保持时间，可以在不同的服务器软件中设定这个时间***

#### URL

`http://www.xxxx.com:8080/news/index.asp?boardID=5&ID=2468&page=1#name`

1. **协议部分**

   http	https 	ftp等

2. **域名部分**

   `www.baidu.com`一个URL中也可以使用IP地址作为域名使用

3. **端口部分**

   端口不是一个URL必须的部分，省略端口部分，将采用默认端口

4. **虚拟目录部分**

   第一个 / 到最后一个 / 为止，是虚拟目录部分，也不是一个URL必须的部分

5. **文件名部分**

   最后一个 / 到 ? 是文件名部分，如果没有 ? 则到 # 为止

   如果都没有则到结束都是文件名部分

   不是URL必须的，如果省略文件名部分，则使用默认的文件名

6. **锚部分**

   从 # 到结束都是锚部分，非必须

7. **参数部分**

   从 ? 到 # 之间都是参数部分，又称为**搜索部分**，**查询部分**，& 为分隔符

#### URL和URI的区别

URI Uniform Resource Identifiers 统一资源标识符

URL Uniform Resource Locator 统一资源定位符

URN Uniform Resource Name 统一资源命名

Web上可用的每种资源，如HTML文档、图像、视频、程序等都是一个URI来定位的

URI一般由三部分组成：访问资源的命名机制、存放资源的主机名、资源自身的名称，由路径表示，着重强调于资源

**URL**一般由三部分组成：**协议(服务方式)**、**存有该资源的主机IP地址(有时也有端口号)**、**主机资源的具体地址，目录和文件名等**

URI是以一种抽象的，高层次概念定义统一资源标识，URL和URN则是具体的资源标识方式，都是一种 URI

Java中URI类不包含任何访问资源的方法，唯一作用就是解析，URL类可以打开一个到达资源的流

URL是URI的子集，是一种具体的URI

#### Request

客户端发送一个HTTP请求到服务器的请求消息包括以下格式：1. **请求行 request line**；2. **请求头部 header**；3. **空行**；4. **请求数据**

1. 请求行

   `GET / HTTP/1.1`

   指定方法、资源路径、协议版本

2. 请求头部

   `Host img.xxx.com`

   `User-Agent Mozilla/5.0 (Windows NT 10.0; WOW64) AppleWebKit/537.35 (KHTML, like Gecko) Chrome/51.0.2704.106 Safari/537.36`

   服务器要使用的附加信息

   Host 请求目的地；User-Agent 服务器端和客户端脚本都能访问它，是浏览器类型检测逻辑的重要基础，由浏览器来定义，在每个请求中自动发送

3. 空行，必须要有

4. 主体，任意数据，也可以为空

#### Response

服务器接收并处理客户端发来的请求后会返回一个HTTP响应消息

**状态行**、**消息报头**、**空行**、**响应正文**

`HTTP/1.1 200 OK
Date: Fri, 22 May 2009 06:07:21 GMT
Conten-Type: text/html; charset=UTF-8`

`<html>...</html>`

1. 状态行

   HTTP协议版本号、状态码、状态消息 三部分组成

2. 消息报头

   Date：生成响应的日期和时间

   Content-Type：指定了MIME类型的HTML(text/html)，编码类型

3. 空行，必须有

4. 响应正文，即服务器返回给客户端的文本信息

#### 状态码

1xx：指示信息——表示请求已经接收，继续处理
2xx：成功——表示请求已经被成功接收、理解、接受
3xx：重定向——要完成请求必须更进一步的操作
4xx：客户端错误——请求有语法错误或请求无法实现
5xx：服务器端错误——服务器未能实现合法的请求

常见的有
200 OK
400 Bad Request 语法错误，不能被服务器所理解
403 Unauthorized 未经授权
404 Not Found 资源不存在
500 Internet Server Error 服务器不可预期的错误
503 Server Unavailable 服务器当前不能处理客户端请求，一段时间后可能恢复正常

#### 请求方法

HTTP/1.0	GET、POST、HEAD
HTTP/1.1	新添了OPTIONS、PUT、DELETE、TRACE、CONNECT

GET：请求指定的页面信息，并返回实体主体
HEAD：类似GET请求，只不过返回的响应中没有具体内容，用户获取报头
POST：向指定资源提交数据进行处理请求(例如提交表单或者上传文件)，数据被包含在请求体中；POST请求可能会导致新的资源的建立或已有资源的修改
DELETE：请求服务器删除指定的页面
CONNECT：HTTP/1.1协议中预留给能够将连接改为管道方式的代理服务器
OPTIONS：允许客户端查看服务器的性能
TRACE：回显服务器收到的请求，主要用于测试或诊断

#### HTTP工作原理

1. HTTP协议定义Web客户端如何从Web服务器请求Web页面，以及服务器如何把Web页面传送给客户端

2. 具体步骤

   1. 客户端连接到Web服务器：一个HTTP客户端，通常是浏览器，与Web服务器的HTTP端口(默认为80)建立一个TCP套接字连接
   2. 发送HTTP请求：通过TCP套接字，客户端向Web服务器发送一个文本的请求报文，一个请求报文由请求行、请求头部、空行和请求数据4部分组成
   3. 服务器接受请求并返回HTTP响应：Web服务器解析请求，定位请求资源。服务器将资源副本写到TCP套接字，由客户端读取，一个响应由状态行、响应头部、空行和响应数据4部分组成
   4. 释放连接TCP连接：若connection模式为close，则服务器主动关闭TCP连接，客户端被动关闭连接，释放TCP连接；若connection模式为keepalive，则该连接会保持一段时间，在该时间内可以继续接收请求
   5. 客户端浏览器解析HTML内容：客户端浏览器首先解析状态行，查看表明请求是否成功的状态代码。然后解析每一个响应头，响应头告知以下为若干字节的HTML文档和文档的字符集，客户端浏览器读取响应数据HTML，根据HTML的语法对其进行格式化，并在浏览器窗口中显示

3. 实例

   在浏览器地址栏输入URL，按下回车

   1. 浏览器向DNS服务器请求解析该URL中的域名所对应的IP地址
   2. 解析出IP地址后，根据该IP地址和默认端口80，和服务器建立TCP连接
   3. 浏览器发出读取文件(URL 中域名后面部分对应的文件)的HTTP请求，该请求报文作为TCP三次握手的第三个报文的数据发送给服务器
   4. 服务器对浏览器请求作出响应，并把对应的html文本发送给浏览器
   5. 释放TCP连接
   6. 浏览器将该html文本并显示内容

#### POST和GET的区别

1. GET提交

   请求的数据会附在URL之后，用?分割URL和传输数据，多个参数用&连接；如果数据是英文字母/数字，原样发送，如果是空格，转换为+，如果是中文/其他字符，则直接把字符串用BASE64加密，得出如：%E4%BD%A0%E5%BD，其中%XX中的XX是该符号以16进制表示的ASCII

   不安全，key/value对的序列(查询字符串)附加到URL上的，GET提交数据会遭到Cross-site request forgery攻击

2. POST提交

   把提交的数据放置在HTTP包的包体中，GET提交的数据会在地址栏中显示出来，而POST提交，地址栏不会改变；GET方式需要使用使用Request.QueryString来取得变量的值，而POST方式通过Request.From来获取变量的值

3. 对请求资源或者是URL的限制

   HTTP协议并没有规范对传输的数据的大小的限制，对URL也没有长度限制；限制的是浏览器或者是服务器

4. GET一般用户获取/查询资源信息，而POST一般用于更新资源信息

#### Request请求头内容

请求行：请求类型、URI、HTTP协议版本

请求头：
host：请求web服务器的域名地址
Connection：表示是否持久连接，keep-alive表示持久连接
**Cache-Control**：指定请求和响应的缓存机制。
		no-cache不能缓存
		no-store在请求消息中发送将使得请求和响应消息都不适用缓存
max-age：客户机可以接受生存期不大于指定时间(秒)的响应
max-stale：客户机可以接受超出超时期间的响应消息
min-fresh：客户机可以接受响应时间小于当前时间加上指定时间的响应
only-if-cached等
**User-Agent**：HTTP协议运行的浏览器类型的详细信息
**Accept**：指浏览器可以接受的内容类型
**Accept-Encoding**：客户端浏览器可以支持的web服务器返回内容压缩编码类型
**Accept-Language**：浏览器支持的语言类型
**Cookie**：某些网站为了辨别用户身份、进行Session跟踪而存储在用户本地终端上的数据(通常经过加密)

header里面分为**Cache头域**、**Client头域**、**Cookie/Login头域**、**Entity头域**、**Miscellaneous头域**、**Transport头域**

**Cookie/Login头域**、**Entity头域**、**Miscellaneous头域**、**Transport头域**

1. Cookie
   最重要的header，将cookie的值发送给HTTP服务器
2. Content-Length
   发送给服务器的数据的长度
   Content-Length: 38
3. Content-Type
4. Referer
   提供了Request的上下文信息的服务器，告诉服务器这个从哪个链接来的，比如统计访问量等
5. Connection
   Connection: keep-alive 当一个网页打开完成后，客户端和服务器之间用于传输HTTP数据的TCP连接不会关闭，如果客户端再次访问这个服务器上的页面，会继续使用这一条已经建立的连接
   Connection: close 一个Request完成之后，客户端和服务器之间用于传输HTTP数据的TCP连接会关闭，再次发送Request时，需要重新建立TCP连接
6. Host
   **发送请求时，该报头域是必须的**
   主要用于指定被请求资源的Internet主机和端口号，通常从HTTP URL中提取出来
   例如请求https://www.xx.com/index.html，浏览器发送的请求中，会包含Host请求报头域，Host: https://www.xx.com

**Client头域**

1. Accept
   指定浏览器端可以接受的媒体类型，Accept: text/html 代表浏览器可以接受服务器回发的类型为text/html，也就是html文档；如果服务器无法返回text/html类型的数据，服务器会返回406错误non acceptable
   Accept: \*/\* 表示浏览器可以处理所有类型，一般浏览器都是发送给服务器这个
2. Accept-Encoding
   浏览器声明自己可以接收的编码方式，通常指定压缩方法，是否支持压缩以及什么压缩方法（不只是字符编码）
   Accept-Encoding: gzip, deflate
3. Accept-Language
   浏览器声明自己可以接收的语言
   Accept-Language: en-us
4. User-Agent
   告诉服务器客户端使用的操作系统和浏览器的名称和版本
   User-Agent: Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.1; Trident/4.0; CIBA; .NET CLR 2.0.50727; .NET CLR 3.0.4506.2152; .NET CLR 3.5.30729; .NET4.0C; InfoPath.2; .NET4.0E)
5. Accept-Charset
   浏览器声明自己接收的字符集

**Cache头域**

1. If-Modified-Since
   浏览器端缓存页面的最后修改时间发送到服务器去，服务器会把这个时间与服务器上实际文件的最后修改时间进行比对，如果时间一致，那么返回304，客户端就直接使用本地缓存文件；不一致则会返回200和新的文件内容，客户端接到之后，会丢弃旧文件，把新文件缓存起来，并显示在浏览器中
2. If-None-Match
   和ETag一起工作，在HTTP Response中添加ETag信息，当用户再次请求资源时，将在Request中加入If-None-Match信息，即ETag的值，如果服务器验证资源的ETag没有改变，将返回一个304状态告诉客户端使用本地缓存文件；否则返回200和新的资源和ETag，使用这样的机制将提高网站的性能
3. Pragme
   防止页面被缓存，在HTTP/1.1中，它和Cache-Control: no-cache作用一样
   唯一用法：Pragme: no-cache
4. **Cache-Control**
   **非常重要的规则，**指定Response-Request遵循的缓存机制
   Cache-Control: Public 可以被任何缓存所缓存
   Cache-Control: Private 内容只缓存到私有缓存中
   Cache-Control: no-cache 所有内容都不会被缓存

#### Response响应头内容

header里面分为**Cache头域**、**Cookie/Login头域**、**Entity头域**、**Miscellaneous头域**、**Transport头域**、**Location头域**

**Cache头域**

1. Date
   生成消息的具体时间和日期
   Date: Sat, 11 Feb 2019 14:15:30 GMT
2. Expires
   浏览器会在指定过期时间内使用本地缓存
   Expires: Tue, 08 Feb 2029 14:15:30 GMT
3. Vary: Accept-Encoding

**Cookie/Login头域**

1. **P3P**
   用于跨域设置Cookie，这样可以解决iframe跨域访问cookie的问题
   P3P: CP=CURa ADMa DEVa PSAo....
2. **Set-Cookie**
   非常重要的header，用于把cookie发送到客户端浏览器，每一个写入cookie都会生成一个Set-Cookie
   Set-Cookie: sc=4c31523a; path=/; domain=.acookie.taobao.com
   Set-Cookie: cookie17=W874i9mqGK4%3D;...
   ...

**Entity头域**

1. ETag
   和If-None-Match配合使用
   ETag: "03f2b33c0bfcc1:0"
2. Last-Modified
   用于指示资源的最后修改日期和时间，对应Request的If-Modified-Since
   Last-Modified: Wed, 21 Dec 2019 09:09:10 GMT
3. Content-Type
   Web服务器告诉浏览器自己响应的对象类型和字符集
   Content-Type: text/html; charset=utf-8
   Content-Type: text/html; charset=GB2312
4. Content-Length
   指明实体正文的长度，以字节方式存储的十进制数字来表示
   Content-Length的方式要预先在服务器中缓存所有数据，然后所有数据再一起发给客户端
   Conten-Length: 19847
5. Content-Encoding/Content-Language
   表明自己使用了什么压缩方法gzip，deflate压缩响应中的对象/响应对象的语言
   Content-Encoding: gzip
   Content-Language: da

**Miscellaneous头域**、**Transport头域**、**Location头域**

1. Server
   指明HTTP服务器的软件信息
   Server: Microsoft-IIS/7.5
2. X-AspNet-Version
   如果网站是用ASP.NET开发的，这个header表示ASP.NET的版本
   X-AspNet-Version: 4.0.30319
3. X-Powered-By
   表示网站是用什么技术开发的
   X-Powered-By: ASP.NET
4. Connection
   Connection: keep-alive 没有关闭TCP连接
   Connection: close TCP连接会关闭
5. Location
   用于重定向一个新的位置，包含新的URL地址，304中



#### HTTP代理

一般来说，浏览器输入URL后，请求Request发送给Web服务器，Web服务器接到Request后进行处理，生成相应的Response，然后发送给浏览器，浏览器解析Response中的HTML，我们就看到了网页

其中，我们的Request有可能是经过了代理服务器的，最后才到达Web服务器

代理服务器优势：
***提高访问速度，大多数的代理服务器都有缓存功能***
***突破限制，科学上网***
***隐藏身份***
http代理服务器的匿名性是指：代理服务器通过删除HTTP报文中的身份特性（比如客户端的IP地址、cookie、URI的会话ID）从而对远端服务器隐藏原始用户的IP地址以及其他细节，同时HTTP代理服务器上也不会记录原始用户访问记录的log

HTTP代理有两种：**普通代理**和**隧道代理Tunneling TCP**

**普通代理**

通过代理可以隐藏IP，代理也可以修改HTTP请求头，通过X-Forwarded-Ip这样的自定义头部告诉服务器真正的客户端IP，但服务器无法验证自定义头部真的是由代理添加，还是客户端修改了请求头
给浏览器显式的指定代理，可以手动修改浏览器或操作系统相关设置，或指定PAC文件自动设置，还有浏览器支持WPAD。显式指定浏览器代理这种方式称为**正向代理**

隐藏服务器IP就是**反向代理**，需要通过修改DNS让域名解析到代理服务器IP，反向代理是Web系统最为常见的一种部署方式

**隧道代理**

客户端通过HTTP的CONNECT方法请求隧道代理，创建一条到达任意目的服务器和端口的TCP连接，并对客户端和服务器之间的后继数据进行盲转发

例如通过代理访问A网站，浏览器首先通过CONNECT请求，让代理创建一条到A网站的TCP连接，一旦连接创建好，代理无脑转发后继流量即可。