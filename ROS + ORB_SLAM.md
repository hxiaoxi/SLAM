# ROS + ORB_SLAM

目标：在ROS平台下实时运行SLAM。

环境：ubuntu16.04 + ROS kinetic

![](https://github.com/hxiaoxi/SLAM/blob/master/image/1.png)

### 一、ROS安装

详见（<http://wiki.ros.org/kinetic/Installation/Ubuntu>）

 

### 二、ROS入门

ROS的基本概念有node、topic、publisher、subscriber，基本原理就是节点（node）发布（publish）信息到一个话题（topic），其他节点就可以通过订阅（subscribe）该话题来获取信息，实现信息实时通信。

每次使用ROS，需要同时启动多个终端，首先需要运行roscore，roscore是ROS的主节点（master），然后在其他终端运行其他节点。

 

开始使用ROS，首先创建一个工作空间，比如这样

```
$ mkdir -p ~/catkin_ws/src
$ cd ~/catkin_ws
$ catkin_make
$ source devel/setup.bash
```

catkin_ws为工作空间名字，可以选择其他你喜欢的名字。catkin_make后会自动生成build、devel、src文件夹。

 

每次启动新的终端需要运行source devel/setup.bash，可以用下面的命令一劳永逸。

```
$ echo "source ~/catkin_ws/devel/setup.bash" >> ~/.bashrc
$ source ~/.bashrc
```



最后，推荐官网的wiki，做的非常好，还是中文版的（http://wiki.ros.org/cn/ROS/Tutorials）



### 三、ORB_SLAM2编译

下载ORB_SLAM2文件夹放在catkin_ws/src下，开始编译（确保已经安装好依赖）

如果你是使用虚拟机，并且对虚拟机性能不够自信，打开build.sh和build_ros.sh，将make -j修改为make，make -j会使用多线程编译，容易导致虚拟机卡死。

```
$ cd ~/catkin_ws/src/ORB_SLAM2/
$ chmod +x build.sh
$ ./build.sh
$ chmod +x build_ros.sh
$ ./build_ros.sh
```

如果遇到编译错误，可以在ORB_SLAM2的github网站的issue里搜索问题，大部分坑都已经有人填了（<https://github.com/raulmur/ORB_SLAM2/issues>）

记录几个常见错误

1. 问题：fatal error: Eigen/Core: No such file or directory

   解决：sudo ln -s /usr/include/eigen3/Eigen /usr/local/include/Eigen

   详见（https://github.com/raulmur/ORB_SLAM2/issues/403）


2. 问题：error adding symbols: DSO missing from command line

   解决：修改Example/ROS/ORB-SLAM2/CMakelist.txt，+表示在原有内容上增加。

   ```
   +find_package(Boost COMPONENTS system)
   
   include_directories(
   +${Boost_INCLUDE_DIRS}
   )
   
   set(LIBS
   +${Boost_LIBRARIES}
   )
   ```

   详见（https://github.com/raulmur/ORB_SLAM2/issues/552）

 

编译成功了，就可以运行ORB_SLAM2节点（需要提前运行roscore）

```
rosrun ORB_SLAM2 Mono ~/catkin_ws/src/ORB_SLAM2/Vocabulary/ORBvoc.txt ~/catkin_ws/src/ORB_SLAM2/Examples/ROS/ORB_SLAM2/xxx.yaml
```

Mono后需要跟两个参数，第一个是ORB vocabulary，第二个xxx.yaml需要填写你自己的相机参数文件。

 

运行后截图：

![](file:///C:/Users/hxx13/AppData/Local/Temp/msohtmlclip1/01/clip_image001.png)

此时因为还没有视频输入，左边的黑框提示“waiting for images”，右边的白框是SLAM构建的轨迹图。



在新的终端运行rosnode list和rostopic list和rostopic info /camera/image_raw

![](file:///C:/Users/hxx13/AppData/Local/Temp/msohtmlclip1/01/clip_image002.png)

可以看到当前运行的节点和话题，ORB_SLAM2订阅了/camera/image_raw，但是这个话题还没有发布者，即没有视频来源，接下来就需要在ROS上用节点发布视频到/camera/image_raw上。



### 四、发布视频

使用官方提供的video_stream_opencv来发布视频。

从github上下载（<https://github.com/ros-drivers/video_stream_opencv>）到~/catkin_ws/src中

video_stream提供了多种方法发布视频，可以通过摄像头、网络流（rtsp、http）、本地视频。

文件中的/scripts/test_video_resource.py是用来快速测试视频源是否有效的，根据视频来源选择下面中一条命令。

```
./test_video_resource.py 0
./test_video_resource.py rtsp://xxx.xxx
./test_video_resource.py /home/youruser/myvideo.mkv
```



video_stream编译：在catkin_ws目录下，终端输入，一般不会有什么错误。

```
$ catkin_make –pkg video_stream_opencv
```

video_stream提供了各种方法的launch文件，用来便捷地启动节点，以rtsp视频源为例，打开/launch/rtsp_stream.launch，修改参数video_stream_provider的值，改为你的rtsp源，比如rtsp://192.168.137.1:8554，然后修改camera_name的值，默认是rtsp，这样运行时video_stream会发布/rtsp/image_raw的话题，为了和ORB_SLAM2订阅的话题对应，将rtsp修改为camera。

 

在终端运行roslaunch video_stream_opencv rtsp_stream.launch，就开始实时重建了。

![](file:///C:/Users/hxx13/AppData/Local/Temp/msohtmlclip1/01/clip_image003.png)

 

 

再运行rostopic info /camera/image_raw，结果和预期一样，发布者和订阅者对应上了。

![](file:///C:/Users/hxx13/AppData/Local/Temp/msohtmlclip1/01/clip_image004.png)

 

参考资料：

（1）<http://wiki.ros.org/ROS/Tutorials>

（2）<http://wiki.ros.org/video_stream_opencv>

（3）<https://blog.csdn.net/lixujie666/article/details/80475451>

（4）<https://www.cnblogs.com/newneul/p/8364924.html>

 

 
