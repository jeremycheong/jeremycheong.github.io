
### 前言
jetson nano 开发板在预设的10W(MAXN)模式下需要用5v4A的DC供电。            
用5v2A的DC或者micro-usb供电建议使用5W模式。          
供电不足会导致掉电关机。


### 显示当前电源模式
```sh
sudo /usr/sbin/nvpmodel -q
```
### 自定义模式
要自定义自己的模式，可将参数添加到`/etc/nvpmodel/nvpmodel_t210_jetson-nano.conf`中        
文件中模式1的实例（usb供电模式）
```sh
< POWER_MODEL ID=1 NAME=5W >
CPU_ONLINE CORE_0 1
CPU_ONLINE CORE_1 1
CPU_ONLINE CORE_2 0
CPU_ONLINE CORE_3 0
CPU_A57 MIN_FREQ 0
CPU_A57 MAX_FREQ 918000
GPU_POWER_CONTROL_ENABLE GPU_PWR_CNTL_EN on
GPU MIN_FREQ 0
GPU MAX_FREQ 640000000
GPU_POWER_CONTROL_DISABLE GPU_PWR_CNTL_DIS auto
EMC MAX_FREQ 1600000000
```
注意其中ID不能重复

### jetson_clocks脚本
默认情况下，DVFS已启用，CPU / GPU / EMC时钟将根据负载而变化  

