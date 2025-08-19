# Day 01

## 1. 二分查找

### 题目描述
---
给定一个 n 个元素有序的（升序）整型数组 nums 和一个目标值 target  ，写一个函数搜索 nums 中的 target，如果 target 存在返回下标，否则返回 -1。

你必须编写一个具有 O(log n) 时间复杂度的算法。

注：
1. 可以假设 nums 中的所有元素是不重复的。
2. n 将在 [1, 10000]之间。
3. nums 的每个元素都将在 [-9999, 9999]之间。

> Leetcode 链接：[https://leetcode.cn/problems/binary-search/](https://leetcode.cn/problems/binary-search/)
---

### 题解
---
> 二分法，无论是左闭右闭还是左闭右开，在操作过程中，最终目的都要围绕让数组中的每个元素都参与到判别中，
#### 左闭右闭区间（二分法）

```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size() - 1;

        while(left <= right){
            int middle = (left + right) / 2;
            if(nums[middle] > target){
                right = middle -1;
            }else if(nums[middle] < target){
                left = middle +1;
            }else{
                return middle;
            }
        }
        return -1;
    }
};
```
> 可以用for循环替代，但第三条件为空

#### 左闭右开区间（二分法）
```cpp
class Solution {
public:
    int search(vector<int>& nums, int target) {
        int left = 0;
        int right = nums.size();
        while(left < right){
            int middle = (left + right) / 2;
            if(nums[middle] < target){
                left = middle + 1 ;
            }else if(nums[middle] > target){
                right = middle;
            }else{
                return middle;
            }
        }
        return -1;
    }
};
```
1. 一切操作遵循所选择的搜索区间！！！！！！！！！！！
2. int right = nums.size();还是int right = nums.size() - 1;仍然需要结合搜索区间，初始阶段，需要保证数组中的所有元素都在搜索区间内。 
3. right = middle 还是 right = middle - 1；关键点在于前序步骤中已经判定middle处的值不是target，同时，还需要考虑，在下一步的搜索中，序号right处的值是否在搜索空间内。

---
### ✅ 时间复杂度：O(log n)：以2为底的对数

随着输入规模增加，算法的执行步骤是以对数的方式增长的，而不是线性增长

---
### ✅ 空间复杂度：O(1)

> O(1)：只使用常数量级的额外空间，不随输入规模n的增加而增加
1. 整个算法过程中只使用了几个`int`类型的变量
2. 不依赖额外的数组、栈、递归等结构
3. 输入参数不计入空间复杂度分析

---

## 2. 移除元素

### 题目描述
---
给你一个数组 nums 和一个值 val，你需要**原地**移除所有数值等于 val 的元素。元素的顺序可能发生改变。然后返回 nums 中与 val 不同的元素的数量。

假设 nums 中不等于 val 的元素数量为 k，要通过此题，您需要执行以下操作：

更改 nums 数组，使 nums 的前 k 个元素包含不等于 val 的元素。nums 的其余元素和 nums 的大小并不重要。
返回 k。

提示：

* 0 <= nums.length <= 100
* 0 <= nums[i] <= 50
* 0 <= val <= 100

> Leetcode 链接：[https://leetcode.cn/problems/remove-element/](https://leetcode.cn/problems/remove-element/)

---
### 题解
#### 暴力解法
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int k = nums.size();
        for (int i = 0; i < k ; i++) {
            if (nums[i] == val) {
                
                for (int j = i + 1; j < k; j++) {
                    nums[j - 1] = nums[j];
                }
                i--;
                k--;
            }
        }
        return k;
    }
};
```
>出现了一些问题：
>1. i--：因为如果不减的话可能会导致直接掠过数组中的某些元素;
>2. 第二就是在两个循环内部，要使用k作为循环的判别条件，而不是nums.size();否则会出现“超出时间限制”的错误；
>3. 同时，无需担心当最后一个元素等于目标值时，会出现越界问题，可以举例[3,2,2,3]判断；当i=3时，j=4,k=3；
---
### 双指针法
错误示范
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow = 0;
        //int fast = 0;

        for(int fast = 0 ; fast < nums.size(); ){
            if(nums[fast] == val){
                fast++;
            }else{
                nums[slow] = nums[fast];
                fast++;
                slow++;
            }
            
        }
        return slow + 1;
    }
};
```
> 返回的应该是slow
正确示范
```cpp
class Solution {
public:
    int removeElement(vector<int>& nums, int val) {
        int slow = 0;
        //int fast = 0;

        for(int fast = 0 ; fast < nums.size(); fast++){
            if(nums[fast] != val){
                nums[slow] = nums[fast];
                slow++;
            }
        }
        return slow;
    }
};
```
>需要注意的是，slow指针只有在指向非目标值时，才会自增！！！！！！！！！！！！！！！！！！
---

## 3. 有序数组的平方

### 题目描述
给你一个按 非递减顺序 排序的整数数组 nums，返回 每个数字的平方 组成的新数组，要求也按 非递减顺序 排序。

提示：

1 <= nums.length <= 104
-104 <= nums[i] <= 104
nums 已按 非递减顺序 排序
 

进阶：

请你设计时间复杂度为 O(n) 的算法解决本问题

> Leetcode 链接：[https://leetcode.cn/problems/squares-of-a-sorted-array/
](https://leetcode.cn/problems/squares-of-a-sorted-array/
)
---
### 题解

#### 暴力解法

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        for(int i = 0; i < nums.size();i++){
            nums[i] *= nums[i];
        }
        sort(nums.begin(),nums.end());
        return nums;
    }
};
```

>时间复杂度分析：首先循环遍历了一次数组，时间复杂度为O(n)；C++ STL 的 sort() 默认使用 Introsort，时间复杂度为：O(n log n)。

#### 双指针法

错误示范

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int i = 0;
        int j = nums.size() - 1;

        vector<int> result(nums.size(),0);
        while(i <= j){
            if(nums[i] * nums[i] > nums[j] * nums[j]){
                result[j] = nums[i] * nums[i];
                i++;
            }else if(nums[i] * nums[i] <= nums[j] * nums[j]){
                result[j] = nums[j] * nums[j];
                j--;
            }
        }
        return result;
    }
};
```

>需要一个单独的指针来指示结果数组的存放位置，如果复用了j，会导致出现覆盖

正确示范

```cpp
class Solution {
public:
    vector<int> sortedSquares(vector<int>& nums) {
        int pos = nums.size() - 1;
        vector<int> result(nums.size(),0);

        for(int i = 0,j = nums.size()-1;i<=j;){
            if( nums[i] * nums[i] <= nums[j] * nums[j]){
                result[pos] = nums[j] * nums[j];
                j--,pos--;
            }else{
                result[pos] = nums[i] * nums[i];
                i++,pos--;
            }
        }
        return result;
    }
};
```

>需要注意的是，此处for循环的第三个选项是空值。
>* 仍然需要关注区间：i<=j，最终目标是确保数组中每一个元素都参与到排序

---