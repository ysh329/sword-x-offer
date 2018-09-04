# 求1+2+3+...+n

求1+2+3+...+n，要求不能使用乘除法、for、while、if、else、switch、case等关键字及条件判断语句（A?B:C）。

## 静态构造函数

- 使用辅助类(assist)构造函数中的静态变量N和sum进行累加运算；
- 通过实例化n个辅助类的实例，即通过构造一个以辅助类为类型、大小为n的数组，重复调用此构造函数n次来实现n次的累加运算  

```cpp
class assist {
public:
    assist() {N++;sum += N;}
    static void reset() {N=0;sum=0;}
    static unsigned int GetSum() {return sum;}
private:
    static int N;
    static int sum;
};

int assist::N = 0;//非const的静态值初始化必须在类外
int assist::sum = 0;

class Solution {
public:
    int Sum_Solution(int n) {
        assist::reset();
        
        assist *p = new assist[n];//new了n次，构造函数里的sum从1加到了n
        delete []p;
        p = nullptr;
        
        return assist::GetSum();
    }
};
```

## 虚函数

- 使用虚函数来构造递归：    
- 基类定义虚函数Sum(n)返回0  
- 派生类定义虚函数的实例Sum(n)返回n  
- 将基类和派生类的两个实例，绑定到指针数组中
- 基类的Sum()返回0来结束递归，派生类的Sum()返回n来累加  
- !!n来构造true(1)，false(0)对指针数组进行控制访问

```cpp
class Base;
Base* Array[2];

class Base {
public:
    virtual unsigned int Sum(unsigned int n) { return 0;}
};

class Derived: public Base {
public:
    virtual unsigned int Sum(unsigned int n) {
        return Array[!!n]->Sum(n-1) + n;//n=0时，!!n=false,  n>=1时，!!n=true
    }
};

class Solution {
public:
    int Sum_Solution(int n) {
        Base a;
        Derived b;
        Array[0] = &a;
        Array[1] = &b;
        return b.Sum(n);
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

实际运行结果如下：

![img](./and_or.png)

实际运行结果与大家想的不一样，原因在于&&和||运算符有一个“短路”的概念。  
先来看：(r1 = para2 < para1) && (r2 = para3 > para4);  
para2 < para1为假，所以r1为0，这个很好理解。此时&&左边就是0，那么不管&&右边是0还是1，也就是不管&&右边是假还是真，整个&&表达式就已经是假了，程序再去运行&&右边的表达式就没有意义了，所以程序不会去运行 (r2 = para3 > para4)，也就是r2的值还是为1。当然，如果&&左边是1的话，那么程序还会继续执运行(r2 = para3 > para4)的，此时r2的值就为0了；  

再来看： (r1 = para2 > para1) || (r2 = para4 < para3);  
 para2 > para1为真，所以r1为1，这个很好理解。此时||左边就是1，那么不管||右边是0还是1，也就是不管||右边是假还是真，整个||表达式就已经是真了，程序再去运行||右边的表达式就没有意义了，所以程序不会去运行 (r2 = para4 < para3)，也就是r2的值还是为1；当然，如果||左边是0的话，那么程序还会继续执运行(r2 = para4 < para3)的，此时r2的值就为0了；  

```cpp
class Solution {
public:
    int Sum_Solution(int n) {
        int ret = n;
        n && (ret += Sum_Solution(n-1)); //后面这个使用了判断，因而不行：sum += sum>0 ? Sum_Solution(--n) : 0;
        return ret;
    }
};
```

## 快速模乘

作者：马客(Mark)

- 复杂度32的，可以说O(logM)吧，M是数值大小，对于int也可以说是O(1)吧虽然常数有点大  
- 原理：类似快速幂，俗称快速模乘    
- a * b 可以这样算  
```cpp
res = 0
while(a) {
    if(a & 1) res += b;
    a >>= 1;
    b <<= 1; 
}
```

原理是把a拆成2的幂的和，a = 2^e0 + 2^e1 + 2^e2....  
那么 a * b = (2^e0 + 2^e1 + 2^e2+...) * b = b * 2^e0 + b * 2^e1 + b * 2^e2 + ...  
= (b << e0) + (b << e1) + ....  

感觉这个解释不清楚，我又查了下资料总结如下：

```
参考：快速乘法/幂 算法详解 - 综合编程类其他综合 - 红黑联盟  
https://www.2cto.com/kf/201505/396902.html
```

对于乘数a=53=110101(2进制)。根据乘法分配律：a*b = (a1+a2+a3+……)*b

那么对于`53 * b`:
```
53 * b = (110101)_2(2进制) * b 
       = (100000+10000+100+1)_2 * b 
       = (100000*1+10000*1+1000*0+100*1+10*0+1*1)_2 * b
```
- 那么设立一个ans = 0用于保存答案，
- 每一位让b*=2(或者用移位操作：b<<=2)，
- 根据 a 的对应位置处是否为 1 ，来决定是否加上此时的b，即可完成快速运算。  
- 例子让b=5，a=53运转流程如下：

|a = 5| i = 0 | b的第i位 | 是否加 | ans=0 |
|:---:|:-----:|:--------:|:------:|:-----:|
|  5  |   1   |     1    |   是   |   5   |
| 10  |   2   |     0    |   否   |   5   |
| 20  |   3   |     1    |   是   |  25   |
| 40  |   4   |     0    |   否   |  25   |
| 80  |   5   |     1    |   是   | 105   |
|160  |   6   |     1    |   是   | 265   |


```cpp
//奇数返回0xffffffff，否则0
#define f(x) ((((x) & 1) << 31) >> 31)
class Solution {
public:
    int Sum_Solution(int n) {
        int a = n, b = n + 1, s = 0;
        //复制32次代替while循环，计算a*b=n*(n+1)
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
         
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
         
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
         
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        s += b & f(a); a >>= 1; b <<= 1;
        return s >> 1; // 计算除以2
    }
};
```

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

## 模板

下面代码不正确，没通过

```cpp
class Solution {    
template <int m> inline int SumTo() { 
    return m + SumTo<m-1>();
}  
template <> inline int SumTo<1>() { return 1; }

public:
    int Sum_Solution(int n) {
        return SumTo(n);
    }
};
```
