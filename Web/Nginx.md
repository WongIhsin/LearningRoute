# Nginx

---

https://zhuanlan.zhihu.com/p/33694543?utm_source=wechat_session&utm_medium=social&utm_oi=1053345363565645824

---

#### 为什么使用Nginx

目前Nginx的主力竞争对手是Apache

1. #### 作为Web服务器

   相比Apache，Nginx使用更少的资源，支持更多的并发连接，体现更高的效率，在高连接并发的情况下，Nginx是Apache服务器不错的替代品，能够支持高达50000个并发连接数的响应。Nginx选择的是epoll and kqueue作为开发模型。

   Nginx作为负载均衡服务器，既可以在内部直接支持Rails和PHP程序对外进行服务，也可以支持作为HTTP代理服务器对外进行服务。C语言编写，系统资源开销和CPU使用效率要好很多。

2. #### Nginx配置简洁，Apache复杂

   Nginx启动特别容易，并且几乎可以做到不间断运行，即使运行数个月也不需要重新启动。还能够不间断服务的情况下进行软件版本的升级

   Nginx静态处理性能比Apache高3倍以上，Apache对PHP支持比较简单，Nginx需要配合其他后端来使用，Apache的组件比Nginx多。

3. #### 最核心区别在于

   apache是同步多进程模型，一个连接对应一个进程，Nginx是异步的，多个连接（万级别）可以对应一个进程

4. #### 两者擅长的领域分别为

   Nginx的优势是处理静态请求，cpu内存使用率低，apache适合处理动态请求，所有现在一般前端用Nginx作为反向代理抗住压力，apache作为后端处理动态请求

---

#### 常用命令

```bash
# 快速关闭Nginx，可能不保存相关信息，并迅速终止web服务
nginx -s stop
# 平稳关闭Nginx，保存相关信息，有安排的结束web服务
nginx -s quit
# 因改变了Nginx相关配置，需要重新加载配置而重载
nginx -s reload
# 重新打开日志文件
nginx -s reopen
# 为Nginx指定一个配置文件，来代替缺省的
nginx -c filename
# 不允许，仅仅测试配置文件。将检查配置文件的语法正确性，并尝试打开配置文件中所引用到的文件
nginx -t
# 显示nginx的版本
nginx -v
# 显示nginx的版本，编译器版本和配置参数
nginx -V
```

#### Nginx实现http反向代理配置

`conf/nginx.conf`是nginx的默认配置文件，也可使用`nginx -c`指定配置文件

```bash
# 运行用户
# user somebody;

# 启动进程，通常设置成和CPU数量相等
worker_processes 1;

# 全局错误日志
error_log logs/error.log;
error_log logs/error.log notice;
error_log logs/info.log info;

# PID文件，记录当前启动的nginx的进程ID
pid logs/nginx.pid;

# 工作模式及连接数上限
events {
    worker_connections 1024;  # 单个后台worker process进程的最大并发连接数
}

# 设定http服务器，利用它的反向代理功能提供负载均衡支持
http {
    # 设定mime类型(邮件支持类型)，类型有mime.types文件定义
    include 		mime.types;
    default_type	application/octet-stream;
    
    # 设定日志
    log_format main '$remote_addr - $ remote_user [$time_local] "$request" '
    				'$status $body_bytes_sent "$http_referer" '
    				'"$http_user_agent" "$http_x_forwarded_for"';
    access_log logs/access.log main;
    rewrite_log on;
    # sendfile 指令指定nginx是否调用sendfile函数(zero copy方式)来输出文件，对于普通应用，必须设置位on，如果用来进行下载等应用磁盘IO重负载应用，可设置为off，以平衡磁盘于网络IO处理速度，降低系统的uptime.
    sendfile on;
    # tcp_nopush on;
    # 连接超时时间
    keepalive_time 120;
    tcp_nodelay on;
    # gzip压缩开关
    # gzip on;
    
    # 设定实际的服务器列表
    upstream zp_server1{
        server 127.0.0.1:8089;
    }
    
    # HTTP服务器
    server {
        # 监听80端口
        listen 80;
        # 定义使用www.xx.com访问
        server_name www.helloworld.com;
        # 首页
        index index.html
        # 指向webapp的目录
        root D:........;
        # 编码格式
        charset itf-8;
        # 代理配置参数
        proxy_connect_timeout 180;
        proxy_send_timeout 180;
        proxy_read_timeout 180;
        proxy_set_header Host $host;
        proxy_set_header X-Forwarder-For $remote_addr;
        
        # 反向代理的路径(和upstream绑定)，location后面设置映射的路径
        location / {
        	proxy_pass http://zp_server1;
        }
        # 静态文件，nginx自己处理
        location ~^/(image|javascript|js|css|flash|media|static)/ {
        	root D:.....;
        	expires 30d;
        }
        # 设定查看Nginx状态的地址
        location /NginxStatus {
        	stub_status		on;
        	access_log 		on;
        	auth_basic 		"NginxStatus";
        	auth_basic_user_file  conf/htpasswd;
        }
        # 禁止访问.htxxx文件
        location ~ /.ht {
        	deny all;
        }
        # 错误处理页面(可选择性配置)
        # error_page 404 	/404.html;
        # error_page 500 502 503 504 /50x.html;
        # location = /50x.html {
        #     root html;
        # }
    }
}
```

#### 负载均衡配置

```bash
http {
    # 设定mime类型，类型由mime.type文件定义
    include 		mime.types;
    default_type	application/octet-stream;
    # 设定日志格式
    access_log 		/var/log/nginx/access.log
    # 设定负载均衡的服务器列表
    upstream load_balance_server {
        # weight参数表示权值，全值越高被分配到的几率越大
        server 192.168.199.100:80 weight=5;
        server 192.168.199.101:80 weight=1;
        server 192.168.199.102:80 weight=6;
    }
    # HTTP服务器
    server {
    	listen 80;
    	server_name www.helloworld.com;
    	location / {
    		root 		/root;  # 定义服务器的默认网站根目录位置
    		index 		index.html index.htm;  # 定义首页索引文件的名称
    		proxy_pass 	http://load_balance_server;  # 请求转向load_balance_server定义的服务器列表
    		# 以下是一些反向代理的配置(可选择性配置)
    		# proxy_redirect off;
    		proxy_set_header Host $host;
    		...
    		...
    	}
    }
}
```

#### HTTPS反向代理配置

HTTPS固定端口号为443，HTTP为80；SSL标准需要引入安全证书

```bash
server {
	listen  443 ssl;
	server_name www.helloworld.com;
	# ssl证书文件位置(常见证书文件格式为：crt/pem)
	ssl_certificate   	 cert.pem;
	# ssl证书key位置
	ssl_certificate_key  cert.key;
	# ssl配置参数(选择性配置)
	ssl_session_cache 	 shared:SSL:1m;
	ssl_session_timeout  5m;
	# 数字签名，此处使用MD5
	ssl_ciphers  HIGH:!aNULL:!MD5;
	ssl_prefer_server_ciphers on;
	location / {
		root /root;
		index index.html index.htm;
	}
}
```

#### 静态站点配置

```bash
server {
	listen 80;
	server_name static.zp.cn;
	
	location / {
		root /app/dist;
		index index.html;
	}
}
```

#### 跨域解决方案

在web领域开发中，经常采用前后端分离模式，这种模式下，前端和后端分别是独立的web应用程序，例如：后端是Java程序，前端是React或Vue应用。

解决跨域问题一般有两种思路：

+ CORS
  在后端服务器设置HTTP响应头，把需要运行访问的域名加入Access-Control-Allow-Origin中
+ jsonp
  把后端根据请求，构造json数据，并返回，前端用jsonp跨域

#### Nginx实现了CORS

举例来说，前后端两个app，前端端口为9000，后端端口为8080，前后端请求会被拒绝，因为跨域问题

首先在`enable-cors.conf`文件中设置cors：

```bash
# allow origin list
set $ACAO '*';

# set single origin
if ($http_origin ~* (www.helloworld.com)$) {
	set $ACAO $http_origin;
}
if ($cors = "trueget") {
	add_header 'Access-Control-Allow-Origin' "$http_origin";
	add_header 'Access_Control-Allow-Credentials' 'true';
	add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS';
    add_header 'Access-Control-Allow-Headers' 'DNT,X-Mx-ReqToken,Keep-Alive,User-Agent,X-Requested-With,If-Modified-Since,Cache-Control,Content-Type';
}

if ($request_method = 'OPTIONS') {
	set $cors "${cors}options";
}

if ($request_method = 'GET') {
 	set $cors "${cors}get";
}

if ($request_method = 'POST') {
 	set $cors "${cors}post";
}
```

接下来，在conf中引入跨域配置

```bash
server {
	location ~ ^/api/ {
		include enable-cors.conf;
		proxy_pass http://api_server;
		rewrite "^/api/(.*)$" /$1 break;
    }
}
```

# 不懂