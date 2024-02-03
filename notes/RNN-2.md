如何定义loss呢？

y1会放到一个reference vector 中，去算cross entropy 。
y2（Taipei）会对应reference vector的destination那个slot的距离越近越好的，。
要注意顺序，arrive要先丢进去后，才能丢Taipei，不然就不知道memory里面的值是多少，
output和reference vector的cross entropy的和，就是我们要关注的对象。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031831592.png)

定义了loss function后，如何去learning？

Backpropagation through time（BPTT），需要考虑时间信息，rnn也是用gradient descent来训练。



rnn的训练比较困难。希望资料越多，loss值越低，但是现实不是如此。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031618479.png)

total loss 对参数的变化很剧烈、很崎岖，意思就是说这个error surface有一些地方非常平坦，有些地方非常陡峭。极端状况下，我在悬崖边，悬崖边的gradient descent很大，但是由于先前遇到的gradient descent很小，设置的learning rate比较大，这个时候显然很大的learning rate是不好的，（参数直接update很多是不好的），所以怎么办呢？？？

Clipping：当gradient 大于某个值的时候，就将gradient设置为那个临界值，当gradient 大于14的时候，就让gradient等于14。

但是为什么rnn有这个特性，error surface有些地方那么得平滑，用一个很简单的方法来：将input来进行微调，直观地来看对输出有多大的影响，就可以测出gradient的变化有多少。下面这个例子来看，只有一层的hidden layer，memory会一直给到下一个时间点的输入，第一个输入是1，其他999个输入都是0，那么y1000等于多少？w^999；
如果说w是我们要训练出来的参数，那如果改变一下w，看看我们的最终结果会受什么影响，w=1.01，y^1000≈20000，有很大的gradient，我们只要把learning rate设置得小一些就好了，同理，w=0.99,和w=0.01,都是y^1000≈0。也就是说在w=1.01处，gradient很大，w在0.01到0.99的gradient都很小，这个时候要设置一个比较大的learning rate。
所以说rnn出现问题，（error surface的崎岖），是因为在时间点的转换过程中，把同样的参数重复使用很多次，w只要有变化，可能没有任何影响，但是一旦有影响，就会有很大的影响，所以说rnn不好训练是因为有high sequence，同样的位置在不同的时间点反复被使用。

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031631978.png)

那如何解决rnn出现的问题？

广泛被使用的是LSTM，可以让error surface不会那么崎岖，可以把特别平坦的地方拿掉，可以解决gradient vanishing的问题，~~但是无法解决gradient discord的问题~~，有些地方仍然是很崎岖的，

为什么LSTM可以做到handle gradient manager的工作呢？可以避免让gradient特别小呢？

gradient vanishing可以被处理，在simple rnn里面，每个时间点的memory的讯息都会被完全覆盖，但是在LSTM中，memory的信息是会乘上一个值再加上input，存储到cell中去的，也就是说在LSTM中，一旦对memory造成影响，这个影响会永远留，除非洗掉。

LSTM有很多的应用：

先前的例子中，input和output的个数是一样的。
many to one:`input：a vecotr sequence`
`output：only one vector`
判断一段话是正向的，还是负向的。
many to many(输入比输出长)：
	语音辨识，Trimming：把重复的东西拿掉，缺点 是没有办法辨别叠词，
	`CTC：connectionist temporal classification`，在output的时候，还要加一个空符号，就可以解决叠字的问题了，

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031703568.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031706309.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031707951.png)

穷举，所有可能性，

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031710234.png)

只要把空白的地方拿掉，不需要告诉哪些字符组成了一个单词，这个是可以通过训练得到的，

Many to Many(sequence to sequence)
不知道输入输出哪个更长。
下面说的是一个字符一个字符进行输出，上一个字符作为下一个hidden layer 的输入，不知道什么时候停止输出字符，应当加一个讯号“===”作为停止输出的提示。
将声音讯号直接转成其他语言，不做语音辨识，收集一些中文的语音讯号，和对应的英文翻译，然后训练。（方言语音辨识很难做，所以这个很有用）

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031715852.png)

![](https://raw.githubusercontent.com/feiguang414/blogImage/main/image/202402031716892.png)

