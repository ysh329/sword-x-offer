# 把数组排成最小的数

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

## 字符串排序

```cpp
class Solution {
    static bool cmp(int a, int b) { // sort函数第三个参数为传入的比较函数名，必须为static，否则报错
        string A = "", B = "";
        A += to_string(a);
        A += to_string(b);
        B += to_string(b);
        B += to_string(a);
        return A<B;
    }
public:
    string PrintMinNumber(vector<int> numbers) {
        if(numbers.empty()) return "";
        sort(numbers.begin(), numbers.end(), cmp);
        string res = "";
        for(int idx = 0; idx < numbers.size(); idx++)
            res += to_string(numbers[idx]);
        return res;
    }
};
```
