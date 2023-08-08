---
title: 重复的DNA序列
categories:
- 算法
- [刷题]
tags:
- 字符串
- 哈希表
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

# 重复的DNA序列

**DNA序列** 由一系列核苷酸组成，缩写为 `'A'`, `'C'`, `'G'` 和 `'T'`.。

- 例如，`"ACGAATTCCG"` 是一个 **DNA序列** 。

在研究 **DNA** 时，识别 DNA 中的重复序列非常有用。

给定一个表示 **DNA序列** 的字符串 `s` ，返回所有在 DNA 分子中出现不止一次的 **长度为 `10`** 的序列(子字符串)。你可以按 **任意顺序** 返回答案。

 

**示例 1：**

```
输入：s = "AAAAACCCCCAAAAACCCCCCAAAAAGGGTTT"
输出：["AAAAACCCCC","CCCCCAAAAA"]
```

**示例 2：**

```
输入：s = "AAAAAAAAAAAAA"
输出：["AAAAAAAAAA"]
```

 

**提示：**

- `0 <= s.length <= 105`
- `s[i]``==``'A'`、`'C'`、`'G'` or `'T'`

```cpp
class Solution {
public:
    int L=10;
    int R=4;
    int RL=pow(4,9);
    vector<string> findRepeatedDnaSequences(string s) {
        vector<int> num;
        vector<string> answer;
        unordered_map<long,int> map;
        for(auto x:s){
            switch(x){
                case 'A':
                    num.push_back(0);
                    break;
                case 'G':
                    num.push_back(1);
                    break;
                case 'C':
                    num.push_back(2);
                    break;
                case 'T':
                    num.push_back(3);
                    break;
            }
        }
        int left=0,right=0;
        long window=0;
        int size=s.size();
        while(right<size){
            window=R*window+num[right];
            right++;
            if(right-left==L){
                if(map.find(window)!=map.end()&&map[window]==1){
                    string temp;
                    for(int i=left;i<right;i++){
                        temp.push_back(s[i]);
                    }
                    answer.push_back(temp);
                }
                map[window]++;
                window=window-RL*num[left];
                left++;
            }
        }
        return answer;
    }
};
```

使用哈希表对四个字母进行唯一化编码，使用哈希的办法来对出现过的字符串进行计数，同时使用滑动窗口的算法，动态的确定一个字符串。

时间复杂度：$O(n)$只遍历了整个字符串

空间复杂度：$O(n)$使用了一个转换过后的数组
