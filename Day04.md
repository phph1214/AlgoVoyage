# Day 04

## 11. 两两交换链表中的节点

### 题目描述
---
给你一个链表，两两交换其中相邻的节点，并返回交换后链表的头节点。你必须在不修改节点内部的值的情况下完成本题（即，只能进行节点交换）。
> Leetcode 链接：[https://leetcode.cn/problems/swap-nodes-in-pairs/description/](https://leetcode.cn/problems/swap-nodes-in-pairs/description/)
---

### 题解
---
```cpp
class Solution {
public:
    ListNode* swapPairs(ListNode* head) {
        ListNode* dh = new ListNode(0);
        dh ->next = head;
        ListNode* cur = dh;
        while(cur->next != nullptr && cur->next->next != nullptr){
            ListNode* tmp = cur->next;
            ListNode* tmp1 = cur->next->next->next;
            cur->next = cur->next->next;

            cur->next->next = tmp;

            cur->next->next ->next = tmp1;

            cur = cur->next ->next;

        }
        ListNode* result = dh->next;
        delete dh;
        return result;
    }
};
```

---
### ✅ 时间复杂度
---
O(n)
---
### ✅ 空间复杂度
---
O(1)
---

## 12. 删除链表的倒数第N个节点

### 题目描述
---

给你一个链表，删除链表的倒数第 n 个结点，并且返回链表的头结点。
> Leetcode 链接：[https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/](https://leetcode.cn/problems/remove-nth-node-from-end-of-list/description/)
---

### 题解
---
```cpp
class Solution {
public:
    ListNode* removeNthFromEnd(ListNode* head, int n) {
        ListNode* dh = new ListNode(0);
        dh->next = head;
        ListNode* tmp = dh;
        ListNode* tmp1 = dh;
        while(n--){
            tmp1 = tmp1->next;
        }
        tmp1 = tmp1->next; //考虑删除第一个节点的情况
        while(tmp1){
            tmp1 = tmp1->next;
            tmp = tmp ->next;
        }
        //tmp->next = tmp->next->next;
        ListNode* tmp2 = tmp->next;
        tmp->next = tmp->next->next;
        delete tmp2;
        return dh->next; 
    }
};
```

> 需要考虑删除第一个节点的情况，故所设节点需要从dh开始
---
### ✅ 时间复杂度
---
O(n)
---
### ✅ 空间复杂度
---
O(1)
---

## 3. 链表相交

### 题目描述
---
给你两个单链表的头节点 headA 和 headB ，请你找出并返回两个单链表相交的起始节点。如果两个链表没有交点，返回 null 。
> Leetcode 链接：[https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/](https://leetcode.cn/problems/intersection-of-two-linked-lists-lcci/description/)
---

### 题解
---
```cpp
class Solution {
public:
    ListNode *getIntersectionNode(ListNode *headA, ListNode *headB) {
        ListNode* inA = headA;
        ListNode* inB = headB;
        int lenA = 0;
        int lenB = 0;
        while(inA){
            inA = inA ->next;
            lenA++;
        }
        while(inB){
            inB = inB ->next;
            lenB++;
        }
        inA = headA;
        inB = headB;
        if(lenB>lenA){
            swap(inA,inB);
            swap(lenA,lenB);
        }
        int diff = lenA - lenB;
        while(diff){
            inA = inA->next;
            diff--;
        }
        while(lenB){
            if(inA == inB){
                return inA;
            }
            inA = inA->next;
            inB = inB->next;
        }
        return NULL;
    }
};
```
> 1. 注意看该函数的返回值是ListNode*，故不要返回交叉节点的值
> 2. 求其交点，故只可能在后半部分相交
> 3. 步骤：移动长链表的指针与短链表对齐，一个节点一个节点的比较指针
---
### ✅ 时间复杂度
---
O(n+m)
---
### ✅ 空间复杂度
---
O(1)
---