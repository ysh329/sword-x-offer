# 数值的整数次方

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

## 分析

- exponent为负数  
- exponent为0  
- base为负数  
- base为0  
- base为0，exponent为负的错误处理  

## 累乘

- O(N)

```cpp
class Solution {
public:
    double Power(double base, int exponent) {
        int pos_exponent = abs(exponent);
        double result = 1.0;
        while(pos_exponent)
        {
            pos_exponent--;
            result *= base;
        }
        if(exponent<0) result = 1/result;
        return result;
    }
};
```

```cpp
class Solution {
public:
    double Power(double base, int exponent) {
        double result = 1;
        for (int i = 1; i <= (exponent>0?exponent:-exponent); i++) // 排除exponent为0得情况
        {
            if (exponent>0)
                result *= base;
            else
                result /= base;
        }
        return result;
    }
};
```

## 简单快速幂

- `abs(exponent)`的二进制表示中1的个数，即循环执行次数  

```cpp
class Solution {
public:
    double Power(double base, int exponent) {
        long long pos_exponent = abs((long long)exponent);
        double res = 1.0;
        while(pos_exponent)
        {
            if(pos_exponent & 1) res *= base; // 最低位一致
            base *= base; // 相当于base左移
            pos_exponent >>= 1; //左边的数右移，因而上一步base需要左移
        }
        return exponent<0 ? 1/res : res;
    }
};
```
