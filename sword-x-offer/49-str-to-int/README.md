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

第一次写的

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

第二次写的

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

更标准的写法：

```cpp
class Solution {
public:
    enum Status{kValid = 0, kInvalid};
    int g_nStatus = kValid;
    int StrToInt(string str) {
        g_nStatus = kInvalid;
        long long num = 0;
        int index = 0;
        if((!str.empty()) && str[index]!='\0') {
            bool minus = false;
            if(str[index]=='+') index++;
            else if(str[index]=='-') {
                index++;
                minus = true;
            }
            if(str[index]!='\0') {
                int length = str.size();
                num = StrToIntCore(str.substr(index,length-index), minus);  //substr获得字符串s中,从第index位开始,长度为(length-index)的字符串,该值默认时的长度为从开始位置到字符串结尾
            }
        }
        return (int)num;
    }
         
    long long StrToIntCore(const string& digit, bool minus) {
        long long num = 0;
        int i = 0;
        while(digit[i]!='\0') { //合法情况
            //if(digit[i]=='+'||digit[i]=='-') i++;
            if('0'<=digit[i] && digit[i]<='9') {
                int flag = minus ? -1 : 1;
                num = num*10+ flag*(digit[i]-'0');
                //越界情况
                if( (minus>0 && num>0x7FFFFFFF) || //long int最大值0x7FFFFFFF
                    (minus<0 && (signed int)num<0x80000000) ) { //int的最小值
                    num = 0;
                    break;
                }
                i++;
            }
            else {//非法情况
                num = 0;
                break;
            }
        }
        g_nStatus = (digit[i]=='\0') ? kValid : g_nStatus;
        return num;
    }
};
```

注意：在使用0X7FFFFFFF，0X80000000这两个的时候要赋值给一个 int 类型的变量，否则0X80000000并不代表int的最小值。

## 二进制位运算

- 字符'0'到'9'的ascii值的低4个二进制位刚好就是0到9  
- 因而，str[i]-'0' 等同于 (str[i] & 0xf)，0xf 即二进制的 0b1111  
- (res << 1) + (res << 3) = res * 2 + res * 8 = res * 10   
- 位运算会比乘法运算效率高那么一点  

```cpp
class Solution {
public:
    int StrToInt(string str) {
        int n = str.size(), s = 1;
        long long res = 0;
        if(!n) return 0;
        if(str[0] == '-') s = -1;
        for(int i = (str[0] ==  '-' || str[0] == '+') ? 1 : 0; i < n; ++i){
            if(!('0' <= str[i] && str[i] <= '9')) return 0;
            res = (res << 1) + (res << 3) + (str[i] & 0xf);
        }
        return res * s;
    }
};
```

更标准的写法还需要带上有效性的标识：

```cpp
class Solution {
public:
    enum Status{kValid = 0,kInvalid};
    int g_nStatus = kValid;
    int StrToInt(string str) {
        int n = str.size(), s = 1;
        long long res = 0;
        if(!n) return 0;
        if(str[0] == '-') s = -1;
        for(int i = (str[0] ==  '-' || str[0] == '+') ? 1 : 0; i < n; ++i){
            if(!('0' <= str[i] && str[i] <= '9')) {g_nStatus=kInvalid; return 0;}
            res = (res << 1) + (res << 3) + (str[i] & 0xf);
        }
        return res * s;
    }
};
```
