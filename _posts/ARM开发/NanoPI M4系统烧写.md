### 背景介绍

Nano Pi M4是一款小巧的低功耗嵌入式开发板，使用RK3399的cpu，性能强劲，但是本身没有内存，系统需烧写到外置SD卡或者购买其配件eMMC。  

[TOC]  

### 官方教程

[NanoPi M4/zh](https://wiki.friendlyelec.com/wiki/index.php/NanoPi_M4/zh)    
系统下载：[下载地址](http://download.friendlyelec.com/NanoPiM4)  
**注意：**

电源选择官方建议使用5V/3A，但是实测5V/2A也可以，如果跑程序较为耗资源，还是使用官方建议的电源；  
系统下载，针对不同的系统烧写方式，官方提供了三种形式:
1. type c 系统镜像，例如：rk3399-typec-friendlycore-lite-focal-5.15-arm64-20220126.zip。通过开发板的type c 接口将系统烧入开发板中，但是该开发板没有板上内存，且该板子的type c 口也为供电口，电脑提供的电流不足以支撑该板子正常运行和系统烧写，40pi的GPIO的2pin（5v）或4pin（5v）， 6pin（GND），也可用于供电。     

2. sd卡系统镜像，例如：rk3399-sd-friendlycore-lite-focal-5.15-arm64-20220126.img.zip。这是最常见的烧写方式，与树莓派相同，烧写成功后直接启动就可进入系统。 

3. eMMC系统镜像，例如：rk3399-eflasher-friendlycore-lite-focal-5.15-arm64-20220126.img.zip。eMMC模块在该开发板上被设计成扩展模块，需额外购买，镜像也有其对应的系统镜像。

*该开发板没有SSD固态硬盘接口。*     
*该开发板没有eDP显示屏接口。*

### 在eMMC模块上烧写镜像

官方教程中一定要注意：  
还是需要一张SD卡，用于烧写带有eflasher字样的系统镜像，然后eMMC模块安装就位，上电后根据步骤再将SD卡中的系统写入到eMMC模块中。    
一般购买eMMC模块都会有一个MicroSD to eMMC adapter的配件，就是可以将eMMC转成SD卡的接口样子，直接插入到读卡器中，此时一定注意!如果使用此方式烧写系统，那么可以通过该配件直接向eMMC模块中烧写SD字样的系统镜像，而不是eflasher字样的系统镜像。  
eMMC模块在开发板上朝向HDMI接口模块方向，与MicroSD to eMMC adapter适配器安装朝SD卡接口相反方向安装。

