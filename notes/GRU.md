# GRU

## 看文章

1. GRU 全称是 Gated Recurrent Unit，是一种简化的LSTM，GRU可以解决sequential data，比如text、speech、time-series data（时间点）。

2. GRU背后的机制是使用gating机制来选择更新hidden state。GRU控制的是information的整个流向，包括information流入，information流出。

3. GRU有两个gating mechanism：reset gate和update gate

4. GRU的计算和结果依赖于updated hidden state。

5. 计算公式
   $$
   Reset\ gate:\ r_{t} = sigmoid(W_{r}*[h_{t-1},x_{t}])
   $$
   
   $$
   Update\ gate:\ z_{t} = sigmoid(W_{z}*[h_{t-1},x_{t}])
   $$

   $$
   Candidate\ hidden\ state:\ h_{t'}=tanh(W_{h}*[r_{t}*h_{t-1},x_{t}])
   $$

   $$
   W_{r},W_{z},and\ W_{h}都是learnable\ weight\ matrices,
   x_{t}是每个时间点的输入，h_{t-1}是前一个时间点的hidden state，h_{t}是当前时间点的hidden state。
   $$

6. LSTM vs GRU
   GRU有三个gate，LSTM有四个gate。
   GRU不含 internal cell state，information存储在hidden state