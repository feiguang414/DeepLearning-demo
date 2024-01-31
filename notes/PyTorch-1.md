# PyTorch学习

两个重要函数

package (pytorch) 有许多工具箱，探索这个工具箱，有两个工具：
	dir()：打开工具，看见工具箱某个分区的结构
	help()：如何使用这个工具

```
IN[1]: import torch
IN[2]: torch.cuda.is_available()
Out[3]: True
IN[4]: dir(torch)
IN[5]: dir(torch.cuda)
IN[6]: dir(torch.cuda.is_available)
//输出的有双下划线表示不能被更改，是一个具体的函数了
```



```
conda activate torch
jupyter notebook

```

选择相应的环境，

## pytorch如何加载数据？

Dataset：可以用于提取数据，和它的真实的label，就能得到某一个具体的数据。
Dataloader：为网络提供不同的数据形式。
	如何获取每一个数据及其label。
	统计总共有多少个数据。

组织形式：训练集，测试集，label信息；也可以将label直接放置在图片的名称上。

```
from torch.utils.data import Dataset
help(Dataset)
Dataset??	//表达更清晰
```

代码实战

```python
from torch.utils.data import Dataset
import PIL import Image
import os //获取所有图片的地址
//import cv2
// conda install opencv-python; pip install opencv-python

class MyData(Dataset):
	def __init__(self,root_dir,label_dir):	//提供全局变量
        self.root_dir = root_dir	//创建的时候进行赋值，就可以成为全局变量
        self.label_dir = label_dir
        self.path = os.path.join(self.root_dir,self.label_dir)
        self.img_path = os.listdir(self.path)
        //函数中的变量无法传递到其他的函数中去，self实际上就是指向类自身首地址的指针，点成员实际就是通过偏移找到相应成员
	
	def __getitem__(self,idx):	//使用idx来获取图片，首先需要获取文件夹，然后才能得到文件夹下面所有的图片
        img_name = self.img_path[idx]
        img_item_path = os.path.join(self.root_dir,self.label_dir,img_name)
        img = Image.open(img_item_path)
        label = self.label_dir
        return img,label
   
	def __len__(self):
        return len(self.img_path)

    
root_dir = "dataset/train"
ants_label_dir = "ants"
bees_label_dir = "bees"
ants_dataset = MyData(root_dir,ants_label_dir)
bees_dataset = MyData(root_dir,bees_label_dir)

train_dataset = ants_dataset + bees_dataset
```

input是照片蚂蚁，label是`ants`

```python
In: from PIL import Image
In: img_path="copy relative path" //注意路径在windows里面\需要加转义字符\\
// /不需要转义，\需要转义为\\
In: img = Image.open(img_path)
In: img.size
In: img.show()
In: dir_path = "dataset/train/ants"
In: img_path_list = os.listdir(dir_path)

In: import os
In: root_dir = "relative address"
In: label_dir = "ants"
In: path = os.path.join(root_dir,label_dir)	//拼接地址

In: img_path = os.listdir(path)

In: ants_dataset[0]
In: img, label = ants_dataset[0]
In: img.show()
In: img, label = ants_dataset[1]
In: img.show()

In: len(ants_dataset)
In: len(bees_dataset)
In: len(tarin_dataset)

In: img,label = train_dataset[124]

```

> ---ants_image
> ------0013035.jpg
> ---ants_label
> -------0013035.txt（注意，命名和image中保持一致）txt里面存的是对应图片的一些label信息
> ---bees_image
>  】- --bees_label



评论：出现报错可能是setuptools 版本过高，需要降级，用这个指令就可以解决：pip install setuptools==59.5.0

## TensorBoard的使用（一）

1. TensorBoard的安装
2. add_scalar()的使用（常用来绘制train/val loss）

TensorBoard在训练时候很有用，不同阶段是如何输出的，

> 评论：取名千万别带test，会报错empty suite的
> 没装包的同学试试conda install -c conda-forge tensorboard 

```python
from torch.utils.tensorboard import SummaryWriter

#创建实例
writer = SummaryWriter("logs")
writer.add_image()
writer.add_scalar()	# add scalar data to summary

# y = x
for i in range(100):
    writer.add_scalar("y = x", i ,i);
writer.close();
# y = 2x
for i in range(100):
    writer.add_scalar("y = x", 2*i ,i);
writer.close();
```

> SummaryWriter:
>
> Writes entries directly to event files in the log_dir to be
> consumed by TensorBoard.

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401311937220.png)

```python
    def add_scalar(
        self,
        tag,
        scalar_value,	#y轴
        global_step=None,	#x轴
        walltime=None,
        new_style=False,
        double_precision=False,
    ):       
        """Add scalar data to summary.

        Args:
            tag (string): Data identifier标签，表头
            scalar_value (float or string/blobname): Value to save
            global_step (int): Global step value to record
           
        Examples::

            from torch.utils.tensorboard import SummaryWriter
            writer = SummaryWriter()
            x = range(100)
            for i in x:
                writer.add_scalar('y=2x', i * 2, i)
            writer.close()

        Expected result:

        .. image:: _static/img/tensorboard/add_scalar.png
           :scale: 50 %

        """
```

TensorBoard的安装

方式1：在terminal中进行设置，`pip install tensorboard`，然后会提示安装成功，安装成功后执行上面代码，会正常出现logs的文件夹。

> 评论：
>
> 1. pip install tensorboard -i https://pypi.tuna.tsinghua.edu.cn/simple
> 2. 修改完记得重启pycharm就成功了
> 3. 还有问题可以试着安装老版本的setuptools
> 4. 还是没moudle的话，pip uninstall setuptools之后再pip install setuptools==58.0.4就好了
> 5. 我这里报错：module 'distutils' has no attribute 'version'
> 6. 如果运行不能生成log文件夹但是在python console中可以生成文件夹的话就把“log”改成“./log”
> 7. 可以打开网址但没有数据的同学尝试把路径改成绝对路径试试
> 8. 如果打开不是（pytorch）的可以下标里面打开command prompt
> 9. 如果terminal不是Pytorch，而是PS的 local + 边上有个下拉箭头，改成command prompt
> 10. 可以右键上层目录，找到open in，然后点击terminal
> 11. 没有坐标的看视频左上角，是SCALARS，默认的是左边的TIME SERIES
> 12. 绝对路径引号千万不要有空格，我试了很多方法结果去掉空格就成功了
> 13. ValueError: Duplicate plugins for name projector怎么回事啊，没有重复安装啊
> 14. 没有logs的同学可以重启一下pycharm就有了
> 15. 把 --logdir = 后面加入绝对路径（注意要将 \ 改为 \\\） 

方式2：新建terminal，输入 `tensorboard --logdir=logs`（logdir=事件文件所在文件夹名）

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401312000084.png)

默认打开6006端口，自行设置端口：`tensorboard --logdir=logs --port=6008`

tag相同就画在了一张表上
	可以删除log文件夹下的文件，
	建议创建新的子文件夹，`SummaryWriter("新文件夹")`。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401312019395.png)



add_image()的使用

```
In: image_path = "图片的相对地址"
In: from PIL import Image
In: img = Image.open(image_path)
In: print(type(img))	#查看图片的类型，注意在add_image()方法中，形参img_tensor规定了图像类型：torch.Tensor,numpy.array,or string/blobname

#使用opencv读取图片，获取numpy类型的图片数据
#去terminal中安装opencv
(torch) pip install opencv-python
In: import numpy as np
In: img_array = np.array(img)
In: print(type(img_array))

```

```python
from torch.utils.tensorboard import SummaryWriter
import numpy as np
from PIL import Image

writer = SummaryWriter("logs")
image_path = "某个图片的相对路径/0013035.jpg"
img_PIL = Image.open(image_path)
img_array = np.array(img_PIL)
print(type(img_array))
print(img_array.shape)	#输出类似（512，768，3），即（H高度，W宽度，C通道数）

#从PIL到numpy，需要在add_image()中指定shape中每个数字/维度表示的含义

writer.add_image("test",img_array,1,dataformats='HWC')
#writer.add_image("train",)
# y = 2x
for i in range(100):
    writer.add_scalar("y=2x",2*i,i)
    
writer.close()
```

然后到网页中进行刷新，IMAGES标签下可以看到图像。<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401312115479.png"  />



```python
    def add_image(self, tag, img_tensor, global_step=None, walltime=None, dataformats='CHW'):
        """Add image data to summary.

        Note that this requires the ``pillow`` package.

        Args:
            tag (string): Data identifier表头
            img_tensor (torch.Tensor, numpy.array, or string/blobname): Image data规定了图像类型
            global_step (int): Global step value to record
           
        Shape:
            img_tensor: Default is :math:`(3, H, W)`. You can use ``torchvision.utils.make_grid()`` to
            convert a batch of tensor into 3xHxW format or call ``add_images`` and let us do the job.
            Tensor with :math:`(1, H, W)`, :math:`(H, W)`, :math:`(H, W, 3)` is also suitable as long as
            corresponding ``dataformats`` argument is passed, e.g. ``CHW``, ``HWC``, ``HW``.

        Examples::

            from torch.utils.tensorboard import SummaryWriter
            import numpy as np
            img = np.zeros((3, 100, 100))
            img[0] = np.arange(0, 10000).reshape(100, 100) / 10000
            img[1] = 1 - np.arange(0, 10000).reshape(100, 100) / 10000

            img_HWC = np.zeros((100, 100, 3))
            img_HWC[:, :, 0] = np.arange(0, 10000).reshape(100, 100) / 10000
            img_HWC[:, :, 1] = 1 - np.arange(0, 10000).reshape(100, 100) / 10000

            writer = SummaryWriter()
            writer.add_image('my_image', img, 0)

            # If you have non-default dimension setting, set the dataformats argument.
            writer.add_image('my_image_HWC', img_HWC, 0, dataformats='HWC')
            writer.close()

        Expected result:

        .. image:: _static/img/tensorboard/add_image.png
           :scale: 50 %

        """
```