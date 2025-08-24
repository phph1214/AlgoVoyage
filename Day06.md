# Day 06

## 17. 四数相加II

### 题目描述
---
给定四个包含整数的数组列表 A , B , C , D ,计算有多少个元组 (i, j, k, l) ，使得 A[i] + B[j] + C[k] + D[l] = 0。

为了使问题简单化，所有的 A, B, C, D 具有相同的长度 N，且 0 ≤ N ≤ 500 。所有整数的范围在 -2^28 到 2^28 - 1 之间，最终结果不会超过 2^31 - 1 。

> Leetcode 链接：[https://leetcode.cn/problems/4sum-ii/description/](https://leetcode.cn/problems/4sum-ii/description/)
---

### 题解
---
```cpp
class Solution {
public:
    int fourSumCount(vector<int>& nums1, vector<int>& nums2, vector<int>& nums3, vector<int>& nums4) {
        unordered_map<int,int> umap;
        for(int a:nums1){
            for(int b:nums2){
                umap[a+b]++;
            }
        }
        int count = 0;
        for(int c:nums3){
            for(int d:nums4){
                if(umap.find(0-(c+d))!=umap.end()){
                    count += umap[-c-d];
                }
            }
        }
        return count;
    }
};
```
> 关键语句：count += umap[-c-d];
---
### ✅ 时间复杂度
---
O(n^2)
---
### ✅ 空间复杂度
---
O(n^2)，最坏情况下A和B的值各不相同，相加产生的数字个数为 n^2
---

## 18. 赎金信

### 题目描述
---

给你两个字符串：ransomNote 和 magazine ，判断 ransomNote 能不能由 magazine 里面的字符构成。

如果可以，返回 true ；否则返回 false 。

magazine 中的每个字符只能在 ransomNote 中使用一次。

提示：

1 <= ransomNote.length, magazine.length <= 105
ransomNote 和 magazine 由小写英文字母组成

> Leetcode 链接：[https://leetcode.cn/problems/ransom-note/description/](https://leetcode.cn/problems/ransom-note/description/)  
---

### 题解
---
#### 暴力解法
```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        for(int i=0;i<magazine.length();i++){
            for(int j=0;j<ransomNote.length();j++){
                if(ransomNote[j] == magazine[i]){
                    ransomNote.erase(ransomNote.begin()+j);
                    break;
                }
            }
        }
        if(ransomNote.length()==0){
            return true;
        }
        return false;
    }
};
```
> break;必须加，否则，会把ransomNote中所有相等的字符全部删除
> 同时，用magzine构造ransomNote，需要用magzine中的字符去匹配ransomNote

#### 哈希表法
```cpp
class Solution {
public:
    bool canConstruct(string ransomNote, string magazine) {
        int record[26] = {0};
        if(ransomNote.size() > magazine.size()){
            return false;
        }
        for(int i=0;i<magazine.length();i++){
            record[magazine[i]-'a']++;
        }
        for(int i=0;i<ransomNote.length();i++){
            record[ransomNote[i]-'a']--;
            if(record[ransomNote[i]-'a']<0){
                return false;
            }
        }
        return true;
    }
};
```
---
### ✅ 时间复杂度
---
O(m+n)
---
### ✅ 空间复杂度
---
O(1)
---


## 19. 三数之和

### 题目描述
---

给你一个整数数组 nums ，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j、i != k 且 j != k ，同时还满足 nums[i] + nums[j] + nums[k] == 0 。请你返回所有和为 0 且不重复的三元组。

注意：答案中不可以包含重复的三元组。

> Leetcode 链接：[https://leetcode.cn/problems/3sum/description/](https://leetcode.cn/problems/3sum/description/)

### 题解
---
```cpp

```
---
### ✅ 时间复杂度
---
---
### ✅ 空间复杂度
---
---


## 20. 四数之和

### 题目描述
---

> Leetcode 链接：[]()
---

### 题解
---
```cpp

```
---
### ✅ 时间复杂度
---
---
### ✅ 空间复杂度
---
---