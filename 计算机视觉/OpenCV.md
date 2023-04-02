## OpenCV

### 入门

#### 读取图像

```c++
cv::imread();//第一个参数是名字或路径，第二个参数是读取图像的方式
```

- cv.IMREAD_COLOR： 加载彩色图像。任何图像的透明度都会被忽视。它是默认标志。
- cv.IMREAD_GRAYSCALE：以灰度模式加载图像
- cv.IMREAD_UNCHANGED：加载图像，包括alpha通道

可以用1，0，-1代替

如果未读取也不会报错



#### 显示图像

```c++
cv::imshow(); //显示窗口，自适应尺寸
```

第一个参数是窗口名称，它是一个字符串。第二个参数是我们的对象。

```c++
cv::imshow('image'，img）;
cv::waitKey(0);//输入0无线等待下一个敲击
cv::destroyAllWindows();//销毁所有窗口
cv::destroywindows();//输入字符串销毁指定窗口
          
```



写入图像

```c++
cv::imwrite('messigray.png'，img);//第一个参数是文件名，第二个是保存的图像
```



### Mat 类

保存图像

#### cols rows

#### `Boolean empty`

判断是否为空



#### split

区分通道

```c++
CV::findContours();

函数原型：
findContours(InputOutputArray image,  
	 // 输入图像，一般为二值图或者经过Canny()等处理过的图像
			  OutputArrayOfArrays contours,
	 // 轮廓集合，类型为：vector<vector<Point>>
              OutputArray hierarchy,
     // 继承关系，类型为：vector<Vec4i>，一个四维向量
              int mode,
     // 定义轮廓的检索模式
     // RETR_EXTERNAL 只检测最外围轮廓，包含在外围轮廓内的内围轮廓被忽略
     // RETR_LIST 检测所有的轮廓，但是轮廓不建立等级关系
     // RETR_CCOMP 检测所有的轮廓，但只建立两个等级关系，外围为顶层，若外围内的内围轮廓还包含了其他的轮廓信息，则内围内的所有轮廓均归属于顶层
     // RETR_TREE， 检测所有轮廓，所有轮廓建立一个等级树结构。
     		  int method, 
     // 定义轮廓的近似方法
     // CHAIN_APPROX_NONE 保存物体边界上所有连续的轮廓点到contours向量内
     // CHAIN_APPROX_SIMPLE 仅保存轮廓的拐点信息
     // CHAIN_APPROX_TC89_L1
     // CV_CHAIN_APPROX_TC89_KCOS使用teh-Chinl chain 近似算法
              Point offset=Point()
     // 偏移量
              )
```



#### ret

