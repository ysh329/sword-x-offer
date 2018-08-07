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

## 尾递归

- 链接：https://www.nowcoder.com/questionTerminal/c6c7742f5ba7442aada113136ddea0c3  
- 来源：牛客网  
- 不是不能用递归，递归本质上是栈，可能导致栈溢出，只要避免溢出就可以了。对于尾递归来说，由于只存在一个调用帧，所以永远不会发生“栈溢出”错误。
这种方法的优势在于，无论n取多大，永远不会出现栈溢出。

```cpp
class Solution {
public:

    int Fibonacci(int n) {
        return Fibonacci(n,0,1);
    }

    int Fibonacci(int n,int acc1,int acc2){
        if(n==0) return 0;
        if(n==1) return acc2;
        else     return Fibonacci(n - 1, acc2, acc1 + acc2);
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

- 链接：https://www.nowcoder.com/questionTerminal/c6c7742f5ba7442aada113136ddea0c3  
- 来源：牛客网

```cpp
class Solution {
public:
    int Fibonacci(int n) {
        int f = 0, g = 1;//f:当前n的结果,g:n+1结果
        while(n-->0) {
            g += f;
            f = g - f;
        }
        return f;
    }
};
```

## 矩阵快速幂

- 链接：https://www.nowcoder.com/questionTerminal/c6c7742f5ba7442aada113136ddea0c3  
- 来源：牛客网

略
