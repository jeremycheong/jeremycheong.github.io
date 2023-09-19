
Linux Mint系统是基于Ubuntu的，说实话用起来感觉还是很不错的，安装Docker到Ubuntu的方法几乎可以完全迁移到Mint上。

1. 安装docker-ce
--------------
1.1 更新包索引，并安装依赖包
```sh
sudo apt-get install -y \
apt-transport-https \
ca-certificates \
curl \
software-properties-common
```
1.2 添加Docker官方的GPG秘钥
```sh
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```
1.3 设置稳定版的仓库（注意：Linux Mint 19.1使用 bionic 的源），注：由于按照Docker官网的安装方式进行安装，使用了lsb_release -cs读取当前的版本（Linux Mint的运行结果为sonya），故出现了无法设置仓库问题。
```sh
sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu bionic stable"
```
1.4 更新并安装docker-ce
```sh
sudo apt-get update
sudo apt-get install docker-ce
```
2. 安装nvidia-docer2
----------------------
参照官方安装 [nvidia docker install](https://github.com/NVIDIA/nvidia-docker)
2.1 Add the package repositories
```sh
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
sudo apt-key add -
```
需要注意的是，官方教程通过 `distribution=$(. /etc/os-release;echo $ID$VERSION_ID)` 来获取系统版本，在 linux mint 上会返回 mint 自己的版本系统，需要将其替换为 ubuntu相对应的版本信息。
```sh
distribution=ubuntu18.04
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
```
2.2 Install nvidia-docker2 and reload the Docker daemon configuration
```sh
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd
```

3. 修改docker默认存储路径
------------
docker 拉去镜像默认的存储的位置在 `/var/lib/docker`， 时间长了容易将根目录空间撑爆。
3.1 停止docker
```sh
/etc/init.d/docker stop
```

3.2 迁移docker路径
```sh
cd /var/lib/
mv docker /your/path/
```

3.3 建立软连接
```sh
ln -sf /your/path/docker /var/lib/docker
```

3.4 重新启动docker
```sh
/etc/init.d/docker start
```

4. 将docker加入用户组
-------------
Docker守候进程绑定的是一个unix socket，而不是TCP端口。这个套接字默认的属主是root，其他是用户可以使用sudo命令来访问这个套接字文件。因为这个原因，docker服务进程都是以root帐号的身份运行的。
4.1 查看docker用户组，如果没有就新建一个
```sh
sudo cat /etc/group | grep docker
```
显示 `docker:x:999:`， 则说明已经有了docker用户组。
如果没有则新建：
```sh
sudo groupadd docker
```
4.2 应用用户加入docker用户组
```sh
sudo usermod -aG docker ${USER}
```
4.3 重启docker服务
```sh
sudo systemctl restart docker
```
4.4 切换或者退出当前账户再从新登入
```sh
su root
su ${USER}
```
4.5 最后使用 `docker info`测试是否修改成功。
如果提示get ......dial unix /var/run/docker.sock权限不够，则修改/var/run/docker.sock权限。
```sh
sudo chmod a+rw /var/run/docker.sock
```


