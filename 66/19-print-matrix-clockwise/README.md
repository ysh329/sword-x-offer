# 顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 1 2 3 4 5 6 7 8 9 10 11 12 13 14 15 16 则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

## 上右下左四部分依次打印

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

## 控制方向判断转弯

```cpp
class Solution {
public:
    int cols, rows;
    vector<vector<int>> v;
    
    bool judgeForward(int i, int j)
    {
        return 0<=i && i<rows &&
               0<=j && j<cols &&
               !v[i][j];
    }
    
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> result;
        if(matrix.size()==0 || matrix[0].size()==0) return result;
        
        rows = matrix.size(); cols = matrix[0].size();
        v = vector<vector <int>>(rows, vector<int>(cols, false));
        const int D[4][2] = {{0,1}, {1,0}, {0,-1}, {-1,0}};
        
        int i=0, j=0, d=0, T=rows*cols;
        while(T--) //T--，不是--T，考虑仅一个元素的矩阵
        {
            result.push_back(matrix[i][j]);
            v[i][j] = true;
            if(!judgeForward(i+D[d][0], j+D[d][1])) (++d)%=4;// 转弯
            i += D[d][0]; j += D[d][1]; // 继续前进
        }
        return result;
    }
};
```
