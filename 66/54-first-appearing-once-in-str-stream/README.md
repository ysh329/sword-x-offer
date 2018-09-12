# 字符流中第一个不重复的字符

请实现一个函数用来找出字符流中第一个只出现一次的字符。例如，当从字符流中只读出前两个字符"go"时，第一个只出现一次的字符是"g"。当从该字符流中读出前六个字符“google"时，第一个只出现一次的字符是"l"。

如果当前字符流没有存在出现一次的字符，返回#字符。

## 哈希表

```cpp
class Solution
{
public:
    string s;
    char hash[256]={0};
    //Insert one char from stringstream
    void Insert(char ch)
    {
        s += ch;
        hash[ch]++;
    }
    //return the first appearence once char in current stringstream
    char FirstAppearingOnce()
    {
        for(int i = 0; i < s.size(); i++) {
            if(hash[s[i]]==1)
                return s[i];
        }
        return '#';
    }
};
```


## 其它

```cpp
class Solution
{
    int t, a[128];
    public:
    Solution(){
        t = 0;
        memset(a, 0, sizeof a);
    }
  //Insert one char from stringstream
    void Insert(char ch) {
       if(a[ch]) a[ch] = -1;
       else a[ch] = ++t;
    }
  //return the first appearence once char in current stringstream
    char FirstAppearingOnce() {
        char res = '#';
        int mi = ~(1 << 31);
        for(int i = 0; i < 128; ++i) if(a[i] > 0 && a[i] < mi) res = i, mi = a[i];
        return res;
    }
};
```
