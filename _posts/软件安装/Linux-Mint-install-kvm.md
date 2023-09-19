
# Linux Mint 18.2 安装kvm虚拟机，压缩镜像
---
## 1. 确认cpu是否支持
执行 `egrep "(svm|vmx)" /proc/cpuinfo`
如果出现vmx或svm即表明支持，如未出现需确认是否支持cpu虚拟化。

## 2. 安装kvm
执行：
```
sudo apt-get install qemu-kvm -y --allow-unauthenticated

sudo apt-get install qemu -y --allow-unauthenticated

sudo apt-get install virt-manager -y --allow-unauthenticated

sudo apt-get install virt-viewer -y --allow-unauthenticated

sudo apt-get install libvirt-bin -y --allow-unauthenticated

sudo apt-get install bridge-utils -y --allow-unauthenticated

sudo apt-get install spice-client-gtk python-spice-client-gtk gir1.2-spice-client-gtk-3.0 -y --allow-unauthenticated
```
为确保以上安装依赖已正常运行，需重启机器（执行 sudo reboot）。

## 3. 启动kvm虚拟机平台
命令行执行 `sudo virt-manager`

## 4.压缩镜像
安装完虚拟机进入磁盘存储目录会发现此时的虚拟机镜像（`disk.qcow2`）文件会很大。
执行
`sudo qemu-img convert -c -O qcow2 ./disk.qcow2 ./disk.gz.img`
对镜像文件进行剪切。


