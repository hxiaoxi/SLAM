#  Ubuntu ROS使用kinetic教程

## 先安装Kinetic驱动

包含三个部分，彼此要求版本适配，这里找到一个repo有提供[Kinetic driver](https://github.com/ZXWBOT/kinect_driver.git)

分别进入三个文件夹运行install文件即可，若有其他问题，该repo也有提示。

在尝试以下命令，若成功显示画面则表示安装成功。

```bash
cd ~/kinect/OpenNI-Bin-Dev-Linux-x64-v1.5.7.10/Samples/Bin/x64-Release
./NiViewer 
```

## ROS使用Kinetic

官网推荐教材只需要两条命令

```bash
# To install only openni_camera:

sudo apt-get install ros-<rosdistro>-openni-camera

# It's also recommended to install openni_launch:

sudo apt-get install ros-<rosdistro>-openni-launch
```

其中rosdistro为你的ros版本,这样就算完成了，快捷迅速方便hhhh