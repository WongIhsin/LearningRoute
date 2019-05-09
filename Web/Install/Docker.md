# Docker安装

官方文档：

CentOS 7：https://docs.docker.com/install/linux/docker-ce/centos/#prerequisites
Ubtuntu：https://docs.docker.com/install/linux/docker-ce/ubuntu/

---

#### CentOS 7——CE, Community Edition

set up repository --> install docker ce (container) --> systemctl start docker

1. **OS requirements**

   1. A maintained version of CentOS 7.
   2. The `centos-extras` repository must be enabled. (Enabled by default)
   3. The `overlay2` storage driver is recommended.

2. **Uninstall old versions**

   ```bash
   $ sudo yum remove docker \
   				  docker-client \
   				  docker-client-latest \
                     docker-common \
                     docker-latest \
                     docker-latest-logrotate \
                     docker-logrotate \
                     docker-engine
   ```

3. **Install Docker CE**
   **Install using the repository**

   1. **Set up the repository**

      1. install required packages. `yum-utils & device-mapper-persistent-data & lvm2` 安装依赖

         ```bash
         $ sudo yum install -y yum-utils \
         					  device-mapper-persistent-data \
         					  lvm2
         ```

      2. Use the following command to set up the **stable** repository. 添加远程仓库

         ```bash
         $ sudo yum-config-manager \
         --add-repo \
         https://download.docker.com/linux/centos/docker-ce.repo
         ```

      3. optional: Enable the nightly or test repositories

         ```bash
         # These repositories are included in `docker.repo` file but are disabled by default.
         # Enables the nightly repository
         $ sudo yum-config-manager --enable docker-ce-nightly
         # Enables the test channel
         $ sudo yum-config-manager --enable docker-ce-test
         # `--disable` to disable the `nightly` or `test`. `--enable` to re-enable it.
         $ sudo yum-config-manager --disable docker-ce-nightly
         ```

   2. **Install docker ce**

      1. install the latest version of Docker CE and containerd, or go to the step2 to install a specific version

         ```bash
         $ sudo yum install docker-ce docker-ce-cli containerd.io
         ```

         fingerprint: `060A 61C5 1B55 8A7F 742B 77AA C52F EB6B 621E 9F35`

      2. list the available versions in the repo

         ```bash
         $ yum list docker-ce --showduplicates | sort -r
         ```

         Install a specific version by its fully qualified package name

         ```bash
         $ sudo yum install docker-ce-<VERSION_STRING> docker-ce-cli-<VERSION_STRING> containerd.io
         ```

   3. **Start Docker**

      ```bash
      $ sudo systemctl start docker
      ```

   4. **Verify** that Docker CE is installed correctly by running the `hello-world` image.

      ```bash
      $ sudo docker run hello-world
      ```

      