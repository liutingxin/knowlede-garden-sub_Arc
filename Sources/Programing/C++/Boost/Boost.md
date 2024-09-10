---
date created: 2024-08-18
date modified: 2024-08-18
tags:
  - boost库
  - cpp
---

## boost基本使用

``` bash
pacman -Ss boost boost-libs
```

在Archlinux下安装的boost库，头文件在/usr/include/boost, 库文件在/usr/lib

使用g++的一般编译链接命令为：

``` bash
# -I 是用来指定头文件的路径；
# -L 是用来指定库文件的路径；
# -l 用来指定要链接的库，默认链接动态库；
# 链接静态库时，需要明确指定静态库的文件路径。
g++ -I/usr/include/boost/ -L/usr/lib -lboost_xxxx source.cpp  -o target
g++ -I/usr/include/boost/xxx -L/usr/lib -lboost_xxxx source.cpp  -o target


g++ -I/usr/include/boost testFile.cpp /usr/lib/libboost_filesystem.a /usr/lib/libboost_system.a -o file_test_static
```

## 动态链接和静态链接的区别：

- **动态库**（文件扩展名 `.so`，例如 `libboost_filesystem.so`）是在程序运行时加载的。动态库会使最终的可执行文件更小，因为库的代码不会被直接嵌入到可执行文件中。程序运行时，动态库会被加载到内存中并使用。
    
- **静态库**（文件扩展名 `.a`，例如 `libboost_filesystem.a`）是在编译期间被嵌入到可执行文件中的。因此，最终的可执行文件会包含所有需要的库代码，并不依赖外部的库文件，运行时无需额外的库加载。

1. 链接动态库：需要指定包含头文件的位置，以及动态库的名称
2. 链接静态库：需要指定包含头文件的位置，以及静态库所在的绝对路径

## ## 如何验证是否使用了动态链接

使用 `ldd` 命令查看一个可执行文件依赖的共享库：

``` bash
ldd target_file
```

如果显示依赖项中有类似于 `libxx  
x_xxx.so`，则表示程序是动态链接的。

## 关于静态库的一些小知识

为了更好的静态链接，需要确保如下几点，否则可能导致链接失败：

1. **确保 xxx静态库完整性**： 例如， boost库中的`libxxx_filesystem.a` 静态库与 `libboost_system.a` 静态库不匹配或缺少依赖库，那么就可能会发生这种错误。可以尝试重新安装 `boost` 开发包，确保所有库都正确。
2. **链接顺序问题**： 链接器在链接静态库时需要考虑库的依赖顺序。可以尝试将 `.cpp` 文件放在静态库之前：

``` bash
g++ -I/usr/include/boost testFile.cpp /usr/lib/libboost_filesystem.a /usr/lib/libboost_system.a -o file_test_static
```

3. **尝试通过动态链接方式测试**： 也可以尝试使用动态链接来排除问题。如果动态链接成功，那么问题可能在于静态库的不完整性或顺序问题：

``` bash
g++ -I/usr/include/boost testFile.cpp -lboost_filesystem -lboost_system -o file_test_dynamic
```

4. **检查 程序库的 版本**： 确保你使用的 库 的版本正确，且所有静态库和头文件来自相同的版本库。
5. **检查其他依赖**： Boost Filesystem 可能还依赖其他库，比如 `pthread` 等。如果是这样，你可以显式添加其他依赖库：

``` bash
g++ -I/usr/include/boost testFile.cpp /usr/lib/libboost_filesystem.a /usr/lib/libboost_system.a -lpthread -o file_test_static
```

<mark style="background: #FF5582A6;">总而言之就是，要尝试将源代码放在静态库之前，确保所有依赖库已正确链接，并通过动态链接测试。</mark>
