
### 命令格式：
```
ffmpeg -i [输入文件名] [参数选项] -f [格式] [输出文件]
ffmpeg [[options][`-i' input_file]]... {[options] output_file}...
```
1、参数选项：   
(1) -an: 去掉音频   
(2) -acodec: 音频选项， 一般后面加copy表示拷贝  
(3) -vcodec:视频选项，一般后面加copy表示拷贝    
2、格式：   
(1) h264: 表示输出的是h264的视频裸流    
(2) mp4: 表示输出的是mp4的视频  
(3)mpegts: 表示ts视频流     
如果没有输入文件，那么视音频捕捉（只在Linux下有效，因为Linux下把音视频设备当作文件句柄来处理）就会起作用。作为通用的规则，选项一般用于下一个特定的文件。如果你给 –b 64选项，改选会设置下一个视频速率。对于原始输入文件，格式选项可能是需要的。缺省情况下，ffmpeg试图尽可能的无损转换，采用与输入同样的音频视频参数来输出。  
### 视频切割

```sh
ffmpeg -hide_banner  -err_detect ignore_err -i ./human206.mp4 -r 25 -codec:v copy -vsync 1 -an -f segment -preset fast -segment_format mpegts -segment_time 10 -force_key_frames  "expr: gte(t, n_forced * 2)" out%d.mp4
```

### 视频转换
* H264视频转ts视频流
```sh
 ffmpeg -i test.h264 -vcodec copy -f mpegts test.ts
```
* H264视频转mp4
```sh
 ffmpeg -i test.h264 -vcodec copy -f mp4 test.mp4
```
* ts视频转mp4
```sh
 ffmpeg -i test.ts -acodec copy -vcodec copy -f mp4 test.mp4
```
* mp4视频转flv
```sh
 ffmpeg -i test.mp4 -acodec copy -vcodec copy -f flv test.flv 
```
* 转换文件为3GP格式
```sh
 ffmpeg -y -i test.mpeg -bitexact -vcodec h263 -b 128 -r 15 -s 176x144 -acodec aac -ac 2 -ar 22500 -ab 24 -f 3gp test.3gp
```
* 转换文件为3GP格式 v2
```sh
 ffmpeg -y -i test.wmv -ac 1 -acodec libamr_nb -ar 8000 -ab 12200 -s 176x144 -b 128 -r 15 test.3gp
```
* 使用 ffmpeg 编码得到高质量的视频
```sh
ffmpeg.exe -i "D:\Video\Fearless\Fearless.avi" -target film-dvd -s 720x352 -padtop 64 -padbottom 64 -maxrate 7350000 -b 3700000 -sc_threshold 1000000000 -trellis -cgop -g 12 -bf 2 -qblur 0.3 -qcomp 0.7 -me full -dc 10 -mbd 2 -aspect 16:9 -pass 2 -passlogfile "D:\Video\ffmpegencode" -an -f mpeg2video "D:\Fearless.m2v"
```
* 转换指定格式文件到FLV格式
```sh
 ffmpeg.exe -i test.mp3 -ab 56 -ar 22050 -b 500 -r 15 -s 320x240 f:\test.flv 
 ffmpeg.exe -i test.wmv -ab 56 -ar 22050 -b 500 -r 15 -s 320x240 f:\test.flv
```
* 转码解密的VOB
```sh
 ffmpeg -i snatch_1.vob -f avi -vcodec mpeg4 -b 800 -g 300 -bf 2 -acodec mp3 -ab 128 snatch.avi
```
（上面的命令行将vob的文件转化成avi文件，mpeg4的视频和mp3的音频。注意命令中使用了B帧，所以mpeg4流是divx5兼容的。GOP大小是300意味着29.97帧频下每10秒就有INTRA帧。该映射在音频语言的DVD转码时候尤其有用，同时编码到几种格式并且在输入流和输出流之间建立映射）

* 转换文件为3GP格式
```sh
 ffmpeg -i test.avi -y -b 20 -s sqcif -r 10 -acodec amr_wb -ab 23.85 -ac 1 -ar 16000 test.3gp
```
（如果要转换为3GP格式，则ffmpeg在编译时必须加上–enable-amr_nb –enable-amr_wb，详细内容可参考：转换视频为3GPP格式）

* 转换文件为MP4格式（支持iPhone/iTouch）
```sh
 ffmpeg  -y  -i input.wmv  -f mp4 -async 1-s 480x320  -acodec libfaac -vcodec libxvid  -qscale 7 -dts_delta_threshold 1 output.mp4
 ffmpeg  -y  -i source_video.avi input -acodec libfaac -ab 128000 -vcodec mpeg4 -b 1200000 -mbd 2 -flags +4mv+trell -aic 2 -cmp 2 -subcmp 2 -s 320x180 -title X final_video.mp4
```
* 将一段音频与一段视频混合
```sh
 ffmpeg -i son.wav -i video_origine.avi video_finale.mpg
```
* 将一段视频转换为DVD格式
```sh
 ffmpeg -i source_video.avi -target pal-dvd -ps 2000000000 -aspect 16:9 finale_video.mpeg
```
（target pal-dvd : Output format ps 2000000000 maximum size for the output file, in bits (here, 2 Gb) aspect 16:9 : Widescreen）

* 转换一段视频为DivX格式
```sh
 ffmpeg -i video_origine.avi -s 320x240 -vcodec msmpeg4v2 video_finale.avi
```
* Turn X images to a video sequence
```sh
 ffmpeg -f image2 -i image%d.jpg video.mpg
```
（This command will transform all the images from the current directory (named image1.jpg, image2.jpg, etc...) to a video file named video.mpg.）

* Turn a video to X images
```sh
 ffmpeg -i video.mpg image%d.jpg
```
（This command will generate the files named image1.jpg, image2.jpg, ... ；The following image formats are also availables : PGM, PPM, PAM, PGMYUV, JPEG, GIF, PNG, TIFF, SGI.）

* 使用ffmpeg录像屏幕(仅限Linux平台)
```sh
 ffmpeg -vcodec mpeg4 -b 1000 -r 10 -g 300 -vd x11:0,0 -s 1024x768 ~/test.avi
```
（-vd x11:0,0 指录制所使用的偏移为 x=0 和 y=0，-s 1024×768 指录制视频的大小为 1024×768。录制的视频文件为 test.avi，将保存到用户主目录中；如果你只想录制一个应用程序窗口或者桌面上的一个固定区域，那么可以指定偏移位置和区域大小。使用xwininfo -frame命令可以完成查找上述参数。）

* 重新调整视频尺寸大小(仅限Linux平台)
```sh
 ffmpeg -vcodec mpeg4 -b 1000 -r 10 -g 300 -i ~/test.avi -s 800×600 ~/test-800-600.avi
```
* 把摄像头的实时视频录制下来，存储为文件(仅限Linux平台)
```sh
 ffmpeg  -f video4linux -s 320*240 -r 10 -i /dev/video0 test.asf
```
* 使用ffmpeg压制H.264视频
```sh
 ffmpeg -threads 4 -i INPUT -r 29.97 -vcodec libx264 -s 480x272 -flags +loop -cmp chroma -deblockalpha 0 -deblockbeta 0 -crf 24 -bt 256k -refs 1 -coder 0 -me umh -me_range 16 -subq 5 -partitions parti4x4+parti8x8+partp8x8 -g 250 -keyint_min 25 -level 30 -qmin 10 -qmax 51 -trellis 2 -sc_threshold 40 -i_qfactor 0.71 -acodec libfaac -ab 128k -ar 48000 -ac 2 OUTPUT
```
（使用该指令可以压缩出比较清晰，而且文件转小的H.264视频文件）

### 网络推送
* udp视频流的推送
```sh
  ffmpeg -re  -i 1.ts  -c copy -f mpegts   udp://192.168.0.106:1234
```
### 视频拼接  
裸码流的拼接，先拼接裸码流，再做容器的封装
```sh
  ffmpeg -i "concat:test1.h264|test2.h264" -vcodec copy -f h264 out12.h264
```
### 图像相关
- 截取一张352x240尺寸大小的，格式为jpg的图片
```sh
 ffmpeg -i test.asf -y -f image2 -t 0.001 -s 352x240 a.jpg
```
- 把视频的前30帧转换成一个Animated Gif
```sh
 ffmpeg -i test.asf -vframes 30 -y -f gif a.gif
```
- 截取指定时间的缩微图,-ss后跟的时间单位为秒
```sh
 ffmpeg -i test.avi -y -f image2 -ss 8 -t 0.001 -s 350x240 test.jpg
```
### 音频处理
- 转换wav到mp2格式
```sh
 ffmpeg -i /tmp/a.wav -ab 64 /tmp/a.mp2 -ab 128 /tmp/b.mp2 -map 0:0 -map 0:0
```
（上面的命令行转换一个64Kbits 的a.wav到128kbits的a.mp2 ‘-map file:index’在输出流的顺序上定义了哪一路输入流是用于每一个输出流的。）

### 其他：
- 分离视频音频流
```sh
ffmpeg -i input_file -vcodec copy -an output_file_video　　//分离视频流
ffmpeg -i input_file -acodec copy -vn output_file_audio　　//分离音频流
```
- 视频解复用
```sh
ffmpeg –i test.mp4 –vcodec copy –an –f m4v test.264
ffmpeg –i test.avi –vcodec copy –an –f m4v test.264
```
- 视频转码
```sh
ffmpeg –i test.mp4 –vcodec h264 –s 352*278 –an –f m4v test.264              //转码为码流原始文件
ffmpeg –i test.mp4 –vcodec h264 –bf 0 –g 25 –s 352*278 –an –f m4v test.264  //转码为码流原始文件
ffmpeg –i test.avi -vcodec mpeg4 –vtag xvid –qsame test_xvid.avi            //转码为封装文件
```
-bf B帧数目控制，-g 关键帧间隔控制，-s 分辨率控制

- 视频封装
```sh
ffmpeg –i video_file –i audio_file –vcodec copy –acodec copy output_file
```
- 视频剪切
```sh
ffmpeg –i test.avi –r 1 –f image2 image-%3d.jpeg        //提取图片
ffmpeg -ss 0:1:30 -t 0:0:20 -i input.avi -vcodec copy -acodec copy output.avi    //剪切视频
```
-r 提取图像的频率，-ss 开始时间，-t 持续时间

- 视频录制
```sh
ffmpeg –i rtsp://192.168.3.205:5555/test –vcodec copy out.avi
```
- YUV序列播放
```sh
ffplay -f rawvideo -video_size 1920x1080 input.yuv
```
- YUV序列转AVI
```sh
ffmpeg –s w*h –pix_fmt yuv420p –i input.yuv –vcodec mpeg4 output.avi
```
- 切割ts分片
```sh
ffmpeg -i input.mp4 -c:v libx264 -c:a aac -strict -2 -f hls -hls_list_size 6 -hls_time 5 output1.m3u8
```
### 常用参数说明：
- 主要参数：

```
-i 设定输入流
-f 设定输出格式
-ss 开始时间
```

- 视频参数：
```
-b 设定视频流量，默认为200Kbit/s
-r 设定帧速率，默认为25
-s 设定画面的宽与高
-aspect 设定画面的比例
-vn 不处理视频
-vcodec 设定视频编解码器，未设定时则使用与输入流相同的编解码器
```
- 音频参数：
```
-ar 设定采样率
-ac 设定声音的Channel数
-acodec 设定声音编解码器，未设定时则使用与输入流相同的编解码器
-an 不处理音频
```