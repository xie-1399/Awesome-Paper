# Awesome-Paper
The trace of Paper Reading about DSA、GPU、LLM and AI System , remark "***" should be read careful and others are just look through.


## DSA (Domain Deep Learning Accelerator)

##### *** Eyeriss: An Energy-Efficient Reconfigurable Accelerator for Deep Convolutional Neural Networks [link](https://ieeexplore.ieee.org/document/7738524)
非常经典的CNN加速器，整体架构如下图
![image-eyerissV1](https://github.com/xie-1399/Awesome-Paper/blob/main/Pic/BackBone/eyerissV1.png)


##### *** OliVe: Accelerating Large Language Models via Hardware-friendly Outlier-Victim Pair Quantization [link](https://arxiv.org/abs/2304.07493)
量化的软硬件结合设计方法，用于解决Transformer中的离群点问题（即量化加速器设计，可以归结为一类问题），
可见DSA中的LLM quantization and architecture co-design.md [link](https://github.com/xie-1399/Awesome-Paper/blob/main/DSA/LLM%20quantization%20and%20architecture%20co-design.md)，主要介绍大模型训练后量化的一些软硬件结合方法

## GPU 

## LLM
#### 【survey】
##### *** Full Stack Optimization of Transformer Inference: a Survey  [link](https://arxiv.org/abs/2302.14017) 

这是一篇关于大模型推理加速的综述

#### 【hardware】

##### FlexGen: High-Throughput Generative Inference of Large Language Models with a Single GPU

一种可以降低LLM推理资源的方案，可以在16GB GPU中进行较大模型的推理(涉及到computation schedule, tensor placement, and computation delegation的策略)

code repository : [link](https://github.com/FMInference/FlexGen)

##### Rethink Memory And Communication Costs for Efficient Large Language Model Training [link](https://arxiv.org/abs/2310.06003)

提出了一种新的大模型训练时的内存消耗与通信成本的优化方法PaRO，可以提高1.19 ~ 2.50X的训练吞吐量

##### Efficient LLM Inference on CPUs [link](https://arxiv.org/abs/2311.00502)
这篇论文介绍了如何在CPU上进行大模型的部署，英特尔设计的加速方法，支持量化到INT4，并实现了一个高效的开源库，但是看上去加速比其实并不是很高，主要的贡献点如下：

```
（1）We propose an automatic INT4 quantization flow and generate the high-quality INT4 models with negligible accuracy loss within <1% from FP32 baseline.
（2）We design a tensor library that supports general CPU instruction sets and latest instruction sets for deep learning acceleration. With CPU tensor library, we develop an efficient LLM runtime to accelerate the inference.
（3）We apply our inference solution to popular LLM models covering 3B to 20B and demonstrate the promising per-token generation latency from 20ms to 80ms, much faster than the average human reading speed about 200ms per token.
```

code repository : [link](https://github.com/intel/intel-extension-for-transformers)

##### GPT4AIGChip: Towards Next-Generation AI Accelerator Design Automation via Large Language Models [link](https://arxiv.org/abs/2309.10730)

这篇论文研究了使用LLM自动化生成AI加速器的可能性，以此来减少对特定领域硬件专家知识的依赖，并设计实现了一个可以指导模型生成加速器的框架（或者可以说是第一个提出使用大模型生成AI加速器的工作），但是大模型在加速器设计上的先验知识是否准确还需要检验（使用大模型生成硬件可能会是一个趋势）

Extension：上下文学习（In-context learning）A Survey on In-context Learning [link](https://arxiv.org/abs/2301.00234)
