

### 1. 安装依赖包
```sh
sudo apt-get install -y build-essential
sudo apt-get install -y checkinstall
sudo apt-get install -y libreadline-gplv2-dev
sudo apt-get install -y libncursesw5-dev
sudo apt-get install -y libssl-dev
sudo apt-get install -y libsqlite3-dev
sudo apt-get install -y tk-dev
sudo apt-get install -y libgdbm-dev
sudo apt-get install -y libc6-dev
sudo apt-get install -y libbz2-dev
sudo apt-get install -y zlib1g-dev
sudo apt-get install -y openssl
sudo apt-get install -y libffi-dev
sudo apt-get install -y python3-dev
sudo apt-get install -y python3-setuptools
sudo apt-get install -y wget
```

### 2. 下载python3.6源码
从[https://www.python.org/ftp/python/](https://www.python.org/ftp/python/)上下载所需的python版本，这里下载 `Python-3.6.9.tar.xz`
解压
```sh
tar xvf Python-3.6.9.tar.xz

```
###3. 进入目录编译安装
进入到python3.6.9的目录中
```sh
./configure --enable-optimizations
sudo make altinstall
sudo make install
```


