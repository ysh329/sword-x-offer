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
