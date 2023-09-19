
### 背景          
在docker内部使用gdb调试时刻遇到了gdb如下报错信息
```
ptrace:Operation not permitted
```
### Docker上涉及到gdb调试权限的特性          
Docker借用了linux对进程设置capabilities，而其子进程继承父进程capabilites特性来完成对容器capacities的控制。Docker create和docker run参数中有下面两个参数可以对容器默认的capabilites进行修改：        
```
--cap-add //添加某个capabilites属性
--cap-del //剔除某个默认的capabilites属性
```          
Docker 将gdb调试需要SYS_PTRACE属性被禁止掉了，所以gdb在调试的时候会显示ptrace被禁止。        
所以想在docker内部调试gdb解决办法就是create和run的时候带上** --cap-add sys_ptrace* **          
### 示例启动命令：          
```sh
docker run -it --runtime=nvidia -p 10000:10000 -v your_souce_code_path:/mount_path --cap-add=SYS_PTRACE --security-opt seccomp=unconfined your_docker_image_name
```
说明： -p 10000:10000是将gdbserver监听的10000端口映射出来 ， 方便远程调试。         
在docker容器中用gdbserver启动程序 `gdbserver localhost:10000 decoder`
