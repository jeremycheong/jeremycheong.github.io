
### 1. 背景          
在程序员办公场景或者家庭局域网场景经常会遇到文件的共享，但是局域网下通过已有网盘进行文件共享简直对不起内网带宽，因此在局域网下搭建本地的共享网盘还是十分有必要。          
`File Browser`可以是个不错的选择，有一下几个优点：          
- 轻量，docker部署。          
- 用户管理与权限管理，适合个人，单个或多个小团队使用。          
- 可实现在线预览图片，视频（个别编码不行），编辑文本。          
- 支持指定的命令行指令。     
- ……          

### 2. 项目地址          
```
https://github.com/filebrowser/filebrowser
```          
### 3. 安装及运行          
支持本地编译运行，二进制文件安装，docker运行，由于在服务器上只是使用，那么这里进行docker的部署。          
1. 拉取docker镜像          
```sh
docker pull filebrowser/filebrowser
```          
2. 创建该网盘访问的根目录          
```sh
mkdir -p /opt/DataDisk/FileBrowser/file_root
```          
3. **创建相应的配置文件**          
```sh
cd /opt/DataDisk/FileBrowser
touch filebrowser.db
```          
4. 运行docker          
```sh
nohup docker run \
-v /opt/DataDisk/FileBrowser/file_root:/srv \
-v /opt/DataDisk/FileBrowser/filebrowser.db:/database.db \
--user $(id -u):$(id -g) \
-p 80:80 \
filebrowser/filebrowser \
>/dev/null 2>&1 &
```          

### 4. 访问                   
本地浏览器访问`http://10.141.17.200:80`          

### 5. 其他
还可以使用另一个功能更强大的私人网盘[Nextcloud](https://github.com/nextcloud/server)。其功能更为强大。
