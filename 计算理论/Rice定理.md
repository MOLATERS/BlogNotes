---
title: Rice 定理
categories:
- 计算理论
tags: 可计算性
---

<head>
    <script src="https://cdn.mathjax.org/mathjax/latest/MathJax.js?config=TeX-AMS-MML_HTMLorMML" type="text/javascript"></script>
    <script type="text/x-mathjax-config">
        MathJax.Hub.Config({
            tex2jax: {
            skipTags: ['script', 'noscript', 'style', 'textarea', 'pre'],
            inlineMath: [['$','$']]
            }
        });
    </script>
</head>

## RICE定理例题

主要在于要说明是语言的性质，结合作业题里面的：

令集合$L=\{<M>|M在所有输入均停机\}$

- ==利用Rice定理证明是非递归可枚举的==

  > 这里重点在于要将此问题转化为语言的性质，将集合$L=\{<M>|L(M)=\Sigma^*\}$规约到当前问题进行解决

- 证明L是非递归可枚举的

  $\bar{L_{acc}} \leq_m L\iff L_{acc} \leq_m\bar{L}$ 
  
  证明过程：
  
  构造一个转换的图灵机M‘：
  
  - 输入为x，实际输入为<M,w>，内部使用$M_u$模拟<M,w>的运行
  
  - 如果M能在|x|步以内接受w，则图灵机M’永不停机
  
  - 如果M不能，那么M‘拒绝
  
  当$<M,w> \in L_{acc} \Rightarrow$  M可以在|x|步以内接受w   $\Rightarrow <M'> \in \bar{L}$
  
  $<M'> \in \bar{L} \Rightarrow$ M可以在|x|步以内接受w $\Rightarrow<M,w>\in L_{acc}$
  
  `说明`$L$`是递归可枚举的`
  
- 证明$\bar{L}$是非递归可枚举的

  $\bar{L_{acc}} \leq_m \bar{L}\iff L_{acc} \leq_mL$

  证明过程：

  构造一个转换的图灵M‘：

  - 输入为x，实际输入为<M,w>，内部使用$M_u$模拟<M,w>的运行
  
  - 如果M能够在|x|步以内接受w，那么图灵机M’接受
  
  - 否则不停机
  
  当$<M,w>\in L_{acc} \Rightarrow$M可以在|x|步以内接受w $\Rightarrow <M'> \in L$
  
  $<M'>\in L \Rightarrow$M可以在|x|步以内接受w$\Rightarrow<M,w>\in L_{acc}$
  
  `说明`$\bar{L}$`递归可枚举的`

## RICE定理的证明
`主要思想`

将TM的识别问题转换到$L_{acc}$上

现将现在的可识别的语言L构造出M‘，模拟M在w上的运行，使用的方法是构造双带图灵机M’。

- 如果M不接受w，那么$M_L$永不接受，所以最后能识别的语言只有$\emptyset$
- 如果M接受w，那么$M_L$接受$L(M_L)$即$L_p$

M接受/拒绝w$\iff$L(M')=$L_p$/$\emptyset$

此时假定的是$\emptyset \notin L_p$

所以这是一个将$L_{acc}\leq_mL_p$的操作，由于$L_{acc}$是不可判定的，所以$L_p$也是不可判定的

![image-20230611215232333](https://typora-slater.oss-cn-beijing.aliyuncs.com/picture/image-20230611215232333.png)

所以说明所有和莱斯定理相关的证明是使用的$L_{acc}$正向规约的理论体系，具有普遍性