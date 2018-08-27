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
    int GetFirstKIdx(vector<int>& data, int k, int start, int end) {
        if(start>end) return -1;
        int mid = (start+end)/2;
        if(data[mid]==k) {
            if(mid==0 || (mid>0 && data[mid-1]!=k))
                return mid;
            else
                end = mid - 1;
        }
        else if(data[mid]>k) end = mid - 1;
        else start = mid + 1;//data[mid]<k 
        return GetFirstKIdx(data, k, start, end);
    }
    int GetLastKIdx(vector<int>& data, int k, int start, int end) {
        if(start>end) return -1;
        int mid = (start+end)/2;
        if(data[mid]==k) {
            if(mid==data.size()-1 || (mid+1<data.size() && data[mid+1]!=k))
                return mid;
            else
                start = mid + 1;
        }
        else if(data[mid]>k) end = mid - 1;
        else start = mid + 1;//data[mid]<k
        return GetLastKIdx(data, k, start, end);
    }
public:
    int GetNumberOfK(vector<int> data, int k) {
        int count = 0;
        if(data.empty()) return count;
        int FirstKIdx = GetFirstKIdx(data, k, 0, data.size()-1);
        int LastKIdx = GetLastKIdx(data, k, 0, data.size()-1);
        if(FirstKIdx!=-1 && LastKIdx!=-1) count = LastKIdx-FirstKIdx+1;
        return count;
    }
};
```

### 非递归

```cpp
class Solution {
    int GetFirstKIdx(vector<int>& data, int k, int len) {
        if(data.empty()) return -1;
        int mid = -1, start = 0;
        int end = len - 1;
        while(start<=end) {
            mid = (start+end)/2;
            if(data[mid]==k) {
                if(mid==0 || 
                   (mid-1>=0 && data[mid-1]!=k)) {
                    break;
                }
                end = mid-1;
            }
            else if(data[mid]>k) end = mid - 1;
            else start = mid + 1;// data[mid]<k
        }
        return start>end?-1:mid;
    }
    int GetLastKIdx(vector<int>& data, int k, int len) {
        if(data.empty()) return -1;
        int mid = -1, start = 0;
        int end = len-1;
        while(start<=end) {
            mid = (start+end)/2;
            if(data[mid]==k) {
                if(mid==len-1 ||
                  (mid+1<len && data[mid+1]!=k))
                    break;
                start = mid + 1;
            }
            else if(data[mid]>k) end = mid - 1;
            else start = mid + 1 ;//data[mid]<k
        }
        return start>end?-1:mid;
    }
public:
    int GetNumberOfK(vector<int> data, int k) {
        int count = 0;
        int FirstKIdx = GetFirstKIdx(data, k, data.size());
        int LastKIdx = GetLastKIdx(data, k, data.size());
        if(FirstKIdx!=-1 && LastKIdx!=-1)
            count = LastKIdx-FirstKIdx+1;
        return count;
    }
};
```
