# 变态跳台阶

一只青蛙一次可以跳上1级台阶，也可以跳上2级……它也可以跳上n级。求该青蛙跳上一个n级的台阶总共有多少种跳法。

## 分析

```
// f(目标台阶数) = 跳法数目
// f(0) = 0
// f(1) = 1 = 1 + f(0)
// f(2) = 2 = 1 + f(1) + f(0)
// f(3) = 4 = 1 + f(2) + f(1) + f(0)  [1.1.1],[1,2].[2,1].[3]
// f(4) = 8 = 1 + f(3) + f(2) + f(1) + f(0)  [1.1.1.1],[1,1,2]x3,[2,2]x1,[3,1]x2,[4]
// ...
// f(n-1) = 1 + f(n-2) + ... + f(0)
// f(n)   = 1 + f(n-1) + ... + f(0)
// =================================================================================
// f(n) - f(n-1) ===> f(n) - f(n-1)=f(n-1) ===> f(n)=2f(n-1)
// thus
// f(n) = 0, n=0
//      = 1, n=1
//      = 2f(n-1), n>1
// 2^(n-1)
```

详细分析：https://www.nowcoder.com/questionTerminal/22243d016f6b47f2a6928b4313c85387

## 递推式

```cpp
class Solution {
public:
    int jumpFloorII(int number) {
        if(number<0)
            return -1;
        else if(number==1 || number==0)
            return 1;
        else
            return 2*jumpFloorII(number-1);
    }
};
```

## 规律

```cpp
class Solution {
public:
    int jumpFloorII(int number) {
        int result = 0;
        if (number>0)
            result = pow(2,number-1);
        return result;
    }
};
```

位操作

```cpp
class Solution {
public:
    int jumpFloorII(int number) {
        return 1<<(number-1);
    }
};
```
