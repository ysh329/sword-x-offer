# 第一个只出现一次的字符

在一个字符串(0<=字符串长度<=10000，全部由字母组成)中找到第一个只出现一次的字符,并返回它的位置, 如果没有则返回 -1（需要区分大小写）.

## 哈希表

未通过

```cpp
class Solution {
public:
    int FirstNotRepeatingChar(string str) {
        int res = -1;
        char dict[256];
        for(int idx = 0; idx < str.length(); idx++)
            dict[str[idx]]++;
        for(int idx = 0; idx < 256; idx++) {
            if(dict[idx]==1) {
                res = str.find(dict[idx]);
                break;
            }
        }
        return res;
    }
};
```
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
