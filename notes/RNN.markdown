# RNN

用来处理那些输入量不确定的问题

**大纲：**
	首先举了一个例子：Slot Filling，智慧客服，智慧订票系统，一个人讲了他的诉求，系统应当给出他反馈，这个问题也可以用一个Feedforward的Neural Network来解决，就是神经网络，有输入，有输出（即反馈）。那么，Slot Filling和有反馈机制的神经网络有什么差别，Slot Filling 在解决这个方面有什么优势呢？

**Slot Filling:**

> The goal of Slot Filling is to identify from a running dialog different slots,which correspond to different parameters of the user's query.For instance,when a user queries for nearby restaurants,key slots for location and preferred food are required for a dialog system to retrieve the appropriate information.Thus, the main callenge in the slot-filling task is to extract the target entity.

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401251827471.png" style="zoom: 50%;" />

从这个图片看出，slot定义了它需要的数据：Destination，time of arrival，也就是说，除了这两类数据，其他的word都不会被记录到slot里面。

**有记忆的neural network 就叫做 Recurrent Neural Network**

**1-of-N Encoding :**
	这是用来将一个word表示成一个vector的方法，也就作为neural network的输入。

**Beyond 1-of-N Encoding:**
	Dimension for "Other"：未被识别的word归类到这个类型去。
	Word hashing：不会出现某个词汇不在词典中的问题，相当于将各个单词拆开了都。

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401270854282.png" style="zoom: 67%;" />



又出现一个问题：
	输入的很像，人想表达的意思不一样，neural network接受到的输入是一样的，怎么样才能得到和人一样的输出动作？

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401270856264.png" style="zoom:67%;" />



<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401270910365.png" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401270926172.png" style="zoom:67%;" />

如果交换输入的顺序，得到的output会是不一样的。

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401270939651.png" style="zoom:67%;" />

一个neural network，被启用了三次。

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401270956101.png" style="zoom:67%;" />

可以是多层的

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401271017965.png" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401271018627.png" style="zoom:67%;" />



同时训练正向和反向的neural network。

终于到了Recurrent Neural Network（有记忆的neural network）：
	似乎借助了中间存储的memory，那也是说能分辨input才能去装入不同的memory啊，这和上面的将“leave”和“arrive”两个动作都划归为“other”是矛盾的。
	Elman Network:这个我们不知道存到memory中的内容是什么，但是也是有效的。
	Jordan Network:将中间的output作为input存储到memory中，这个时候我们是能够掌握存到memory中的内容是什么的。
	**Bidirectional RNN：**双向读取，那得到的数据就有了两个，和先前的处理图片得到更多的数据有相似之处。
	**Long Short-term Memory (LSTM)：**memory是如何工作的，它处于什么位置，和哪些数据和部件进行打交道。输入闸门，存入的数据内容，输出闸门。
	Multiple-layer LSTM：

在Keras 可以支持LSTM，直接调用即可。Keras还支持GRU，SimpleRNN，这是三种RNN。

LSTM是一个复杂的东西。

需要的参数量比较少，比较能够避免over fitting，

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401271023532.png" style="zoom:67%;" />

四个input，一个output，Short-term Memory只记得上一个节点的信息，Long Short-term Memory 

Connectionist Temporal Classfication (CTC)：解决叠字问题。
	CTC如何做训练，

将声音讯号和要转换到的语言翻译，跳过去语音辨识的工作。

Beyond Sequence

Sequence-to-sequence Auto-encoder:



RNN v.s. Structured Learning



每个cell的四个input 是x通过乘不同的vector得到的



<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401281014571.png" style="zoom:67%;" />



在非常小的区域，得到的gredent 天差地别，这是RNN的问题。RNN的问题不是因为activate function，而是因为有在多次被使用  。

解决方法：

为什么把RNN换成LSTM，
	不是说RNN用的就是LSTM的方法吗？
	因为LSTM can deal with gradient vanishing(not gradient explode)的问题。会留下之前的记忆，影响一但产生，影响会一直留存，除非Forget Gate被关闭、Cell内容被清洗掉。并且在实际应用的时候，会给到一个比较大的bias，尽量不要产生关闭Forget Gate的情况。

GRU只有两个Gate，所以需要的参数量比较小，simpler than LSTM，降低over fitting的概率，旧的不去，新的不来，input gate和forget gate联动，清洗了旧值，才能放入新值。

Many to Many (output is shorter)
	语音辨识，许多个input的vector对应一个文字。
	CTC：Connectionist Temporal Classfication，output一个分隔符号。虽然不知道某个问题对应某些vector，但是可以穷尽所有的排列组合，有巧妙的演算法来解决。

Many to Many (no limitation)
	英文翻译成中文，中文翻译成中文，通过a symbol"==="（断）来结束，这个是有效的，



得到树状的结构，
词汇一模一样，顺序不同，得到的态度可能截然相反。
Sequence-to-sequence Auto-encoder适合用于表达文法
想要得到语义，应该

比如把逗变成vector，water vector，做语音的搜寻，不用做语音辨识，直接扫描所有的声音讯号来做比对，

**Attention-based Model 也用到了memory**

会自动忽略掉无关的事情，会有很大的记忆容量database，中央处理器丢到信息到





