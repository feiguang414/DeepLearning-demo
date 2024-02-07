# Transformer

transformer 是 一个 Seq2Seq 的model

输入输出的长度不确定、没有绝对的关系，
语音辨识：比如输入一串声音讯号（就是一些vector）--》speech recognition--》语音辨识的结果。由机器自己决定输出几个字，什么时候结束。
机器翻译：从一种语言翻译成另外一种语言
语音翻译：声音讯号到翻译后的结果（有些语言都没有文字）

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051448155.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051456027.png)



语音合成：Text-to-Speech(TTS) Synthesis，给语音，得到翻译后的语音。

很多Natural Language Processing applications:
	Question Answering(QA)，QA可以用seq2seq来解决，但是特制化的model比单用seq2seq要更合适、更好。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051503195.png)

什么叫本来不是用seq2seq来解决的问题，硬用seq2seq也能解决？
	Syntactic Parsing：文法翻译， 

> 评论弹幕：
>
> 1. 组合数学中树结构可以对应序列
> 2. 开个脑洞，那seq2seq能用来做编译器吗？

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051511690.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051540941.png)



seq2seq的组成

encoder：给一排vector，输出一排vector，在transform中用self-attention，
decoder：

batch normalization：

residual connection ：input和output加起来，得到一个输入
layer normalization：不用考虑batch的资讯

fully connected：也用了residual connection，再做一次

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051549039.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051552899.png)



Decoder---Autoregressive（speech recognition as example）

输入一段声音讯号--》encoder--》得到一排vector sequence
---》decoder---》语音辨识的结果

BOS ：begin of sentence，特殊的符号token，每个token都可以用一个one-hat vector来表示，

先从BOS开始，output出来第一个结果（经过 softmax ）后得到“机”，将“机”也作为输入，再得到下一个output“器”，循环往复。也就是说，decoder的这个时间点的输入是上个时间点的输出，那么也就是说decoder的输入可能就是错误的。
这里就会问一个问题：error propagation的问题，一步错，步步错，会出现吗？？？

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051741132.png)

如果把中间盖起来，encoder和decoder基本没有什么差别。
Masked-attention和self-attention的区别：
	self的时候，会考虑所有的资讯；masked的时候，第二个位置的就不会考虑第二个位置以后的资讯。因为decoder的token是一个一个产生的，所以只能考虑左边的资讯，无法考虑右边的资讯，所以是masked的attention。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051745968.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051750123.png)

不会停下来，所以用 `===断` 来结束这个decoder 的输出。可以将开始和结束用同一个符号。在decoder的最后一个输入时候，应该知道语音辨识已经结束了，输出end。



简短讲一下Decoder -- non-autoregressive（NAT）

一次产生一个句子，吃一整排的begin，一次产生一排token。不知道输出多少，那么也不知道输入应该丢进去多少个begin。怎么解决？？👇

1. 单独去预测output的长度，classification
2. 直接给句子上限长度的begin个数，在输出end的右边就当作没有输出。
3. NAT平行化，速度快，效率高，不管句子长度多寡，一个步骤完成任务。
4. 在有了self-attention后，NAT变得热门。
5. NAT优点：比较能够控制它的输出长度。
6. 语音合成可以用seq2seq来做，最知名的是 `Tacotron` 模型（AT decoder），另外还有一个模型 `FastSpeech`（NAT decoder）。
7. 像NAT的decoder，因为有一个专门预测长度的classify，所以如果像说话慢一倍，那么可以让output长度变成先前的两倍，输出是语音。
8. NAT不如AT 的decoder。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051811811.png)



Encoder - Decoder 是如何传递资讯的？（就是刚才遮起来的那里）

cross attention，encoder提供两个箭头，decoder提供一个箭头。
接下来讲一下这个板块实际运作的过程。

1. encoder 输入一排向量，输出一排向量，a1，a2，a3。
2. decoder首先输入一个special token BEGIN。
3. BEGIN进入self-attention，这个attention是有做Mask的（Mask是不会去看这个当前输入的右边的资讯的。
4. 输入一个vector，输出一个vector，然后乘上一个矩阵，做一个transform，得到一个query `q`，
5. 从encoder得到的一排向量a1，a2，a3得到k1，k2，k3。
6. q和k1，k2，k3进行计算attention的分数得到 α1，α2，α3。
7. 可以对 α1，α2，α3 做一个`softmax`进行归一化，所以在下面的图片中 α1，α2，α3加上了`点号’`，成为 `α’1，α‘2，α’3`。
8. 从a1，a2，a3得到了v1，v2，v3。
9. v1，v2，v3和  `α’1，α‘2，α’3`进行相乘，再weight sum加权得到 v。
10. 把v丢到Fully connected。
11. q来自decoder，k和v来自encoder，这个过程叫做`Cross attention`。decoder 凭借着产生一个q，去decoder那边抽取资讯当作接下来decoder的FC的input。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051812162.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051812988.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051850588.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051851184.png)



每一层的decoder都要看最后一层的encoder的输出，为什么是这样，能不能有更多连接方式的可能性。

> 弹幕评论：
>
> 1. 我记下来了，等我要水论文的时候就去做排列组合研究

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402051903061.png)



Training

inference 就是 testing
接下来讲怎么做训练。

每次在产生一个中文字的时候，其实就是做了一层classification，得到了一个`softmax` 后的结果。