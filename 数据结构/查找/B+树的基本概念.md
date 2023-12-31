---
title: B+树的基本概念
categories:
- 数据结构
- 查找
- 笔记
tags:
- B+树
- 查找
mathjex: true
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

# B+树的基本概念

B+树是应数据库所需而出现的一种B树的变形树。

一棵m阶的B+树满足以下条件：

1. 每个分支结点最多有m棵子树（孩子节点）
2. 非叶根节点至少有两棵子树，其他分支结点至少有$\\lceil m/2\rceil$棵子树。
3. 结点的子树个数与关键字个数相等。
4. 所有叶节点包含全部关键字及指向相应记录的指针，叶节点中将关键字按照大小顺序排列，并且相邻叶节点按照大小顺序相互链接起来。
5. 所有分支结点（可以视为索引的索引）中仅仅包含它的各个子结点（即下一级的索引块）中关键字的最大值及其指向其子结点的指针。



## m阶的B+树和B树的**主要差异**：

1. 在B+树中，具有n个关键字的结点只含有n棵子树，即每一个关键字对应一个子树，而在B树中，具有n个关键字的结点含有n+1个子树。
2. 在B+树中，每个结点（非根内部节点）的关键字个数范围为$\lceil m/2\rceil\leq n\leq m$（根结点：$2\leq n\leq m-1$）。
3. 在B+树中叶结点包含信息，所有非叶结点仅起索引作用，非叶结点中每个索引项只含有对应子树的最大关键字和指向该子树的指针，不含有该关键字对应的存储地址。
4. 在B+树中，叶节点包含了全部关键字，即在非叶结点中出现的关键字也会出现在叶节点中;而在B树中，叶节点（最外层内部节点）包含的关键字和其他结点包含的关键字书不重复的。

在B+树中，分支节点的某个关键字是其子树中**最大关键字的副本**。通常在B+树中有**两个头指针**；一个指向根节点，另一个指向关键字最小的叶节点。可以对B+树进行两种查找运算：一种是从最小关键字开始的**顺序查找**，另一种是从根节点开始的**多路查找**。B+树在查找是永远都是一条**从根节点到叶节点的路径**。