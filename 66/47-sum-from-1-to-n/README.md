# 求1+2+3+...+n

求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

## 二位数组+sizeof

[Using the GNU Compiler Collection (GCC): Variable Length](https://gcc.gnu.org/onlinedocs/gcc/Variable-Length.html)

Variable-length automatic arrays are allowed in ISO C99, and as an extension GCC accepts them in C90 mode and in C++. These arrays are declared like any other automatic arrays, but with a length that is not a constant expression. The storage is allocated at the point of declaration and deallocated when the block scope containing the declaration exits.

```cpp
class Solution {
public:
    int Sum_Solution(int n) {
        bool a[n][n+1];//(1+n)*n/2, sizeof(bool)=1
        return sizeof(a)>>1;
    }
};
```


## 短路运算

```cpp

```

## 递归

递归代替循环

```cpp
class Solution {
public:
    int Sum_Solution(int n) {
        int sum = n;
        sum += sum>0 ? Sum_Solution(--n) : 0;
        return sum;
    }
};
```
