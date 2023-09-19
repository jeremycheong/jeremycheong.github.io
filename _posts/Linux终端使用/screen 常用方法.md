

### 1. 界面大小设置         
在使用远程进行`screen`连接时，会出现会话界面被resize的情况，显示内容被换行；            
修改`screen`的系统设置，打开配置文件`/etc/screenrc`，修改如下：         
```
termcapinfo xterm 'is=\E[r\E[m\E[2J\E[H\E[?7h\E[?1;4;6l'
修改为：
termcapinfo xterm* 'is=\E[r\E[m\E[2J\E[H\E[?7h\E[?1;4;6l'
```     
### 2. 常用命令         
2.1 创建或者恢复到已有的会话            
```sh
screen -R test
```         
该指令如果没有`test`会话，则会创建该会话，若已存在`test`会话，则会打开进入。            
2.2 查看已创建的会话            
```sh
screen -ls
```     
2.3 将会话放后台            
```
使用组合键`Ctrl + a + d`
或者
直接关闭终端或断开连接
```     
2.4 退出或删除会话          
```sh
screen -S test -X quit
或者
在会话界面使用 Ctrl + d 逐层退出
```         
2.5 强制进入某会话          
```sh
screen -D -r test
```         
2.6 分屏操作            
上下分屏： `Ctrl + a` 然后按`Shift + s`           
左右分屏： `Ctrl + a` 然后按`Shift + |`         
分屏创建终端： `Ctrl + a` 然后按`c`         
切换分屏： `Ctrl + a` 然后按`Tab`           
关闭当前分屏： `Ctrl + a` 然后按`shift + x`               
2.7 其他               
`Ctrl + a` 然后按`Shift + k`，关闭当前窗口


