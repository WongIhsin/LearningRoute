# Docker Usage

---

官方网站：https://docs.docker.com/get-started/

---

#### 一些疑问

在Docker的容器里修改/etc/等文件，修改后的东西保存在哪里，是不是也类似的提供了一个完整的体系

---

# PART 1: Orientation

#### Images and Containers

An **image** is an executable package that includes everything needed to run an application--the code, a runtime, libraries, environment variables, and configuration files.

A **container** is a runtime instance of an image--what the image becomes in memory when executed (that is, an image with state, or a user process).

You can see a list of your running containers with the command `docker ps`

#### Test Docker version

```bash
[root@MyCloud ~]# docker --version
Docker version 19.03.0-beta3, build cc55e026
# more details
[root@MyCloud ~]# docker info
[root@MyCloud ~]# docker version
```

#### Test Docker installation

```bash
[root@MyCloud ~]# docker run hello-world
[root@MyCloud ~]# docker image ls
[root@MyCloud ~]# docker container ls --all
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS                     PORTS               NAMES
c2fcf0618d72        hello-world         "/hello"            2 seconds ago       Exited (0) 1 second ago                        cool_ride
```

#### Recap and cheat sheet

```bash
## list Docker CLI commands
docker
docker container --help
## Display Docker version and info
docker --version
docker version
docker info
## Execute Docker image
docker run hello-world
## List Docker images
docker image ls
## List Docker containers (running, all, all in quiet mode)
docker container ls
docker container ls -all
docker container ls -aq
```

---

# PART 2: Containers

With Docker, you can just grab a portable Python runtime as an image, no installation necessary. Then, your build can include the base Python image right alongside your app code, ensuring that your app, its dependencies, and the runtime, all travel together.

Portable images are defined by something called a `Dockerfile`.

#### Define a container with `Dockerfile`

`Dockerfile` defines what goes on in the environment inside your container. Access to resources like **networking interfaces** and **disk drivers** is virtualized inside this environment, which is isolated from the rest of your system, so you need to map ports to the outside world, and be specific about what files you want to "copy in" to that environment. However, after doing that, you can expect that the build of your app defined in this `Dockerfile` behaves exactly the same wherever it runs.

##### `Dockerfile`

Create an empty directory on your local machine. Change directories ( `cd` ) into the new directory, create a file called `Dockerfile`, copy-and-paste the following content into that file, and save it.

```bash
# Use an official Python runtime as a parent image
FROM python:2.7-slim
# Set the working directory to /app
WORKDIR /app
# Copy the current directory contents into the container at /app
COPY . /app
# Install any needed packages specified in requirements.txt
RUN pip install --trusted-host pypi.python.org -r requirements.txt
# Make port 80 available to the world outside this container
EXPOSE 80
# Define environment variable
ENV NAME World
# Run app.py when the container launches
CMD ["python", "app.py"]
```

Create two more files,  `app.py` and `requirements.txt`, and put them in the same folder with the `Dockerfile`.

**`requirements.txt`**

```
Flask
Redis
```

**`app.py`**

```python
from flask import Flask
from redis import Redis, RedisError
import os
import socket

# Connect to Redis
redis = Redis(host="redis", db=0, socket_connect_timeout=2, socket_timeout=2)

app = Flask(__name__)

@app.route("/")
def hello():
    try:
        visits = redis.incr("counter")
    except RedisError:
        visits = "<i>cannot connect to Redis, counter disabled</i>"
    
    html = "<h3>Hello {name}!</h3>" \
    	   "<b>Hostname:</b> {hostname}<br/>" \
           "<b>Visits:</b> {visits}"
    return html.format(name=os.getenv("NAME", "world"), hostname=socket.gethostname(), visits=visits)

if __name__ == "__main__":
    app.run(host='0.0.0.0', port=80)
```

**Note**: Accessing the name of the host when inside a container retrieves the container ID, which is like the process ID for a running executable.

(默认容器的hostname为container ID，也可以在开启的时候-h参数来指定)

#### Build the app

Now run the build command. This creates a Docker image, which we're going to name using the `--tag` option. Use `-t` if you want to use the shorter option.

```bash
docker build --tag=friendlyhello .
```

Image is in your machine's local Docker image registry.

Tag defaulted to `latest`. The full syntax for the tag option would be something like `--tag=friendlyhello:v0.0.1`.

#### Run the app

Run the app, mapping your machine's port 4000 to the container's published port 80 using `-p`:

```bash
$ docker run -p 4000:80 friendlyhello

# You can use the curl command in a shell to view the content.
$ curl http://localhost:4000
<h3>Hello World!</h3><b>Hostname:</b> 71bc8a7cd656<br/><b>Visits:</b> <i>cannot connect to Redis, counter disabled</i>
```

Now let's run the app in the background, in detached mode:

```bash
docker run -d -p 4000:80 friendlyhello
```

Now your container is running in the background.

Now use docker container stop to end the process, using the `CONTAINER ID`, like so:

```bash
docker container stop 29ff25686d41
```

#### Share your image

A registry is a collection of repositories, and a repository  is a collection of images--sort of like a GitHub repository, except the code is already built. 

注册官网：https://hub.docker.com/

#### Log in with your Docker ID

Log in to the Dokcer public registry on your local machine.

```bash
$ docker login
```

#### Tag the image

The notation for associating a local image with a repository on a registry is `username/repository:tag`. 

Now, run `docker tag image` with your username, repository, and tag names so that the image uploads to your desired destination.

```bash
# docker tag image username/repository:tag
docker tag friendlyhello gordon/get-started:part2
```

#### Publish the image

Upload your tagged image to the repository.

```bash
docker push username/repository:tag
```

#### Pull and run the image from the remote repository

From now on, you can use `docker run` and run your app on any machine with this command:

```bash
docker run -p 4000:80 ihsin/get-started:part2
```

#### Recap and cheat sheet

```bash
# Create image using this directory's Dockerfile
docker built -t friendlyhello .
# Run 'friendlyhello' mapping port 4000 to 80
docker run -p 4000:80 friendlyhello
# Same thing, but in detached mode
docker run -d -p 4000:80 friendlyhello
# List all running containers
docker container ls
# List all containers, even thost not running
docker container ls -a
# Gracefully stop the specified container
docker container stop <hash>
# Force shutdown of the specified container
docker container kill <hash>
# Remove specified container from this machine
docker container rm <hash>
# Remove all containers
docker container rm $(docker container ls -a -q)
# List all image on this machine
docker image ls -a
# Remove specified image from this machine
docker image rm <image id>
# Remove all image from this machine
docker image rm $(docker image ls -a -q)
# Log in this CLI session using your Docker credentials
docker login
# Tag <image> for upload to registry
docker tag <image> username/repository:tag
# Upload tagged image to registry
docker push username/repository:tag
# Run image from a registry
docker run username/repository:tag
```

---

# PART 3: Services

#### About services

In a distributed application, different pieces of the app are called "**services**". For example, if you imagine a video sharing site, it probably includes a service for storing application data in a database, a service for video transcoding in the background after a user uploads something, a service for the front-end, and so on.

Services are really just "containers in production." A service only runs one image, but it codifies the way that image runs--what ports it should use, how many replicas of the container should run so the service has the capacity it needs, and so on. Scaling a service changes the number of container instances running that piece of software, assigning more computing resources to the service in the process.

Luckily it's very easy to define, run, and scale services with the Docker platform -- just write a `docker-compose.yml` file.

#### Your first `docker-compose.yml` file

A YAML file that defines how Dokcer containers should behave in production.

##### `docker-compose.yml`

Save this file as `docker-compose.yml` wherever you want.

```yaml
version: "3"
services:
  web:
    # replace username/repo:tag with your name and image details
    image: username/repo:tag
    deploy:
      replicas: 5
      resources:
        limits:
          cpus: "0.1"
          memory: 50M
        restart_policy:
          condition: on-failure
      ports:
        - "4000:80"
      networks:
        - webnet
networks:
  webnet:
```

This file tells Docker to do the following:

+ Pull the image we uploaded in step 2 from the registry.
+ Run 5 instances of that image as a service called `web`, limiting each one to use, at most, 10% of a single core of CPU time (this could also be e.g. "1.5" to mean 1 and half core for each), and 50MB of RAM.
+ Immediately restart containers if one fails.
+ Map port 4000 on the host to `web`'s port 80.
+ Instruct `web`'s containers to share port 80 via a load-balanced network called `webnet`. (Internally, the containers themselves publish to `web`'s port 80 at an ephemeal port.)
+ Define the `webnet` network with the default settings (which is a load-balanced overlay network).

#### Run your new load-balanced  app

Before we can use the `docker stack deploy` command we first run:

```bash
docker swarm init
```

Now let's run it. You need to give your app a name. Here, it is set to `getstartedlab`:

```bash
docker stack deploy -c docker-compose.yml getstartedlab
# 这里没有跑通
# bash输出信息为：webnet Additional property webnet is not allowed
# 经检查是缩进问题。。。。。。
# 正确输出
# Creating network getstartedlab_webnet
# Creating service getstartedlab_web
```

One single service stack is running 5 container instances of our deployed image on one host.

Get the service ID for the one service in our application:

```bash
docker service ls
```

Alternatively, you can run `docker stack services`, followed by the name of your stack.

```bash
docker stack services getstartedlab
ID                  NAME                MODE                REPLICAS            IMAGE                     PORTS
psqozgtlv82a        getstartedlab_web   replicated          5/5                 ihsin/get-started:part2   *:4000->80/tcp
```

A single container running in a service is called a **task**. Tasks are given unique IDs that numerically increment, up to the number of `replicas` you defined in `docker-compose.yml`.

List the tasks for your service:

```bash
docker service ps getstartedlab_web
```

Tasks also show up if you just list all the containers on your system, though that is not filtered by service:

```bash
docker container ls -q
```

You can run `curl -4 http://localhost:4000` several times in a row. (-4: IPv4)

The container ID changes, demonstrating the load-balancing; with each request, one of the 5 tasks is chosen, in a round-robin fashion, to respond.

You can run `docker stack os` followed by your app name, as shown in the following example:

```bash
docker stack ps getstartedlab
```

#### Scale the app

You can scale the app by changing the `replicas` value in `docker-compose.yml`, saving the change, and re-running the `docker stack deploy` command:

```bash
docker stack deploy -c docker-compose.yml getstartedlab
```

Docker performs an in-place update, no need to tear the stack down first or kill any containers.

#### Take down the app and the swarm

+ Take the app down with `docker stack rm`:

  ```bash
  docker stack rm getstartedlab
  ```

+ Take down the swarm.

  ```bash
  docker swarm leave --force
  ```

#### Recap and cheat sheet

The true implementation of a container in production is running it as a service. Services codify a container's behavior in a Compose file, and this file can be used to scale, limit, and redeploy our app. Changes to the service can be applied in place, as it runs, using the same command that launched the service: `docker stack deploy`.

```bash
# List stacks or apps
docker stack ls
# Run the specified Compose file
docker stack deploy -c <composefile> <appname>
# List running services associated with an app
docker service ls
# List tasks associated with an app
docker service ps <service>
# Inspect task or container 检查一个task或者container，输入ID，输出很长的一个map
docker inspect <task or container>
# List container IDs
docker container ls -q
# Tear down an application
docker stack rm <appname>
# Take down a single node swarm from the manager
docker swarm leave --force
# 貌似stack和service好像差不多
# task和container也差不多，但两者的ID不一样，一个task是一个运行在service中的container
```

---

# PART 4: Swarms (群)

#### Introduction

You deploy this application onto a cluster, running it on multiple machines. Mult-container, multi-machine applications are made possible by joining multiple machines into a "Dockerized" cluster called a **swarm**.

#### Understanding Swarm clusters

A swarm is a group of machines that are running Docker and joined into a cluster. After that has happened, you continue to run the Docker commands you're used to, but now they are executed on a cluster by a **swarm manager**. The machines in a swarm can be physical or virtual. After joining a swarm, they are referred to as **nodes**.

Swarm managers can use several strategies to run containers, such as "emptiest node" -- which fills the least utilized machines with containers. Or "global", which ensures that each machine gets exactly one instance of the specified container. You instruct the swarm manager to use these strategies in the Compose file, just like the one you have already been using.

Swarm managers are the only machines in a swarm that can execute your commands, or authorize other machines to join the swarm as **workers**. Workers are just there to provide capacity and do not have the authority to tell any other machine what it can and cannot do.

Up until now, you have been using Docker in a single-host mode on your local machine. But Docker also can be switched into **swarm mode**, and that's what enables the use of swarms. Enabling swarm mode instantly makes the current machine a swarm manager. From then on, Docker runs the commands you execute on the swarm you're managing, ranther than just on the current machine.

#### Set up your swarm

A swarm is made up of multiple nodes, which can be either physical or virtual machines. The basic concept is simple enough: run `docker swarm init` to enable swarm mode and make your current machine a swarm manager, then run `docker swarm join` on other machines to have them join the swarm as workers. We use VMs to quickly create a two-machine cluster and turn it into a swarm.

**VMs on your local machine**

```bash
docker-machine create --driver virtualbox myvm1
docker-machine create --driver virtualbox myvm2
```

**List the VMs and get their IP address**

```bash
docker-machine ls
NAME    ACTIVE   DRIVER       STATE     URL                         SWARM   DOCKER        ERRORS
myvm1   -        virtualbox   Running   tcp://192.168.99.100:2376           v17.06.2-ce
myvm2   -        virtualbox   Running   tcp://192.168.99.101:2376           v17.06.2-ce
```

**Initialize the swarm and add nodes**

The first machine acts as the manager, which executes management commands and authenticates workers to join the swarm, and the second is a worker.

```bash
$ docker-machine ssh myvm1 "docker swarm init --advertise-addr <myvm1 ip>"
Swarm initialized: current node <node ID> is now a manager.

To add a worker to this swarm, run the following command:
  
  docker swarm join \
  --token <token> \
  <myvm ip>:<port>

To add a manger to this swarm, run 'docker swarm join-token manager' and follow the instructions.
```

port 2377 (the swarm management port)
prot 2376 (the Docker daemon port)

As you can see, the response to `docker swarm init` contains a pre-configured `docker swarm join` command for you to run on any nodes you want to add. Copy this command, and send it to `myvm2` via `docker-machine ssh` to have `myvm2` join your new swarm as a worker:

```bash
$ docker-machine ssh myvm2 "docker swarm join \
--token <token> \
<ip>:2377"

This node joined a swarm as a worker.
```

Congratulations, you have created your first swarm!

```bash
$ docker-machine ssh myvm1 "docker node ls"
ID                            HOSTNAME            STATUS              AVAILABILITY        MANAGER STATUS
brtu9urxwfd5j0zrmkubhpkbd     myvm2               Ready               Active
rihwohkh3ph38fhillhhb84sk *   myvm1               Ready               Active              Leader
```

##### Leaving a swarm

If you want to start over, you can run `docker swarm leave` from each node.

#### Deploy your app on the swarm cluster

##### Configure a `docker-machine` shell to the swarm manager

##### Deploy the app on the swarm manager

`docker stack deploy` command you used in part 3 may take a few seconds to complete and the deployment takes some time to be available. Use the `docker service ps <service_name>` command on a swarm manager to verify that all services have been redeployed.

```bash
docker stack deploy -c docer-compose.yml getstartedlab
```

And that's it, the app is deployed on a swarm cluster.

+ if your image is stored on a private registry instead of Docker Hub, you need to be logged in.

  ```bash
  docker login registry.example.com
  docker stack deploy --with-registry-auth -c docker-compose.yml getstartedlab
  ```

#### Accessing your cluster

You can access your app from the IP address of **either** `myvm1` or `myvm2`.

There are five possible container IDs all cycling by randomly, demonstrating the load-balancing.

The reason both IP addresses work is that nodes in a swarm participate in an ingress **routing mesh**. This ensures that a service deployed at a certain port within your swarm always has that port reserved to itself, no matter what node is actually running the container.

Keep in mind that to use the ingress network in the swarm, you need to have the following ports open between the swarm nodes before you enable swarm mode:

+ Port 7946 TCP/UDP for container network discovery.
+ Port 4789 UDP for the container ingress network.

#### Iterating and scaling your app

#### Cleanup and reboot

##### Stacks and swarms

You can tear down the stack with `docker stack rm`. For example:

```bash
docker stack rm getstartedlab
```

##### Keep the swarm or remove it

```bash
# worker
docker swarm leave
# manager
docker swarm leave --force
```

#### Recap and cheat sheet

```bash
# View nodes in swarm (while logged on to manager)
docker node ls
# Deploy an app
docker stack deploy -c <file> <appname>

# initialize the swarm and add nodes
docker swarm init --advertise-addr 192.168.199.100
docker swarm join \
	--token SWMTKN-1-196eaghaw... \
	192.168.199.100:2377
# View the nodes in this swarm:
docker node ls
# Deploy an app
docker stack deploy -c docker-compose.yml getstartedlab
# 还不太清楚service和stack的关系
# docker service ps getstartedlab_web 这两个结果一样的
docker stack ps getstartedlab

# tear down the stack
docker stack rm getstartedlab
# remove this swarm
docker swarm leave
docker swarm leave --force
```

---

# PART 5: Stacks (堆栈)

In part4, you learned how to set up a swarm, which is a cluster of machines running Docker, and deployed an application to it, with containers running in concert on multiple machines.

Now you reach the top of the hierarchy of distributed applications: the **stack**. A stack is a group of interrelated services that share dependencies, and can be orchestrated and scaled together. A single stack is capable of defining and coordinating the functionality of an entire application (though very complex applications may want to use multiple stacks).

Here, you can take what you've learned, make multiple services realte to each other, and run them on multiple machines.

#### Add a new service and redeploy

It's easy to add services to our `docker-compose.yml` file. First, let's add a free visualizer service that lets us look at how our swarm is scheduling containers.

1. Open up `docker-compose.yml` in an editor and replace its contents with the following. Be sure to replace `username/repo:tag` with your image details.

   ```yaml
   version: "3"
   services:
     web:
       # replace username/repo:tag with your name and iamge details
       image: ihsin/get-started:part2
       deploy:
         replicas: 5
         restart_policy:
           condition: on-failure
         resources:
           limits:
             cpus: "0.1"
             memory: 50M
       ports:
         - "80:80"
       networks:
         - webnet
     visualizer:
       image: dockersamples/visualizer:stable
       ports:
         - "8080:8080"
       volumes:
         - "/var/run/docker.sock:/var/run/docker.sock"
       deploy:
         placement:
           constraints: [node.role == manager]
       networks:
         - webnet
   networks:
     webnet:
   ```

   The only thing new here is the peer service to `web`, named `visualizer`. Notice two new things here: a `volumes` key, giving the visualizer access to the host's socket file for Docker, and a `placement` key, ensuring that this service only ever runs on a swarm manager -- never a worker.

2. Make sure your shell is configured to talk to `myvm1`.

3. Re-run the `docker stack deploy` command on the manager, and whatever services need updating are updated:

   ```bash
   $ docker stack deploy -c docker-compose.yml getstartedlab
   Updating service getstartedlab_web (id: k4nhsd1pm4ebaolgy2q6fsjp)
   Creating service getstartedlab_visualizer
   ```

4. Take a look at the visualizer
   You saw in the Compose file that `visualizer` runs on port 8080. Get the IP address of one of your nodes by running `docker-machine ls`. Go to either IP adderss at port 8080 and you can see the visualizer running.
   You can corroborate this visualization by running `docker stack ps <stack>`:

   ```bash
   docker stack ps getstartedlab
   ```

#### Persist the data

Let's go through the same workflow once more to add a Redis database for storing app data.

1. Save this new `docker-compose.yml` file, which finally adds a Redis service.

   ```yaml
   version: "3"
   services:
     web:
       # replace username/repo:tag with your name and iamge details
       image: ihsin/get-started:part2
       deploy:
         replicas: 5
         restart_policy:
           condition: on-failure
         resources:
           limits:
             cpus: "0.1"
             memory: 50M
       ports:
         - "80:80"
       networks:
         - webnet
     visualizer:
       image: dockersamples/visualizer:stable
       ports:
         - "8080:8080"
       volumes:
         - "/var/run/docker.sock:/var/run/docker.sock"
       deploy:
         placement:
           constraints: [node.role == manager]
       networks:
         - webnet
     redis:
       image: redis
       ports:
         - "6379:6379"
       volumes:
         - "/home/wyx/test/data:/data"
       deploy:
         placement:
           constraints: [node.role == manager]
       command: redis-server --appendonly yes
       networks:
         - webnet
   networks:
     webnet:
   ```

   Redis has an official image in the Docker library and has been granted the short `image` name of just `redis`, so no `username/repo` notation here.

   There are a couple of things in the `redis` specification that make data persist between deployments of this stack:

   + `redis` always runs on the manager, so it's always using the same filesystem.
   + `redis` accesses an arbitrary directory in the host's file system as `/data` inside the container, which is where Redis stores data.

2. Create a `./data` directory on the manager:

   ```bash
   docker-machine ssh myvm1 "mkdir ./data"
   ```

3. Make sure your shell is configured to talk to `myvm1`

4. Run `docker stack deploy` one more time.

   ```bash
   $ docker stack deploy -c docker-compose.yml getstartedlab
   ```

5. Run `docker service ls` to verify that the three services are running as expected.

   ```bash
   $ docker service ls
   ```

6. Check the web page.

---

# PART 6: Deploy your app

Docker Engine - Community

#### Install Dokcer Engine --- Community

#### Create your swarm

#### Deploy your app

#### RUN SOME SWARM COMMANDS TO VERIFY THE DEPLOYMENT

+ Use `docker node ls`  to list the nodes in your swarm.
+ Use `docker service ls` to list services.
+ Use `docker service ps <service>` to view tasks for a service.

#### OPEN PORTS TO SERVICES ON CLOUD PROVIDER MACHINES

