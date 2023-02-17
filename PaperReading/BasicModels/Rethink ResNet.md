# Rethink ResNet

<center>2015. CVPR 2016 Best Paper Honorable Mention</center>

![](https://raw.githubusercontent.com/BigbigShark/Reading-23/master/paperReading/Classifications/imgs/ResNet_Img_Title.png)

## 发现问题Problems

- 当网络变深时，无论是在训练时，还是在测试时，模型表现都会表现得更差，即所谓训练不动了，退化。Degradation problem, differed from overfitting. 

- 补充：收敛是有好坏之分的，单纯的收敛没有意义，要看收敛在什么地方；如果收敛在不好的地方，一般就说训练不动了。

  ![](https://raw.githubusercontent.com/BigbigShark/Reading-23/master/paperReading/Classifications/imgs/ResNet_Img_Problems.png)

## 分析问题Analysis

- 对于一个已经学好的浅层网络，如果我们往上面加一些层，那么该模型不应该变得更差，因为新加的层，是可以学成一个恒等映射的。Identity mapping.
- 理论上新加的层可以学出恒等映射，但实际上做不到。Current solvers (e.g. SGD) on hand are unable to find solutions to learn an identity mapping.

## 解决问题Solutions

![](https://raw.githubusercontent.com/BigbigShark/Reading-23/master/paperReading/Classifications/imgs/ResNet_Img_Solutions.png)

- 人为添加恒等映射。**Shortcut connections** (Highway connections). Explicitly construct identity mappings, which ensures deeper models will perform no worse than shallower models.

  - 假设上一层的输出是$x$，原来要学的是$H(x)$，但是，既然$x$已经学到了，那么下一层就不用再学$x$了，直接让下一层学一个残差$F(x) = H(x) - x$，而$x$直接通过捷径（即恒等映射）直接到下一层的输出处和$F(x)$加起来。
  - 相当于某层的输出不再是自己的输出，而是自己的输出再加上自己的输入。

- 统一输入和输出的通道。Unify the channels between input and output.

  - Zero padding
  - 投影（论文选择）。**Projections** using $1 \times 1$ convolutions.
  - 所有捷径都做个投影（即使输入输出形状一样，依旧通过1x1卷积做一个投影）（效果最好，但贵）

- Deeper than 50 layers: **Bottleneck** (to save computations)

  ![](https://raw.githubusercontent.com/BigbigShark/Reading-23/master/paperReading/Classifications/imgs/ResNet_Img_Solutions_Bottleneck.png)

## 贡献Contributions

![image-20230217151627809](https://raw.githubusercontent.com/BigbigShark/Reading-23/master/paperReading/Classifications/imgs/ResNet_Img_Countributions)

- 解决了网络特别深时，梯度爆炸或者梯度消失的问题。It solves the exploding/vanishing gradients as the model goes deeper.
- 收敛得更快。Converge faster.

![](https://raw.githubusercontent.com/BigbigShark/Reading-23/master/paperReading/Classifications/imgs/ResNet_Img_Gradients.png)

- 使得后续研究可以更加关注网络架构，而不受深度束缚。Allow following researches pay more attention to model structure design, not bound by depth.

## Why does the plain model can't learn identity mapping and we should add shortcut connections by hand

- The intrinsic character attributes of the design.
- Actually the shortcut connections cut down the complexity of the model, which ensures the model can learn a simpler and better mapping from origin learning space.

> For example, transformer or VIT is so large, but they don't overfit on the dataset easily.