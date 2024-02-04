# seq2seq

sequence-to-sequence model，用来解决sequential data，有两个基本的组成部分：encoder，decoder。用来解决machine translation systems。在seq2seq之前，用的是 `phrase-based statistical machine translation(SMT)`，SMT从名字就可以看出来，这是基于短语的，自然无法处理长句子中的上下文关系。

encoder是处理input sequence，然后把它transform成一个特定大小的hidden representation；decoder用的是hidden representation去生成output。可以处理输入输出长度不确定的情况。

优点：“Attention is all you need！”，这个model的main idea是在处理language-related tasks时候，attention layers 和不同的encoder、decoder stacks 表现得十分有效。

编码器和解码器：

![*Encoder and Decoder Stack in seq2seq model*](https://media.geeksforgeeks.org/wp-content/uploads/seq2seq.png)

​				*Encoder and Decoder Stack in seq2seq model*