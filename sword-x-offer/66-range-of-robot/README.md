# 机器人的运动范围

- 地上有一个m行和n列的方格。一个机器人从坐标0,0的格子开始移动，每一次只能向左，右，上，下四个方向移动一格，但是不能进入行坐标和列坐标的数位之和大于k的格子。  
- 例如，当k为18时，机器人能够进入方格（35,37），因为3+5+3+7 = 18。但是，它不能进入方格（35,38），因为3+5+3+8 = 19。请问该机器人能够达到多少个格子？

```cpp
class Solution {
public:
    int dfs(int threshold, int rows, int cols, int r, int c, int* flags) {
        if(0>r || r>=rows ||
           0>c || c>=cols ||
           (!enter(threshold, r, c)) ||
           flags[r*cols+c]==1)
            return 0;
        flags[r*cols+c] = 1;
        return (dfs(threshold, rows, cols, r-1, c, flags) +
                dfs(threshold, rows, cols, r+1, c, flags) +
                dfs(threshold, rows, cols, r, c-1, flags) +
                dfs(threshold, rows, cols, r, c+1, flags))+1;
    }
    
    int movingCount(int threshold, int rows, int cols) {
        if(threshold<=0 || rows<=0 || cols<=0)
            return 0;
        int *flags = (int *)calloc(rows*cols, sizeof(int));
        int count = dfs(threshold, rows, cols, 0, 0, flags);
        free(flags);
        return count;
    }
    
    bool enter(int threshold, int r, int c) {
        int sum = 0;
        while(r) { sum+=r%10; r/=10;}
        while(c) { sum+=c%10; c/=10;}
        return sum<=threshold ? true : false;
    }
};
```
