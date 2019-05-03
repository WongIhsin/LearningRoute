2019.5.3

#### SOCKET

Socket套接字

用来描述IP地址和端口，是通信链的句柄

应用程序可以通过Socket向网络发送请求或者应答网络请求

Socket是支持TCP/IP协议的网络通信的基本操作单元，是对忘了通信过程中端点的抽象表示

Socket包含了进行网络通信所必须的五种信息：链接所使用的协议；本地主机IP；本地主机端口；远程主机IP；远程主机端口。

#### CURL

在Linux种curl是一个利用URL规则在命令行下工作的文件传输工具

***文件传输工具***

1. 基本用法

   `curl https://www.baidu.com`

   可以测试一台服务器是否可以到达一个网站

2. 获取请求页面或接口的请求头信息

   `curl -I https://www.baidu.com`

   ![Screenshot from 2019-05-03 16-43-44](/home/wyx/Desktop/Screenshot from 2019-05-03 16-43-44.png)

   `curl -i https://www.baidu.com`

   -i参数会返回请求头和响应消息

3. 发送带参数的请求，默认为post方式提交

   `curl -d "cb=123&cid=aaa&pid=abc" https://www.xx.com/xx/process.action`

   也可以使用-X GET来指定使用GET方式提交请求

   `curl -d "cb=123&cid=aaa&pid=abc" -X GET https://www.xx.com/xx.action`

   或使用-G参数

   `curl -d "cb=123&cid=aaa&pid=abc" -G https://www.xx.com/xx.action`

   其中-d "@data.txt"也可以读取文件

4. 自定义头信息

   `curl -H "User-Agent:Mozilla/5.0 (Windows NT 6.1; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/69.0.3497.100 Safari/537.36" -H "Referer:http://www.iqiyi.com" http://vip.iqiyi.com`

5. 跟踪链接URL重定向，有些页面或接口被重定向，直接使用curl url会返回

   `<html><head><title>301 Moved Permanently</title>...`

   使用

   `curl -L http://www.A.com`

   访问链接A的时候，就会自动跳转到其重定向的链接B，最终获取到链接B的内容

6. 保存访问的网页

   使用Linux的重定向功能保存

   `curl https://baidu.com >> test.html`

   可以使用内置的 option -o 来保存网页

   `curl -o test.html https://www.baidu.com`

   执行后显示如下，100%表示保存成功

   ![Screenshot from 2019-05-03 15-38-18](/home/wyx/Desktop/Screenshot from 2019-05-03 15-38-18.png)

7. 可以使用curl内置的 option -O 来保存网页中的文件，但是必须具体到某个文件，不然抓不下来

   `curl -O http://www.linux.com/hello.sh`

8. 测试网页返回值

   `curl -o /dev/null -s -w %{http_code} www.baidu.com`

   ![Screenshot from 2019-05-03 15-54-22](/home/wyx/Desktop/Screenshot from 2019-05-03 15-54-22.png)

   在脚本中，这是很常见的测试网站是否正常的用法

9. 指定代理服务器proxy以及其端口

   `curl -x 192.168.100.100:1080 https://www.baidu.com`

   ***不太懂***

   这里需要和SSH -D 以及SOCKS5协议一起看一下

   `ssh -D 1080 -fN 192.168.199.xx`

   `curl --socks5 127.0.0.1:1080 http://www.google.com`

10. cookie

    保存http的response里面的cookie信息，内置option -c

    `curl -c cookiec.txt https://www.baidu.com`

    保存http的response里面的header信息，内置option -D

    `curl -D head.txt https://www.baidu.com`

11. 使用cookie

    很多网站通过监视你的cookie信息来判断你是否按规则访问的，需要使用保存的cookie信息，内置option -b

    `curl -b cookiec.txt https://www.baidu.com`

12. **模仿浏览器**

    有些网站需要特定浏览器去访问，还有些需要使用特定的版本，curl内置option -A可以指定浏览器去访问网站

    `curl -A "Mozilla/4.0 (compatible; MSIE 8.0; Windows NT 5.0)" https://www.baidu.com`

    这样服务器就会认为是使用IE8.0去访问的

13. 伪造referer（盗链）

    很多服务器会检查http访问的referer从而来控制访问，比如，首先访问主页，然后再访问主页中的邮箱界面，这个访问邮箱的referer地址就是访问主页成功后的页面地址，如果服务器发现对邮箱页面的referer地址不是主页的地址，就断定那个是盗链了，curl内置option -e 可以设定referer

    `curl -e "www.baidu.com" https://mail.baidu.com`

    此时服务器会以为你是从www.baidu.com点击某个链接过来的

14. 下载文件

    1. 内置option -o

       `curl -o xx.jpg https://www.baidu.com/xx.jpg`

    2. 内置option -O大写

       `curl -O https://www.baidu.com/xx.jpg`

       会直接以服务器上的名称保存文件到本地

    3. 循环下载

       `curl -O https://www.baidu.com/xx[1-5].jpg`

       会下载xx1，xx2，xx3，xx4，xx5到本地

    4. 下载重命名

       `curl -o #1_#2.JPG http://www.linux.com/{hello,hi}/xx[1-5].JPG `

       会将hello下的xx1.JPG保存为hello_xx1.JPG，以此类推，避免覆盖

    5. 分块下载

       下载的东西太大，可以使用内置option -r

       `curl -r 0-100 -o xx_part1.jpg https://wwww.xx.com/xx.jpg`

       `curl -r 100-200 -o xx_part2.jpg https://wwww.xx.com/xx.jpg`

       `curl -r 200- -o xx_part3.jpg https://wwww.xx.com/xx.jpg`

       `cat xx_part* > xx.jpg`

    6. 通过ftp下载

       curl提供了两种从ftp下载的语法

       `curl -O -u 用户名:密码 ftp://www.xx.com/xx.jpg`

       `curl -O ftp://用户名:密码@www.xx.com/xx.jpg` 

    7. 显示下载进度条

       `curl -# -O http://www.xx.com/xx.jpg`

       不显示下载进度条

       `curl -s -O http://www.xx.com/xx.jpg`

    8. 断点续传

       curl可使用内置option -C达到迅雷一样的断点续传效果

       如果在下载xx.jpg的过程中突然掉线了，可以使用如下方式续传

       `curl -C -O http://www.xx.com/xx.jpg`

15. 上传文件

    使用内置option -T实现上传

    `curl -T xxx.jpg -u 用户名:密码 ftp://www.xx.com/img/`

16. ***显示抓取错误***

    `curl -f http://www.xx.com/error`

    啥?

    没懂

#### SOCKS5

1. 假如A想访问C却因为防火墙而不能直接访问，A可以访问B，B可以访问C，此时可以通过SOCKS5来实现

   假设A的IP为113.113.113.113，B的IP为123.123.123.123，C的IP为133.133.133.133

   需要在A上操作

   `ssh -D 1080 -fN 123.123.123.123`

   `curl --socks5 127.0.0.1:1080 http://133.133.133.133`

   成功访问

   在图形界面则借助火狐浏览器firefox或者谷歌浏览器chrome的SOCKS Host填入127.0.0.1:1080可以实现访问地址C

2. Linux搭建SOCKS5

   1. 首先是安装开发工具包

      `yum install pam-devel openldap-devel openssl-devel`

   2. 安装socks5

      `wget http://...(网上找吧)`

      然后解压、make、make install，常规步骤安装

   3. 修改配置，开启用户验证

      `vim /etc/opt/ss5/ss5.conf`

      将auth的Authentication由-改为u，以及permit的Auth的-改为u

   4. 添加socks5用户

      `vim /etc/opt/ss5/ss5.passwd`

      添加用户、密码

   5. 启动socks5

      默认ss5文件没有执行权限，可以`chmod u+x /etc/rc.d/init.d/ss5 `，也可以在/etc/sysconfig/ss5中将`SS5_OPTS=" -u root"`取消注释 

   6. ***如果不加用户验证的话，是不是就可以直接像上面一样***

      ***`curl --socks5 127.0.0.1:1080 https://www.google.com`***

      ***未实际操作，还不知道结果***

3. VPN搭建使用PPTP，但现在貌似玩一玩还行，实际不行了，好像是干扰严重还是啥

4. VPN和SS/SSR

   科学上网技术使用VPN或者是SS/SSR

   其中VPN并不是为了科学上网，而是为企业内部应用而生，只不过是可以实现科学上网而已，如果有人说VPN是违法的，那他一定是水货，并不了解VPN

   SS即为ShadowSocks，以SOCKS5协议为基础写的，SSR为SS的改进版，纯粹是为了科学上网而生的