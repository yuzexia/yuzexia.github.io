---
title: shell命令实现批量精准剪切视频
catalog: true
date: 2018-12-20 11:22:14
subtitle:
header-img:
tags: [ffmpeg, 视频剪辑, shell]
---

# shell命令截取视频

[参考链接1:用 ffmpeg 实现批量剪切视频](https://blog.csdn.net/miao9999/article/details/79189534)

[参考链接2:ffmpeg视频精准剪切](https://blog.csdn.net/matrix_laboratory/article/details/53157383)


```shell
#!/bin/bash

# $1 为输入的文件名
# $2 为需要截取的长度（秒）

# 开始时间
startTime=0
# 结束时间
endTime=0
# 获取视频长度
duration=$(ffmpeg -i $1 2>&1 | grep "Duration" | cut -d " " -f 4 | sed s/,//)
# 00:02:20
array=(${duration//:/ })
# 视频长度
length=`expr ${array[0]} \* 3600 + ${array[1]} \* 60 + ${array[2]%%.*}`
echo $length
i=0
str=$1
dir=${str%%.*}
interval=15
ls -al $1

mkdir $dir
if [ -n "$2" ] ; then
    interval=$2
fi
while [ $endTime -le $length ]; do
    #statements
    i=$[$i+1]
    endTime=$[$startTime+$interval]
    ffmpeg -ss $startTime -t $interval -accurate_seek -i $1 -acodec copy -vcodec copy -avoid_negative_ts 1 $dir/$i.mp4
    startTime=$[endTime]

done
```