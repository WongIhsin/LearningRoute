# Docker

2013年发布

#### Docker原理不是很清楚

以后有时间再看

---

#### 环境配置的难题

软件开发最大的麻烦事之一，就是**环境配置**
用户必须保证两件事情：操作系统的设置，各种库和组件的安装。只有它们都正确，软件才能运行。如要安装一个Python应用，计算机必须要有Python引擎，还必须要有各种依赖，可能还要配置环境变量

解决方法：软件带环境安装

#### 虚拟机

虚拟机**virtual machine**就是带环境安装的一种解决方案。虽然用户可以通过虚拟机还原软件的原始环境，但存在如下缺点：

+ 占用资源多
  虚拟机会独占一部分内存和硬盘空间，运行的时候，其他程序就不能使用这些资源了，哪怕虚拟机里面的应用程序真正使用的内存只有1MB，虚拟机依然需要几百兆的内存才能运行
+ 冗余步骤多
  虚拟机是一个完整的操作系统，一些系统级别的操作步骤，往往无法跳过，比如用户登录
+ 启动慢
  启动操作系统需要多久，启动虚拟机就需要多久，可能要等几分钟，应用程序才能真正运行

---

#### Linux容器

Linux发展出的一种虚拟化技术：Linux容器（**Linux Containers**，LXC）
Linux容器不是模拟一个完整的操作系统，而是对进程进行隔离。即是在正常进程外面套了一个保护层，对于容器里面的进程来说，它接触到的各种资源都是虚拟的，从而实现与底层系统的隔离

+ 启动快
  启动容器相当于启动本机的一个进程，而不是启动一个操作系统，速度就快很多
+ 资源占用少
  容器只占用需要的资源，不占用那些没有用到的资源。虚拟机由于是完整的操作系统，不可避免要占用所有资源
+ 体积小
  容器只要包含用到的组件即可，容器文件比虚拟机文件要小很多

---

#### Docker是什么

Docker属于Linux容器的一种封装，提供简单易用的容器使用接口，是目前最流行的Linux容器解决方案

Docker将应用程序与该程序的依赖，打包在一个文件里面，运行这个文件，就会产生一个虚拟容器，程序在这个虚拟容器中运行，就好像在真实的物理机上运行一样

---

#### Docker的用途

+ **提供一次性的环境**：本地测试他人的软件、持续集成的时候提供单元测试和构建的环境
+ **提供弹性的云服务**：Docker容器可以随开随关，很适合动态扩容和缩容
+ **组建微服务架构**：通过多个容器，一台机器可以跑很多个服务，因此在本机就可以模拟出微服务架构

---

#### Docker和虚拟机

虚拟机更擅长于彻底隔离整个运行环境，例如云服务提供商

Docker通常用于隔离不同的应用

VM会对硬件资源进行虚拟化，docker则直接使用硬件资源

---

#### Docker安装

安装依赖 ---> 添加远程仓库（软件源） ---> 安装docker-ce ---> 启动服务systemctl start docker/service docker start

---

#### image文件

Docker把应用程序及其依赖，打包在image文件里面，通过这个文件才能生产Docker容器
一个image文件，可以生产多个同时运作的容器实例

image文件是一个二进制文件，一个image往往通过继承另一个image文件，加上一些个性化的设置而生成

```bash
# 列出本机的所有image文件
$ docker image ls
# 删除image文件
$ docker image rm [imageName]
```

一般是使用别人制作好的image文件。Docker的官方仓库Docker Hubhttps://hub.docker.com/，是最重要、最常用的image仓库

---

#### 实例

```bash
# 将image文件从仓库抓取到本地，非必须，run可以从远程仓库自动抓取
$ docker image pull library/hello-world
# 官方提供的image文件，都放在library组里面，是默认组，可以省略
$ docker image pull hello-world

# 运行一个image文件, docker run hello-world 也可以
$ docker container run hello-world
# docker container run会自动抓取image文件，如果发现本地没有指定的image文件，就会从仓库自动抓取

# 对于不会自己结束的容器，他提供的是服务，需要手动关闭`docker container kill`
$ docker container run -it ubtuntu bash
$ docker container kill [containID]  # containID 可以之传入一部分ID值，会自动寻找
```

#### image

可以直接的`docker run [image_name]`自动从远程仓库下载image文件，并生成一个container实例
关闭实例可以用`docker container kill [containID]`

----

#### 容器文件

image文件生成的容器实例，本身也是一个文件，称为**容器文件**。一旦容器生成，就会同时存在两个文件：**image文件**和**容器文件**

关闭容器并不会删除容器文件，只是容器停止运行而已

```bash
# 列出本机正在运行的容器
$ docker container ls
# 列出本机所有容器，包括终止运行的容器
$ docker container ls -all
# 停止一个运行的容器
$ docker container kill [containerID]
$ docker container stop [containerID]
# 删除一个容器文件
$ docker container rm [containerID]
```

---

#### Dockerfile文件与实例

如何生成image文件？Dockerfile是一个文本文件，用来配置image，Docker根据该文件生成二进制的image文件

```bash
# 以某个github项目为例
$ git clone https://github.com/ruanyf/koa-demos.git
$ cd koa-demos
```

1. 首先先新建一个文本文件`.dockerignore`并写入如下内容：

   ```bash
   .git
   node_modules
   npm-debug.log
   ```

   表示这三个路径要排除，不要打包进image文件

2. 在项目的根目录下，新建一个文本文件`Dockerfile`并写入如下内容：

   ```shell
   FROM node:8.4  # 该文件继承官方的node image，冒号表示标签，即8.4版本的node
   COPY . /app  # 将当前目录下的所有文件，都拷贝进入image文件的/app目录
   WORKDIR /app  # 指定接下来的工作路径为/app
   RUN npm install --registry=https://registry.npm.taobao.org  # 在/app目录下，运行npm install命令安装依赖
   EXPOSE 3000  # 将容器的3000端口暴露出来，允许外界连接这个端口
   ```

3. 创建image文件

   ```bash
   $ docker image build -t koa-demo .  # -t 指定image文件的名字，后面还可以接冒号指定标签。不指定标签，默认为latest，最后的点表示Dockerfile文件所在的路径
   $ docker image build -t koa-demo:0.0.1 .
   # 运行后生成image文件
   ```

4. 生成容器

   ```bash
   $ docker container run -p 8000:3000 -it koa-demo /bin/bash
   $ docker container run -p 8000:3000 -it koa-demo:0.0.1 /bin/bash
   ```

   + -p：容器的3000端口映射到本机的8000端口
   + -it：容器的Shell映射到当前的Shell，然后你在本机窗口输入的命令，会传入容器
   + /bin/bash：容器启动以后，内部第一个执行的命令，这里是启动Bash，保证用户可以使用Shell

5. 可以容器结束之后自动删除容器文件

   ```bash
   $ docker container run --rm -p 8000:3000 -it koa-demo /bin/bash
   ```

---

#### 发布image文件

去`hub.docker.com`或`cloud.docker.com`注册一个账户

```bash
$ docker login
$ docker image tag [iamgeName] [username]/[repository]:[tag]
$ docker image push [username]/[repository]:[tag]
```

---

#### 其他指令

+ docker container start

  ```bash
  $ docker container start [containerID]
  ```

+ docker container stop

  ```bash
  $ docker container stop [containerID]
  ```

  kill 相当于向容器里面的主进程发送SIGKILL信号，强制结束进程，会丢失正在进行的操作；
  stop 相当于向容器里面的主进程发送SIGTERM信号，过一段时间再发出SIGKILL信号，自动进行收尾清理工作

+ docker container logs

  ```bash
  $ docker container logs [containerID]
  ```

  查看容器里面Shell的标准输出。如果docker run命令运行容器的时候，没有使用-it参数，就需要使用这个命令查看输出

+ docker container exec

  ```bash
  $ docker container exec -it [containerID] /bin/bash
  ```

  进入一个正在运行的docker容器，如果docker run命令运行容器的时候，没有使用-it参数，就要使用这个命令进入容器，一旦进入容器，就可以在容器的Shell执行命令了

+ docker container cp（不懂）

  ```bash
  $ docker container cp [containerID]:[/path/to/file] .
  ```

  用于从正在运行的Docker容器里面，将文件拷贝到本机

---

在container里面exit相当于`docker container stop [containerID]`

---

#### 一些个人理解

+ docker是一个包装进程的容器工具

+ docker安装需要先安装依赖工具，设置远程仓库，安装docker-ce，然后还需要启动服务

  ```bash
  $ docker image pull [repository]/[image_file_name]
  $ docker image ls  # 列出所有的image文件
  $ docker image rm [image_file_name]
  
  $ docker container ls  # 列出所有的container文件
  $ docker container la -all  # 列出所有的停止的container文件
  $ docker container rm [containerID]  # 删除指定的container文件
  # container退出并不会删除container文件，除非在run的时候加上 --rm，会在退出container的时候删除container文件
  
  $ docker container run [image_file_name]  # run包含了pull的抓取image文件的功能
  # 本地仓库没有，会自动从远程仓库抓取
  $ docker container stop [containerID]  # stop会发送SIGTERM信号，自动收尾后结束
  $ docker container kill [containerID]  # kill会发送SIGKILL信号强制结束
  
  -p  # 暴露容器的某个端口，使得外界可以访问到容器里面 -p 8000:3000
  # 具体的对应关系还么有实战，不知道如何使用
  -it  # -it 将容器的shell映射到当前的shell，即在本机窗口输入命令会传入到容器
  -v  # 海哥的脚本里面的应该是映射路径，可以映射多个路径
  在[image_file_name]后可以加一个/bin/bash  # 即启动容器后内部执行的第一个命令，启动bash，保证使用者可以使用shell
  
  # 开启容器后，进入的环境由Dockerfile文件指定
  # FROM node:8.4  # 继承关系
  # COPY . /app  # 将本地的.路径下的东西全都打包进/app，可以指定指定路径的文件，从而实现配置好环境变量的功能(自己的理解)
  # WORKDIR /app  # 指定接下来的工作路径为/app
  # RUN ...  # 可以设置一些环境变量之类的东西，处理后的东西都将打包进入image文件(猜测在此处来设置一些环境变量)
  # EXPOSE 3000  # 将容器的3000端口暴露出来，允许外部连接这个端口
  # 生成image文件
  $ docker image build -t [[username]/[repository]:]koa-demo:0.0.1
  ```

+ 猜测：container开启后，会在-v映射的实际路径里面使用真实路径，其他路径下的修改会写在container文件里面

+ 猜测：container里面提供的一个虚拟的环境，是怎么达成的？可能是核心还是不变的，只是提供了一些其他的依赖