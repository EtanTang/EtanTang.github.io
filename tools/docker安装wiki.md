# 用Docker搭建Wiki.js
## 1. 特点
可以自建的开源项目（GitHub 15.7k star）
支持多平台部署（Docker、Heroku、Linux、macOS、Windows）
支持多用户
易部署，易管理（Docker一下即可）
权限管理功能丰富
支持外部存储
性能好（基于Node.js）
搜索功能强大，支持全局、按关键字搜索
支持标签功能，可按标签浏览
简洁的web页面
支持多语言，支持中文
拥有多种编辑器，目前有code（可编写html页面），markdown（在编辑页面可看到页面效果），visual editor（功能强大的文本编辑器，所见即所得）
团队维护更新积极
## 2. 项目展示
GitHub原项目地址：https://github.com/Requarks/wiki  
官网地址：https://js.wiki/  
官方文档地址：https://docs.requarks.io/  
本教程用的镜像：https://hub.docker.com/r/linuxserver/wikijs  
Demo:https://docs.requarks.io/  
## 3. 搭建环境
安装好Docker、Docker-compose。

## 4. 搭建
创建一下安装的目录：
```bash
mkdir -p /root/data/docker_data/wikijs
cd /root/data/docker_data/wikijs
nano docker-compose.yml
```
docker-compose.yml填入以下内容：
```yml
---
version: "2.1"
services:
  wikijs:
    image: lscr.io/linuxserver/wikijs
    container_name: wikijs
    environment:
      - PUID=1000        # 如何查看当前用户的PUID和PGID，直接命令行输入id就行
      - PGID=1000
      - TZ=Asia/Shanghai
    volumes:
      - ./config:/config  # 配置文件映射到本地，数据不会因为Docker停止而丢失
      - ./data:/data  # 数据映射到本地，数据不会因为Docker停止而丢失
    ports:
      - 8080:3000   # 左边的8080可以自己调整端口号，右边的3000不要改
    restart: unless-stopped
```
没问题的话，ctrl+x退出，按y保存，enter确认。

然后运行：
```bash
docker-compose up -d 
```
访问：http:服务ip:8080 即可。
*如果出现权限问题，可检查映射到宿主机文件夹的用户访问权限，及docker-compose.yml的配置：
PUID和PGID是否正确，亦可在映射文件位置添加rw（./config:/config:rw）。*

## 5. 更新
```bash
cp -r /root/data/docker_data/wikijs /root/data/docker_data/wikijs.archive  # 万事先备份，以防万一
cd /root/data/docker_data/wikijs  # 进入docker-compose所在的文件夹
docker-compose pull    # 拉取最新的镜像
docker-compose up -d   # 重新更新当前镜像
```
利用Docker-compose搭建的应用，更新非常容易。
## 6. 卸载
```bash
cd /root/data/docker_data/wikijs  # 进入docker-compose所在的文件夹
docker-compose down    # 停止容器，此时不会删除映射到本地的数据
rm -rf /root/data/docker_data/wikijs  # 完全删除映射到本地的数据
```
此时，执行：
```bash
ls -al
```
可以看到三个文件夹，config、 data、 docker-compose.yml
如果想要删除配置文件和数据，重新搭建的话，执行：
```bash
rm -rf config/
rm -rf data/
```
如果想要全部删除的话，执行：
```bash
cd ..  # 退回到/root/data/docker_data目录
rm -rf /root/data/docker_data/wikijs  # 完全删除映射到本地的数据
```
