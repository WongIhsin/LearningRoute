# SSH详解

零、前言
	SSH服务分为两个openssh-client和openssh-server
	默认情况下，Ubtuntu安装了openssh-client
	yum install openssh-server
	sudo apt-get install openssh-server

一、密码登录
	1、SSH只是协议
	2、主要用于远程登录
		ssh user@host
		当本地用户名和远程用户名一样时，可以省略用户名
		ssh host
		默认端口22，-p参数可以指定端口号
		ssh -p 2222 user@host
	3、安全和公钥
		1、客户端发起登录请求，远程主机将自己的公钥发给用户
		2、客户端使用该公钥将登录密码加密后发送给远程主机
		3、远程主机使用私钥解密登录密码，如密码正确则允许客户端登录
	4、MIMT Man in the middle attack 中间人攻击
		如果有人插在用户与远程主机之间，截获登录请求，然后冒充远程主机，将伪造的公钥发给客户端，那么客户端很难辨别真伪
	5、第一次连接主机
		会提示无法确认主机真实性，指知道他的公钥指纹，是否继续连接，其实并没有什么有效便捷的方式确认公钥指纹的真实性
			公钥指纹fingerprint：采用RSA算法
		确认接收远程主机公钥后，系统会提示远程主机已加入受信主机列表
	6、远程主机的公钥被接受以后，他就会被保存在文件/.ssh/know_hosts之中
		下次再连接这台主机，系统会发现他的公钥已经保存在本地了，从而跳过警告部分，直接提示输入密码
二、公钥登录
	1、原理：用户将自己的公钥存储在远程主机上，登录的时候，远程主机会向用户发送一端随机字符串，用户用自己的私钥加密后，再发回来，远程主机用事先存储的公钥进行解密，如果成功，证明用户是可信的，直接允许登录shell，不在要求输入密码
	2、公钥登录需要用户提供自己的公钥，一般保存在/.ssh/目录下，id_rsa是私钥，id_rsa.pub是公钥
	3、通过ssh-keygen生成公钥和私钥
		ssh-keygen -t rsa 
			-t 指定算法rsa
		需要把公钥发送到远程主机
		ssh-copy-id [-i [identity_file]] [user@]machine
		ssh-copy-id root@10.0.0.12
	4、此后再登录就不需要输入密码了
		如果不行，需要远程主机SSH配置/etc/ssh/sshd_config打开并注释掉并重启ssh服务
			#RSAAuthenticationyes
			#PubkeyAuthenticationyes
			#AuthorizedKeysFile.ssh/authorized_keys
		authorized_keys文件
			远程主机将用户的公钥，保存在$HOME/.ssh/authorized_keys中，公钥是一段字符串，也可以手动追加到远程主机authorized_keys文件中，每行一个
	5、可以通过如下命令替代ssh-copy-id
		ssh user@host 'mkdir -p .ssh && cat >> .ssh/authorized_keys' < ~/.ssh/id_rsa.pub
三、远程操作
	ssh可以用于直接在远程主机上操作
四、端口转发
	端口转发才是重点
	下面重新写一点
	本地转发
		把本地主机端口通过待登录主机端口转发到远程主机端口上去
		ssh -L 5000:www.google.com:80 user@host
		ssh -L localport:remotehost:remotehostport sshserver
			-f：后台启用
			-N：不打开远程shell，处于等待状态，不加-N则直接登录进去
			-g：启用网关功能
			-C：压缩数据传输
		访问本地5000端口就相当于访问远程主机www.google.com的80端口，并且这是通过登录主机来安全地转发数据的，当不能直接访问远程主机某端口，而登录主机可以访问时，可使用这种方式将远程主机端口绑定到本地
		-L X:Y:Z：将IP为Y的机器的Z端口通过中间服务器映射到本地机器的X端口
	远程转发
		把登录主机端口通过本地主机端口转发到远程主机
		ssh -R 8080:localhost:80 user@host
		ssh -R sshserverport:remotehost:remotehostport sshserver
		访问登录主机的8080端口就相当于访问localhost:80
			例如：我在本机起了一个web服务，希望别人从外网访问或测试，但是外网是不能直接访问我的内网机器，所以我可以在本机上执行上面的命令，这样就可以通过访问登录主机的8080端口，来访问本机的80端口了，从而实现外网访问内网的应用
		-R X:Y:Z：就是把我们内部的Y机器的Z端口映射到远程机器的X端口上
	动态转发
		不需要制定特定的目标主机和端口号，可以实现不加密的网络连接，全部走SSH连接，从而提高安全性
		ssh -D 5000 user@host
		·不理解·

五、scp命令可以利用SSH安全连接与远程主机互相复制文件
	scp [-r] /root/1.txt user@192.168.1.1:/tmp
	scp [-r] user@192.168.1.1:/etc/passwd /root/passwd.txt
		-r：表示目录递归
		-p：表示文件属性不发生变化
		-P：指定端口号
六、sftp：
	可以利用SSH安全连接与远程主机上传、下载文件，采用了与FTP类似的登录过程和交互式环境，便于目录资源管理，如下
		sftp user@192.168.1.1

····································
····································
····                           ·····
····         端口转发           ·····
····                           ·····
····································
····································
端口转发才是重点好吗
一、优势
	1、防火墙限制了一些网络端口的使用的话，只要还允许SSH连接，就能够通过TCP用端口转发来使用SSH进行通讯
	2、SSH加密
	3、通过具备访问权限的第三者，突破防火墙对自己的限制，或者隐身
二、注意点
	1、自动重连
		某些原因可能会断开端口转发，机器重启或长时间没有数据通信而被路由器切断等待
		ssh -o TCPKeepAlive=yes默认开启
	2、SSH端口转发是通过SSH连接建立起来的，必须保持这个SSH连接以使端口转发保持生效，一旦关闭了这个连接， 端口转发也会随之关闭
	3、只能建立SSH连接的同时创建端口转发，而不能给一个已经存在的SSH连接增加端口转发
	4、主流SSH实现中，本地端口转发绑定的是lookback接口，只有localhost或者127.0.0.1才能使用本机的端口转发，其他机器发起的连接只会得到connection refused
		SSH同时提供了GatewayPorts关键字，可以通过指定它与其他机器共享这个本地端口转发
		ssh -g -L localport:remotehost:remoteport sshhostname
	5、Autossh 和 开机启动 和 定时重连
		autossh -M 10000 [之后和ssh参数一样]
		/etc/rc.d/rc.local中添加如上
		crontab -e
		