# MIME类型

推荐阅读：https://developer.mozilla.org/zh-CN/docs/Web/HTTP/Basics_of_HTTP/MIME_Types

---

媒体类型，Multipurpose Internet Mail Extensions是一种标准，用来表示文档、文件或字节流的性质和格式

浏览器通常使用MIME类型（而不是文件扩展名）来确定如何处理URL，因此Web服务器在响应头中添加正确的MIME类型非常重要。如果配置不正确，浏览器可能会曲解文件内容，网站将无法正常工作，并且下载的文件也会被错误处理

#### 通用结构

## type/subtype

对于text文件类型若没有特定的subtype，就使用**text/plain**

二进制文件没有特定或已知的subtype，即使用**application/octet-stream**

**application/octet-stream**是应用程序文件的默认值，意思是未知的应用程序文件，浏览器一般不会自动地执行或询问执行

**text/plain**是文本文件默认值，意味着未知的文本文件，但浏览器认为是可以直接展示的