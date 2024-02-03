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
	输入的很像，人想表达的意思不一样，neural network接受到的输入是一样的，怎么样才能得到和人一样的输出动作？下面的例子是机票购买的例子。

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401270856264.png" style="zoom:67%;" />



<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401270910365.png" style="zoom:67%;" />

必须要给memory一个初始值（在没有放进去任何东西的时候）。这里的neural network假设所有的weight都是1，bias都是0，activation function都是linear。

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401270926172.png" style="zoom:67%;" />

如果交换输入的顺序，得到的output会是不一样的。

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401270939651.png" style="zoom:67%;" />

同一个neural network，在三个不同的时间点，被启用了三次。这也就叫做

hidden layer 写做 
$$
a^{1}
$$
这个hidden layer每层都是一排neuron 的output，所以a1是一个vector，根据这个hidden layer我们产生y1，这个y1就是`arrive`属于哪个slot的机率，把上一个时间点hidden layer的outputa1丢到memory里面（蓝色盒子），再把第二个单词`Taipei`丢到x2中，a1和x2一起丢到hidden layer a2中，得到y2（y2是 `Taipei`属于哪个slot的机率）。注意当前输入会考虑到上一个时间点的信息。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402030955273.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402030956878.png)



> 评论：
>
> 1. 为啥是隐藏层a1作为输入而不是真正的输出y1？
> 2. 既在下一层使用，又当本层输出

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401270956101.png" style="zoom:67%;" />

可以是多层的，每次调用的是同样的一批neuron。那么在下一个时间点的时候呢，每一个hidden layer会把上一个时间点的各个hidden layer 相应读取到这一个时间点的hidden layer，

> - Elman网络通常更适合于那些需要捕捉输入序列内部状态信息的任务，例如语言模型，其中下一个单词的预测依赖于前面单词的语义和语法结构。
> - Jordan网络可能在那些输出的序列本身具有结构的任务中表现得更好，例如在生成音乐或者一些控制任务中，其中系统的下一个输出部分地取决于之前的输出。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031002311.png)

传说，Jordan network得到的performance更好，左边的hidden layer是隐藏层，但是y有输出信息，就可以比较清楚我们放到memory里面的是什么东西。
作为下一个时间点的输入，需要乘上一个weight Wh。

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401271017965.png" style="zoom:67%;" />

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401271018627.png" style="zoom:67%;" />



同时训练正向和逆向的neural network，这个Bidirectional RNN的好处是在产生output的时候，看的范围是比较广的，可以看完整个句子。假如是slot filling的话，network等于是看了整个sentence以后，才决定每一个词汇的slot应该是什么。

上面讲的只是一个最simple的rnn；每个新的时间点，neural network都会把memory洗掉，所以只记得前一个时间点的事情。

------

下面开始讲LSTM（Long Short-term Memory）

可以随时把值存到memory中去，也可以随时把值读出来。常用的memory称为LSTM，比较复杂，有三个gate，想要写入，得通过input gate，如果被关着，无法被写入，这是靠neural network自己学习来决定在什么时间点关闭input gate，什么时间点打开input gate。同理，output gate可以决定外界是否可以读出output的内容；forget gate决定什么时间忘记memory cell的值，什么时间保留这个值。
有四个input，只有一个output。
short-term 的memory，只是比较long的short-term memory

<img src="https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202401271023532.png" style="zoom:67%;" />



1. 操控input gate 的是一个scalar zi，是一个数值。操控forget gate的是一个scalar zf，操控output gate的是一个scalar zo。
2. input的内容Z 进入到activation function g(z)中；zi通过input gate 经过activation function f(zi)，
3. 通常activation function f是一个sigmoid function ，因为得到的值是介于0和1之间的，这个0~1的值代表了这个gate被打开的程度，如果f的结果是1，代表input gate是打开状态，
4. g(z)*f(zi)
5. zf也通过sigmoid function得到f(zf)
6. 把memory里的值c和f(zf)相乘，即c*f(zf)
7. `c' = g(z)*f(zi) + c*f(zf)`, c' 是存到memory中的新值。
8. forget gate是1的时候，代表记得memory cell里的值；forget gate是0的时候代表忘记。
9. c' 通过activations function h 得到h(c')
10. `a = f(zo)*h(c')`

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031051438.png)

------

LSTM计算的具体实例：
假设我们的neural network里面只有一个LSTM的cell，我们的input是三维的vector，output是1维的。
这个三维的input vector和output有什么关系，和memory里面的值有什么关系？——假设x2==1，x1被存到memory中去；x2=-1，memory被reset；x3=1的时候，output被打开，才能看到输出。
注意这个说到的是x2的作用是能够改变下一个时间点memory，而不是这个时间点的memory。![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031059343.png)

LSTM有四个input scalar，这四个怎么来的？那个三维的vector乘上一个linear 的transform以后，得到的四个scalar，（也就是x1,x2,x3分别乘上weight，加起来后，再加上一个bias，就得到input；这些weight、bias参数是通过training data，gradient descent学到的），下面的例子是假设说，我已经知道这些值是多少了，现在用输入进行模拟，看看输出是怎样的。
forget gate 的bias 是正值，平时都是打开的状态，只有给x2一个比较大的负值的时候，才会关闭forget gate。
output gate 的bias是负值，平时是关闭的状态，只有给x3一个比较大的正值的时候，才会打开output gate。



![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031103440.png)

1. 对于先前的simple rnn的neural network来说：
   会把每个input乘上不同的weight作为neuron的输入，每个neuron都是一个function，输入一个scalar，输出一个scalar，
2. 对于LSTM：
   将LSTM的memory cell想成一个neuron就好了，也就是将先前的简单neuron换成一个memory cell，可以有两个neuron，不会只有两个neuron，比如说有1000个neuron，也就是1000个LSTM的memory cell。
3. 在原来的traditional rnn中，一个neuron是一个input和一个output，但在LSTM中，需要四个input，并且各不相同。所以说，当两种类型的neural network的neuron个数相同时候，LSTM需要的参数量是simple rnn的4倍，容易产生over fitting

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031503821.png)

先前只需要黄色和蓝色两种weight，注意不同的线代表不同的权重，乘上不同的权重就得到了不同的vector。

有一排neuron（LSTM），cell里面的值排起来是一个vector，C^(t-1)，丢到cell里面的值，只是每一个vector的一个dimension，
在时间点t，input一个vector `xt`，这个vector `xt`会先乘上一个linear的transform，乘上一个matrix，变成另外一个vector `z`，`z`的第一维度会被送入到第一个LTSM的cell里面，第二维度的值会被送到第二个cell里面，`xt`分别乘上不同的transform来得到不同的向量，放入到不同的输入。
所有的cell可以共同一起被计算，如何？
	`Zi`经过activation function后（得到的是一个数）和 `Z` 相乘，`Zf`  经过activation function 后和cell里面的值 `C^(t-1)` 相乘，然后把cell现在里面存的值和输入端的值相加，output 是将刚刚的那个vector经过activation function后再和output gate经过activation function后的值相乘，得到`yt`。![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031548388.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031549393.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031549408.png)

但这个不是LSTM的最终形态。

1. 真正的LSTM会将hidden layer的输出ht-1作为下一个时间点的input，也就是说下一个时间点操控gate的值，不是只看那个时间点的input x，还看前一个时间点的output h

2. 还会加一个属性peephone：就是memory cell里面的值也拉过来。

3. LSTM通常不是只有一层，一般是五六层叠起来的样子，样子如下，很复杂：![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031555135.png)

   这个是forget gate，input gate，output gate的计算公式：
   $$
   f _ { t } = ( W _ { f } \left[ h _ { t - 1 } , x _ { t } \right] + b _ { f } ) 
   $$

   $$
   i _ { t } = ( W _ { i } \left[ h _ { t - 1 } , x _ { t } \right] + b _ { i } )
   $$

   $$
   o _ { t } = ( W _ { o } \left[ h _ { t - 1 } , x _ { t } \right] + b _ { o } )
   $$

   ![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031820489.png)

4. 现在的rnn，指的就是用LSTM

5. > 在长短期记忆网络（LSTM）的上下文中，通常会提到多个不同的输出：
   >
   > 1. **隐藏状态（( h_t ) 或 ( a_t )）**：这是LSTM单元在时间步( t )的内部状态，它编码了到目前为止观察到的信息。这个状态通过时间传递，并且可以用于LSTM单元的下一个时间步。
   > 2. **单元状态（( c_t )）**：这是LSTM的另一种内部状态，与隐藏状态并行存在。它也被称为细胞状态，它负责长期信息的维护，同时与隐藏状态一起决定下一个时间步的隐藏状态和单元状态。
   > 3. **输出（( y_t )）**：这是LSTM单元或整个LSTM网络在时间步( t )的实际输出。这个输出通常是经过隐藏状态处理的，通过一个或多个全连接层和激活函数来得到，它可以是一个预测值、概率分布或任何其他形式的输出。
   >
   > 在LSTM处理序列数据的过程中，隐藏状态( a_{t} )在每个时间步被更新，并且传递到下一个时间步作为LSTM单元的一部分输入。这是因为LSTM的设计目的是为了在序列的每一步捕获和保持信息，以利用序列中的时间依赖性。隐藏状态( a_{t} )承载了之前时间步的信息，这对于预测序列中下一个时间步的事件至关重要。
   >
   > 隐藏状态( a_{t} )通常会作为下一个LSTM单元的输入（除了当前单元的新输入( x_{t+1} )），因为它包含了序列到当前步骤为止的重要上下文信息。而真正的输出( y_t )是基于隐藏状态计算得到的，输出( y_t )通常对应于序列中的某种目标或预测，它可能不包含足够的信息去直接影响下一个时间步的预测。因此，将隐藏状态( a_{t} )而不是输出( y_{t} )作为下一个时间步的输入，能够更好地保持和利用序列的内部状态信息。

Zf这个vector经过sigmoid function，Zi经过sigmoid function的值和Z相乘，

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031536567.png)



![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031511338.png)

这四个vector的维度和cell的个数是相同的，这四个vector合起来就会去操控这些memory的运作。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031539919.png)







------

​	**Long Short-term Memory (LSTM)：**memory是如何工作的，它处于什么位置，和哪些数据和部件进行打交道。输入闸门，存入的数据内容，输出闸门。
​	Multiple-layer LSTM：

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





