# 调整数组顺序使奇数位于偶数前面

输入一个整数数组，实现一个函数来调整该数组中数字的顺序，使得所有的奇数位于数组的前半部分，所有的偶数位于数组的后半部分，并保证奇数和奇数，偶数和偶数之间的相对位置不变。

## 开辟新数组

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
