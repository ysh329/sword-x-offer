# 数字在排序数组中出现的次数

统计一个数字在排序数组中出现的次数。

## 顺序查找

```cpp
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        int count = 0;
        for(auto e:data){
            if(e==k) count++;
            else if(e>k) break;
        }
        return count;
    }
};
```

## 二分查找

```cpp
class Solution {
    int GetFirstKIdx(vector<int> &data, int k, int start_idx, int end_idx) {
        if (start_idx>end_idx) return -1;
        int mid_idx = (start_idx+end_idx)/2;
        if (data[mid_idx]==k) {
            if(mid_idx==0 || (mid_idx>0 && data[mid_idx-1]!=k))
                return mid_idx;
            else
                end_idx = mid_idx - 1;
        }
        else {
            if (data[mid_idx]>k)
                end_idx = mid_idx - 1;
            else // data[mid_idx]<k
                start_idx = mid_idx + 1;
        }
        return GetFirstKIdx(data, k, start_idx, end_idx);
    }
    int GetLastKIdx(vector<int> &data, int k, int start_idx, int end_idx) {
        if (start_idx>end_idx) return -1;
        int mid_idx = (start_idx+end_idx)/2;
        if (data[mid_idx]==k) {
            if (mid_idx==data.size()-1 || (mid_idx+1<data.size() && data[mid_idx+1]!=k))
                return mid_idx;
            else
                start_idx = mid_idx + 1;
        }
        else {
            if (data[mid_idx]>k)
                end_idx = mid_idx - 1;
            else // data[mid_idx]<k
                start_idx = mid_idx + 1;
        }
        return GetLastKIdx(data, k, start_idx, end_idx);
    }
public:
    int GetNumberOfK(vector<int> data ,int k) {
        int count = 0;
        if (data.empty()) return count;
        int firstKIdx = GetFirstKIdx(data, k, 0, data.size()-1);
        int lastKIdx = GetLastKIdx(data, k, 0, data.size()-1);
        if (firstKIdx!=-1 && lastKIdx!=-1)
            count = lastKIdx - firstKIdx + 1;
        return count;
    }

};
```
