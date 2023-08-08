---
title: 和为K的子数组
categories:
- 数据结构
- [刷题]
tags:
- 数组
- 哈希表
- 前缀法
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

# 和为K的子数组

给你一个整数数组 `nums` 和一个整数 `k` ，请你统计并返回 *该数组中和为 `k` 的连续子数组的个数* 。

**示例 1：**

```
输入：nums = [1,1,1], k = 2
输出：2
```

**示例 2：**

```
输入：nums = [1,2,3], k = 3
输出：2
```

**提示：**

- `1 <= nums.length <= 2 * 104`
- `-1000 <= nums[i] <= 1000`
- `-107 <= k <= 107`

## 暴力算法：遍历所有可能的数组组合

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
    int start=0;
    int count=0;
    for(;start<nums.size();start++){
        int sum=0;
        for(int end=start;end>=0;end--){
            sum+=nums[end];
            if(sum==k)count++;
        }
    }
    return count;
    }
};
```

**复杂度分析**

- 时间复杂度：$O(n^2)$，其中 n为数组的长度。枚举子数组开头和结尾需要 $O(n^2)$的时间，其中求和需要 O(1)的时间复杂度，因此总时间复杂度为$O(n^2)$。
- 空间复杂度：O(1)。只需要常数空间存放若干变量。

## 使用哈希表和前缀法进行优化：

```c++
class Solution {
public:
    int subarraySum(vector<int>& nums, int k) {
        unordered_map<int,int> map;
        int pre=0;
        int count=0;
        map[0]=1;
        for(int i=0;i<nums.size();i++){
            pre=pre+nums[i];
            if(map.find(pre-k)!=map.end()){
                count+=map[pre-k];
            }
            map[pre]++;
        }
        return count;
    }
};
```

**复杂度分析**

- 时间复杂度：O(n)，其中 n为数组的长度。我们遍历数组的时间复杂度为 O(n)，中间利用哈希表查询删除的复杂度均为 O(1)，因此总时间复杂度为 O(n)。
- 空间复杂度：O(n)，其中 n为数组的长度。哈希表在最坏情况下可能有 n个不同的键值，因此需要 O(n)的空间复杂度。