# 最小的K个数

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

# 排序

```cpp
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        if(input.size()<k) return vector<int>();
        sort(input.begin(), input.end());
        vector<int> res(input.begin(), input.begin()+k);
        return res;
    }
};
```

## 常规解法

```cpp
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        if(input.size()<k) return vector<int>();
        vector<int> min_k(input.begin(), input.begin()+k);
        sort(min_k.begin(), min_k.end());
        for(int eidx = k; eidx < input.size(); eidx++) {
            for(int m = min_k.size()-1; m >= 0; m--) {
                if(input[eidx]<min_k[m]) {
                    min_k[m] = input[eidx];
                    sort(min_k.begin(), min_k.end());
                    break;
                }
            }
        }
        return min_k;
    }
};
```

## 最大堆

```cpp
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        // method3 O(log2N) 最大堆
        if(input.empty() || k>input.size() || k<=0) return vector<int>();
        vector<int> min_k_vec(input.begin(), input.begin()+k);
        make_heap(min_k_vec.begin(), min_k_vec.end());
        for(int eidx = k; eidx < input.size(); eidx++) {
            if(input[eidx]<min_k_vec[0]) {
                pop_heap(min_k_vec.begin(), min_k_vec.end());
                min_k_vec.pop_back();
                
                min_k_vec.push_back(input[eidx]);
                push_heap(min_k_vec.begin(), min_k_vec.end());
            }
        }
        sort_heap(min_k_vec.begin(), min_k_vec.end());
        return min_k_vec;
    }
};
```
