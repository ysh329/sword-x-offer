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
