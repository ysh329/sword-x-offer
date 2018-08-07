# 矩形覆盖

我们可以用2\*1的小矩形横着或者竖着去覆盖更大的矩形。请问用n个2\*1的小矩形无重叠地覆盖一个2\*n的大矩形，总共有多少种方法？

## 分析

```
n  方法数  方法
0	 0				
1	 1	     |			
2	 2       ||	=		
3	 3	     |||	`=|		
4	 5	     ||||	||=	`==	
5	 8	     |||||	`=|||	`==|	
6	13	     ||||||	`=||||	`==||	`===
...
```

得到

- f(0) = 0  
- f(1) = 1  
- f(2) = 2  
- f(3) = 3
......

总结：  
- f(n) = n, 0<=n<=3  
- f(n) = f(n-1) + f(n-2), n>3

## 递推式

```cpp
class Solution {
public:
    int rectCover(int number) {
        if(0<=number && number<=2)
            return number;
        else if(number<0)
            return 0;
        else
            return rectCover(number-1)+rectCover(number-2);
    }
};
```

## 其它解法

略，参考07-fibonacci-sequence，斐波那契数列。
