# 把数组排成最小的数

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

```cpp
class Solution {
    static bool cmp(int a, int b) {
        string A = "";
        string B = "";
        A += to_string(a);
        A += to_string(b);
        B += to_string(b);
        B += to_string(a);
        return A < B;
    }
public:
    string PrintMinNumber(vector<int> numbers) {
        if (numbers.empty()) return "";
        sort(numbers.begin(), numbers.end(), cmp);
        string min_num_string = "";
        for (int nidx = 0; nidx < numbers.size(); nidx++)
            min_num_string += to_string(numbers[nidx]);
        return min_num_string;
    }
};
```
