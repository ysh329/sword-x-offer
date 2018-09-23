# 把数组排成最小的数

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

## 字符串排序

### 常规解法

```cpp
class Solution {
    static bool cmp(int a, int b) { // sort函数第三个参数为传入的比较函数名，必须为static，否则报错
        return to_string(a) + to_string(b) < to_string(b) + to_string(a);
    }
public:
    string PrintMinNumber(vector<int> numbers) {
        sort(numbers.begin(), numbers.end(), cmp);
        string s = "";
        for(int i = 0; i < int(numbers.size()); ++i)
            s += to_string(numbers[i]);
        return s;
    }
};
```

### 匿名函数

```cpp
//more about lambda, reference: http://blog.csdn.net/taoyanqi8932/article/details/52541312
class Solution {
public:
    string PrintMinNumber(vector<int> numbers) {
        sort(numbers.begin(), numbers.end(),
             [](const int& a, const int& b) {
                 return to_string(a)+to_string(b) < to_string(b)+to_string(a);
             }
            );
        string res;
        for(auto num:numbers)
            res += to_string(num);
        return res;
    }
};
```

### 输入输出流

```cpp
class Solution {
public:
    static int cmp(const string& st1, const string& st2) {
        string s1 = st1 + st2, s2 = st2 + st1;
        return s1 < s2;
    }
    string PrintMinNumber(vector<int> numbers) {
        string res;
        vector<string> v;
        for(int i = 0; i < numbers.size(); i++) {
            stringstream ss;//输入输出流 #include<sstream>
            ss << numbers[i];//读入数字给流处理
            string s = ss.str();//转换字符串
            v.push_back(s);//将字符串s压入vec中
        }
        sort(v.begin(), v.end(), cmp);
        for(int i = 0; i < v.size(); i++)
            res.append(v[i]);//拼接字符串，这就是最小的数字
        return res;
    }
};
```

### 实现itos

- 链接：https://www.nowcoder.com/questionTerminal/8fecd3f8ba334add803bf2a06af1b993
- 来源：牛客网

作者自己实现了整型数转字符串的函数itos。

```cpp
class Solution {
    string itos(int x) {
        return (x > 9 ? itos(x / 10) : "") + char(x % 10 + '0');//ASCII值，实现一个itos
    }
    bool cmp(int a, int b) {
        return itos(a) + itos(b) < itos(b) + itos(a);
    }
public:
    string PrintMinNumber(vector<int> a) {
        sort(a.begin(), a.end(), cmp);
        string s = "";
        for(int i = 0; i < int(a.size()); ++i) s += itos(a[i]);
        return s;
    }
};
```

### 冒泡排序

```cpp
class Solution {
    static bool cmp(int& a, int& b) {
        return to_string(a)+to_string(b) < to_string(b)+to_string(a);
    }
public:
    string PrintMinNumber(vector<int> numbers) {
        for(int idx1 = 0; idx1 < numbers.size(); idx1++) {
            for(int idx2 = idx1+1; idx2 < numbers.size(); idx2++) {
                if(!cmp(numbers[idx1], numbers[idx2]))
                    swap(numbers[idx1], numbers[idx2]);
            }
        }
        string res;
        for(int e:numbers) res += to_string(e);
        return res;
    }
};
```
