# 表示数值的字符串

- 请实现一个函数用来判断字符串是否表示数值（包括整数和小数）。  
- 例如，字符串`"+100"`,`"5e2"`,`"-123"`,`"3.1416"`和`"-1E-16"`都表示数值。  
- 但是`"12e"`,`"1a3.14"`,`"1.2.3"`,`"+-5"`和`"12e+4.3"`都不是。


结果不对 
```cpp
class Solution {
public:
    bool isNumeric(char* s)
    {
        if(*s=='\0') return false;
        int s_len = strlen(s);
        int int_sym = 0; //整数正负号 0:没找到，1:找到为正，-1:找到为负
        int dec_sym = 0; //小数科学计数法正负
        int e_sym = 0;
        int dot_sym = 0;
        
        int int_part = 0;
        int dec_part = 0;

        for(int i=0; i<strlen(s); i++) {
            if('0'<=*s && *s<='9') {
                if(i==0) int_sym = 1;
                if(dot_sym==0)
                    int_part *=10 + (*s - '0');
                else
                    dec_part *=10 + (*s - '0');
            }
            else if(*s=='+' || *s=='-') {
                if(i==0 && *s=='+') int_sym = 1;
                if(i==0 && *s=='-') int_sym = -1;
                if(dec_sym!=0) return false;
                if(int_sym!=0 && int_part==0) return false;
                if(int_part!=0 && dec_sym==0)
                    dec_sym = (*s=='+') ? 1 : -1;
            }
            else if(*s=='e' || *s=='E') {
                if(e_sym==0) e_sym=1;
                else return false;
            }
            else if(*s=='.') {
                if(dot_sym==0) dot_sym=1;
                else return false;
            }
        }
        return true;
    }

};
```

## 逐字符遍历

```cpp
class Solution {
public:
    bool isNumeric(char* str) {
        // 标记符号、小数点、e是否出现过
        bool sign = false, decimal = false, hasE = false;
        for (int i = 0; i < strlen(str); i++) {
            if (str[i] == 'e' || str[i] == 'E') {
                if (i == strlen(str)-1) return false; // e后面一定要接数字
                if (hasE) return false;  // 不能同时存在两个e
                hasE = true;
            } else if (str[i] == '+' || str[i] == '-') {
                // 第二次出现+-符号，则必须紧接在e之后
                if (sign && str[i-1] != 'e' && str[i-1] != 'E') return false;
                // 第一次出现+-符号，且不是在字符串开头，则也必须紧接在e之后
                if (!sign && i > 0 && str[i-1] != 'e' && str[i-1] != 'E') return false;
                sign = true;
            } else if (str[i] == '.') {
              // e后面不能接小数点，小数点不能出现两次
                if (hasE || decimal) return false;
                decimal = true;
            } else if (str[i] < '0' || str[i] > '9') // 不合法字符
                return false;
        }
        return true;
    }
};
```

```cpp
链接：https://www.nowcoder.com/questionTerminal/6f8c901d091949a5837e24bb82a731f2
来源：牛客网

先说一下思路吧：
1.整数开始部分遇到+、-号跳过
2.小数点只能出现一次
3.小数点之前不能存在e
4.e之前必须有整数
5.e只能出现一次
6.e之后可存在+、-号，但+-之后必须有整数
 
class Solution {
public:
    bool isNumeric(char* str)
    {
        if (str == NULL)
        return false;
    if (*str == '+' || *str == '-')
        ++str;
    if (*str == '\0')
        return false;
    int x = 0;    //标记整数部分
    int digit = 0; //标记小数点
    int e = 0;     //标记e的状态
    while (*str != '\0')
    {
        //标记整数部分的状态
        if (*str >= '0' && *str <= '9')
        {
            ++str;
            x = 1;
        }
        //小数点
        else if (*str == '.')
        {
            //前面已经出现过小数点或小数点之前存在e，则返回false
            if (digit > 0 || e > 0)
                return false;
            ++str;
            digit = 1;    //标记小数点已经出现过
        }
 
        //e
        else if (*str == 'e' || *str == 'E')
        {
            //e之前没有整数或e已经出现过，则返回false
            if (x == 0 || e > 0)
                return false;
            ++str;
            e = 1;     //标记e表示已经出现过
 
            //e之后可以出现+-号再加整数
            if (*str == '+' || *str == '-')
                ++str;
            if (*str == '\0')
                return false;
        }
        else
            return false;
    }
    return true;
    }
 
};
```

## 正则表达式

```cpp
//正则表达式解法
public class Solution {
    public boolean isNumeric(char[] str) {
        String string = String.valueOf(str);
        return string.matches("[\\+\\-]?\\d*(\\.\\d+)?([eE][\\+\\-]?\\d+)?");
    }
}
/*
以下对正则进行解释:
[\\+\\-]?            -> 正或负符号出现与否
\\d*                 -> 整数部分是否出现，如-.34 或 +3.34均符合
(\\.\\d+)?           -> 如果出现小数点，那么小数点后面必须有数字；
                        否则一起不出现
([eE][\\+\\-]?\\d+)? -> 如果存在指数部分，那么e或E肯定出现，+或-可以不出现，
                        紧接着必须跟着整数；或者整个部分都不出现
*/
 
```

## 自动机

### 自动机1

![auto](自动机.jpg)

```cpp
class Solution {
public:
    bool isNumeric(char* string)
    {
        int i = 0;
        if(string[i]=='+' || string[i]=='-' || IsNum(string[i])){
            while(string[++i]!='\0' && IsNum(string[i]));
            if(string[i]=='.'){
                if(IsNum(string[++i])){
                    while(string[++i]!='\0' && IsNum(string[i]));
                    if(string[i]=='e'||string[i]=='E'){
                        i++;
                        if(string[i]=='+' || string[i]=='-' || IsNum(string[i])){
                            while(string[++i]!='\0' && IsNum(string[i]));
                            if(string[i]=='\0') return true;
                            else return false;
                        }else return false;
                    }else if(string[i]=='\0') return true;
                    else return false;
                }else if(string[++i]=='\0') return true;
                else return false;
            }else if(string[i]=='e'||string[i]=='E'){
                i++;
                if(string[i]=='+' || string[i]=='-' || IsNum(string[i])){
                    while(string[++i]!='\0' && IsNum(string[i]));
                    if(string[i]=='\0') return true;
                    else return false;
                }else return false;
            }else if(string[i]=='\0') return true;
            else return false;           
        }else return false;
    }
     
    bool IsNum(char ch)
    {
        if(ch<'0'||ch>'9') return false;
        else return true;
    }
};
```

### 自动机2

```cpp
class Solution {
public:
    char arr[10] = "+-n.ne+-n";
    int turn[10][9] = {
       //+  -  n  .  n  e  +  -  n
        {1, 1, 1, 0, 0, 0, 0, 0, 0},    // # start
        {0, 0, 1, 1, 0, 0, 0, 0, 0},    // +
        {0, 0, 1, 1, 0, 0, 0, 0, 0},    // -
        {0, 0, 1, 1, 0, 1, 0, 0, 0},    // n
        {0, 0, 0, 0, 1, 0, 0, 0, 0},    // .
        {0, 0, 0, 0, 1, 1, 0, 0, 0},    // n
        {0, 0, 0, 0, 0, 0, 1, 1, 1},    // e
        {0, 0, 0, 0, 0, 0, 0, 0, 1},    // +
        {0, 0, 0, 0, 0, 0, 0, 0, 1},    // -
        {0, 0, 0, 0, 0, 0, 0, 0, 1}     // n
    };
    bool isNumeric(char* string) {
        int cur = 0;
        for(int j, i = 0; string[i]; i++) {
            for(j = 0; j < 9; j++) {
                if(turn[cur][j]) {
                    if(('0' <= string[i] && string[i] <= '9' && arr[j] == 'n') ||
                        (string[i] == 'E' && arr[j] == 'e')||
                        string[i] == arr[j]) {
                        cur = j + 1;
                        break;
                    }
                }
            }
            if(j == 9) return false;
        }
        if(cur == 3 || cur == 4 || cur == 5 || cur == 9)
           return true;
        return false;
    }
};
```

### 自动机3

![auto](./自动机2.jpg)

```cpp
链接：https://www.nowcoder.com/questionTerminal/6f8c901d091949a5837e24bb82a731f2
来源：牛客网

class Solution {
private:
    enum STATUS{ END = 0, START, SIGNED1, INTEGER, POINT, FLOAT, EXPONENT, SIGNED2, SCIENCE };
    STATUS dfa[256][9] = { END };
public:  
    Solution(){
        for (int i = 0; i < 256; ++i)
        {
            for (int j = 0; j < 9; ++j)
            {
                dfa[i][j] = END;
            }
        }
        initDFA();
    }
    bool isNumeric(char* string)
    {
        STATUS current = START;
        while (*string && current != END)
        {
            current = DFA(current, *string);
            ++string;
        }
        switch (current)
        {
        case INTEGER:
        case FLOAT:
        case SCIENCE:
            return true;
        }
        return false;
    }
private:
    void initDFA(){
        char d = '0';
        // 1. START 变迁
        dfa['+'][START] = SIGNED1;
        dfa['-'][START] = SIGNED1;
        dfa['.'][START] = POINT;
        for (d = '0'; d <= '9'; ++d)
        {
            dfa[d][START] = INTEGER;
        }
 
        // 2. SIGNED1 变迁
        for (d = '0'; d <= '9'; ++d)
        {
            dfa[d][SIGNED1] = INTEGER;
        }
        dfa['.'][SIGNED1] = POINT;
 
        // 3. INTEGER 变迁
        for (d = '0'; d <= '9'; ++d)
        {
            dfa[d][INTEGER] = INTEGER;
        }
        dfa['.'][INTEGER] = FLOAT;
        dfa['E'][INTEGER] = EXPONENT;
        dfa['e'][INTEGER] = EXPONENT;
 
        // 4. POINT 变迁
        for (d = '0'; d <= '9'; ++d)
        {
            dfa[d][POINT] = FLOAT;
        }
 
        // 5. FLOAT 变迁
        for (d = '0'; d <= '9'; ++d)
        {
            dfa[d][FLOAT] = FLOAT;
        }
        dfa['E'][FLOAT] = EXPONENT;
        dfa['e'][FLOAT] = EXPONENT;
 
        // 6. EXPONENT 变迁
        for (d = '0'; d <= '9'; ++d)
        {
            dfa[d][EXPONENT] = SCIENCE;
        }
        dfa['+'][EXPONENT] = SIGNED2;
        dfa['-'][EXPONENT] = SIGNED2;
 
        // 7. SIGNED2 变迁
        for (d = '0'; d <= '9'; ++d)
        {
            dfa[d][SIGNED2] = SCIENCE;
        }
 
        // 8. SCIENCE 变迁
        for (d = '0'; d <= '9'; ++d)
        {
            dfa[d][SCIENCE] = SCIENCE;
        }
 
        // 其余情况均变迁到  END

    }

    STATUS DFA(STATUS current, char input)
    {
        STATUS ret = START;
        return dfa[input][current];
    }
};
```
