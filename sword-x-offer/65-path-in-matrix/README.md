# 矩阵中的路径

- 请设计一个函数，用来判断在一个矩阵中是否存在一条包含某字符串所有字符的路径。  
- 路径可以从矩阵中的任意一个格子开始，每一步可以在矩阵中向左，向右，向上，向下移动一个格子。  
- 如果一条路径经过了矩阵中的某一个格子，则之后不能再次进入这个格子。   
- 例如 a b c e s f c s a d e e 这样的3 X 4 矩阵中包含一条字符串"bcced"的路径，但是矩阵中不包含"abcb"路径，因为字符串的第一个字符b占据了矩阵中的第一行第二个格子之后，路径不能再次进入该格子。

```cpp
class Solution {
public:
    char mat2d[100][100];
    int vis[100][100] = {0};
    int dr[4] = {-1, 1, 0, 0}, dc[4] = {0, 0, -1, 1};
    int col_2d, row_2d;
    
    void build(char* matrix, int rows, int cols) {
        int count = 0;
        for(int r = 0; r < rows; r++)
            for(int c = 0; c < cols; c++)
                mat2d[r][c] = matrix[count++];
    }

    bool hasPath(char* matrix, int rows, int cols, char* str) {
        if(!matrix || rows<=0 || cols<=0 || !str)
            return false;
        col_2d = cols; row_2d = rows;
        build(matrix, rows, cols);
        memset(vis, 0, sizeof(vis));
        for(int r = 0; r < rows; r++) {
            for(int c = 0; c < cols; c++) {
                if(mat2d[r][c]==str[0] && dfs(r, c, 1, str))
                   return true;
            }
            memset(vis, 0, sizeof(vis));
        }
        return false;
    }
    
    bool dfs(int r, int c, int cur_len, char* str) {
        if(cur_len >= strlen(str))
            return true;
        vis[r][c] = 1;
        for(int i = 0; i < 4; i++) {
            int new_r = r + dr[i];
            int new_c = c + dc[i];
            if(0<=new_c && new_c<col_2d && 0<=new_r && new_r<row_2d &&
               vis[new_r][new_c]==0 && str[cur_len]==mat2d[new_r][new_c])
                if(dfs(new_r, new_c, cur_len+1, str))
                    return true;
        }
        return false;
    }
};
```
