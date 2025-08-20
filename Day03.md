# Day 03

## 8. 移除链表元素

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
### ✅ 时间复杂度
---
TC：O(n);
SC：O(1);
---
### ✅ 空间复杂度
---
TC：O(n);
SC：O(1);
---

## 9. 设计链表

### 题目描述
---
你可以选择使用单链表或者双链表，设计并实现自己的链表。

单链表中的节点应该具备两个属性：val 和 next 。val 是当前节点的值，next 是指向下一个节点的指针/引用。

如果是双向链表，则还需要属性 prev 以指示链表中的上一个节点。假设链表中的所有节点下标从 0 开始。

实现 MyLinkedList 类：

* MyLinkedList() 初始化 MyLinkedList 对象。
* int get(int index) 获取链表中下标为 index 的节点的值。如果下标无效，则返回 -1 。
* void addAtHead(int val) 将一个值为 val 的节点插入到链表中第一个元素之前。在插入完成后，新节点会成为链表的第一个节点。
* void addAtTail(int val) 将一个值为 val 的节点追加到链表中作为链表的最后一个元素。
* void addAtIndex(int index, int val) 将一个值为 val 的节点插入到链表中下标为 index 的节点之前。如果 index 等于链表的长度，那么该节点会被追加到链表的末尾。如果 index 比长度更大，该节点将 不会插入 到链表中。
* void deleteAtIndex(int index) 如果下标有效，则删除链表中下标为 index 的节点。

> Leetcode 链接：[https://leetcode.cn/problems/design-linked-list/description/](https://leetcode.cn/problems/design-linked-list/description/)
---

### 题解
---
```cpp
class MyLinkedList {
public:
    // 定义链表节点结构体
    struct LinkedNode {
        int val;
        LinkedNode* next;
        LinkedNode(int val):val(val), next(nullptr){}
    };

    // 初始化链表
    MyLinkedList() {
        _dummyHead = new LinkedNode(0); // 这里定义的头结点 是一个虚拟头结点，而不是真正的链表头结点
        _size = 0;
    }

    // 获取到第index个节点数值，如果index是非法数值直接返回-1， 注意index是从0开始的，第0个节点就是头结点
    int get(int index) {
        if (index > (_size - 1) || index < 0) {
            return -1;
        }
        LinkedNode* cur = _dummyHead->next;
        while(index--){ // 如果--index 就会陷入死循环
            cur = cur->next;
        }
        return cur->val;
    }

    // 在链表最前面插入一个节点，插入完成后，新插入的节点为链表的新的头结点
    void addAtHead(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        newNode->next = _dummyHead->next;
        _dummyHead->next = newNode;
        _size++;
    }

    // 在链表最后面添加一个节点
    void addAtTail(int val) {
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        while(cur->next != nullptr){
            cur = cur->next;
        }
        cur->next = newNode;
        _size++;
    }

    // 在第index个节点之前插入一个新节点，例如index为0，那么新插入的节点为链表的新头节点。
    // 如果index 等于链表的长度，则说明是新插入的节点为链表的尾结点
    // 如果index大于链表的长度，则返回空
    // 如果index小于0，则在头部插入节点
    void addAtIndex(int index, int val) {

        if(index > _size) return;
        if(index < 0) index = 0;        
        LinkedNode* newNode = new LinkedNode(val);
        LinkedNode* cur = _dummyHead;
        while(index--) {
            cur = cur->next;
        }
        newNode->next = cur->next;
        cur->next = newNode;
        _size++;
    }

    // 删除第index个节点，如果index 大于等于链表的长度，直接return，注意index是从0开始的
    void deleteAtIndex(int index) {
        if (index >= _size || index < 0) {
            return;
        }
        LinkedNode* cur = _dummyHead;
        while(index--) {
            cur = cur ->next;
        }
        LinkedNode* tmp = cur->next;
        cur->next = cur->next->next;
        delete tmp;
        //delete命令指示释放了tmp指针原本所指的那部分内存，
        //被delete后的指针tmp的值（地址）并非就是NULL，而是随机值。也就是被delete后，
        //如果不再加上一句tmp=nullptr,tmp会成为乱指的野指针
        //如果之后的程序不小心使用了tmp，会指向难以预想的内存空间
        tmp=nullptr;
        _size--;
    }

    // 打印链表
    void printLinkedList() {
        LinkedNode* cur = _dummyHead;
        while (cur->next != nullptr) {
            cout << cur->next->val << " ";
            cur = cur->next;
        }
        cout << endl;
    }
private:
    int _size;
    LinkedNode* _dummyHead;

};
```

```cpp
class MyLinkedList {
public:
    
    struct LinkNode{
        int val;
        LinkNode* next;
        LinkNode(int val):val(val),next(nullptr){} 
    };

    MyLinkedList() {
        sz=0;
        dh= new LinkNode(0);
    }
    
    int get(int index) {
        LinkNode* cur = dh->next;
        if((index>(sz-1))||(index<0)){
            return -1;
        }    
        while(index--){
            cur = cur->next;
        }
        return cur->val;
    }
    
    void addAtHead(int val) {
        LinkNode* cur = dh;
        LinkNode* tmp = new LinkNode(val);
        tmp->next = cur->next;
        cur->next = tmp;
        sz++;
    }
    
    void addAtTail(int val) {
        LinkNode* cur = dh;
        LinkNode* tmp = new LinkNode(val);
        while(cur->next !=nullptr){
            cur = cur->next;
        } 
        cur->next = tmp;
        sz++;
    }
    
    void addAtIndex(int index, int val) {
        if(index>sz) return;
        if(index<0)  index = 0;
        LinkNode* cur = dh;
        LinkNode* tmp = new LinkNode(val);
        while(index--){
            cur = cur->next;
        }          
        tmp->next = cur->next;
        cur->next = tmp;
        sz++;
    }
    
    void deleteAtIndex(int index) {
        if(index>=sz) return;
        if(index<0)  index = 0;
        LinkNode* cur = dh;
        LinkNode* tmp = new LinkNode(0);
        while(index--){
            cur = cur->next;
        }               
        tmp = cur->next;
        cur->next = cur->next->next;
        
        tmp->next = nullptr;
        delete tmp;
        sz--;
    }


private:
    int sz;
    LinkNode* dh;
};

/**
 * Your MyLinkedList object will be instantiated and called as such:
 * MyLinkedList* obj = new MyLinkedList();
 * int param_1 = obj->get(index);
 * obj->addAtHead(val);
 * obj->addAtTail(val);
 * obj->addAtIndex(index,val);
 * obj->deleteAtIndex(index);
 */
```

> 1. 应注意cur的初始化位置
> 2. cur->next = tmp;  addathead函数此处出错
> 3. addathead函数无需使用cur
---
### ✅ 时间复杂度
---
TC：涉及 index 的相关操作为 O(index), 其余为 O(1)
---
### ✅ 空间复杂度
---
O(n)
---

## 10. 翻转链表

### 题目描述
---
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。
> Leetcode 链接：[https://leetcode.cn/problems/reverse-linked-list/](https://leetcode.cn/problems/reverse-linked-list/)
---

### 题解
---
```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* cur = nullptr;
        while(head->next != nullptr){
            head -> next = cur;
            cur = head;
            head=head->next;

        }
        return head;
    }
};
```
> 错误
```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* tmp;
        ListNode* cur = head;
        ListNode* pre = nullptr;
        while(cur){
            tmp = cur->next;
            cur ->next = pre;
            pre = cur;
            cur = tmp;
        }
        return pre;
    }
};
```
```cpp
class Solution {
public:
    ListNode* reverseList(ListNode* head) {
        ListNode* pre = nullptr;
        ListNode* tmp;
        while(head){
            tmp = head->next;
            head->next = pre;
            pre = head;
            head = tmp;
        }
        return pre;
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