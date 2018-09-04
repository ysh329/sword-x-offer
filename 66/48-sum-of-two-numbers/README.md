# 不用加减乘除做加法

写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

## 二进制

### 循环

```cpp
class Solution {
public:
    int Add(int num1, int num2) {
        while(num2) {
            int bit_sum = num1 ^ num2; //1.位异或，计算各位相加不进位
            num2 = (num1 & num2) << 1; //2.位于，计算进位并左移
            num1 = bit_sum; //3.直到while的判断num2==0,即没有进位
        }
        return num1;
    }
};
```

### 递归

```cpp
class Solution {
public:
    int Add(int num1, int num2) {
        return num2 ? Add(num1^num2, (num1&num2)<<1) : num1;
        // num1 = num1^num2, 即不进位的按位相加结果
        // num2 = (num1&num2)<<1，即相加进位的结果
        // 首先判断num2是否为0，为0则返回num1，不为0则递归地继续计算
    }
};
```

##

```cpp
class Solution {
public:
    int Add(int num1, int num2) { //a是char数组，把num1的值赋给a的首地址，再取a[num2]的地址
        char* a = reinterpret_cast<char*>(num1);
        return reinterpret_cast<long>(&(a[num2]));
        // reinterpret_cast<type-id> (expression)
        // type-id 必须是一个指针、引用、算术类型、函数指针或者成员指针。
        // 它可以把一个指针转换成一个整数，也可以把一个整数转换成一个指针
        //（先把一个指针转换成一个整数，再把该整数转换成原类型的指针，还可以得到原先的指针值）。
    }
};
```
