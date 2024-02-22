# pytorch踩坑记录

## 一、安装opencv-python

问题：ERROR: Failed building wheel for opencv-python Failed to build opencv-python

解决方法：

1. 完全退出clash

2. 设定opencv-python的版本

3. ```python
   pip install -i https://pypi.douban.com/simple/ pip install opencv-python==4.3.0.38
   ```

参考博客：[python安装opencv报错ERROR: Could not build wheels for opencv-python, which is required to install pyproj-CSDN博客](https://blog.csdn.net/qq_41821678/article/details/128913990) (超级迅速完成)

## 二、gpu

colab下载d2l的问题

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402150959109.png)

