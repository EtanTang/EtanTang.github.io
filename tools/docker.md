# 如何在 Ubuntu 20.04 上安装和使用 Docker
Docker 是一个开源的容器化平台，你可以用它来构建，测试，并且作为可移动的容器去部署应用，这些容器可以在任何地方运行。一个容器表示一个应用的运行环境，并且包含软件运行所需要的所有依赖软件。
Docker 是现代软件开发，持续集成，持续交付的一部分。
本文将为大家介绍如何在 Ubuntu 上安装 Docker。
Docker 在标准的 Ubuntu 20.04 软件源中可用，但是可能不是最新的版本。我们将会从 Docker 的官方软件源中安装最新的 Docker 软件包。
## 1. 安装 Docker
在 Ubuntu 上安装 Docker 非常方便。通过 Docker 软件源，导入 GPG key，就可以安装软件包。
首先，更新软件包索引，并且安装必要的依赖软件，来添加一个新的 HTTPS 软件源：
```bash
sudo apt update
sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common
```
使用下面的 curl 导入源仓库的 GPG key：
```bash
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
将 Docker APT 软件源添加到你的系统：
```bash
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```
```bash
sudo add-apt-repository "deb [arch=armhf] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"
```

现在，Docker 软件源被启用了，你可以安装软件源中任何可用的 Docker 版本。
运行下面的命令来安装 Docker 最新版本。
```bash
sudo apt update
sudo apt install docker-ce docker-ce-cli containerd.io
```
安装完成后，Docker 服务将会自动启动。输入下面的命令来验证：
```bash
sudo systemctl status docker
```
## 2. 卸载 Docker
在卸载 Docker 之前，你最好移除所有的容器，镜像，卷和网络。
运行下面的命令停止所有正在运行的容器，并且移除所有的 docker 对象:
```bash
docker container stop $(docker container ls -aq)
docker system prune -a --volumes
```
接下来你可以使用apt命令来卸载 Docker：
```bash
sudo apt purge docker-ce
sudo apt autoremove
```
## 3. 安装 Docker Compose
Docker Compose 是一个二进制文件。安装非常简单直接。我们会将该文件下载到一个目录，并添加到系统的 PATH 环境变量，同时将该文件设置为可执行。
使用curl将 Compose 文件下载到/usr/local/bin目录：
```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose
```
下载完成后，将该文件设置为可执行：
```bash
sudo chmod +x /usr/local/bin/docker-compose
```
运行下面的命令验证是否安装成功并查看Compose 的版本：
```bash
docker-compose --version
```
输出界面如下：
```bash
docker-compose version 1.25.5, build b02f1306
```
## 4. Docker Compose 入门
接下来，我们将会使用 Docker Compose 来构建一个多容器 WordPress 应用。

1. 创建一个项目目录：
```bash
mkdir my_app
cd my_app
```
2. 打开你的文本编辑器，创建一个名为docker-compose.yml的文件，放在项目目录下：
```bash
nano docker-compose.yml
```
3. 粘贴下面的内容：
```yml
version: '3'
services:
  db:
    image: mysql:5.7
    restart: always
    volumes:
      - db_data:/var/lib/mysql
    environment:
      MYSQL_ROOT_PASSWORD: password
      MYSQL_DATABASE: wordpress
  wordpress:
    image: wordpress
    restart: always
    volumes:
      - ./wp_data:/var/www/html
    ports:
      - "8080:80"
    environment:
      WORDPRESS_DB_HOST: db:3306
      WORDPRESS_DB_NAME: wordpress
      WORDPRESS_DB_USER: root
      WORDPRESS_DB_PASSWORD: password
    depends_on:
       - db
volumes:
    db_data:
    wp_data:
```
docker-compose.yml文件第一行指定了 Compose file的版本。这里有一些不同的 Compose 版本，每个版本支持指定的 Docker 发行版。

4. 配置服务器，db 和 wordpress。
当 docker-compose 运行，每个服务器运行一个镜像，创建一个独立的容器。
服务器可以使用 DockerHub 上可用的镜像，或者从 Dockerfile 文件本地构建的镜像。此外，还可以指定一些设置，例如：暴露端口，数据卷，环境变量，依赖，和其他的 Docker 命令。
在项目目录运行下面的命令来启动 WordPress 应用：
```bash
docker-compose up
```
Compose 会拉取镜像，启动容器，并且创建wp_data目录。
在你的浏览器中输入[http://0.0.0.0:8080/](http://0.0.0.0:8080/)，你将会看到 Wordpress 安装屏幕。此时，WordPress 应用已经启动并且运行了，你可以开始安装主题或者插件了。你可以按CTRL+C来停止 Compose。
你还可以通过在 Compose 后面加上-d选项，以后台模式启动 Compose：
```bash
docker-compose up -d
```
使用ps选项，检查运行的服务：
```bash
docker-compose ps
```
输出如下：
```bash
Name                     Command               State          Ports        
----------------------------------------------------------------------------------
my_app_db_1          docker-entrypoint.sh mysqld      Up      3306/tcp, 33060/tcp 
my_app_wordpress_1   docker-entrypoint.sh apach ...   Up      0.0.0.0:8080->80/tcp
```
运行以下命令停止服务：
```bash
docker-compose stop
```
还可以使用down命令停止、移除应用容器和网络
```bash
docker-compose down
```
更新镜像
```bash
cp -r /root/data/docker_data/wikijs /root/data/docker_data/wikijs.archive  # 万事先备份，以防万一
cd /root/data/docker_data/wikijs  # 进入docker-compose所在的文件夹
docker-compose pull    # 拉取最新的镜像
docker-compose up -d   # 重新更新当前镜像
```

## 5. 卸载 Docker Compose
卸载 Docker Compose，只需要简单删除二进制文件即可，输入以下命令：
```bash
sudo rm /usr/local/bin/docker-compose
```