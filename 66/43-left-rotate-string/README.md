# 左旋转字符串

汇编语言中有一种移位指令叫做循环左移（ROL），现在有个简单的任务，就是用字符串模拟这个指令的运算结果。对于一个给定的字符序列S，请你把其循环左移K位后的序列输出。例如，字符序列S=”abcXYZdef”,要求输出循环左移3位后的结果，即“XYZdefabc”。是不是很简单？OK，搞定它！

## 开辟新空间

```cpp
class Solution {
public:
    string LeftRotateString(string str, int n) {
        int len = str.length();
        if(len<=0 || n<=0) return str;
        n%=len;
        str += str;
        string res(str.begin()+n, str.begin()+n+str.size()/2);
        return res;
    }
};
```

```cpp
class Solution {
public:
    string LeftRotateString(string str, int n) {
        string ret;
        int len = str.size();
        if(len <= 0) return "";
        n = n%len;
        ret = str+str.substr(0,n);
        ret = ret.substr(n,str.size());
        return ret;
    }
};
```

## 复制剪切

```cpp
class Solution {
public:
    string LeftRotateString(string str, int n) {
        int len = str.length();
        if(len==0) return "";
        n %= len;
        str += str;
        return str.substr(n, len);
    }
};
```

## 反转字符串

### 反转字符串1

- 原理：YX = (X^T Y^T)^T

```cpp
class Solution {
public:
    string LeftRotateString(string str, int n) {
        int len = str.size();
        if(len == 0) return str;
        n %= len;
        for(int i = 0, j = n - 1; i < j; ++i, --j) swap(str[i], str[j]);
        for(int i = n, j = len - 1; i < j; ++i, --j) swap(str[i], str[j]);
        for(int i = 0, j = len - 1; i < j; ++i, --j) swap(str[i], str[j]);
        return str;
    }
};
```

### 反转字符串2

- 核心是应聘者是不是可以灵活利用字符串翻转。假设字符串abcdef，n=3，设X=abc，Y=def，所以字符串可以表示成XY  
- 如题干，问如何求得YX。假设X的翻转为XT，X^T=cba，同理Y^T=fed，那么YX=(X^TY^T)^T，三次翻转后可得结果  

```cpp
class Solution {
public:
    void fun(string &s,int start,int end) {
        char temp;
        while(start<end) {
            temp=s[start];
            s[start]=s[end];
            s[end]=temp;
            start++;
            end--;
        }
    }
    string LeftRotateString(string str, int n) {
        int len=str.length();
        if (0==len || 0==n) return str;
        string &temp=str;
        fun(temp,0,n-1);
        fun(temp,n,len-1);
        fun(temp,0,len-1);
        return str;
    }
};
```

### 反转字符串3

```cpp
class Solution {
public:
    string LeftRotateString(string str, int n) {
        reverse(str.begin(), str.end());
        reverse(str.begin(), str.begin() + str.size() - n);
        reverse(str.begin() + str.size() - n, str.end());
        return str;
    }
};
```
