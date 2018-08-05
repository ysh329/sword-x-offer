# 二维数组中的查找

在一个二维数组中（每个一维数组的长度相同），每一行都按照从左到右递增的顺序排序，每一列都按照从上到下递增的顺序排序。请完成一个函数，输入这样的一个二维数组和一个整数，判断数组中是否含有该整数。

## 顺序查找

- O(cols \* rows)

```cpp
class Solution {
public:
    //二刷 2018年8月5日
    //method1 顺序查找 O(cols*rows)
    /*    
    bool Find(int target, vector<vector<int> > array) {
        
        bool result = false;
        if(array.size()==0 || array[0].size()==0)
            return result;
        int rows = array.size();
        int cols = array[0].size();
        int r = 0;
        int c = cols - 1;
        while(0<=r&&r<rows && 0<=c&&c<cols)
        {
            if(array[r][c]==target)
            {
                result = true;
                break;
            }
            else if(array[r][c]>target)
                c--;
            else
                r++;
        }
        return result;
        
    }
    */
```   
## 二分查找

- O(rows \* log_2{cols})

```cpp
class Solution {
public:
    bool Find(int target, vector<vector<int> > array) {
        bool result = false;
        if(array.empty() || array[0].size()==0)
            return result;
        for(int row_idx = 0; row_idx < array.size() && !result; row_idx++)
        {
            int low = 0;
            int high = array[row_idx].size() - 1;
            while(low<=high)
            {
                int midx = (low+high)/2;
                if(array[row_idx][midx]==target)
                {
                    result = true;
                    break;
                }
                else if(array[row_idx][midx]>target)
                    high = midx - 1;
                else
                    low = midx + 1;
            }
        }
        return result;
    }
};
