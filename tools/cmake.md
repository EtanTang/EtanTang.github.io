# 安装cmake

```bash
apt install cmake
```

# 创建工作目录

创建如下工作目录：  
```
hello
|  
+---CMakeLists.txt  
|  
+---build/  
|  
+---main.c  
|  
+---src/  
    |  
    +---CMakeLists.txt 
    |
    +---hello.c 
```

# 编辑CMakeLists.txt
第一层目录中CMakeLists.txt的内容：
```bash
cmake_minimum_required(VERSION 2.8.9)
project(project_main)
add_subdirectory( src )
aux_source_directory(. src_list)
add_executable(main ${src_list})
target_link_libraries(main hello)
```

第一层目录中main.c的内容：
```
extern sayHello(void);

int main(void)
{
    sayHello();

    return 0;
}
```

第二层目src中CMakeLists.txt的内容：
```
aux_source_directory(. dir_hello_src)
add_library(hello ${dir_hello_src})
```
第二层目录src中hello.c的内容：
```
#include<stdio.h>

void sayHello(void)
{
    printf("Hello, World!");
}
```

# 执行cmake
切换到build目录中，执行如下操作：
```bash
cd build
cmake ..
make
```

最后，在build目录中会生成hello可执行文件。
```
./hello
```


