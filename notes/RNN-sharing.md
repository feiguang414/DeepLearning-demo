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