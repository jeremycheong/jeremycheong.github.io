
安装samba服务
```sh
sudo apt install samba samba-common
```

新建共享目录          
```sh
# 新建目录，用于共享
sudo mkdir /usr/local/volumes
# 更改权限信息
sudo chown nobody:nogroup /usr/local/volumes
# 给所有用户添加读写权限
sudo chmod 777 /usr/local/volumes
```

添加用户
```sh
sudo smbpasswd -a zhangym
```

配置samba          
修改 /etc/samba/smb.conf,在最后面添加以下配置：          
```sh
[Volumes]
  comment = Data
  path = /usr/local/volumes
  browseable = yes
  writable = yes
  available = yes
  valid users = zhangym
```

重启samba服务
```sh
sudo service smbd restart
```

性能调优：          
```
[global]
workgroup = WORKGROUP
netbios name = Cubietruck
server string = Cubietruck
interfaces = lo eth0 wlan0
max connections = 0
deadtime = 10
log file = /var/log/samba/log.%m
max log size = 50
security = user
encrypt passwords = yes
guest account = nobody
guest ok = no
admin users = root

#以下是清除log文件内报错的，可以不必添加
load printers = no
printing = bsd
printcap name = /dev/null
disable spoolss = yes
#以下为优化项
follow symlinks = no
wide links = no
# enable some read/write tuning，启用读写缓存等等
aio read size = 16384
aio write size = 16384
aio write behind = true
write cache size = 2097152
max xmit = 65536
large readwrite = yes

# Use sendfile for reading files efficiently:以下几个我测试貌似影响不大
use sendfile = yes
min receivefile size = 16384
getwd cache = true

[cubie]
path = /home
read only = no
valid users = cubie,root
```
