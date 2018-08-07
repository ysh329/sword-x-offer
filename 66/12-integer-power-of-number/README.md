# 数值的整数次方

给定一个double类型的浮点数base和int类型的整数exponent。求base的exponent次方。

## 分析

- exponent为负数  
- exponent为0  
- base为负数  
- base为0  

## 常规解法

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
