# 201-`AlexNet`

###### 卷积层输出特征图的尺寸

计算卷积层中输出特征图的尺寸缩小情况确实需要考虑卷积核的尺寸（kernel size）和步幅（stride）。

一般情况下，对于一个二维卷积操作，输出特征图的尺寸可以通过以下公式计算：
```python
output_size = (input_size - kernel_size + 2 * padding) / stride + 1
```

其中，input_size是输入特征图的尺寸，kernel_size是卷积核的尺寸，padding是填充的大小，stride是步幅。

对于步幅为1的情况，公式简化为：
```python
output_size = input_size - kernel_size + 1
```

如果你想计算图片缩小到的几分之几，可以使用公式 `(kernel_size - 2 * stride) / kernel_size`。这个公式表示每次卷积操作后，输入特征图尺寸相对于上一层的尺寸缩小了多少。

需要注意的是，这个公式只适用于步幅为1的情况。如果步幅不为1，公式中的 `(kernel_size - 2 * stride)` 部分需要进行相应调整。

希望这个解答对你有所帮助！如果还有其他问题，请随时提问。