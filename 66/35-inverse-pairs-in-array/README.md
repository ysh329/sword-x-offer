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

```cpp
class Solution {
public:
    int InversePairs(vector<int> data) {
        long long count = 0;
        if(data.empty() || data.size()==1) return count;
        vector<int> copy;
        for (int eidx=0; eidx<data.size(); eidx++)
            copy.push_back(data[eidx]);
        count = InversePairsCore(data, copy, 0, data.size()-1);
        return count%1000000007;
    }

    long long InversePairsCore(vector<int> &data,vector<int> &copy,int start,int end) {
       if(start==end) {
            copy[start] = data[start];
            return 0;
       }
       int length = (end-start)/2;
       long long left = InversePairsCore(copy,data,start,start+length);
       long long right = InversePairsCore(copy,data,start+length+1,end); 
        
       int i = start+length;
       int j = end;
       int indexcopy = end;
       long long count = 0;
       while(i>=start && j>=start+length+1) {
             if(data[i] > data[j]) {
                  copy[indexcopy--] = data[i--];
                  count = count+j-start-length;//count=count+j-(start+length+1)+1;
             }
             else
                  copy[indexcopy--] = data[j--];
       }
       for(; i>=start; i--)
           copy[indexcopy--] = data[i];
       for(; j>=start+length+1; j--)
           copy[indexcopy--] = data[j];
       return left+right+count;
    }
};
```

下面代码没通过：
```cpp
class Solution {
    long long mergeSort(vector<int>& copy, vector<int>& data, int start, int end) {
        if(start==end) {
            copy[start] = data[start];
            return 0;
        }
        int mid = (start+end)/2;
        int left = mergeSort(copy, data, start, mid);
        int right = mergeSort(copy, data, mid+1, end);
        int low = mid, high = end, idx_copy = end;
        long long count = 0;
        while(low>=start && high>=mid+1) {
            if(data[low] > data[high]) {
                count = count + (high-mid);
                copy[idx_copy--] = data[low--];
            }
            else
                copy[idx_copy--] = data[high--];
        }
        for(; low>=start; low--) copy[idx_copy--] = data[low];
        for(; high>=mid+1; high--) copy[idx_copy--] = data[high];
        return left+right+count;
    }
public:
    int InversePairs(vector<int> data) {
        long long count = 0;
        if(data.size()<=1) return count;
        vector<int> copy(data.begin(), data.end());
        count = mergeSort(copy, data, 0, data.size()-1);
        return count%1000000007;
    }
};
```

```cpp
class Solution {
    int cnt=0;
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
        if(array.empty()) return 0;
        mergeUp2Down(array, 0, array.size()-1);
        return cnt;
    }
};
```

```cpp
class Solution {
public:
    int InversePairs(vector<int> data) {
        return mergeSort(data, 0, data.size() - 1);
    }
private:
    long long mergeSort(vector<int> &data, int s, int e) {
        if (s >= e) return 0;
        int mid = (e - s) / 2 + s;
        long long num = mergeSort(data, s, mid) + mergeSort(data, mid + 1, e);
        int i = s, j = mid + 1;
        int cnt = 0;
        vector<int> tmp;
        while (i <= mid || j <= e) {
            if (i > mid) {
                tmp.push_back(data[j++]);
            }
            else if (j > e) {
                num += cnt;
                tmp.push_back(data[i++]);
            }
            else if (data[i] > data[j]) {
                cnt++;
                tmp.push_back(data[j++]);
            }
            else {
                num += cnt;
                tmp.push_back(data[i++]);
            }
        }
        for (int i = s; i <= e; ++i) {
            data[i] = tmp[i - s];
        }
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
