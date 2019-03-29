### YOLOv3笔记

#### Windows下

一种目标检测网络，经历了YOLOv1,v2到v3三个版本，其中v3对前两个版本做了些许改进，在目前来说是一个较为先进的目标实施检测系统。具体的相关解释可以参考<https://blog.csdn.net/leviopku/article/details/82660381>

这里直接提供在windows利用pytorch实现YOLOv3的一种方法（我的TensorFlow出了点问题，被迫使用pytorch，效果一样）

环境要求：

-   win10
- Python3.6.8（python3.6.x都可以，主要是为了支持我的cuda）
-  cuda9.0+cudnn（电脑显卡为940MX）
- pytorch

安装过程

首先安装cuda9.0+cudnn（安装对应版本，我的电脑是940MX，具体请百度），然后pytorch，官网<https://pytorch.org/> 直接选择对应版本

![1](https://github.com/hxiaoxi/SLAM/blob/master/image/1.png)

执行命令

```
pip3 install
https://download.pytorch.org/whl/cu90/torch-1.0.1-cp36-cp36m-win_amd64.whl
pip3install torchvision
```

找到pytorch对应的YOLOv3代码，git下来

<https://github.com/ayooshkathuria/pytorch-yolo-v3>

这上面提供的代码的python文件以及实现了检测多种渠道数据的功能，当然，我们这里只运行检测功能，暂时不拓展训练功能，因此从网上下载已有的权重，并放到与代码文件一个文件目录下。运行。

> python detect.py --images imgs --det det 

其中imgs为读入数据的来源，可以是一张图片，也可以是一个文件夹，同理，det为写入目录或者文件。

> python video_demo.py --video.avi

该程序为读入本机的avi视频数据进行检测,好像只支持avi，因为我转了文件格式，提供一个转视频格式的有用网站:<https://www.onlinevideoconverter.com/media-converter>.

![2](SLAM/image/2.png)

> python cam_demo.py

调用摄像头进行实时目标检测，这里cv2的cv2.Videocapture()函数可以支持多种形式的视频流，包括rtsp数据流。这里将cam_demo.py中“摄像头”设置为rtsp地址，

> cap = cv2.VideoCapture("rtsp://192.168.137.215:8554")

这里我使用手机连接电脑WiFi，效果如下：

![3](SLAM/image/3.png)

#### Linux下

在Linux上运行YOLOv3代码

requirements：

- python3.5
- opencv
- pytorch0.4

针对ubantu16.04自带的python2.7进行升级，执行命令

> sudo apt-get install python3.5

接着修改默认的python版本为3.5

> sudo rm -rf  /usr/bin/python
> sudo ln -s /usr/bin/python3.5  /usr/bin/python

对于新版的python，需要重新安装pip，这里执行命令

> `sudo apt-get install python3-pip ` 

安装好pip3之后，往后所有的关于pip的命令都采用pip3进行。同样进入pytorch管网，选择合适选项

![4](SLAM/image/4.png)

运行

```
pip3 install https://download.pytorch.org/whl/cpu/torch-1.0.1.post2-cp35-cp35m-linux_x86_64.whl
pip3 install torchvision
```

理论上来说这个过程比较平稳，前提是pip使用得当。安装好torch之后，同样把yolo3的代码git clone下来，

> git clone https://github.com/ayooshkathuria/pytorch-yolo-v3

运行相关程序，这时候出现的问题主要是缺少一些包，一一安装即可。
