
### 前言
在实际的项目开发中使用官方发布的TensirRT可能已经满足不了模型自定义的一些网络层的解析与实现，这时需要下载并编译TensorRT OSS工程进行修改并编译。

### 下载工程
```sh
git clone -b master https://github.com/nvidia/TensorRT TRTOSS
cd TRTOSS 
git submodule update --init --recursive
export TRT_SOURCE=`pwd`
```

### 在jetson nano上编译
更新cmake 版本
打开源码的`CMakeLists.txt`可看到cmake版本的最低要求是3.13，因此，需要从源码编译cmake.          
下载cmake-3.17
```sh
cd ~/Downloads
wget -e "https_proxy=http://10.10.1.83:12333" https://github.com/Kitware/CMake/releases/download/v3.17.3/cmake-3.17.3.tar.gz
tar -zxvf cmake-3.17.3.tar.gz
cd cmake-3.17.3/
./bootstrap # 仅安装cmake，不含gui
make -j4
sudo make install
sudo ldconfig
```
可通过`cmake --vserson` 查看cmake版本是否切换过来。          

编译TensorRT OSS          
```sh
mkdir build && cd build
cmake \
    -DTRT_LIB_DIR=/usr/lib/aarch64-linux-gnu \
    -DTRT_OUT_DIR=`pwd`/out \
    -DCMAKE_BUILD_TYPE=Release \
    -DCUDA_VERISON=10.2 \
    -DCUDNN_VERSION=8.0 \
    -DTRT_PLATFORM_ID=aarch_64 \
    ..
```
cmake 参数重点解释几个：
```
TRT_LIB_DIR： 官方版的TensorRT的lib文件路径
TRT_OUT_DIR： 编译输出的路径
TRT_PLATFORM_ID： 这个参数在TensorRT OSS工程的参数解释中没有，查看其CMakeLists.txt可知道，如果没有定义则默认是x86_64，这将影                                       响编译过程中下载protobuf的版本，那么jetson nano对应的应该是aarch_64，可到protobuf的release页面中确认
```
不出意外的话可以编译成功。

### 为什么不用官方提供的docker进行编译
TensorRT OSS官方提供了jetson系列设备的交叉编译的方法，经过尝试有以下缺点：
1. 首先需要刷jetson系统的`sdk-manager`软件，进行安装，这个软件安装系统要求是ubuntu18.04(20.04不可以的), 要使用这个docker，那么本机需要有nvidia 显卡，显卡驱动需要至少440版本[\摊手]。          
2. 需要下载jetpack软件，用于交叉编译，在sdk-manager这个软件下载巨慢...          
3. docker build过程中需要在docker中下载一些软件，具体看官方`docker`文件夹下的`ubuntu-cross-aarch64.Dockerfile`文件，也是慢到没朋友而且还经常报错导致镜像建立不成功，大部分都是由于网络原因。        
所以，不如简单粗暴直接在jetson nano上编译，这种库不是经常编译升级。

