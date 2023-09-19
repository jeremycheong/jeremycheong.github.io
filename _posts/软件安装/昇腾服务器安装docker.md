
### 前言          
通过之前查看内核和系统版本信息可知昇腾服务器为`aarch64`， `Centos7`系统。

### 1. 查看仓库信息
```sh
yum repolist all|grep "CentOS-7 - Extras"
```
centos-extra仓库必须处于“enabled”状态。这是操作系统默认配置，如果你已经设置成“disabled”，则需要重新设置。          
若没有开启，则需要修改仓库状态
```sh
 yum-config-manager --enable "CentOS-7 - Extras - mirrors.huaweicloud.com"
```

### 2. 安装依赖包
```sh
sudo yum install -y yum-utils device-mapper-persistent-data lvm2
```

### 3. 配置软件仓库
```sh
sudo yum-config-manager --add-repo https://download.docker.com/linux/centos/docker-ce.repo
```

### 4. 安装`docker-ce`
```sh
sudo yum install docker-ce docker-ce-cli containerd.io
```

