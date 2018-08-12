# 顺时针打印矩阵

输入一个矩阵，按照从外向里以顺时针的顺序依次打印出每一个数字，例如，如果输入如下4 X 4矩阵： 
```
 1  2  3  4
 5  6  7  8
 9 10 11 12
13 14 15 16
```

则依次打印出数字1,2,3,4,8,12,16,15,14,13,9,5,6,7,11,10.

## 常规解法 - 上右下左四部分 分别按圈取值  

- 剑指offer

```cpp
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> r;
        if(matrix.empty() || matrix[0].empty()) return r;
        int rows = matrix.size();
        int cols = matrix[0].size();
        int circle_idx = 0;
        while(circle_idx*2<rows && circle_idx*2<cols)
        {
            vector<int> circle_result = printMatClockWisely(matrix,
                                                            rows,
                                                            cols,
                                                            circle_idx);
            //for(int i=0; i<circle_result.size(); i++)
            //    r.push_back(circle_result[i]);
            r.insert(std::end(r),
                     std::begin(circle_result),
                     std::end(circle_result));
            circle_idx++;
        }
        return r;
    }
    
    vector<int> printMatClockWisely(vector<vector<int> > &mat, int rows, int cols, int circle_idx)
    {
        vector<int> circle_r;
        if(mat.empty() || mat[0].empty()) return circle_r;
        int endX = cols - circle_idx - 1;
        int endY = rows - circle_idx - 1;
        // left to right
        for(int i=circle_idx; i<=endX; i++)
            circle_r.emplace_back(mat[circle_idx][i]);
        // top to bottom
        if(circle_idx<endY)
            for(int i=circle_idx+1; i<=endY; i++)
                circle_r.emplace_back(mat[i][endX]);
        // right to left
        if(circle_idx<endX && circle_idx<endY)
            for(int i=endX-1; i>=circle_idx; i--)
                circle_r.emplace_back(mat[endY][i]);
        // bottom to top
        if(circle_idx<endX && circle_idx<endY-1)
            for(int i=endY-1; i>circle_idx; i--)
                circle_r.emplace_back(mat[i][circle_idx]);
        return circle_r;
    }
};
```

- 牛客网:先算圈数  

```cpp
/*解题思路：顺时针打印就是按圈数循环打印，一圈包含两行或者两列，在打印的时候会出现某一圈中只包含一行，要判断从左向右打印和从右向左打印的时候是否会出现重复打印，同样只包含一列时，要判断从上向下打印和从下向上打印的时候是否会出现重复打印的情况*/
class Solution {
public:
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int>res;
        res.clear();
        int row=matrix.size();//行数
        int collor=matrix[0].size();//列数
        //计算打印的圈数
        int circle=((row<collor?row:collor)-1)/2+1;//圈数
        for(int i=0;i<circle;i++){
            //从左向右打印
            for(int j=i;j<collor-i;j++)
                res.push_back(matrix[i][j]);         
            //从上往下的每一列数据
            for(int k=i+1;k<row-i;k++)
                res.push_back(matrix[k][collor-1-i]);
            //判断是否会重复打印(从右向左的每行数据)
            for(int m=collor-i-2;(m>=i)&&(row-i-1!=i);m--)
                res.push_back(matrix[row-i-1][m]);
            //判断是否会重复打印(从下往上的每一列数据)
            for(int n=row-i-2;(n>i)&&(collor-i-1!=i);n--)
                res.push_back(matrix[n][i]);}
        return res;
    }
};
```

- 牛客网

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

## 模拟魔方旋转

```cpp
class Solution {
public:
    vector<vector<int> > turnMatrix(vector<vector<int> > &mat)
    {
        vector<vector<int> > new_mat;
        int rows = mat.size();
        int cols = mat[0].size();
        for(int c = cols-1; c >= 0; c--)
        {
            vector<int> new_row;
            for(int r = 0; r < rows; r++)
                new_row.emplace_back(mat[r][c]);
            new_mat.emplace_back(new_row);
        }
        return new_mat;
    }
    
    vector<int> printMatrix(vector<vector<int> > matrix) {
        vector<int> result;
        while(!matrix.empty() || !matrix[0].empty())
        {
            result.insert(result.end(),
                          matrix[0].begin(),
                          matrix[0].end());
            matrix.erase(matrix.begin());
            if(matrix.empty() || matrix[0].empty())
                break;
            matrix = turnMatrix(matrix);
        }
        return result;
    }
};
```

## python

```python
链接：https://www.nowcoder.com/questionTerminal/9b4c81a02cd34f76be2659fa0d54342a
来源：牛客网

class Solution:
 
    def printMatrix(self, matrix):
        res = []
        while matrix:
            res += matrix.pop(0)
            if matrix and matrix[0]:
                for row in matrix:
                    res.append(row.pop())
            if matrix:
                res += matrix.pop()[::-1]
            if matrix and matrix[0]:
                for row in matrix[::-1]:
                    res.append(row.pop(0))
        return res
```
