# PyDicom

pydicom 是一个纯 Python 包，用于处理 DICOM 文件，例如医学图像、报告和放射治疗对象。

pydicom 可以轻松地将这些复杂的文件读取为自然的 python 结构，以便于操作。修改后的数据集可以再次写入 DICOM 格式文件。

pydicom 不是 DICOM 服务器，并且主要不是用于查看图像。它旨在让您使用 Python 代码操作 DICOM 文件中的数据元素。



## 使用指南

### pydicom 中的核心元素

#### Dataset

 Dataset 是我们主要操作的类，包含一个字典，key是 DICOM 的 tag，值是对应的 DataElement 

> 数据集的迭代器生成 DataElement 实例，例如字典的值而不是通常通过迭代字典生成的键。

创造一个Dataset：

```python
import pydicom
from pydicom.data import get_testdata_file
# get some test data
filename = get_testdata_file("rtplan.dcm")
ds = pydicom.dcmread(filename)
```

显示整个数据集：

```python
>>> ds 
Dataset.file_meta -------------------------------
(0002, 0000) File Meta Information Group Length  UL: 156
(0002, 0001) File Meta Information Version       OB: b'\x00\x01'
-------------------------------------------------
(0008, 0012) Instance Creation Date              DA: '20030903'
(0008, 0013) Instance Creation Time              TM: '150031'
...
```

访问和修改数据：

```python
>>> ds.PatientName
'Last^First^mid^pre'
>>> ds[0x10,0x10].value
'Last^First^mid^pre'
>>> ds.PatientID = "12345"
>>> ds.SeriesNumber = 5
>>> ds[0x10,0x10].value = 'Test'
```

直接使用标签会返回键值对，使用 value 得到值



DataElement 实例中可以包含下列任意一个元素：

- 常规数字、字符串或文本值，如 int、float、str、bytes 等
- 常规值列表（例如 3-D 坐标）
- Sequence 实例，其中 Sequence 是 Dataset 实例的列表



使用 [_dicom_dict.py](https://github.com/pydicom/pydicom/blob/main/src/pydicom/_dicom_dict.py) 查看所有 tag 名称



#### 常用方法

```python
>>> ds.dir("pat")
['PatientBirthDate', 'PatientID', 'PatientName', 'PatientSetupSequence', 'PatientSex']
# Dataset.dir() 将返回数据集中任何在关键字中任意位置具有指定字符串的非私有元素关键字
>>> "PatientName" in ds  # or (0x0010,0x0010) in ds
True
# 判断是否有关键字
>>> del ds.SoftwareVersions  # or del ds[0x0018, 0x1020]
# 删除关键字
```



#### DataElement

DataElement 类通常不直接在用户代码中使用，但由 Dataset 广泛使用。 DataElement 是一个简单的对象，它存储以下内容：

- tag – 元素的标签。
- VR – 元素的值表示 – 两个字母的 str，描述存储值的格式。
- VM – 元素的值多重性（整数）。这是根据值的内容自动确定的。
- value – 元素的实际值。常规值，例如数字或字符串（如果 VM > 1，则为它们的列表）或序列。



### 编写 DICOM 文件

#### 介绍

最常见的是读取现有的 DICOM 文件，更改一些项目然后再存储

#### Codify

pydicom 有一个工具 `codify` ，其可以使用一个已存在的 DICOM 文件，然后生成代码，这个代码可以生产 DICOM 文件

> 注意：这个生产文件也会包含 DICOM 文件中的敏感信息，比如病人敏感信息等

`codify` 不会创造对大项目创造代码，比如`pixel data`

#### 从头开始写 DICOM

可以参考以下文件：https://github.com/pydicom/pydicom/blob/main/examples/input_output/plot_write_dicom.py

> 注意：如果要从头开始编写文件，注意一定要填写自己UID，实现名称等



### 操作像素数据

许多 DICOM SOP 类包含大量像素数据，通常用于表示一个或多个图像帧。在这些 SOP 类中，像素数据通常包含在 (7FE0,0010) 像素数据元素中。唯一的例外是参数化地图存储，它可能包含 (7FE0,0008) 浮点像素数据或 (7FE0,0009) 双浮点像素数据元素中的数据。



#### `Dataset.pixel_array`

Dataset.pixel_array 返回包含像素数据的 numpy.ndarray



#### 颜色空间

当使用像素数据的 Pixel_array 时，每个像素的 (0028,0002) 样本值为 3，则返回的像素数据将位于 (0028,0004) 光度解释给出的颜色空间中（例如 RGB、YBR_FULL、YBR_FULL_422 等） 。

pydicom 通过 Convert_color_space() 函数在 8 位/通道 YBR 和 RGB 颜色空间之间进行转换的能力有限。更改颜色空间时，您还应该更改光度解释的值以进行匹配。



#### 调色板

一些 DICOM 数据集将其输出图像像素值存储在查找表 (LUT) 中，其中像素数据中的值是相应 LUT 条目的索引。当数据集的 (0028,0004) 光度解释值为 PALETTE COLOR 时，可以使用 apply_color_lut() 函数将调色板颜色 LUT 应用于像素数据以生成 RGB 图像。