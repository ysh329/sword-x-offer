# 最小的K个数

输入n个整数，找出其中最小的K个数。例如输入4,5,1,6,2,7,3,8这8个数字，则最小的4个数字是1,2,3,4,。

# 排序

```cpp
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        if(input.size()<k || input.empty() || k<=0) return vector<int>();
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
        if(input.size()<k || input.empty() || k<=0) return vector<int>();
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

- O(log2N)  

```cpp
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        if(input.empty() || input.size()<k || k<=0) return vector<int>();
        vector<int> res(input.begin(), input.begin()+k);
        make_heap(res.begin(), res.end());
        for(int idx = k; idx < input.size(); idx++) {
            if(input[idx] < res[0]) {
                pop_heap(res.begin(), res.end());
                res.pop_back();
                res.push_back(input[idx]);
                push_heap(res.begin(), res.end());
            }
        }
        sort_heap(res.begin(), res.end());
        return res;
    }
};
```

## 快速排序思想

- 随机快速排序思想启发：随机选择一数字，调整数字次序，使比该数小的都在该数左侧，反之右侧

```cpp
class Solution {
    int Partition(vector<int>& input, int low, int high) {
        int pivot = input[low];
        while(low<high) {
            while(low<high && pivot<input[high]) high--;
            swap(input[low], input[high]);
            while(low<high && input[low]<=pivot) low++;
            swap(input[low], input[high]);
        }
        return low;
    }
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        if(input.empty() || input.size()<k || k<=0) return vector<int>();
        int start = 0, end = input.size()-1;
        int idx = Partition(input, start, end);
        while(idx != (k-1)) {
            if(idx > (k-1)) {
                end = idx - 1;
                idx = Partition(input, start, end);
            }
            else {
                start = idx + 1;
                idx = Partition(input, start, end);
            }
        }
        vector<int>res(input.begin(), input.begin()+k);
        return res;
    }
};
```

## 红黑树  

- multiset集合  利用仿函数改变排序顺序 时间复杂度O（nlogk）

```cpp
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        if(input.empty() || input.size()<k || k<=0) return vector<int>();
        multiset<int, greater<int> > min_k_set(input.begin(), input.begin()+k);// greater<T>模板
        for(vector<int>::iterator vec_iter = input.begin()+k;
            vec_iter != input.end();
            vec_iter++) {
            multiset<int>::iterator max_iter = min_k_set.begin();
            if(*vec_iter < *(min_k_set.begin())) {
                min_k_set.erase(max_iter);
                min_k_set.insert(*vec_iter);
            }
        }
        return vector<int>(min_k_set.begin(), min_k_set.end());
    }
};
```
