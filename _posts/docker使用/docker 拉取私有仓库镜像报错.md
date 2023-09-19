
### 报错信息：          
```
docker Error response from daemon: Get https://***: http: server gave HTTP response to HTTPS client
```

### 解决方法：          
修改 `/etc/docker/daemon.json`中的内容。        
原内容：
```
 {
      "runtimes": {
          "nvidia": {
          "path": "nvidia-container-runtime",
          "runtimeArgs": []
          }
      }
}
```
增加为：
```
 { 
      "runtimes": {
          "nvidia": {
          "path": "nvidia-container-runtime",
          "runtimeArgs": []
          }
      }，
     "insecure-registries": [
         "192.168.2.100:5000"
     ]
}
```
重启docker 服务，拉取镜像。
```
sudo systemctl restart docker
```
