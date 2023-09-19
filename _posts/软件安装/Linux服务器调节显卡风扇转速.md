

### 背景
服务器ubuntu系统启动后GPU风扇不转，不管温度多高都不转，但是手动设置风扇转速，风扇又是可以正常工作，具体原因未知，至少风扇没有坏。使用手动设置风扇转速来解决这个问题。

### 无桌面ubuntu服务器设置        
使用`coolgpus`脚本进行调节          
工程地址：`https://github.com/andyljones/coolgpus`        
使用pypi安装     
```sh
sudo pip3 install coolgpus
```          

### 使用示例
```sh
# 将gpu风扇转速设置为99%
sudo $(which coolgpus) --kill --speed 99 99

# 关闭设置
sudo $(which coolgpus) --kill

# 或者也可以设置线性控制
# 这个模式下20℃以下转速为5%， 20-55℃之间转速为30%，依次类推
sudo $(which coolgpus) --kill --temp 20 55 80 --speed 5 30 99
```          
如果需要将coolgpus脚本当作一个系统服务长期运行的话，如果你的服务器采用systemd管理server的话，可以在`/etc/systemd/system/coolgpus.service`创建模板。
```sh
[Unit]
Description=Headless GPU Fan Control
After=syslog.target

[Service]
ExecStart=/usr/local/bin/coolgpus --kill --temp 20 40 60 80 90 --speed 5 30 50 70 99
Restart=on-failure
RestartSec=5s
ExecStop=/bin/kill -2 $MAINPID
KillMode=none

[Install]
WantedBy=multi-user.target
```       
然后可以通过如下命令进行控制。
```sh
sudo systemctl enable coolgpus
sudo systemctl start coolgpus
```   

### 注意
使用该脚本的运行需要关闭`x servers`服务，由于是服务器使用，可以直接将其关闭。          
```sh
sudo systemctl set-default multi-user.target
sudo reboot
```          
需要开启时可用          
```sh
sudo systemctl set-default graphical.target
sudo reboot
```

