
1. 安装完docker后查看是否存在docker用户组
```sh
sudo cat /etc/group | grep docker
```
如果存在，则不需要创建，如果不存在则创建docker用户组。

2. 创建docker用户组（可选）
```sh
sudo groupadd -g 999 docker
```
-g 999： 为组id，可以不指定。

3. 将相应用户添加到该分组
```sh
sudo usermod -aG docker $USER
```

4. 修改守护进程绑定的套接字的权限，能够被docker分组访问
```sh
sudo chmod 666 /var/run/docker.sock
```

5. 修改docker镜像的存储路径
编辑 /etc/docker/daemon.json 文件,没有可以新建该文件：
```
{
"registry-mirrors": ["http://hub-mirror.c.163.com"],
"data-root": "/new/path/to/docker"
}
```

6. 重启docker-daemon
```sh
sudo systemctl daemon-reload
sudo systemctl restart docker
```

