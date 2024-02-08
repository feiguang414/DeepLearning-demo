# word embedding

1-of-N encoding

word class：只能分成多个class，无法看到各个class之间的关系，或者说一个东西在不同的class中存在的方式。

clustering

word embedding：只知道输入，不知道输出如何，
	词汇的含义可以看context上下文信息。如何利用上下文，如何处理上下文信息？count based 和 predication based。
	预测下一个出现的word是什么，做prediction的时候。连接到 吗



![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402081504155.png)

如何让 `Wi` 和 `Wj` 的权重一样，首先，需要让二者的初始值是一样的。在训练的过程中，为了让两个权重一样，在update到不同的权重的时候，就要同时减去对方减去的gradient descent，两个权重永远都是一样的。
$$
w _ { i } \leftarrow  w _ { i } - n \frac { \partial C } { \partial w _ { i } } - \eta \frac { \partial C } { \partial w _ { j } } \\ w _ { j } \leftarrow  w _ { j } - n \frac { \partial C } { \partial w _ { j } } - \eta \frac { \partial C } { \partial w _ { i } }
$$
CBOW：拿前后两个词去predicate中间的词汇

Skip-gram：拿一个词去predicate前后的词汇

如果两个东西类似，把二者的word vector相减，会很接近它相近的东西。这是凭借上下文来train出来的。



Multi-domain Embedding：可以做影像辨识。阅读大量文章后，了解到了一些知识。

Beyond Bag of Word：词汇一样， 语义不同。