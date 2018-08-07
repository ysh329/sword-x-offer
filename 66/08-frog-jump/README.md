# 跳台阶

一只青蛙一次可以跳上1级台阶，也可以跳上2级。求该青蛙跳上一个n级的台阶总共有多少种跳法（先后次序不同算不同的结果）。

## 不推荐的解法

```cpp
class Solution {
public:
    int jumpFloor(int number) {
        // 推导出规律：和斐波那契数列一致
        // f(0) = 0
        // f(1) = 1
        // f(2) = 2
        // f(3) = 3, [1,1,1],[1,2],[2,1]
        if(0<=number && number<=3)
            return number;
        else if(number<0)
            return 0;
        else
            return jumpFloor(number-1) + jumpFloor(number-2);
        
    }
};
```

## 推荐解法

参考05-fibonacci-sequence，斐波那契数列。

