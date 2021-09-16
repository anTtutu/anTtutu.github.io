---
title: "deepfacelab使用介绍"
date: 2021-03-29T00:29:47+08:00
tags: [ "python", "deepfacelab" ]
description: "deepfacelab使用介绍"
categories: [ "python", "deepfacelab" ]
toc: true
---

## 1、前言
应朋友要求，研究下deepfacelab的环境搭建和使用步骤，记录下中间的问题和过程。

## 2、deepfacelab核心库
```bash
git clone --depth=1 https://github.com/iperov/DeepFaceLab
```
## 3、成熟的deepfacelab安装项目包
win10环境，软件磁力连接（迅雷复制下载）：
```bat
magnet:?xt=urn:btih:3384316e126920d7e1cbd2acb16b9513a57b2645&dn=DeepFaceLab&tr=udp%3a%2f%2ftracker.coppersurfer.tk%3a6969%2fannounce
```

## 4、安装前的准备
1、如果用的步骤3中的成熟项目包只需要依赖python3  
2、如果用的步骤2中的核心库自行搭建的话，需要安装如下清单
```python
numpy
h5py
Keras
opencv-python
tensorflow-gpu
plaidml
plaidml-keras
scikit-image
tqdm
ffmpeg-python

nvidia-CUDA
```

## 5、执行如下步骤
本着快速应用的地步，优先选择步骤3的完整项目包
![](/posts/deepface/menu.png)

目录名|说明
-|-
_internal|完整项目工程包自带的第三方工具
worksapce|工作区
worksapce_iron|测试的钢铁侠和马斯克视频换脸工作区
*.bat|运行脚本
changelog.html|修改说明
步骤.txt|运行脚本的顺序，自增给朋友说明的

### 5.1 在workspace目录下准备好2份视频，分别命名data_dst.mp4、data_src.mp4
name|说明
-|-
data_dst.mp4|需要被换脸的视频
data_src.mp4|可以提取脸的源视频

### 5.2 在workspace下检查工作目录，没有就参考如下截图创建空目录
workspace目录:
![](/posts/deepface/workspace.png)
data_dst目录:
![](/posts/deepface/dst.png)
data_src目录:
![](/posts/deepface/src.png)

## 6、开始工作
脚本中间的具体参数很好理解，这个工程也有完整的中文版，实在不熟悉的可以网上找下翻译好的中文完整工具包下载，这里不做过多说明，只简单告诉步骤
### 6.1、把源视频拆分成图片
执行脚本
```bat
2) extract images from video data_src.bat
```
参考结果：
![](/posts/deepface/srcpic.png)

### 6.2、把目标视频拆分成图片
执行脚本
```bat
3) extract images from video data_dst FULL FPS.bat
```
参考结果：
![](/posts/deepface/dstpic.png)

### 6.3、从源图片中提取人脸，也叫切脸
执行脚本
```bat
4) data_src extract faces extract.bat
```
参数|说明|备注
-|-|-
f|全脸|
wf|带额全脸|
head|连头发一起换|越大越消耗GPU运算

参考结果：
![](/posts/deepface/srcface.png)

### 6.4、从目标图片中提取人脸
执行脚本
```bat
5) data_dst extract faces extract.bat
```
参考结果：
![](/posts/deepface/dstface.png)

### 6.5、训练模型，耗时，不会自己结束
执行脚本
```bat
6) train Quick96.bat
```
>1、SAEHD模型做出来的视频质量更好，但是要求的配置更高。  
>2、退出后再次点击train Quick96.bat 可以继续训练，进度不会丢失  

如下是几个小时训练的进度，分别是低于1000，1000 ~ 2000，2000 ~ 3000 3个阶段的，建议更多时间训练高于万次或者自己满意为止

900次训练：
![](/posts/deepface/900.png)
1000次训练：
![](/posts/deepface/1000.png)
2000次训练：
![](/posts/deepface/2000.png)
3000次训练：
![](/posts/deepface/3000.png)
最后生成自己训练的模型文件：如下图
![](/posts/deepface/model.png)

### 6.6、图片换脸
执行脚本
```bat
7) merge Quick96.bat
```

### 6.7、把图片合成视频
执行脚本
```bat
8) merged to mp4.bat
```
参考结果：
<video src="/posts/deepface/result.mp4" width="800px" height="250px" controls="controls"></video>