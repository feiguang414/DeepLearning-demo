# Graph Neural Network

 ![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401301049497.png)

graph 上节点类型，位置安排，安排的顺序都会影响到性质。
如何将graph放入到neural network，保留graph的性质。
GNN可以做什么事情？
	分析某个分子的性质，比如会不会导致基因突变。classfication
	生产新药，生产出成本低的药，生成出有效的药。不同的需要，设置不同的要求。
如何将一个问题抽象成GNN来处理？
	比如寻找凶手，角色直接的关系比较复杂，

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401301109257.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401301121169.png)



**Tasks,Dataset,and Benchmark**

如何测试不同的model 的表现性能，

**Spatial-based GNN**

Aggregate：用neighbor feature update下一层的 hidden state，大部分时候，还会用自己的信息。
Readout：最后一层的feature 集合起来代表整个graph

**NN4G** (Neural Networks for Graph)

Hidden layer 0 --> Hidden layer 1

