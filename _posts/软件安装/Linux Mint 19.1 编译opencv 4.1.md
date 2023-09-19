**nvidia 显卡驱动， cuda， cudnn安装不再赘述。**

1. 安装依赖软件
```sh
sudo apt install -y build-essential libgtk2.0-dev pkg-config
sudo apt install -y libavcodec-dev libavformat-dev libswscale-dev libavutil-dev libavresample-dev libxvidcore-dev libx264-dev libv4l-dev libgstreamer1.0-dev libgstreamer-plugins-base1.0-dev
sudo apt install -y libtbb2 libtbb-dev libjpeg-dev libpng-dev libtiff-dev libdc1394-22-dev
sudo apt install -y python-dev python-numpy python3-dev python3-numpy gfortran libblas-dev liblapack-dev libeigen3-dev liblapacke-dev
```
安装其他第三方库
```sh
sudo apt-get install libprotobuf-dev libleveldb-dev libsnappy-dev libhdf5-serial-dev protobuf-compiler
sudo apt-get install libatlas-base-dev
sudo apt-get install libgflags-dev libgoogle-glog-dev liblmdb-dev
sudo apt-get install --no-install-recommends libboost-all-dev
```          
** 特别注意 **    
按照网上教程安装 `ibjasper-dev`时在linux mint 19上会出现无法定位该package。
```sh
sudo add-apt-repository “deb http://security.ubuntu.com/ubuntu xenial-security stable main”
sudo apt update
sudo apt install -y libjasper1 libjasper-dev
```
2. 创建编译目录，设置编译参数
```sh
mkdir build && cd build
```
cmake 参数配置如下：          
```sh
cmake \
-D CMAKE_BUILD_TYPE=RELEASE \
-D CMAKE_INSTALL_PREFIX=/usr/local/opencv4.1 \
-D OPENCV_EXTRA_MODULES_PATH=../../opencv_contrib-4.1.0/modules \
-D WITH_CUDA=ON \
-D WITH_CUBLAS=ON \
-D CUDA_FAST_MATH=ON \
-D WITH_CUFFT=ON \
-D WITH_NVCUVID=ON \
-D WITH_V4L=ON \
-D WITH_LIBV4L=ON \
-D WITH_OPENGL=ON \
-D WITH_FFMPEG=ON \
-D INSTALL_C_EXAMPLES=OFF \
-D BUILD_EXAMPLES=OFF \
-D OPENCV_GENERATE_PKGCONFIG=ON \
-D ENABLE_CXX11=ON \
-D BUILD_opencv_python3=ON \
-D BUILD_opencv_python2=OFF \
-D TINYDNN_USE_OMP=ON \
-D WITH_OPENMP=ON \
-D MKL_WITH_OPENMP=ON \
-D WITH_TBB=OFF \
-D TINYDNN_USE_TBB=OFF \
-D BUILD_TESTS=OFF \
-D BUILD_PERF_TESTS=OFF \
..
```
3. 编译并安装
```sh
make -j6
sudo make install
```
** 注意： 编译opencv4.x时需要讲 nvidia 的解码库 **          
需要下载 `NVIDIA VIDEO CODEC SDK` 放入cuda对应的目录
[nvidia video codec SDK](https://developer.nvidia.com/nvidia-video-codec-sdk)

