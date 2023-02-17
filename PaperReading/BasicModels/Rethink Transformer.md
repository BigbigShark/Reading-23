# Rethink Transformer

<center>2017. NIPS 2017</center>

![](D:\CV_GitHub\Reading-23\PaperReading\BasicModels\imgs\Transformer_Title.png)

## 发现问题Problems

- RNN难以并行
  - 慢
  - 早期时许信息在后期可能被“忘记”（如果要存，内存开销会非常大）
- CNN视野小
  - 可以通过叠层扩大视野，但是会增大开销

## 分析问题Analysis

- 尝试一个完全基于attention机制的encoder-decoder架构
- 模拟CNN多输出通道：Multi-Head Attention

> Auto-regressive: The output is fed back to the model as input.

## 解决问题Solutions

## 贡献Contributions

- significantly more parallelization
