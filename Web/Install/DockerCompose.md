# DockerCompose 安装

---

官网：https://docs.docker.com/compose/install/#alternative-install-options

Install Compose on Linux systems

1. Run this command to download the current stable release of Dokcer Compose:

   ```bash
   sudo curl -L "https://github.com/docker/compose/releases/download/1.24.0/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
   ```

2. Apply executable permissions to the binary:

   ```bash
   sudo chmod +x /usr/local/bin/docker-compose
   ```

3. Test the installation.

   ```bash
   $ docker-compose --version
   docker-compose version 1.24.0, build 0aa59064
   ```

大概十几兆

在官网上么有找到用pip安装的

