# RNN-sharing

1. slot filling是什么，key slot

2. 什么情况下需要有记忆的neural network？

3. RNN的输入顺序发生改变，为什么会影响输出结果？（input 的sequence‘s order）

4. 在RNN中，hidden layer是怎样的？
   被多次调用，是一样的。注意时间点这个概念。
   
5. RNN的过程，hidden layer，hidden layer可以是多层的。

6. Elman Network 和 Jordan Network（可能better）。

7. Bidirectional RNN可以看完整个句子。

8. LSTM的输入输出和simple RNN有什么区别？

9. 一个LSTM的计算实例。

10. LSTM的最终形态，输入不止一个，有三个。

11. 四个vector的维度和cell的个数是相同的。

12. 如何定义loss值？

13. rnn的训练比较困难，为什么？值被重复使用。

14. Clippling解决error surface的崎岖问题。

15. RNN出现的问题：gradient vanishing梯度消失

16. 为什么LSTM可以做到handle gradient manager的工作呢？可以避免让gradient特别小呢？

17. simple RNN 和 LSTM 的区别是什么？memory存储的时间长度有什么不同？
    为什么说simple RNN 只有很短的记忆short-term，到下一个时间点的时候，就会忘记上个时间点的memory，
    
18. LSTM有很多的应用：多对多（输入比输出长），多对多（sequence to sequence）

19. CTC：就是加了一个空标志。

20. sigmoid和softmax有什么区别？
    Sigmoid函数用于二分类问题的输出层，Softmax函数用于多分类问题的输出层。这两种函数都将输出转化为概率形式，但Softmax保证了多个输出之间的概率总和为1，适合概率分布的表示，而Sigmoid则适用于独立的二分类概率预测。
    
21. RNN碾压LSTM
    初始化为单位矩阵，激活函数用ReLu，这时候RNN更好。
    初始化为随机数的矩阵，LSTM效果更好。
    
22. > 为什么说RNN每次都刷新memory cell的值。
    > 之前我解释说不是这样，memory cell其实是累计了之前输入的影响
    >
    > 进一步澄清：memory cell是累计了之前输入的影响，新的memory cell是考虑新的输入和老的memory cell的值算出来的。但是新算出来的值被完全使用，而没有考虑保留原来老memory cell的值。LSTM就不一样，它也会根据新的输入和老的memory cell的值计算新值，但是原来老memory cell的值也会因为forget gate的作用被部分保留，而不是完全用新值为memory cell赋值

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402082011280.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402082011535.png)

simple RNN是将memory里面的内容放到了input中，作为输入之一，然后进入激活函数。但是LSTM是当前时间点的输入和memory的内容，上个时间的信息一起作为输入，进入激活函数，然后再去考虑保存memory cell里面的内容。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031555135.png)
