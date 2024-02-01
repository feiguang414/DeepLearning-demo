# pytorch学习

transform的结构

```python
from torchvision import transforms
from PIL import Image
from torch.utils.tensorboard import SummaryWriter

img_path = "relative address/"
img_path_abd = "D:\\..."
img = Image.open(img_path)
#print(img)

writer = SummaryWriter("logs")

// 使用transforms里面的工具
tensor_trans = transforms.ToTensor()	#工具实例化
tensor_img = tensor_trans(img)
#print(tensor_img)

writer.add_image("Tensor_img",tensor_img)
writer.close()
#在terminal中：tensorboard --logdir=logs

```

> 评论弹幕：
>
> 1. 这边的确是讲错了，object是默认的父类，现在可以不用写，属于默认继承；这明明是继承了object类，下面的__call__方法才是转换方法
> 2. alt+enter导入包
> 3. open报错的把import改成这个from PIL import Image
> 4. 相对路径不行的可以试试./dataset/.......
> 5. 大家有遇到这种报错吗 UserWarning: Failed to load image Python extension: Could not find module 
> 6. 装好了torch就有torchvision了
> 7. tensor_img = transforms.ToTensor()(img)也可以
> 8. 直接调用类原来就是运行call；__call__魔术方法，把实例好的对象当作方法（函数），直接加括号就可以调用

1. transforms.py 工具箱
2. 特定格式的图片进行转换成需要的格式，也就是图片的格式转换。
3. tensor数据类型
   通过 transforms.ToTensor去看两个问题，一是transforms应该如何使用，二是Tensor数据类型有什么独特性（优势）。
   tensor包含了有关_backward_hooks,__grad等类型，很常用。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402011247587.png)

```
In: pip insgtall opencv-python
In: 使用tensor处理的图片的步骤
In: import cv2
In: cv_img = cv2.imread(img_path)

In:
```



常见的 transforms

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402011503079.png)

关注输入和输出。

```python
from PIL import Image
from torchversion import transforms
img = Image.open("")
#print(img)



#/test/CallTest.py
class Persion:
    def __call__(self,name):
        print("__call__"+"Hello " + name)
    
    def hello(self,name):
        print("hello"+name)

preson = Person()
person("zhangsan")
person.hello("lisi")
```

ToTensor的使用

```python
from PIL import Image
from torchversion import transforms

writer = SummaryWriter("logs")
img = Image.open("relative address")
print(img)

# ToTensor的使用
trans_totensor = transform.ToTensor()
img_tensor = trans_totensor(img)
writer.add_image("ToTensor",img_tensor)
#writer.close()

# ToPILImage的使用
# Normalize的使用
# output[channel] = (input[channel] - mean[channel]) / std[channel]
print(img_tensor[0][0][0])
trans_norm = transform.Normalize([0.5,0.5,0.5],[0.5,0.5,0.5])
#trans_norm = transform.Normalize([1,3,4],[3,2,1])
img_norm = trans_norm(img_tensor)
print(img_norm[0][0][0])
writer.add_image("Normalize",img_norm)

# Resize
print(img.size)
trans_resize = transforms.Resize((512,512))
img_resize = trans_resize(img) #需要PIL类型的image
#img_resize PIL -> totensor -> img_resize tensor
img_resize = trans_totensor(img_resize)
writer.add_image("Resize",)
print(img_resize)

# Compose()的使用 --- resize
trans_resize_2 = transform.Resize(512)
# PIL --resize--> PIL ---totensor---> tensor
trans_compose = transform.Compose([trans_resize_2,trans_totensor])
# z
img_resize_2 = trans_compose(img)	#PIL类型的image
writer.add_image("Resize",img_resize_2,1)

writer.close()
```

tensorboard --logdir=logs

> 弹幕评论：
>
> 1. 这个公式开头应该是output[channel]，新版本已更正
> 2. 我的版本没看到call。但是有forward函数可以实现。2.1.1版本
> 3. 这里有些问题，图像像素每个通道原本是-255到255的，toTensor自动进行了归一化，单纯的Norm无法保证都在0和1之间的
> 4. 这个是标准化   ，归一化是0-255弄成0-1，
> 5. PyCharm小技巧设置：忽略大小写，进行提示匹配，setting-general-代码补全
> 6. 是类对象，只要调用类对象自动执行魔术方法，这里的魔术方法返回一个值而已
> 7. 报错的 注意512 512是两个括号
> 8. compose需要你提供一个转换的列表；相当于一个合并的功能，按照写的列表去执行流程，比如：compose -- resize -- totensor
> 9. 俺的理解是：Compose()就是把两个参数功能整合，这里第一个参数是改变图像大小，第二个参数是转换类型。