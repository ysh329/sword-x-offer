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

```cpp
class Solution {
public:
    int GetNumberOfK(vector<int> data, int k) {
        return count(data.begin(), data.end(), k);
    }
};
```

## 二分查找

### 递归1

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

### 递归2 巧妙

```cpp
class Solution {
private:
    int binaryFind(vector<int> &data, int begin, int end, int k){
        int idx = -1;
        if(begin>=end) return -1;
        int mid = (end + begin) / 2;
        if(k == data[mid]) return mid;
        if((idx = binaryFind(data,begin,mid,k)) != -1) return idx;
        if((idx = binaryFind(data,mid+1,end,k)) != -1) return idx;
        return -1;
    }
public:
    int GetNumberOfK(vector<int> data, int k) {
        int res = binaryFind(data, 0, data.size(), k);//先找到有k的下标
        if(res == -1) return 0;
        int pos = res, cnt = 1;
        while(--pos >= 0 && k == data[pos]) ++cnt;//对k左侧找
        while(++res < data.size() && k == data[res]) ++cnt;//对k右侧找
        return cnt;
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

### 巧妙的二分

- 链接：https://www.nowcoder.com/questionTerminal/70610bf967994b22bb1c26f9ae901fa2
- 来源：牛客网
- 因为data中都是整数，所以可以稍微变一下，不是搜索k的两个位置，而是搜索k-0.5和k+0.5。这两个数应该插入的位置，然后相减即可。

```cpp
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        return biSearch(data, k+0.5) - biSearch(data, k-0.5) ;
    }
private:
    int biSearch(const vector<int> & data, double num){
        int s = 0, e = data.size()-1;     
        while(s <= e){
            int mid = (e - s)/2 + s;
            if(data[mid] < num)
                s = mid + 1;
            else if(data[mid] > num)
                e = mid - 1;
        }
        return s;
    }
};
```

## 其它

- 利用C++ stl的二分查找

```cpp
class Solution {
public:
    int GetNumberOfK(vector<int> data ,int k) {
        auto resultPair = equal_range(data.begin(), data.end(),k);
        return resultPair.second - resultPair.first;
    }
};
```

- 利用STL的multimap容器底层以红黑树为基础,构造成本O(n),查询成本O(log n)

```cpp
class Solution {
public:
    int GetNumberOfK(vector<int> data, int k) {
        multiset<int> msData(data.begin(), data.end());
        return msData.count(k);
    }
};
```

- 利用STL库函数lower_bound()和upperBound(),O(log n)

```cpp
class Solution {
public:
    int GetNumberOfK(vector<int>& A, int target) {
        if (A.empty()) return 0;
        auto it_lb = std::lower_bound(A.begin(), A.end(), target);
        auto it_ub = std::upper_bound(A.begin(), A.end(), target);
        return (it_ub - it_lb);
    }
};
```
