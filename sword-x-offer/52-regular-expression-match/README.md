# 正则表达式匹配

- 请实现一个函数用来匹配包括`'.'`和`'*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示它前面的字符可以出现任意次（包含0次）。   
- 在本题中，匹配是指字符串的所有字符匹配整个模式。  
- 例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但是与`"aa.a"`和`"ab*a"`均不匹配。

 ## 递归

```cpp
class Solution {
public:
    bool match(char* str, char* pattern) {
        if(*pattern=='\0' && *str=='\0') return true;
        if(*pattern=='\0' && *str!='\0') return false;
        if(*(pattern+1)!='*') {
            if(*str==*pattern || (*str!='\0' && *pattern=='.')) //pattern匹配str || str匹配pattern的.即任意
                return match(str+1, pattern+1);
            else
                return false;
        }
        else {
            if(*str==*pattern || (*str!='\0' && *pattern=='.')) //pattern匹配str || str匹配pattern的.即任意
                return match(str, pattern+2) || match(str+1, pattern);//当前匹配则继续下一种模式(pattern+2)，或者仍旧该模式，去匹配下一个str+1
            else //pattern匹配到0个str
                return match(str, pattern+2);
        }
    }
};
```

## 动态规划


- 时间复杂度：O(n^2)
- .* 是.的意义可以重复多次，还是同一个字符重复多次（也就是`.*`能不能匹配`abcdef`），虽然我猜根本没有这种数据（hehe）  
- `dp(a, b)`表示`s[a..n]`和`p[b..m]`的匹配结果，枚举一个可匹配的前缀进行转移，记忆化避免重复计算  

注：下面有两版动态规划代码，第一个是C/C++混编，第二个是C++，第二版的实现更好一些。

### 动态规划1

```cpp
class Solution {
    char *s, *p;
    int n, m;
    char dp[1000][1000]; //此处本应是动态申请f[n + 1][m + 1]，为了方便简洁就算了
    char judge(int sidx, int pidx) {
        if(sidx > n || pidx > m) return 0;//越界退出
        if(~dp[sidx][pidx]) return dp[sidx][pidx];//f初始化元素为0xff(即0b11111111),在执行该函数过程中某些元素值可能变为0，这句没太看懂（去掉这句也对）
        char &ret = dp[sidx][pidx];//引用，char类型，后面赋值1或者0，如若不匹配为0匹配为1
        if(sidx == n && pidx == m) return ret = 1;//到字符结尾退出，完全匹配
        if(p[pidx + 1] != '*') {
            if(p[pidx] == '.' || s[sidx] == p[pidx]) return ret = judge(sidx + 1, pidx + 1);
            else return ret = 0;
        }
        else{
            for(int i = sidx; i <= n; ++i) {
                if(judge(i, pidx + 2)) return ret = 1;
                if(s[i] != p[pidx] && p[pidx] != '.') return ret = 0;
            }
            return ret = 0;
        }
    }
public:
    bool match(char* str, char* pat){
        s = str, n = strlen(s);
        p = pat, m = strlen(p);
        memset(dp, 0xff, sizeof dp);
        return judge(0, 0);
    }
};
```

`void *memset(void *buffer, int c, int count)`用法：  
- 将buff所指向的某一块内存中的每个字节的内容全部设置为c指定的ASCII值,介于[0,255]之间，0xff即255是?（表示什么不重要，用的是其值），二进制形式0b11111111，~0xff为-0b100000000     
- 块的大小由count指定  
- 这个函数通常为新申请的内存做初始化工作  

函数返回赋值语句
- 函数返回一个赋值语句表示：把这个值作为函数的返回

### 动态规划2

```cpp
class Solution {
public:
    bool match(char* str, char* pattern)
    {
        string s = str;
        string p = pattern;
        vector<vector<char>> dp(s.size() + 1,vector<char>(p.size() + 1,-1));
        return isMatch(0,s,0,p,dp);
    }
    bool isMatch(int i, string& s, int j, string &p, vector<vector<char>> &dp)
    {
        if(dp[i][j] > -1) return dp[i][j];
        int pn = p.size(), sn = s.size();
        if(j==pn) return dp[i][j] = i==sn;
        if(j+1<pn && p[j+1]=='*')
        {
            if(isMatch(i,s,j+2,p,dp) ||
               i<sn && (p[j] == '.' || s[i] == p[j]) && isMatch(i+1,s,j,p,dp))
                return dp[i][j] = 1;
        }
        else if (i<sn && (p[j]=='.'|| s[i]==p[j]) && isMatch(i+1,s,j+1,p,dp))
            return dp[i][j] = 1;
         
        return dp[i][j] = 0;
    }
};
```

## 标准库

```cpp
#include <regex>
class Solution {
public:
    bool match(char* str, char* pattern)
    {
        regex reg(pattern);
        return regex_match(str,reg);
    }
};
```
