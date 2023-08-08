---
title: 螺旋矩阵II
categories:
- 数据结构
- [刷题]
tags:
- 矩阵
---
# 螺旋矩阵II

给你一个正整数 `n` ，生成一个包含 `1` 到 `n2` 所有元素，且元素按顺时针顺序螺旋排列的 `n x n` 正方形矩阵 `matrix` 。

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/11/13/spiraln.jpg)

```
输入：n = 3
输出：[[1,2,3],[8,9,4],[7,6,5]]
```

**示例 2：**

```
输入：n = 1
输出：[[1]]
```

**提示：**

- `1 <= n <= 20`

```c++
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> answer(n,vector<int> (n,0));
        int left_high[]={0,0};
        int right_high[]={0,n-1};
        int left_low[]={n-1,0};
        int right_low[]={n-1,n-1};
        int count=1;
        while(left_high[1]<=right_high[1]&&left_low[1]<=right_low[1]){
            for(int i=left_high[1];i<=right_high[1];i++){
                answer[left_high[0]][i]=count;
                count++;
            }
                left_high[0]++;
                left_high[1]++;
                right_high[0]++;
                right_high[1]--;
            for(int i=right_high[0];i<=right_low[0];i++){
                answer[i][right_low[1]]=count;
                count++;
            }
                right_low[1]--;
                right_low[0]--;
            for(int i=right_low[1];i>=left_low[1];i--){
                answer[left_low[0]][i]=count;
                count++;
            }
                left_low[0]--;
            for(int i=left_low[0];i>=left_high[0];i--){
                answer[i][left_low[1]]=count;
                count++;
            }
                left_low[1]++;
        }
        return answer;
    }
};
```

使用四个边角作为约束，不断缩小约束范围，时间复杂度和空间复杂度固定