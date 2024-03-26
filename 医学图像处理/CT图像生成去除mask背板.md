



# CT图像去除背板

CT（计算机断层扫描）图像是一种先进的医学成像技术，通过使用计算机处理X射线数据来生成人体内部的横断面图像。这些图像可以提供关于人体内部结构的详细信息，特别是骨骼、血管和软组织的状况。

由于CT图像对于密度更高的物体识别效果更好，这也就导致了在扫描的过程中，可能会有一部分仪器被扫描进入图像当中，这些图像在图像处理的时候往往是不需要的，我们可以在生成mask的时候将背板删除。

![一张CT影像图，可以看到在其中两个视图中都有很明显的背板](https://lsky.atoposlibra.top/i/2024/03/25/66015ab85c5fd.png)



获得去除背板的mask的步骤如下：

1. 二值化将CT图转换为二值图
2. 开运算：断开狭小的连接，避免背板和人体连接在一起
3. 保留最大联通区域
4. 填补连通区域内的空洞



由于该任务是一个CPU密集型任务，python单核的特性不适合直接进行计算，可以使用进程池的方法，同时使用多个CPU核进行图像处理，提高计算速度。



在使用程序在自己数据集时需要修改的地方：

1. 我个人使用的数据集是NRRD格式的，所以调用了python中nrrd包进行图像的读取，并获取到numpy类型的数据，如果你使用的数据集不是NRRD格式的，你需要修改`process_subdir(arg)`中的读取图像和存储图像的方法。

2. 在主函数中两个需要修改的地方，`file_paths`是用于将参数传入子进程的list，其中每一项保存了CT文件路径、序号、存储文件夹和参数，其中重点需要修改CT文件的路径。

   ```python
       # 1.需要修改的地方，获取参数list file_paths,其中每项有两个内容，文件路径和序号
       dataroot = '/media/chen/98C07D728CE2DA37/data/HaN-Seg/set_1/CT'
       file_paths = [(os.path.join(dataroot, subdir), count+1 ,save_path, args) for count, subdir in enumerate(sorted(os.listdir(dataroot)))
                       if not os.path.isdir(os.path.join(dataroot, subdir))]
   
       # 2.设置核心数量，默认为全核
       with mp.Pool(processes=mp.cpu_count()-2) as pool:
           pool.map(process_subdir, file_paths)
   ```

3. 代码中提供了三个可以从命令行中传入的参数：文件存储路径、是否绘制效果图、是否保存去除背板后的CT文件；生成的效果图如下所示，展示了获取mask的四个步骤效果：

    ![生成mask的步骤效果图](https://lsky.atoposlibra.top/i/2024/03/25/66015a9ebbd10.png)



源代码如下：

```python
import os
import numpy as np
import matplotlib as mpl
from matplotlib import pyplot as plt
from scipy import ndimage
import re
import glob
from PIL import Image
from skimage.morphology import label
from collections import OrderedDict
import skimage
import argparse
import nrrd
import multiprocessing as mp
import traceback

# 求一张二值图片的最大连通分量
def mcc(mask01):
    # 定义连通分量有序字典
    region_volume = OrderedDict()
    # 获取各连通分量map和个数
    mask_map, numregions = label(mask01 == 1, return_num=True)
    # 连通分量个数
    region_volume['num_region'] = numregions
    # 总点个数，容量
    total_volume = 0
    # 最大的区域容量
    max_region = 0
    # 最大区域的flag
    max_region_flag = 0
    # 枚举每个连通分量
    for l in range(1, numregions + 1):
        # 第l个连通分量的容量
        region_volume[l] = np.sum(mask_map == l)  # * volume_per_volume
        # 如果大于最大容量，则该连通分量为最大连通分量
        if region_volume[l] > max_region:
            max_region = region_volume[l]
            max_region_flag = l
        # 计算总容量
        total_volume += region_volume[l]
        # 最初的mask初始化
    maskmcc = mask01.copy()
    # 保留最大连通分量的点
    maskmcc[mask_map != max_region_flag] = 0
    maskmcc[mask_map == max_region_flag] = 1
    return maskmcc


# 去除主干之外的杂讯
def get_mask(a, patientid, j):
    mpl.use('TkAgg')  # !IMPORTANT 更改在这里！！！！！！！！！
    ct_np = a
    # 1二值化，生成mask
    mask = np.zeros_like(ct_np).astype(np.uint8)
    mask[ct_np > (-300 + 1000)] = 1
    if mask.max() == 0:
        return mask

    plt.subplot(221)
    plt.imshow(mask * 255, cmap='gray')

    # 2开运算 使狭窄的连接断开和消除细毛刺 消亮点亮条
    kernel = skimage.morphology.disk(1)
    mask = skimage.morphology.opening(mask, kernel)

    plt.subplot(222)
    plt.imshow(mask * 255, cmap='gray')

    # 3最大连通分量
    mask = mcc(mask)
    plt.subplot(223)
    plt.imshow(mask * 255, cmap='gray')

    # 4闭运算 弥合狭窄的间断， 填充小的孔洞 消暗点暗条
    kernel = skimage.morphology.disk(15)  # 圆核
    mask = skimage.morphology.closing(mask, kernel)

    contours = skimage.measure.find_contours(mask, 0.8)

    for n, contour in enumerate(contours):
        contour_coords = contour.round().astype(int)
        rr, cc = skimage.draw.polygon(contour_coords[:, 0], contour_coords[:, 1])
        mask[rr, cc] = 1

    plt.subplot(224)
    plt.imshow(mask * 255, cmap='gray')

    if not os.path.exists(f'./visual/mask/{patientid}'):
        os.makedirs(f'./visual/mask/{patientid}')
    if j % 10 == 0:
        plt.savefig(f'./visual/mask/{patientid}/{patientid}_{j}.jpg')
    plt.close()
    return mask


# 删除空白slice并获取mask
def remove_empty_slices_and_bed(rsct, patientid):
    l = rsct.shape[0]
    ctnpy = []
    masknpy = []
    for j in range(l):
        if rsct[j].max() != 0:
            mask = get_mask(rsct[j], patientid, j)
            rsct[j] = mask * rsct[j]
            ctnpy.append(rsct[j])
            masknpy.append(mask)
        else:
            ctnpy.append(rsct[j])
            masknpy.append(rsct[j])

    ctnpy = np.array(ctnpy).astype(np.int16)
    masknpy = np.array(masknpy).astype(np.int16)
    
    return ctnpy, masknpy

# 画slices图
def plot_slices(ctnpy, patient_id, type):
    if not os.path.exists(f'./visual/plot/{type}/{patient_id}/'):
        os.makedirs(f'./visual/plot/{type}/{patient_id}/')
    for j, k in enumerate(ctnpy):
        if j % 10 == 0:
            plt.imshow(k, cmap=plt.cm.bone)
            plt.savefig(f'./visual/plot/{type}/{patient_id}/{patient_id}_{j}.jpg')
            plt.close()


def process_subdir(arg):
    import os
    import nrrd
    plt.rcParams['font.sans-serif'] = ['SimHei']
    mpl.rcParams['axes.unicode_minus'] = False
    file_path, counter, save_path, args = arg
    try:

        # 获取文件名
        file_name = os.path.basename(file_path)
        # 获取文件路径
        base_name, ext = os.path.splitext(file_name)
        # 在文件名后添加'remove_bed'文件名
        new_file_name = f"{base_name}_remove_bed{ext}"
        mask_name = f"{base_name}_mask{ext}"
        # 获取存储路径
        if save_path is None:
            save_file_path = os.path.join(os.path.dirname(file_path), new_file_name)
            save_mask_path = os.path.join(os.path.dirname(file_path), mask_name)
        else:
            save_file_path = os.path.join(save_path, new_file_name)
            save_mask_path = os.path.join(save_path, mask_name)
        os.makedirs(os.path.dirname(save_file_path),exist_ok=True)    
        if not os.path.exists(save_mask_path) and os.path.exists(file_path):
            print("source:",file_path)
            nrrd_data, nrrd_options = nrrd.read(file_path)
            nrrd_array = np.asarray(nrrd_data)
            nrrd_array = np.swapaxes(nrrd_array, 0, 2)
            nrrd_array += 1000
            # 删除背板
            ctnpy, masknpy = remove_empty_slices_and_bed(nrrd_array, counter)
            ctnpy = ctnpy - 1000
            if args.draw:
                plot_slices(ctnpy, counter, 'CT')
            ctnpy = np.swapaxes(ctnpy, 0, 2)
            masknpy = np.swapaxes(masknpy, 0, 2)
            
            if args.save_origin:
                print("save file:", save_file_path)
                nrrd.write(save_file_path, ctnpy, nrrd_options)
            print("save mask:", save_mask_path)
            nrrd.write(save_mask_path, masknpy,nrrd_options)
        
    except Exception as e:
        # 捕获所有异常，并将异常信息写入到文件中
        with open("error_log.txt", "a") as file:
            file.write(f"Error in process {mp.current_process().name}: {e}\n")
            file.write(traceback.format_exc())  # 这将写入完整的堆栈跟踪信息

if __name__ == "__main__":

    parser = argparse.ArgumentParser()
    parser.add_argument("-d", "--draw", type=bool, default=False,help="是否绘制可视化图")
    parser.add_argument("--save_position", type=str, default=None,help="文件保存路径，默认为ct所在文件夹")
    parser.add_argument("--save_origin", type=bool, default=False,help="是否保存去除背板后的原始图像")
    args = parser.parse_args()
    save_path = args.save_position
    
    # 1.需要修改的地方，获取参数list file_paths,其中每项有两个内容，文件路径和序号
    dataroot = '/media/chen/98C07D728CE2DA37/CodeFile/MR-CT_Synthesis_and_Segmentation/data/HaN-Seg/set_1/CT'
    file_paths = [(os.path.join(dataroot, subdir), count+1 ,save_path, args) for count, subdir in enumerate(sorted(os.listdir(dataroot)))
                    if not os.path.isdir(os.path.join(dataroot, subdir))]

    # 2.设置核心数量，默认为全核
    with mp.Pool(processes=mp.cpu_count()-2) as pool:
        pool.map(process_subdir, file_paths)
```



