### Transformer Backbone

------

#### 一  NLP Architecture

![image-20231128161812171](https://github.com/xie-1399/Awesome-Paper/blob/main/Pic/BackBone/Trans1.png)

###### Encoder

（1）Transformer 中单词的输入表示 **x**由**单词 Embedding** 和**位置 Embedding** 相加得到，单词Embedding有很多种方式可以获取，例如可以采用Word2Vec、Glove等算法预训练得到，而位置编码相当于通过某种计算位置的公式给输入编码增加了位置信息

（2）Multi-Head计算注意力矩阵

（3）Add & Norm 层由 Add 和 Norm 两部分组成，Add用于残差连接，**Norm**指 Layer Normalization

（4）Feed Forward 层比较简单，是一个两层的全连接层，第一层的激活函数为 Relu，第二层不使用激活函数，Feed Forward 最终得到的输出矩阵的维度与输入一致

###### Encoder的一个直观例子：

![img](https://pic3.zhimg.com/80/v2-45db05405cb96248aff98ee07a565baa_720w.webp)

###### Decoder

需要注意的是**decoder不是并行的，而是RNN的那种模式，一个一个生产的，有时序概念**，大部分模块和Encoder是一样的，但是在两个计算注意力时不太相同，而且是通过最后的softmax单元预测下一个单词的概率（包含终止符）

第一个输入是【SOS】（即start of sentence）

（1）mask attention ： 通过掩码的方式计算注意力

（2）第二个Multi-Head Attention 变化不大，主要的区别在于其中 Self-Attention 的 **K, V**矩阵不是使用 上一个 Decoder block 的输出计算的，而是使用 **Encoder 的编码信息矩阵 C** 计算的。根据 Encoder 的输出 **C**计算得到 **K, V**，根据上一个 Decoder block 的输出 **Z** 计算 **Q** (如果是第一个 Decoder block 则使用输入矩阵 **X** 进行计算)

###### Decoder的一个直观例子：

![img](https://pic2.zhimg.com/80/v2-5367bd47a2319397317562c0da77e455_720w.webp)

#### 二  Attention

假设输入为X，首先需要得到Q、K、V，**注意 X, Q, K, V 的每一行都表示一个单词**

![img](https://pic3.zhimg.com/80/v2-4f4958704952dcf2c4b652a1cd38f32e_720w.webp)

算出输入句子对应的Q、K、V矩阵后，就可以进行Attention的计算了，即下面的公式，其中Softmax计算每一个单词对于其他单词的attention系数，公式中的 Softmax 是对矩阵的每一行进行 Softmax，即每一行的和都变为1

![img](https://pic2.zhimg.com/v2-9699a37b96c2b62d22b312b5e1863acd_r.jpg)

![img](https://pic4.zhimg.com/80/v2-7ac99bce83713d568d04e6ecfb31463b_720w.webp)

因此最后的输出也是每一行代表一个单词，而结构中其实出现的是Multi-Head Attention，包含多个 Self-Attention 层，首先将输入**X**分别传递到 h 个不同的 Self-Attention 中，计算得到 h 个输出矩阵**Z**

![img](https://pic1.zhimg.com/80/v2-6bdaf739fd6b827b2087b4e151c560f4_720w.webp)

得到这些输出矩阵后，拼接这些矩阵在一起，然后传入一个**Linear**层，得到 Multi-Head Attention 最终的输出**Z**，可以看到 Multi-Head Attention 输出的矩阵**Z**与其输入的矩阵**X**的维度是一样的

![img](https://pic4.zhimg.com/80/v2-35d78d9aa9150ae4babd0ea6aa68d113_720w.webp)

###### Masked Attention

之所以会需要Masked Attention结构，因为在翻译的过程中是顺序翻译的，即翻译完第 i 个单词，才可以翻译第 i+1个单词。通过 Masked 操作可以防止第 i 个单词知道i+1个单词之后的信息

其实计算方式和self attention类似，只是在计算 attention score时，在 Softmax 之前需要使用**Mask**矩阵遮挡住每一个单词之后的信息

![img](https://pic2.zhimg.com/80/v2-35d1c8eae955f6f4b6b3605f7ef00ee1_720w.webp)

![img](https://pic4.zhimg.com/80/v2-58f916c806a6981e296a7a699151af87_720w.webp)

Ref ： [Transformer模型详解（图解最完整版） - 知乎 (zhihu.com)](https://zhuanlan.zhihu.com/p/338817680)

#### 三  Vision Transformer



#### 四  补充算子

##### 1  Normalization

Normalization：规范化或标准化，就是把输入数据X，在输送给神经元之前先对其进行平移和伸缩变换，将X的分布规范化成在固定区间范围的标准分布。Normalization 的作用很明显，把数据拉回标准正态分布，因为神经网络的Block大部分都是矩阵运算，一个向量经过矩阵运算后值会越来越大，为了网络的稳定性，我们需要及时把值拉回正态分布，Normalization根据标准化操作的维度不同可以分为batch Normalization和Layer Normalization。

![img](https://pic2.zhimg.com/80/v2-6d444305489675100aef96293cc3a34d_720w.webp)
