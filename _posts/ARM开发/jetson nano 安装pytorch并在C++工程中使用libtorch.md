### 前言
pytorch是很常用的模型训练框架，一般在工程化中，使用模型进行infer推理的框架为TensorRT，但是有些后处理为了快速和方便会用到libtorch中的数据类型，对于这类应用，需要将libtorch的C++库安装到jetson nano。

### libtorch的另一种安装思路
1. pytorch官网提供三种方式使用pytorch， python安装， libtorch，和源码编译。          
libtorch是编译好的库，在arm64架构上肯定是不能用的，源码编译的话（emmmmmmmmm），还是算了。          
那么就剩下python的安装包了，pytorch这类训练框架为了计算效率，底层一般是C++实现的，外部用python封装，而且python的包为`.whl`文件，其实就是压缩文件，可以解压出来，去里面寻找一下。         
 
2. nvidia 提供了jetson 系列的pytorch安装包，可在[这里](https://forums.developer.nvidia.com/t/pytorch-for-jetson-nano-version-1-5-0-now-available/72048)下载合适的版本。

3. 解压pytorchxxx.whl包
```sh
mv torch-1.5.0-cp36-cp36m-linux_aarch64.whl torch-1.5.0-cp36-cp36m-linux_aarch64.zip
unzip torch-1.5.0-cp36-cp36m-linux_aarch64.zip
```
解压后有三个文件：`caffe2`， `torch`， `torch-1.5.0.dist-info`，（寻找中...）        
摊牌了，在`/PATH/UNZIP/FOLDER/torch/share/cmake/Torch`下找到了cmake相关的文件。同时发现解压的文件下里面包含有`include`和`lib`这类C++熟悉的文件夹。        
这就比较明朗了。

4. CMakeLists.txt中引用
```sh
set(Torch_DIR ${CMAKE_CURRENT_SOURCE_DIR}/3rdLibs/torch_lib/torch/share/cmake/Torch)
find_package(Torch REQUIRED)
message(STATUS "TORCH_INCLUDE_DIRS: ${TORCH_INCLUDE_DIRS}")
message(STATUS "TORCH_LIBRARIES: ${TORCH_LIBRARIES}")
```          

### 问题解决
经过上面的操作，已经可以将libtorch在C++工程中使用了，但是在实际编译过程中发现，torch-1.5需要C++14，然后就用1.2版本了。          
编译完成还会发生一些链接库没有的现象：
```
/usr/bin/ld: warning: libmpi_cxx.so.20, needed by /home/koala/Workspace/AlgoLib/3rdLibs/torch_lib/torch/lib/libtorch.so, not found (try using -rpath or -rpath-link)
/usr/bin/ld: warning: libmpi.so.20, needed by /home/koala/Workspace/AlgoLib/3rdLibs/torch_lib/torch/lib/libtorch.so, not found (try using -rpath or -rpath-link)
/usr/bin/ld: warning: libopenblas.so.0, needed by /home/koala/Workspace/AlgoLib/3rdLibs/torch_lib/torch/lib/libtorch.so, not found (try using -rpath or -rpath-link)
/home/koala/Workspace/AlgoLib/3rdLibs/torch_lib/torch/lib/libtorch.so: undefined reference to `MPI_Comm_rank'
/home/koala/Workspace/AlgoLib/3rdLibs/torch_lib/torch/lib/libtorch.so: undefined reference to `MPI_Win_unlock'
/home/koala/Workspace/AlgoLib/3rdLibs/torch_lib/torch/lib/libtorch.so: undefined reference to `MPI_Abort'
/home/koala/Workspace/AlgoLib/3rdLibs/torch_lib/torch/lib/libtorch.so: undefined reference to `ompi_mpi_comm_null'
/home/koala/Workspace/AlgoLib/3rdLibs/torch_lib/torch/lib/libtorch.so: undefined reference to `MPI_Type_create_subarray'
/home/koala/Workspace/AlgoLib/3rdLibs/torch_lib/torch/lib/libtorch.so: undefined reference to `ompi_mpi_char'
...
```
直接安装这两个库          
```sh
sudo apt install libopenmpi-dev libopenblas-dev --no-install-recommends -y
```
再次编译成功！
