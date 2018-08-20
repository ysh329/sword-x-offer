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
        // method3 O(log2N) 最大堆
        if(input.size()<k || input.empty() || k<=0) return vector<int>();
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

## Partiton

```cpp
class Solution {
public:
    void swap(int &fir,int &sec) {
        int temp = fir;
        fir = sec;
        sec = temp;
    }
    int getPartition(vector<int> &input,int start,int end) {
        if(input.empty() || start>end) return -1;
        int temp = input[end];
        int j = start - 1;
        for(int i=start;i<end;++i) {
            if(input[i]<=temp) {
                ++j;
                if(i!=j) swap(input[i],input[j]);                   
            }
        }
        swap(input[j+1],input[end]);
        return (j+1);
    }
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        vector<int> result;       
        if(input.empty() || k>input.size() || k<=0) return result;
         
        int start = 0;
        int end = input.size()-1;
        int index = getPartition(input,start,end);
         
        while(index != (k-1)) {
            if(index > (k-1)) {
                end = index - 1;
                index = getPartition(input,start,end);
            }
            else {
                start = index + 1;
                index = getPartition(input,start,end);
            }
        }
         
        for(int i=0;i<k;++i)
            result.push_back(input[i]);
        return result;
    }
};
```

## 红黑树  

- multiset集合  利用仿函数改变排序顺序 时间复杂度O（nlogk）

```cpp
class Solution {
public:
    vector<int> GetLeastNumbers_Solution(vector<int> input, int k) {
        int len=input.size();
        if(len<=0||k>len) return vector<int>();
        //仿函数中的greater<T>模板，从大到小排序
        multiset<int, greater<int> > leastNums;
        vector<int>::iterator vec_it=input.begin();
        for(;vec_it!=input.end();vec_it++) {
            //将前k个元素插入集合
            if(leastNums.size()<k)
                leastNums.insert(*vec_it);
            else {
                //第一个元素是最大值
                multiset<int, greater<int> >::iterator greatest_it=leastNums.begin();
                //如果后续元素<第一个元素，删除第一个，加入当前元素
                if(*vec_it<*(leastNums.begin())) {
                    leastNums.erase(greatest_it);
                    leastNums.insert(*vec_it);
                }
            }
        }
        return vector<int>(leastNums.begin(),leastNums.end());
    }
};
```
