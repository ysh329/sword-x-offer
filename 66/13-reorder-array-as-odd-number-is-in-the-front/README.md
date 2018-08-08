# 调整数组顺序使奇数位于偶数前面

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

## 开辟新数组

- 时间换空间  
- 时间：O(N)，空间：O(N)  

```cpp
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        if(array.size()<=1)
            return;
        vector<int> result;
        for(int eidx=0; eidx<array.size(); eidx++)
            if((array[eidx]&0x1)==1)
                result.push_back(array[eidx]);
        for(int eidx=0; eidx<array.size(); eidx++)
            if((array[eidx]&0x1)==0)
                result.push_back(array[eidx]);
        array = result;
        return;
    }
};
```

## 冒泡

```cpp
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        for (int loop_num = 0; loop_num<array.size(); loop_num++)
        {
            for (int i = array.size()-1; i>loop_num; i--)
            {
                // swap, if pre is even, latter is odd
                if (array[i]%2!=0 && array[i-1]%2==0) 
                    swap(array[i], array[i-1]);
            }
        }
    }
};        
```

```cpp
class Solution {
public:
    void reOrderArray(vector<int> &array) {
        for(int i = 0; i < (array.size())/2; i++)
            for(int j = 0; j < (array.size()-i); j++)
                if(((array[j]%2) == 0) && ((array[j+1]%2) != 0))
                    swap(array[j], array[j+1]);
    }
};        
```

```cpp
class Solution {
public:
    static bool isOk(int n){ return ((n & 1) == 1); }

    void reOrderArray(vector<int> &array) {
        stable_partition(array.begin(), array.end(), isOk);
    }
};
```

- `partition`和`stable_partition`这两个方法都用来将指定容器的元素根据指定的`predicate`函数分成两个子序列，其中满足`predicate()`函数的，即返回值为`true`的作为第一个序列`[v.begin(), bound)`, 而`[bound, v.end())`的作为第二个序列。  
- 两个方法的区别在于，`partition()`对于两个子序列中的元素并不排序，而`stable_partition()`则对两个子序列的元素也进行排序。  
- BidirectionalIterator partition ( BidirectionalIterator first, BidirectionalIterator last, Predicate pred );  
- BidirectionalIterator stable_partition ( BidirectionalIterator first, BidirectionalIterator last, Predicate pred );
