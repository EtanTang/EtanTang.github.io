# 搭建一个Ubuntu的桌面系统（带VNC/noVNC）随时随地可以通过浏览器访问
在Docker里面跑一个Ubuntu的桌面环境，可以实现一些日常使用的功能，需要持久保留的目录可以映射到本地，如果之后不想用了也可以直接删除，不会把系统“搞脏”。
系统镜像中带有：
- 中文语言包
- Xfce4桌面
- TigerVNC服务
- noVNC服务（HTML5WEBVNC）
- Chromium浏览器
- Deluge2.0.3
- qBittorrent4.2.1
- Transmission2.9.4
- Telegram
- ibus-pinyin输入法

## 1. 项目展示
GitHub项目地址：https://github.com/fcwu/docker-ubuntu-vnc-desktop  
Docker Hub：https://hub.docker.com/r/imlala/ubuntu-xfce-vnc-novnc

## 2. 搭建环境
安装好Docker、Docker-compose。（参考[docker安装](/docker/docker安装)）

## 3. 搭建方式
服务器初始设置
```bash
apt update -y  # 升级packages
apt install wget  #如果出现 wget:command not found,可以用这个命令安装
```
创建一下安装的目录：
```bash
mkdir -p /root/data/docker_data/Ubuntu_desktop
cd /root/data/docker_data/Ubuntu_desktop
nano docker-compose.yml
```
docker-compose.yml填入以下内容：
```yml
version: '3.5'

services:
    ubuntu-xfce-vnc:
        container_name: xfce
        image: imlala/ubuntu-xfce-vnc-novnc:latest
        shm_size: "1gb"  # 防止高分辨率下Chromium崩溃,如果内存足够也可以加大一点点
        ports:
            - 5900:5900   # TigerVNC的服务端口（保证端口是没被占用的，冒号右边的端口不能改，左边的可以改）
            - 6080:6080   # noVNC的服务端口，注意事项同上
        environment: 
            - VNC_PASSWD=PAS3WorD    # 改成你自己想要的密码
            - GEOMETRY=1280x720      # 屏幕分辨率，800×600/1024×768诸如此类的可自己调整
            - DEPTH=24               # 颜色位数16/24/32可用，越高画面越细腻，但网络不好的也会更卡
        volumes: 
            - ./Downloads:/root/Downloads  # Chromium/Deluge/qBittorrent/Transmission下载的文件默认保存位置都是root/Downloads下
            - ./Documents:/root/Documents  # 映射一些其他目录
            - ./Pictures:/root/Pictures
            - ./Videos:/root/Videos
            - ./Music:/root/Music
        restart: unless-stopped
```
没问题的话，ctrl+x退出，按y保存，enter确认。

然后运行：
```bash
docker-compose up -d 
```
访问：http:服务ip:6080 即可。

## 附录：
还有一个类似的项目：
docker run -itd --restart=always -p 6080:80 -e HTTP_PASSWORD=mypassword -v /root/data/docker_data/Ubuntu_desktop/dev/shm:/dev/shm dorowu/ubuntu-desktop-lxde-vnc

还可以添加声音，具体见GitHub：https://github.com/fcwu/docker-ubuntu-vnc-desktop