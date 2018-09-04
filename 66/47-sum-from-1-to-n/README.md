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

## pow

```cpp
class Solution {
public:
    int Sum_Solution(int n) {
        return (int)(pow(n, 2) + n) >> 1;//计算n的2次幂
    }
};
```


## 短路运算

在谈&&和||两个运算符的短路运算之前，先看一段程序：
```c
#include <stdio.h> 
int main() {
    int para1 = 1, para2 = 2, para3 = 3, para4 = 4;
    int r1 = 1, r2 =1;
    (r1 = para2 < para1) && (r2 = para3 > para4);
    printf("r1 = %d, r2 = %d\n", r1, r2);
 
    r1 = 1; r2 = 1;
    (r1 = para2 > para1) || (r2 = para4 < para3);
    printf("r1 = %d, r2 = %d\n", r1, r2);
    return 0;
}
```
上面程序运行后，r1和r2分别是多少？

有不少同学认为第一个printf函数应该会输出： 
r1 = 0, r2 = 0
因为para2 < para1为假，所以r1为0，para3 > para4也为假，所以r2也为0.

第二个printf函数应该会输出：  
r1 = 1, r2 = 0
因为 para2 > para1为真，所以r1为1，para4 < para3为假，所以r2为0.

实际运行结果如下：![img](./and_or.png)

实际运行结果与大家想的不一样，原因在于&&和||运算符有一个“短路”的概念。  
先来看：(r1 = para2 < para1) && (r2 = para3 > para4);  
para2 < para1为假，所以r1为0，这个很好理解。此时&&左边就是0，那么不管&&右边是0还是1，也就是不管&&右边是假还是真，整个&&表达式就已经是假了，程序再去运行&&右边的表达式就没有意义了，所以程序不会去运行 (r2 = para3 > para4)，也就是r2的值还是为1。当然，如果&&左边是1的话，那么程序还会继续执运行(r2 = para3 > para4)的，此时r2的值就为0了；  

再来看： (r1 = para2 > para1) || (r2 = para4 < para3);  
 para2 > para1为真，所以r1为1，这个很好理解。此时||左边就是1，那么不管||右边是0还是1，也就是不管||右边是假还是真，整个||表达式就已经是真了，程序再去运行||右边的表达式就没有意义了，所以程序不会去运行 (r2 = para4 < para3)，也就是r2的值还是为1；当然，如果||左边是0的话，那么程序还会继续执运行(r2 = para4 < para3)的，此时r2的值就为0了；  

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
