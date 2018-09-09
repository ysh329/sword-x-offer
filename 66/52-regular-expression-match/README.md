# 正则表达式匹配

- 请实现一个函数用来匹配包括`'.'`和`'*'`的正则表达式。模式中的字符`'.'`表示任意一个字符，而`'*'`表示它前面的字符可以出现任意次（包含0次）。   
- 在本题中，匹配是指字符串的所有字符匹配整个模式。  
- 例如，字符串`"aaa"`与模式`"a.a"`和`"ab*ac*a"`匹配，但是与`"aa.a"`和`"ab*a"`均不匹配。

 ## 递归
 
 ### 递归1

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

### 递归2

- 时间复杂度：O(n^2)
- .* 是.的意义可以重复多次，还是同一个字符重复多次（也就是`.*`能不能匹配`abcdef`），虽然我猜根本没有这种数据（hehe）  
- `f(a, b)`表示`s[a..n]`和`p[b..m]`的匹配结果，枚举一个可匹配的前缀进行转移，记忆化避免重复计算  

```cpp
class Solution {
    char *s, *p;
    int n, m;
    char f[1000][1000];   //此处本应是动态申请f[n + 1][m + 1]，为了方便简洁就算了
    char judge(int a, int b) {
        if(a > n || b > m) return 0;
        if(~f[a][b]) return f[a][b];
        char &ret = f[a][b];
        if(a == n && b == m) return ret = 1;
        if(p[b + 1] != '*') {
            if(p[b] == '.' || s[a] == p[b]) return ret = judge(a + 1, b + 1);
            else return ret = 0;
        }
        else{
            for(int i = a; i <= n; ++i) {
                if(judge(i, b + 2)) return ret = 1;
                if(s[i] != p[b] && p[b] != '.') return ret = 0;
            }
            return ret = 0;
        }
    }
public:
    bool match(char* str, char* pat){
        s = str, n = strlen(s);
        p = pat, m = strlen(p);
        memset(f, 0xff, sizeof f);
        return judge(0, 0);
    }
};
```

`void *memset(void *buffer, int c, int count)`用法：  
- 将buff所指向的某一块内存中的每个字节的内容全部设置为c指定的ASCII值   
- 块的大小由count指定  
- 这个函数通常为新申请的内存做初始化工作

## 动态规划

```cpp
#include<regex>
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
#include<regex>
class Solution {
public:
    bool match(char* str, char* pattern)
    {
        regex reg(pattern);
        return regex_match(str,reg);
    }
};
```
