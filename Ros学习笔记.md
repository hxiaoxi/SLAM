# Ros学习笔记

## catkin

cmake类库 但更适合复杂的机器系统

### package

包含cmakelist 和xml等多个文件夹

#### 分类

##### cmakelist

编译属性 catkin版本 消息 链接 测试单元等等 具体的需要学习cmake

##### Xml

描述了pkg的属性 自我介绍 依赖

##### 脚本文件

python或shell

##### 头文件

C++的头文件

##### 源文件

c++的源文件

##### 通信格式

消息 动作

##### lanuch 以及配置文件

lanuch文件 一次包含多个可执行文件 

配置文件 .yaml 比如相机标定数据

#### package相关指令

| rospack           | 查找包相关内容 或地址   |
| ----------------- | ------------- |
| roscd             | 跳转到某个pkg的路径下  |
| rosls             | 列出pkg下所有的文件内容 |
| rosed             | 编辑pkg的文件      |
| catkin_create_pkg | 创建一个pkg       |
| rosdep            | 安装某个pkg需要的依赖  |

### Metapacakge

包的集合 一系列

| Name           | Describle         | link          |
| -------------- | ----------------- | ------------- |
| navigation     | 导航                | navigation    |
| moveit         | 运动规划 机械臂          | moveit        |
| image_pipeline | 图像获取 及处理          | image_common  |
| vision_opencv  | ros & opencv 交互处理 | vision_opencv |
| turtulebot     | turlebot机器人相关     | turtlebot     |
| pr2_robot      | pr2机器人驱动相关        | pr2_robot     |
|                |                   |               |

(link 为github搜索关键词)

## Ros 通信架构

官方说法为计算图级

### master

管理node，node需要在master注册 

`roscore`

(同步启动了 master ,rosout:日志输出, parameter server : 参数服务器)

### node

代表一个进程，pkg里可执行文件运行的实例

#### 节点相关

启动node：

`rosrun [pkg_name] [node_name]`

当前运行的node信息：

`rosnode list`

显示某个node的详细信息：

`rosnode info [node_name]`

结束某个node：

rosnode kill [node_name]

启动master和多个node：

`roslauch [pkg_name][file_name.launch]`

launch :启动规则 XML规范

## Ros 通信方式

### Topic

Node间通过publish-subsribe机制通信 异步  多对多

#### Message

topicn内容的数据类型，定义在.msg文件中，可以类比为`class`

#### 相关指令

| CMD                          | Info          |
| ---------------------------- | ------------- |
| rostopic list                | 列出当前topic     |
| rostopic info /topic_name    | 显示某个topic属性信息 |
| rostopci echo /topic_name    | 显示topic的内容    |
| rostopic pub /topic_name ... | 向某个topic发布内容  |
| rosmsg list                  | 列出系统上所有msg    |
| rosmsg show /msg_name        | 显示msg内容       |
|                              |               |

### Service

同步通信方式 (同步意思是 wait for reply)

request-reply方式，需要时才调用 多对一

#### srv

通信格式 定义在 *.srv文件中

#### 相关指令

| rosservice list                   | 列出活跃的service   |
| --------------------------------- | -------------- |
| rosservice info service_name      | 显示service的属性信息 |
| rosservice call service_name args | 调用某个service    |
| rossrv list                       | 列出系统所有srv      |
| ros show srv_name                 | 显示srv内容        |

### Parameter Server

维护字典 包括可用命令行 launch文件 和nodeAPI读写

#### 相关指令

| rosparam list                     | 列出参数    |
| --------------------------------- | ------- |
| rosparam get param_key            | 获取参数    |
| rosparam set param_ke param_value | 设置参数值   |
| rosparam dump  file_name          | 保存参数为文件 |
| rosparam load file_name           | 从文件加载参数 |
| rosparam delete param_key         | 删除参数    |

### Action

Servie-Client 架构 双向通信，api传递

## 常用包

### Gazbo

模拟仿真

### Rviz

可视化工具

### Rqt

基于qt的可视化工具，效果更好

### Rosbag

对软件包的操作集合函数库

### Rosbridge

Ros系统与其他系统的信息传递

### Moveit！

机器人相关包括运动规划，操纵，3D感知，运动导航等

## Roscpp

ros与c++

暂略

## Rospy

ros与python

暂略

## TF

urdf等等 挺重要 但暂略