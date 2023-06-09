# ros

### 函数详解部分

#### ros::init()

```c++
void ros::init	(	int & 	argc,
					char ** 	argv,
					const std::string & 	name,
					uint32_t 	options = 0 
)		
```

ros::init()的参数分别是：
 argc：remapping参数的个数
 argv：remapping参数的列表
 name：节点名，必须是一个基本名称，不能包含命名空间
 options：[可选]用于启动节点的选项（ros::init_options中的一组位标志）

功能：初始化节点

#### **ros::NodeHandle**

类似于服务类，提供方法

主要函数：
advertise发布函数 返回一个publisher



#### 创建文件流程

```shell
madir sample_1
cd sample
mkdir src
cd src
catkin_init_workspace
catkin_create_pkg std_msgs roscpp rospy
catkin_make
roscrore//运行内核
source devel/setup.bash
rosrun talker talker
```



#### tips和踩过的坑:

##### 找不到ros.h

在includepath里面添加

"/opt/ros/noetic/include/**"



##### 编译完rosrun找不到程序

包里面cmakelist取消catkin_pakeage()和add_executable(${PROJECT_NAME}_node src/chatter.cpp)注释

${PROJECT_NAME}_node 这个字符串为devel文件夹下的可执行程序



##### ！！！编译完运行的还是旧程序

把devel和build里面同名的包全删了，重新编译



##### 运行完python后报错：解释器错误: 没有那个文件或目录

rosrun listener listener.py
/opt/ros/noetic/bin/rosrun: /home/chen/Documents/sample_1/src/listener/scripts/listener.py：/use/bin/python3：解释器错误: 没有那个文件或目录
/opt/ros/noetic/bin/rosrun: 行 150: /home/chen/Documents/sample_1/src/listener/scripts/listener.py: 成功



把对应包中

```cmake
catkin_install_python(PROGRAMS
  scripts/listener.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```

取消注释
