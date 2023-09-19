### 常用设置          
```sh
docker run -it --name <define name> \
--device=/dev/video0 \
-v /home/zhangym/Data/AlgoProjects:/home/zhangym/AlgoProjects \
-v /tmp/.X11-unix:/tmp/.X11-unix \
-e DISPLAY=$DISPLAY \
--runtime=nvidia \
--net=host \
-e LANG=C.UTF-8 \
--cap-add=SYS_PTRACE \
--security-opt seccomp=unconfined \
<image name> \
bash
```          
其中，`--device=/dev/video0`将usb摄像头映射进去，使得docker中可检测到摄像头。        
`-v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY`在docker中可使用主机设备显示图像。        
主机端需要运行`xhost +`

nvidia docker 映射能力
```sh
docker run -it --name CityManager2.0 -v /home/zhangym/Data/AlgoProjects:/home/osmagic/AlgoProjects -v /tmp/.X11-unix:/tmp/.X11-unix -e DISPLAY=$DISPLAY --runtime=nvidia -e NVIDIA_DRIVER_CAPABILITIES=video,compute,utility --net=host -e LANG=C.UTF-8 --cap-add=SYS_PTRACE --security-opt seccomp=unconfined 192.168.2.100:5000/osmagic/city_sdk:v2.0 bash
```