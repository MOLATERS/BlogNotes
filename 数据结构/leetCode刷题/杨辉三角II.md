---
title: 杨辉三角
categories:
- 数据结构
- [刷题]
tags:
- 数组
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

# 杨辉三角II

给定一个非负索引 `rowIndex`，返回「杨辉三角」的第 `rowIndex` 行。

在「杨辉三角」中，每个数是它左上方和右上方的数的和。

> 示例 1:
输入: rowIndex = 3
输出: [1,3,3,1]

> 示例 2:
输入: rowIndex = 0
输出: [1]

> 示例 3:
输入: rowIndex = 1
输出: [1,1]

**提示:**

- `0 <= rowIndex <= 33`

 不一样的解题方式：使用递归的方法

```c++
class Solution {
public:
    vector<int> getRow(int rowIndex) {
        vector<int> answer;
        answer.push_back(1);
        if(rowIndex==0)return answer;
        vector<int> pre=getRow(rowIndex-1);
        for(int i=0;i<pre.size()-1;i++){
            answer.push_back(pre[i]+pre[i+1]);
        }
        answer.push_back(1);
        return answer;
    }
};
```

空间复杂度：$O(n)$此时只使用了逐次递归的栈空间

时间复杂度： $O(n^2)$没有进行计算的优化
