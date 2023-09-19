
### 前言
在jetson系列的板子上，查看GPU使用并没有在PC机上`nvidia-smi`的命令，推荐使用jtop查看，可同时查看CPU，内存等系统信息。          

### jtop安装
安装依赖包
```sh
sudo apt-get install cmake python3-dev python3-pip libhdf5-serial-dev hdf5-tools libatlas-base-dev gfortran
```

更换国内python源
```sh
mkdir ~/.pip
vim ~/.pip/pip.config
```
输入内容：
```sh
[global]
index-url = https://mirrors.aliyun.com/pypi/simple
[install]
trusted-host = mirrors.aliyun.com
```
安装jtop
```sh
sudo pip3 install --upgrade pip
sudo pip3 install jetson-stats
```
