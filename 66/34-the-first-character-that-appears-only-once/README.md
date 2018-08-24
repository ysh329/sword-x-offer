# 第一个只出现一次的字符

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.

## 哈希表

### 哈希表1

```cpp
class Solution {
public:
    int FirstNotRepeatingChar(string str) {
        int dict[256] = {0};
        for(int cidx=0; cidx<str.size(); cidx++)
            dict[str[cidx]]++;
        for(int cidx=0; cidx<str.size(); cidx++)
            if(dict[str[cidx]]==1)
                return cidx;
        return -1;
    }
};
```

### 哈希表2

```cpp
class Solution {
public:
    int FirstNotRepeatingChar(string str) {
        int res = -1;
        map<char, int> str_times_map;
        for (int cidx = 0; cidx < str.size(); cidx++)
            str_times_map[str[cidx]]++;
        for (int cidx = 0; cidx < str.size(); cidx++)
            if (str_times_map[str[cidx]]==1) {
                res = cidx;
                break;
            }
        return res;
    }
};
```
