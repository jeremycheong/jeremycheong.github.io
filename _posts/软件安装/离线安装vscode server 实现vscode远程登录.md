
### 背景          
使用`vscode`进行远程开发时候，通过`remote-ssh`可以连接到远程服务器或设备进行代码开发，前提是远程服务器需要可以访问外网，这样可以自动下载，安装并运行对应版本的vscode server服务。如果远程服务器或者设备不支持连接外网，那么只能手动下载对应版本的vscode server进行安装。          
### 1. 本机vscode版本查看          
```sh
code --version
```          
将显示如下信息：
```
1.57.0
b4c1bd0a9b03c749ea011b06c6d2676c8091a70c
x64
```          
中间的信息为本机安装的vscode的commit_id，需要记下，后面下载vscode server版本有用。          

### 2. 下载对应commit_id版本的vscode server       
打开浏览器，在地址栏中输入 `https://update.code.visualstudio.com/commit:${commit_id}/server-linux-arm64/stable`   
其中： 
```
${commit_id}替换为上述的字符串；        
server-linux-arm64可替换为其他版本，如server-linux-x64
```          
下载完成后，上传到远程服务器或者设备中。          
ssh到远程服务器或设备命令行操作如下：          
```sh
commit_id=b4c1bd0a9b03c749ea011b06c6d2676c8091a70c
mkdir -p ~/.vscode-server/bin/${commit_id}
tar zxvf /path/to/vscode-server-linux-arm64.tar.gz -C ~/.vscode-server/bin/${commit_id} --strip 1
touch ~/.vscode-server/bin/${commit_id}/0
```

### 3. 本机远程测试
本机再次使用vscode远程方式进行连接，检查是否成功。
