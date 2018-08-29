# 数组中的逆序对

在数组中的两个数字，如果前面一个数字大于后面的数字，则这两个数字组成一个逆序对。输入一个数组,求出这个数组中的逆序对的总数P。并将P对1000000007取模的结果输出。 即输出P%1000000007

```
题目保证输入的数组中没有的相同的数字
数据范围：
	对于%50的数据,size<=10^4
	对于%75的数据,size<=10^5
	对于%100的数据,size<=2*10^5
  
输入：1,2,3,4,5,6,7,0  
输出：7
```

## 超时的方法

```cpp
class Solution {
public:
    int InversePairs(vector<int> data) {
        int res = 0;
        for(int i=0; i<data.size()-1; i++)
            for(int j=i+1; j<data.size(); j++)
                if(data[i]>data[j]) res++;
        return res%1000000007;
    }
};
```

## 归并排序

### 归并排序1

```cpp
class Solution {
    long long InversePairsCore(vector<int> &data,vector<int> &copy,int start,int end) {
        if(start==end) {
            copy[start] = data[start];
            return 0;
        }

        int mid = (end+start) >> 1;
        long long count = InversePairsCore(copy, data, start, mid) + 
	                  InversePairsCore(copy, data, mid+1, end);
        int lo = mid; int hi = end; int indexcopy = end;
        long long count = 0;

        while(lo>=start && hi>=mid+1) {
            if(data[lo] > data[hi]) {
                copy[indexcopy--] = data[lo--];
                count = count+hi-mid;
            }
            else copy[indexcopy--] = data[hi--];
        }
        for(; lo>=start; lo--) copy[indexcopy--] = data[lo];
        for(; hi>=mid+1; hi--) copy[indexcopy--] = data[hi];
        return left+right+count;
    }
public:
    int InversePairs(vector<int> data) {
        long long count = 0;
        if(data.size()<=1) return count;
        vector<int> copy(data.begin(), data.end());
        count = InversePairsCore(data, copy, 0, data.size()-1);
        return count%1000000007;
    }
};
```

### 归并排序2

```cpp
class Solution {
    int cnt = 0;
    void mergeUp2Down(vector<int>& a, int start, int end) {
        if(start>=end) return;
        int mid = (end+start)>>1;
        mergeUp2Down(a, start, mid);
        mergeUp2Down(a, mid+1, end);
        merge(a, start, mid, end);
    }
    void merge(vector<int>& a, int start, int mid, int end){
        vector<int> temp(end-start+1);
        int i=start, j=mid+1, index=0;
        while(i<=mid && j<=end){
            if(a[i] > a[j]) {
                temp[index++] = a[j++];
                cnt += mid-i+1;
                cnt = cnt>1000000007 ? cnt%1000000007 : cnt;
            }else temp[index++] = a[i++];
        }
        while(i<=mid) temp[index++]=a[i++];
        while(j<=end) temp[index++]=a[j++];
        for(int k=0;k<(int)temp.size();k++) a[start+k]=temp[k];
    }
public:
    int InversePairs(vector<int> array) {
        if(array.size()<=1) return 0;
        mergeUp2Down(array, 0, array.size()-1);
        return cnt;
    }
};
```

### 归并排序3

```cpp
class Solution {
public:
    int InversePairs(vector<int> data) {
        return mergeSort(data, 0, data.size() - 1);
    }
private:
    long long mergeSort(vector<int> &data, int s, int e) {//s:start, e:end
        if(s >= e) return 0;
        int mid = (e+s) >> 1;
        long long num = mergeSort(data, s,   mid) +
	                    mergeSort(data, mid+1, e);
        int lo = s, hi = mid + 1;//从左往右
        int cnt = 0;
        vector<int> sorted;
        while(lo <= mid || hi <= e) {
            if(lo > mid)//左边插完了，插右边剩下的
                sorted.push_back(data[hi++]);
            else if(hi > e) {//右边插完了，插左边剩下的
                num += cnt;
                sorted.push_back(data[lo++]);
            }
            else if(data[lo] > data[hi]) {
                cnt++;
                sorted.push_back(data[hi++]);
            }
            else {
                num += cnt;
                sorted.push_back(data[lo++]);
            }
        }
        for(int i = s; i <= e; ++i)
            data[i] = sorted[i - s];
        return num%1000000007;
    }
};
```

## 树状数组

- [掌握树状数组~彻底入门 - 霜雪千年 - 博客园](https://www.cnblogs.com/acgoto/p/8583952.html#4046752)

```cpp
#define lb(x) ((x) & -(x))
typedef long long ll;
class BIT{
public:
    int n;
    vector<int> a;
    BIT(int n) : n(n), a(n + 1, 0){}
    void add(int i, int v){
        for(; i <= n; i += lb(i)) a[i] += v;
    }
    ll sum(int i){
        int r = 0;
        for(; i; i -= lb(i)) r += a[i];
        return r;
    }
};

class Solution {
public:
    int InversePairs(vector<int> d) {
        int n = d.size();
        vector<int> t(d);
        sort(t.begin(), t.end()); //排序
        t.erase(unique(t.begin(), t.end()), t.end()); //去除重复元数（当然这题没有重复元素）
        for(int i = 0; i < n; ++i) //离散化
            d[i] = lower_bound(t.begin(), t.end(), d[i]) - t.begin() + 1;
        BIT bit(t.size());
        ll ans = 0;
        for(int i = n - 1; i >= 0; --i){
            ans += bit.sum(d[i] - 1);
            bit.add(d[i], 1);
        }
        return ans % 1000000007;
    }
};
```
