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



#### tips:

##### 找不到ros.h

在includepath里面添加

"/opt/ros/noetic/include/**"

