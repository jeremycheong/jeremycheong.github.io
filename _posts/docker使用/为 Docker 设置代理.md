
docker是一个非常方便的容器环境，但docker常常需要拉取大量的镜像，而docker hub的主机又架设在国外，如果使用国内网络访问，将十分的慢，因此我们可以使用国外的代理访问。

docker这个程序只是一个控制台程序，用于attach，真正操作docker的是运行在后台的docker daemon，也就是我们需要通过systemctl start docker来启动docker daemon。所以说即使我们设置了环境变量http_proxy，那么也只是针对前台docker console使用，而真正访问pull镜像的确是后台的daemon，因此，需要设置daemon访问proxy。

```sh
sudo mkdir -p /etc/systemd/system/docker.service.d
sudo vim /etc/systemd/system/docker.service.d/http-proxy.conf
```
在里面输入
```sh
[Service]
Environment="HTTP_PROXY=http://127.0.0.1:12333/"
Environment="HTTPS_PROXY=http://127.0.0.1:12333/"
Environment="NO_PROXY=localhost,127.0.0.1,192.168.2.100"
```
之后执行以下命令重启docker就可以了：
```sh
sudo systemctl daemon-reload
sudo systemctl restart docker
```
