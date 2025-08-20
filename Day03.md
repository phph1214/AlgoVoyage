# Day 03

## 1. 

### 题目描述
---
给你一个链表的头节点 head 和一个整数 val ，请你删除链表中所有满足 Node.val == val 的节点，并返回 新的头节点 。
> Leetcode 链接：[https://leetcode.cn/problems/remove-linked-list-elements/description/](https://leetcode.cn/problems/remove-linked-list-elements/description/)
---

### 题解
---
#### 使用原来的链表进行移除操作
```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        while(head != NULL && head->val == val){
            ListNode* tmp = head;
            head = head->next;
            delete tmp;
        }
        ListNode* cur = head;
        while(cur != NULL && cur->next !=NULL){
            if(cur->next->val == val){
                ListNode* tmp = cur->next;
                cur -> next = cur->next->next;
                delete tmp;
            }else{
                cur = cur->next;
            }
        }
        return head;
    }
};
```
> 这里加head != NULL是为了防止解引用空指针，导致运行时错误（Segmentation Fault）。

#### 使用虚拟头节点
```cpp
class Solution {
public:
    ListNode* removeElements(ListNode* head, int val) {
        ListNode* dh = new ListNode(0);
        dh->next = head;
        ListNode* cur = dh;
        while(cur->next !=NULL){
            if(cur->next->val ==val){
               ListNode* tmp = cur->next;
               cur->next = cur->next->next;
               delete tmp;
            }else{
                cur= cur->next;
            }
        }
        head = dh->next;
        delete dh;
        return head;
    }
};
```
> 1. 在使用虚拟头节点时，循环的判别条件不再需要cur != NULL，因为dummyhead是自己创建的，肯定不是NULL。
> 2. 最后应该删除虚拟头节点。
---
### ✅ 时间复杂度：O(log n)：以2为底的对数
---
TC：O(n);
SC：O(1);
---
### ✅ 空间复杂度：O(1)
---
TC：O(n);
SC：O(1);
---
