---
title: 路径总和
categories:
- 数据结构
- [刷题]
tags:
- 二叉树
---
# 路径总和II

给你二叉树的根节点 `root` 和一个整数目标和 `targetSum` ，找出所有 **从根节点到叶子节点** 路径总和等于给定目标和的路径。

**叶子节点** 是指没有子节点的节点。

 

**示例 1：**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsumii1.jpg)

```
输入：root = [5,4,8,11,null,13,4,7,2,null,null,5,1], targetSum = 22
输出：[[5,4,11,2],[5,8,4,5]]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2021/01/18/pathsum2.jpg)

```
输入：root = [1,2,3], targetSum = 5
输出：[]
```

**示例 3：**

```
输入：root = [1,2], targetSum = 0
输出：[]
```

 

**提示：**

- 树中节点总数在范围 `[0, 5000]` 内
- `-1000 <= Node.val <= 1000`
- `-1000 <= targetSum <= 1000`

```cpp
class Solution {
public:
    vector<vector<int>> answer;
    vector<int> temp;
    vector<vector<int>> pathSum(TreeNode* root, int targetSum) {
        if(!root)return answer;
        path(root,targetSum,temp);
        return answer;
    }
    void path(TreeNode* root, int targetSum,vector<int> temp) {
        if(root->val==targetSum&&root->left==nullptr&&root->right==nullptr){
            temp.push_back(root->val);
            answer.push_back(temp);
            return;
        }
        else{
            temp.push_back(root->val);
            if(root->right!=nullptr)path(root->right,targetSum-root->val,temp);
            if(root->left!=nullptr)path(root->left,targetSum-root->val,temp);
        }
        return;
    }
};
```

时间复杂度为:O(n)只遍历一遍所有的节点

空间复杂度：O(log2n)需要深度的递归空间