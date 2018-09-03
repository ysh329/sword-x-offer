# 求1+2+3+...+n

求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

## 位运算

```cpp
class Solution {
public:
    int Sum_Solution(int n) {
        bool a[n][n+1];//(1+n)*n/2, sizeof(bool)=1
        return sizeof(a)>>1;
    }
};
```
