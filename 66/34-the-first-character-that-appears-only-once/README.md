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
        for(int idx = 0; idx < str.length(); idx++) {
            if(dict[str[idx]]==1) {
                res = idx;
                break;
            }
        }
        return res;
    }
};
```
