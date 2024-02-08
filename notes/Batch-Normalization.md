# Batch Normalization

optimization（优化问题），这是为了更好地找到一个Loss值很低的model，也就是一个不错的模型。

假设有两个参数，对loss的斜率变化不一样，横向坐标看来，对Loss的影响比较小；纵向坐标看来，对Loss的影响值比较大一些。在这种情况下不好找到更小的Loss值怎么办？

1. 需要adapted learning rate，eda 等进阶方法来得到比较好结果。
2. 直接把难做的error surface给改掉，改图。
   如果给一个small的input，就乘上一个比较大的权重；如果是一个large的input，就乘上一个比较小的权重。目的是要得到一个在不同维度上train，error surface能有类似的变化范围。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402081543110.png)



Feature Normalization
	让每个的dimension的平均值是0，方差是1，也就是一个维度里面的值都是在0上下。

得到 Feature Normalization 的结果后，将结果丢到neural network中，

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402081557868.png)

有个large network，来计算一大把数据，得到这些数据的平均值和方差，所以需要一个比较大的batch，来做batch normalization。