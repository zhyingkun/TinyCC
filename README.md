# TinyCC

[![Build Status](https://travis-ci.com/zhyingkun/TinyCC.svg)](https://travis-ci.com/zhyingkun/TinyCC)

---

## 基本介绍

1. 本工程是 TinyCC 及其 C 示例的构建工程，用于运行调试
2. 所有 TinyCC 源码来自[TCC 官方](https://bellard.org/tcc/)0.9.27 版本
3. 仅保留 Mac/Linux/Win 三个平台的源码，去除了其他平台特定代码（为了足够简单）
4. 增加了源码注释，支持 Debug/Release 编译模式
5. 整个工程 PC 端编译构建采用 cmake 来管理，用于代替 TCC 官方的编译脚本

---

## 如何编译

##### 1. 在 Mac 上采用 Xcode 编译

```bash
cd TinyCC/
mkdir buildXcode && cd buildXcode
cmake -DCMAKE_INSTALL_PREFIX=./install -G "Xcode" ..
# cmake -DCMAKE_INSTALL_PREFIX=/usr/local/zyk -G "Xcode" ..
```

此时已经在 buildXcode 文件夹下生成了 Xcode 工程，直接打开并编译即可

##### 2. 直接命令行编译（支持 Mac 和 Linux）

```bash
cd TinyCC/
mkdir build && cd build
cmake -DCMAKE_INSTALL_PREFIX=./install .. # default is Debug
# cmake -DCMAKE_INSTALL_PREFIX=/usr/local/zyk ..
# for Debug: cmake -DCMAKE_BUILD_TYPE=Debug ..
# for Release: cmake -DCMAKE_BUILD_TYPE=Release ..
make
# for more details: make VERBOSE=1
make install
```

make 命令会自动编译好各个模块

##### 3. 在 Windows 上使用 Cygwin + Visual Studio 2017 进行编译

```bash
cd TinyCC/
mkdir buildVS && cd buildVS
cmake -DCMAKE_INSTALL_PREFIX=./install -G "Visual Studio 15 2017 Win64" ..
```

此时已经在 buildVS 文件夹下生成了 Visual Studio 工程，双击打开并编译即可

---

## 文件夹说明

1. demo：TinyCC 库相关用法和应用构建
2. example：TinyCC 官方源码中附带的用法示例，做了一些小修改
3. tcc：TinyCC 官方源码，加了注释，编译出 TinyCC 动态链接库
4. 所有 CMakeLists.txt：用于管理整个工程

## 问题记录

1. TCC 源码编译中有一个 ONE_SOURCE 模式，需要添加编译参数-DONE_SOURCE=0，tcc.c 也需要
2. 源码可分为三部分，分别是 tcc 编译器的动态库、tcc 命令、runtime 静态库
3. Ubuntu 下编译 runtime 库可能需要安装 gcc-multilib，具体命令为`sudo apt-get install gcc-multilib`
4. 针对 64 位 Linux 系统，使用 tcc 进行编译可能需要添加库搜索路径：`-L/usr/lib/x86_64-linux-gnu`

## Windows+Cygwin 环境下操作流程

1. 根据上述编译指引，生成 Visual Studio 工程并执行生成 ALL_BUILD 目标和 INSTALL 目标
2. Cygwin 的 shell 下，执行如下命令流程：

```bash
cd TinyCC/runtime/win32
make # build libtcc1.a
make run # run demo
make clean
```

3. 此时 tcc 的 runtime 库已经编译好并执行 demo 打印相关日志
4. 运行其他 Demo，执行如下命令流程：

```bash
cd TinyCC/examples
make
# make run
```
