

### 环境准备
1. 安装VS2019 VC++ 开发环境 （略）
2. 设置vscode terminal为VS develop X64 terminal
打开vscode -> settings -> 搜索 “terminal.integrated.shell.windows” -> Edit in settings.json;         
将下面内容添加进来          

```
    "terminal.integrated.shell.windows": "C:\\WINDOWS\\system32\\cmd.exe",
    "terminal.integrated.shellArgs.windows": [
        "/k",
        "D:\\Program Files (x86)\\Microsoft Visual Studio\\2019\\Professional\\VC\\Auxiliary\\Build\\vcvars64.bat"
    ]
```               

**注意：** 将里面的路径替换为自己电脑上的VS2019安装路径。            
       
3. 保存设置文件，关闭vscode后重新打开，点击 Terminal -> New Terminal 显示如下则说明成功               

```
**********************************************************************
** Visual Studio 2019 Developer Command Prompt v16.8.4
** Copyright (c) 2020 Microsoft Corporation
**********************************************************************
[vcvarsall.bat] Environment initialized for: 'x64'

E:\Workspace\ncnn-20201218-full-source>
```

### 编译ncnn源码
1. 下载ncnn源码可访问[ncnn-release](https://github.com/Tencent/ncnn/releases),选择`ncnn-xxx-full-source.zip`字样的文件，并解压。               

2. 将解压后的文件夹拖入vscode中并打开命令行，执行一下操作关闭`example`，`benchmark`, `tools`，`Vulkan`的开关：               

```sh
mkdir build-vs2019-nmake 

cd build-vs2019-nmake

cmake -DCMAKE_BUILD_TYPE=Release -DNCNN_BUILD_BENCHMARK=OFF -DNCNN_BUILD_TOOLS=OFF -DNCNN_BUILD_EXAMPLES=OFF -DNCNN_VULKAN=OFF -DNCNN_BUILD_TESTS=OFF -G "NMake Makefiles"  ..

nmake

nmake install
```               

完成后，编译好的`release`版本在`build-vs2019-nmake\install`中，将其复制到工程里面。

**注意：** 这里编译的为**release**版本，在引用工程中也必须为**release**，如果要使用`debug`版，可将`-DCMAKE_BUILD_TYPE`设置为`Debug`。

