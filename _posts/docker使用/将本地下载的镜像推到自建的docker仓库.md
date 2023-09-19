
1、将本地已存在的docker镜像设置tag          
```sh
docker tag scalabel/www:latest 192.168.2.100:5000/scalabel/www:v1.0
```          
docker tag source-image target-image-tag

2、登录本地docker仓库
```sh
docker login 192.168.2.100:5000
```
输入对应的账户名和密码

3、 将新的镜像推向自建仓库
```sh
docker push 192.168.2.100:5000/scalabel/www:v1.0
```
