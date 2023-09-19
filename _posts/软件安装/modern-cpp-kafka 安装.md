### 背景          
C++使用kafka推送消息，`modern-cpp-kafka`是kafka的C++客户端的封装。          

### 依赖          
`librdkafka`版本需要至少**1.7.0**版本，直接使用`apt`安装的版本过低，很多定义没有。          
`libboost-program-options-dev`可直接使用`apt`进行安装。

### 编译安装                  
1. 编译安装`librdkafka`        
下载[librdkafka](https://github.com/edenhill/librdkafka)的release版`1.8.0`；       
     
```sh
cd librdkafka-1.8.0
#automatically install dependencies using the system's package manager:
./configure --install-deps
make -j4
sudo make install
```          

2. 安装`libboost-program-options-dev`        
```sh
sudo apt install libboost-program-options-dev
```          

3. 编译安装`modern-cpp-kafka`      
下载[modern-cpp-kafka](https://github.com/morganstanley/modern-cpp-kafka)的release版`v2021.08.25`；    

```sh
# 在CMakeLists.txt中取消test子目录的编译，不引入GTest库
mkdir build
cmake ..
make -j4
sudo make install
```     
  
