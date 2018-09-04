# 不用加减乘除做加法

写一个函数，求两个整数之和，要求在函数体内不得使用+、-、*、/四则运算符号。

## 二进制

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
