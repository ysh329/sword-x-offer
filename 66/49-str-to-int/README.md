# 把字符串转换成整数

将一个字符串转换成一个整数(实现Integer.valueOf(string)的功能，但是string不符合数字要求时返回0)，要求不能使用字符串转换整数的库函数。 数值为0或者字符串不是一个合法的数值则返回0。

- 输入一个字符串,包括数字字母符号,可以为空  
- 如果是合法的数值表达则返回该数字，否则返回0  

```
输入
+2147483647
1a33

输出
2147483647
0
```

## 索引遍历

### 索引遍历1

```cpp
class Solution {
public:
    int StrToInt(string str) {
        int result = 0;
        // 空串
        if(str.empty()) return result;
        // 长度为1，非数字(或者仅有正负号)
        else if(str.size()==1 &&
                (str[0]<'0' || str[0]>'9'))
            return result;
        
        int sidx = 0;
        //跳过空格
        while(sidx<str.size() && str[sidx]==' ')  {
            str[sidx] = '0';
            sidx++;
        }
        // 正负号
        bool positive = true;
        if(sidx<str.size() && str[sidx]=='+') {
            str[sidx] = '0';
            sidx++;//positive
        }
        else if(sidx<str.size() && str[sidx]>='0' && str[sidx]<='9')
            ;// positive
        else if(sidx<str.size() && str[sidx]=='-') {
            str[sidx] = '0';
            positive = false;
            sidx++;
        }
        else return result;
        // 数字结果
        for(sidx = 0; sidx < str.size(); sidx++) {
            if(str[sidx]<'0' || str[sidx]>'9') {
                result = 0;
                break;
            }
            result = result*10 + str[sidx] - '0';
        }
        return positive ? result : -result;
    }
};
```

### 索引遍历2

```cpp
class Solution {
public:
    int StrToInt(string str) {
        int res = 0;
        //空，0
        if(str.empty()) return res;
        int i = 0;
        //正负号
        int positive = 1;
        if(str[i]=='-') {positive = 0; i++;}
        else if(str[i]=='+') {i++;}
        //数字
        for(; i<str.size(); i++) {
            if('0'<=str[i] && str[i]<='9')
                res = res*10 + (str[i]-'0');
            else return 0;
        }
        return positive ? res : -res;
    }
};
```

### 索引遍历3

```cpp
class Solution {
public:
    int StrToInt(string str) {
        if(str.empty()) return 0;
        int positive = str[0]=='-' ? 0 : 1;
        int res = 0;
        for(int i=0; i<str.size(); ++i){
            if(!i && (str[i]=='+' || str[i]=='-')) continue;
            if(str[i]<'0' || str[i]>'9') return 0;
            res = res*10 + (str[i]-'0');
        }
        return positive ? res : -res;
    }
};
```
