---
title: megvii-01
date: 2020-11-11 09:08:42
tags: 
    - 旷视觉天元开源框架
categories: 
    - megvii
---

# Background - MegEngine Reading
因为硕士快毕业了，论文也发了，所以就想好好深入剖析一下 MegEngine 的黑科技哈。
笔者尽量给大家带来生动形象的源码解读，不过由于笔者目前的水平原因，本系列文章仅仅适合小白入门哈，大神请绕道～～
当然了，我也会尽我所能，一点点地从浅入深的剖析MegEngine。 废话不多说，开始正题的讲解，今天为大家带来的是框架（下文对MegEngine的简称）的整体架构。

# 框架所需要的编译环境
安装方式各种各异啊，只是使用的话使用pip 安装就好了。
1. 框架本身是提供```pip``` 的安装的，命令如下 ： 
``` bash
python3 -m pip install megengine -f https://megengine.org.cn/whl/mge.html
``` 
官方是已经做好了二进制的文件，对于大多数人，直接使用就行了。
所需的安装环境也不复杂, 具体如下所示：

$OS$
- Linux: 64bit
- Windows: 64bit
- MacOS: 10.14+

$Python$
- 3.5 To 3.8

不知道为什么不支持 3.9 吼～ 这边要入职了去请教一下

$Other Requirement$
- CUDA >= 10.1 （Nvidia 的 runtime 驱动以及相关的 编译器啊之类的）
- cuDNN >= 7.6  （一些dnn 的计算库 需要和 cuda版本配套）
- TensorRT >= 5.1.5 （推理优化模型，合并卷积层，减少参数，int8 优化等）
- LLVM/Clang >= 6.0 （编译前端clang （和大名鼎鼎的 g++ 类似）和后端llvm）
- python-numpy (矩阵运算的 不懂的话 apt 安装一下， ```windows``` ```用conda``` 或者 ```pip``` 搞一波)

环境就是这些接下来是我们 BFS党（build from source） 的一些教程 

# 编译环境安装

- cmake （3.15+） 小编搞得最新的 [cmake官网](https://cmake.org/download/)
- git (一般 linux 自带)
- build-essential (```sudo apt-get install build-essential```) 一些 unix makefile 的编译环境
- TensorRT [下载地址](https://developer.nvidia.com/nvidia-tensorrt-7x-download) 注意安装完 ```LD_LIBRARY_PYTH``` 指向库目录
- Cuda Cudnn 野鸡教程搜一下，随处都是。 我这边是 11.0 就不赘述了 需要注意 cudnn 的软链接 不能 ```cp``` 自己 ```ls -ahl``` 看一下指向 不放心的话 ```tree``` 一下。 这里需要说一下 框架中寻找cmake 的 路径是 ```cmake/cudnn.cmake ``` 源码类似下面这样.

``` cmake
find_package(PkgConfig)
if(${PkgConfig_FOUND})
    pkg_check_modules(PC_CUDNN QUIET CUDNN)
endif()

if(NOT "$ENV{LIBRARY_PATH}" STREQUAL "")
    string(REPLACE ":" ";" SYSTEM_LIBRARY_PATHS $ENV{LIBRARY_PATH})
endif()

```

note： 这里需要说一下就是，cmake 是通过 pkgconfig 找到lib的 路径的， 需要添加下 LIBRARY_PATH 或者 自己在 `/etc/ld.config.so.d` 下面 定义文件 然后 `ldconfig` 同理哈，后续的 TensorRT CUDA CUDNN 都得这样，以后不提示了哈。

- llvm 这个比较大哈，这个解释下就是编译器的后端， 不理解的可以详细差资料。
**以下可以跳过哈～～** 
*讲个故事： 9几年的时候  GNU 搞得 gcc 一片大伙，c++ 的 gdb，gprof 调试都挺不错的，但是 后面几年 多线程搞出来了，诶 gdb 有个问题 不支持线程调试，然后不知道啥原因一直没加， APPLE 就是乔布斯那个（知道吧？？？） 结果出来和GCC说：“兄蝶你帮我搞几个功能” 但是 gcc 当时不吊人家，好了， 一句名言出来了“当年你对我不屑一顾，现在我让你高攀不起”， apple 03年的时候 自己搞了个团队搞编译器， 哇草还真被他们搞出来了，就是所谓 llvm ，还支持多线程调试。 吊吧，后面搞了个前端编译 clang 完美兼容 gcc。 呵呵，gcc 傻眼了*

言归正传，小编跑到[llvm官网](https://releases.llvm.org/download.html#11.0.0) 下载了 llvm 和 clang 然后 用 cmake 装的，这个是为了支持JIT （后续文章会说，这边还没深入到那么多）
``` cmake
cmake --build .
cmake --build . --target install
```
这边 . 是当前目录，可以自己选择一个目录  一般是 搞个 ```build``` ```然后到那个build```

简单的不带cuda 的编译 非常简单， 安装下 MKL 就行 
MKL 是微软的一套 高效计算库

带cuda的编译比较麻烦，安装 llvm tensorRT cudnn mkl 
具体可以看下 /scirpt/cmake-build/host_build.sh 脚本里面

``` bash
cmake \
-DCMAKE_BUILD_TYPE=$BUILD_TYPE \
-DMGE_INFERENCE_ONLY=$MGE_INFERENCE_ONLY \
-DMGE_WITH_CUDA=$MGE_WITH_CUDA \
-DCMAKE_INSTALL_PREFIX=$INSTALL_DIR \
${EXTRA_CMAKE_ARGS} \
$SRC_DIR
```

需要注意的是， halide 的编译 非常奇怪 

```
find_package(LLVM 6.0 REQUIRED CONFIG)
```
这边我改成了 llvm 11.0 的版本， 没有使用 6.0 哈 （结果出错了）

敲黑板，改代码成为11会造成问题 ， 虽然可以通过一部分的编译 但是 halide 是需要 6.0 的llvm 的
# 项目架构
好了编译的前期环境做完了，接下来来说一下整体项目的架构 ... 

