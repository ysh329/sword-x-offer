# 把数组排成最小的数

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

## 字符串排序

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

结果不对还需调整

```c++
class Solution {
    int compare(int& a, int& b) {
        string s1 = to_string(a) + to_string(b);
        string s2 = to_string(b) + to_string(a);
        return s1<s2;
    }
public:
    string PrintMinNumber(vector<int> numbers) {
        if(numbers.empty()) return "";
        int j;
        for(int i = 0; i < numbers.size(); i++) {
            for(j = i; j > 0 && compare(numbers[i], numbers[j-1]) < 0; j--)
                numbers[j] = numbers[j-1];
            numbers[j] = numbers[i];
        }
         
        string result = "";
        for(int e:numbers)
            result += to_string(e);
        return result;
    }
};
```

```cpp
class Solution {
    string itos(int x) {
        return (x > 9 ? itos(x / 10) : "") + char(x % 10 + '0');
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
