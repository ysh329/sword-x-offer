# 和为S的两个数字

输入一个递增排序的数组和一个数字S，在数组中查找两个数，使得他们的和正好是S，如果有多对数字的和等于S，输出两个数的乘积最小的。

对应每个测试案例，输出两个数，小的先输出。

## 双指针

### 双指针1

```cpp
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int> array, int sum) {
        vector<vector<int>> res;
        if (array.size()<2) return vector<int>();
        int start_idx = 0, end_idx = array.size()-1;
        while(start_idx<end_idx) {
            int cur_sum = array[start_idx] + array[end_idx];
            if (cur_sum==sum) {
                res.emplace_back(vector<int>());
                vector<int>& sub_res = res[res.size()-1];
                sub_res.emplace_back(array[start_idx]);
                sub_res.emplace_back(array[end_idx]);
                start_idx++; end_idx--;
            }
            else if(cur_sum>sum) end_idx--;
            else start_idx++;// cur_sum < sum 
        }
        vector<int> min_res;
        if(res.size()) {
            int min_prod = res[0][0]*res[0][1];
            min_res.emplace_back(res[0][0]);
            min_res.emplace_back(res[0][1]);
            for(int vidx = 1; vidx < res.size(); vidx++) {
                int cur_prod = res[vidx][0] * res[vidx][1];
                if(cur_prod<min_prod) {
                    min_prod = cur_prod;
                    min_res[0] = res[vidx][0];
                    min_res[1] = res[vidx][1];
                }
            }
        }
        return min_res;
    }
};
```

### 双指针2

- a + b = sum  
- a与b差距越大，乘积越小  
- 一头一尾两个指针往内靠近的过程中，找到的是乘积越来越小的情况

```cpp
class Solution {
public:
    vector<int> FindNumbersWithSum(vector<int>& a, int sum) {
        vector<int> res;
        int i=0, j=a.size()-1;
        while(i<j) {
            if(a[i]+a[j]==sum) {
                res.emplace_back(a[i]);
                res.emplace_back(a[j]);
                break;
            }
            while(i<j && a[i]+a[j]<sum) i++;
            while(i<j && a[i]+a[j]>sum) j--;
        }
        return res;
    }
};
```
