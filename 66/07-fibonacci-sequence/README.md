# 斐波那契数列

大家都知道斐波那契数列，现在要求输入一个整数n，请你输出斐波那契数列的第n项（从0开始，第0项为0）。
n<=39

## 不能通过的解法

```cpp
class Solution {
public:
    int Fibonacci(int n) {
        if(n<0)
            return 0;
        else if(n<=1)
            return n;
        else
            return Fibonacci(n-1)+Fibonacci(n-2);
    }
};
```

## 正序求解

```cpp
class Solution {
public:
    int Fibonacci(int n) {
        int result = 0;
        int n0 = 0;
        int n1 = 1;
        // f(0) = 0
        // f(1) = 1
        // f(n) = f(n-1) + f(n-2)
        for(int i = 0; i<=n; i++)
        {
            if(i==0)
                result = n0;
            else if(i==1)
                result = n1;
            else
            {
                result = n1+n0;
                n0 = n1;
                n1 = result;
            }
        }
        return result;
    }
};
```

## 动态规划
