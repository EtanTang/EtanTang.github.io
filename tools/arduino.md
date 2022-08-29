# Arduino IDE

# 安装arduino
从官网下载[arduino ide](https://www.arduino.cc/en/software)，并安装。

# 安装开发板
1. 从GitHub下载[esp32.zip](https://github.com/espressif/arduino-esp32/releases)、package_esp32_index.json和[tools](https://github.com/espressif/arduino-esp32/releases)。
2. 将package_esp32_index.json放到C:\Users\Administrator\Documents\ArduinoData。
3. 将esp32.zip和esptool放到C:\Users\Administrator\Documents\ArduinoData\staging。
4. 打开arduino -> 文件 -> 首选项，配置URL为https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json 。
5. 打开arduino -> 工具 -> 开发板，安装开发板esp32([参考](https://docs.espressif.com/projects/arduino-esp32/en/latest/installing.html))。

# 安装库
1. 从官网下载[blinker的arduino库](https://diandeng.tech/dev)。
2. 通过Arduino IDE 菜单 -> 项目 -> 加载库 -> 添加.ZIP库 导入到库。