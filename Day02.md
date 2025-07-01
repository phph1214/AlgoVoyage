# 🚀 Day 02

## 长度最小的数组

### 🧩 题目描述

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其总和大于等于 target 的长度最小的 子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

> Leetcode 链接：[https://leetcode.cn/problems/minimum-size-subarray-sum/
](https://leetcode.cn/problems/minimum-size-subarray-sum/
)

---

### 🧠 解题思路
#### 暴力解法
```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int result = INT32_MAX;
        int sublength = 0;
        for (int i = 0; i < nums.size(); i++) {
            int sum = 0;
            for (int j = i; j < nums.size(); j++) {
                sum += nums[j];
                if (sum >= target) {
                    sublength = j - i + 1;
                    result = sublength < result ? sublength : result;
                    break;
                }
            }
        }
        return result == INT32_MAX ? 0 : result;
    }
};
```
>出现了一些问题：
>1. result最初设为0，但此时sublength不可能小于result;
>2. 注意内层循环中，应该是j，不是i小于nums.size();
>3. 应注意最终返回的结果，如果不存在满足条件的情况，应该返回0；
>4. 32位有符号整数（int32_t）能够表示的最大值
>5. 条件判别语句：result = sublength < result ? sublength : result;
>6. 应注意最后一句的判别语句；
>7. sum>=target；
>8. break;！！！！！！！！！！！！！！！！！！！！

#### 滑动窗口
```cpp
class Solution {
public:
    int minSubArrayLen(int target, vector<int>& nums) {
        int start = 0;
        int sum = 0;
        int result = INT32_MAX;
        int sublength = 0;
        for (int stop = 0; stop < nums.size(); stop++) {
            sum += nums[stop];
            while (sum >= target) {
                sum -= nums[start];
                sublength = stop - start + 1;
                result = sublength > result ? result : sublength;
                start++;
            }
        }
        return result == INT32_MAX ? 0 : result;
    }
};
```
>需要注意的是，应该使用while循环，因为如果去掉start位置的数值后，仍然可能大于sum


## 螺旋矩阵II

### 🧩 题目描述

给定一个含有 n 个正整数的数组和一个正整数 target 。

找出该数组中满足其总和大于等于 target 的长度最小的 子数组 [numsl, numsl+1, ..., numsr-1, numsr] ，并返回其长度。如果不存在符合条件的子数组，返回 0 。

> Leetcode 链接：[https://leetcode.cn/problems/spiral-matrix-ii/
](https://leetcode.cn/problems/spiral-matrix-ii/)

---

### 🧠 解题思路

```cpp
class Solution {
public:
    vector<vector<int>> generateMatrix(int n) {
        vector<vector<int>> res(n,vector<int>(n,0));
        int loop = n/2;
        int startx = 0, starty = 0;
        int offset = 1;
        int num = 1;
        while(loop--){
            int i = startx;
            int j = starty;

            for(j;j<n-offset;j++){
                res[i][j] = num++; 
            }
            for(i;i<n-offset;i++){
                res[i][j] = num++;
            }
            for(;j>startx;j--){
                res[i][j] = num++;
            }
            for(;i>starty;i--){
                res[i][j] = num++;
            }
            offset++;

            startx++;
            starty++;
        }

        if(n%2){
            res[startx][starty] = num;
        }
        return res;
    }
};
```
>关键点在于后面两个for循环，j>startx，而不是j>offset；



## 螺旋矩阵II

### 🧩 题目描述

在一个城市区域内，被划分成了n * m个连续的区块，每个区块都拥有不同的权值，代表着其土地价值。目前，有两家开发公司，A 公司和 B 公司，希望购买这个城市区域的土地。 

现在，需要将这个城市区域的所有区块分配给 A 公司和 B 公司。

然而，由于城市规划的限制，只允许将区域按横向或纵向划分成两个子区域，而且每个子区域都必须包含一个或多个区块。 为了确保公平竞争，你需要找到一种分配方式，使得 A 公司和 B 公司各自的子区域内的土地总价值之差最小。 

注意：区块不可再分。

> Leetcode 链接：[https://kamacoder.com/problempage.php?pid=1044
](https://kamacoder.com/problempage.php?pid=1044)

---

### 🧠 解题思路





## 区间和

### 🧩 题目描述
给定一个整数数组 Array，请计算该数组在每个指定区间内元素的总和。

输入示例
5
1
2
3
4
5
0 1
1 3
输出示例
3
9

提示信息
    数据范围：
    0 < n <= 100000

> Leetcode 链接：[https://kamacoder.com/problempage.php?pid=1070
](https://kamacoder.com/problempage.php?pid=1070)

### 🧠 解题思路
#### 暴力解法（超时）
```cpp
#include <iostream>
#include <vector>

using namespace std;

int main(){
    int n,a,b;
    cin>>n;

    vector<int> res(n,0);

    for(int i=0;i<n;i++) cin>>res[i];

    while(cin>>a,cin>>b){
        int sum = 0;
        for(a;a<=b;a++){
            sum+=res[a];
        }
        cout<<sum<<endl;
    }
}

```
> 需要注意的是：
> 1. cin >> n;
> 2. cout << sum << endl； 


运行时间超限
```cpp
#include <iostream>
#include <vector>
using namespace std;

int main(){
    int n,a,b;
    int m=0;
    cin >> n;

    vector<int> res(n,0);

    for(int i=0;i<n;i++) cin>>res[i];

    vector<int> tmp(n,0);

    for(int j=0;j<n;j++){
        for(int i = 0;i<=m;i++)  {
            tmp[m] += res[i];
        }
        m++;
    }

    while(cin>>a,cin>>b){
        int sum = 0;
        if(a>0){
            sum = tmp[b] - tmp[a-1];
        }else {sum = tmp[b];}
        
        cout << sum <<endl;

    }
}
```

```cpp
#include <iostream>
#include <vector>
using namespace std;

int main(){
    int n,a,b;
    int m=0;
    cin >> n;

    vector<int> res(n,0);
    int prbset = 0;
    vector<int> tmp(n,0);

    for(int i =0;i<n;i++){
        cin>> res[i];
        prbset+=res[i];
        tmp[i] = prbset;
    }

    while(cin>>a,cin>>b){
        int sum = 0;
        if(a>0){
            sum = tmp[b] - tmp[a-1];
        }else {sum = tmp[b];}
        
        cout << sum <<endl;

    }
}
```
区别在于时间复杂度



## 开发商购买土地
### 🧩 题目描述

在一个城市区域内，被划分成了n * m个连续的区块，每个区块都拥有不同的权值，代表着其土地价值。目前，有两家开发公司，A 公司和 B 公司，希望购买这个城市区域的土地。 

现在，需要将这个城市区域的所有区块分配给 A 公司和 B 公司。

然而，由于城市规划的限制，只允许将区域按横向或纵向划分成两个子区域，而且每个子区域都必须包含一个或多个区块。 为了确保公平竞争，你需要找到一种分配方式，使得 A 公司和 B 公司各自的子区域内的土地总价值之差最小。 

注意：区块不可再分。

> Leetcode 链接：[https://kamacoder.com/problempage.php?pid=1044
](https://kamacoder.com/problempage.php?pid=1044)

---
### 🧠 解题思路
```cpp
#include <iostream>
#include <vector>
#include <climits>//!!!!!!!!!!!!!!!!!!!!!!

using namespace std;

int main(){
    int n,m;

    cin>>n>>m;

    vector<vector<int>> res(n,vector<int>(m,0));

    int sum = 0;

    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            cin >> res[i][j];
            sum += res[i][j];        
        }
    }

    vector<int> hang(n,0);
    for(int i=0;i<n;i++){
        for(int j=0;j<m;j++){
            hang[i] += res[i][j];
        }
    }

    vector<int> lie(m,0);//!!!!!!!!!!!!!!
    for(int i = 0;i<m;i++){
        for(int j=0;j<n;j++){
            lie[i] += res[j][i];
        }
    }

    int result = INT_MAX;

    int hangcut = 0;
    int liecut=0;

    for(int i=0;i<n;i++){
        hangcut += hang[i];
        result = min(result,abs(sum-hangcut-hangcut));
    }

    for(int j=0;j<m;j++){
        liecut += lie[j];
        result = min(result,abs(sum-liecut-liecut));
    }
    //return result;
    cout<<result<<endl;
}
```
>注意点：
>1. #include <climits> 是 C++ 中的一个头文件（也适用于 C 语言），它提供了与整数类型相关的各种 常量限制值，例如某种类型所能表示的最大值和最小值；
>2. 返回值；
>3. 使用了区间和的方法，关键点在于最后两个循环中的累加环节；