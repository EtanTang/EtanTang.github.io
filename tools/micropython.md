# micropython

ESP8266安装MicroPython，[官方文档](http://docs.micropython.org/en/latest/esp8266/tutorial/intro.html)。

# 下载固件
从MicroPython downloads page [ESP8266](https://micropython.org/download/#esp8266),
[ESP32](https://micropython.org/download/#esp32)下载固件。
# 安装esptool
```bash
pip install esptool
```

# 擦除flash
使用esptool.py擦除flash
```bash
esptool.py --port /dev/ttyUSB0 erase_flash
```
OR Windows
```bash
esptool.py --port COM0 erase_flash
```

# 刷写固件
切换到固件所在目录，执行下面指令  
ESP8266
```bash
esptool.py --port /dev/ttyUSB0 --baud 460800 write_flash --flash_size=detect 0 esp8266-20170108-v1.8.7.bin
```
ESP32
```bash
esptool.py --chip esp32 --port /dev/ttyUSB0 write_flash -z 0x1000 esp32-20180511-v1.9.4.bin
```
OR windows  
ESP8266
```bash
esptool.py --port COM0 --baud 460800 write_flash --flash_size=detect 0 esp8266-20170108-v1.8.7.bin
```
ESP32
```bash
esptool.py --chip esp32 --port COM0 write_flash -z 0x1000 esp32-20180511-v1.9.4.bin
```

# Thonny IDE
[Thonny](https://thonny.org/)一款不错的轻量级python ide。