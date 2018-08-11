# 顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

## 上下左右依次打印

```cpp
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> result;
        int rows = matrix.size();
        int cols = matrix[0].size();
        if(rows==0 || cols==0) return result;
        
        int left = 0, right = cols-1;
        int top = 0, bottom = rows-1;
        while(top<=bottom && left<=right)
        {
            for(int i=left; i<=right; i++) result.push_back(matrix[top][i]);
            for(int i=top+1; i<=bottom; i++) result.push_back(matrix[i][right]);
            for(int i=right-1; i>=left && top<bottom; i--) result.push_back(matrix[bottom][i]);
            for(int i=bottom-1; i>top && left<right; i--) result.push_back(matrix[i][left]);
            ++top; ++left; --bottom; --right;
        }
        return result;
    }
};
```
