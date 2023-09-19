### Docker 启动错误：Bind for xxx failed“port is already allocated”

1. 查看指定端口情况，如`8081`端口               
```sh
lsof -i:8081
```               
2. 被 `docker-proxy` 占用强行重置               
```sh
# 1. 停止 docker 服务
sudo service docker stop
# 2. 删除 docker-proxy 的 db 文件
sudo rm -f /var/lib/docker/network/files/local-kv.db
# 3. 重启 docker 服务
sudo service docker start
```
---
### windows WSL中docker服务启动
```sh
sudo service docker start
```
---
