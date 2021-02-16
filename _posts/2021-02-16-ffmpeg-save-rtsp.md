---
layout: post
title: ffmpeg save RTSP stream to local file
subtitle: 
categories: ffmpeg, linux
tags: [ffmpeg]
---

## 背景
`ffmpeg`经常被用来处理视频，现在想要将rtsp视频流保存成本地mp4文件，并且需要保存一定的时间段后自动结束

## 分割视频
```sh
ffmpeg -rtsp_transport tcp -i "rtsp://userName:password@ip:port/h264/ch33/main/av_stream" -f segment -segment_time 120 -segment_format mp4 -t 10 -reset_timestamps 1 -strftime 1 -b:v 1600k -vcodec copy -r 60 -y test-%Y%m%d-%H%M%S.mp4
```
重要参数说明：      
`-f segment`： 将视频分段     
`-segment_time 120`： 每段时长为120s       
`-segment_format mp4`： 分段格式为mp4     
`-t 10`： 拉流持续时间为10s       
`-strftime 1`： 格式化时间，保存的文件名字使用

**注意：**      
由于设置的每段时长大于拉流的持续时间，因此在持续时间到后会自动退出程序。      
如果拉流持续时间大于每段时长，那么当到达时长后会自动再按照当前时间生成一个视频文件来继续保存数据。      
如果不使用`-t`参数，那么程序会一直拉流，并且每隔一个`-segment_time`时间保存一个视频文件。     

## 分割图片
1. 按帧计数命名：       
```sh
ffmpeg -i video_file.mp4 -qscale:v 2 output_dir/video_file-%07d.jpg
```

## 图片合成视频
```sh

```


