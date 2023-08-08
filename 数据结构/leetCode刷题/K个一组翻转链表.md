---
title: K个一组翻转链表
categories:
- 数据结构
- [刷题]
tags:
- 链表
---

# K个一组翻转链表

给你链表的头节点 `head` ，每 `k` 个节点一组进行翻转，请你返回修改后的链表。

`k` 是一个正整数，它的值小于或等于链表的长度。如果节点总数不是 `k` 的整数倍，那么请将最后剩余的节点保持原有顺序。

你不能只是单纯的改变节点内部的值，而是需要实际进行节点交换

**示例 1：**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex1.jpg)

```
输入：head = [1,2,3,4,5], k = 2
输出：[2,1,4,3,5]
```

**示例 2：**

![img](https://assets.leetcode.com/uploads/2020/10/03/reverse_ex2.jpg)

```
输入：head = [1,2,3,4,5], k = 3
输出：[3,2,1,4,5]
```

 **提示：**

- 链表中的节点数目为 `n`
- `1 <= k <= n <= 5000`
- `0 <= Node.val <= 1000`

 **进阶：**你可以设计一个只用 `O(1)` 额外内存空间的算法解决此问题吗？

```cpp
/**
 * Definition for singly-linked list.
 * struct ListNode {
 *     int val;
 *     ListNode *next;
 *     ListNode() : val(0), next(nullptr) {}
 *     ListNode(int x) : val(x), next(nullptr) {}
 *     ListNode(int x, ListNode *next) : val(x), next(next) {}
 * };
 */
class Solution {
public:
    ListNode* reverseKGroup(ListNode* head, int k) {
    if(head==nullptr)return nullptr;
    ListNode* a;
    ListNode* b;
    a=b=head;
    for(int i=0;i<k;i++){
        if(b==nullptr)return head;
        b=b->next;
    }
    ListNode* newhead=reverse(a,b);
    a->next=reverseKGroup(b,k);
    return newhead;
    }
    ListNode* reverse(ListNode* a,ListNode* b){
        ListNode* cur=a;
        ListNode* pre=nullptr;
        ListNode* nxt=a;
        while(cur!=b){
            nxt=cur->next;
            cur->next=pre;
            pre=cur;
            cur=nxt;
        }
        return pre;
    }
};
```

时间复杂度：O(n)

空间复杂度：O(1)