# 把数组排成最小的数

输入一个正整数数组，把数组里所有数字拼接起来排成一个数，打印能拼接出的所有数字中最小的一个。例如输入数组{3，32，321}，则打印出这三个数字能排成的最小数字为321323。

## 字符串排序

```cpp
class Solution {
    static bool cmp(int a, int b) { // sort函数第三个参数为传入的比较函数名，必须为static，否则报错
        return to_string(a) + to_string(b) < to_string(b) + to_string(a);
    }
public:
    string PrintMinNumber(vector<int> numbers) {
        sort(numbers.begin(), numbers.end(), cmp);
        string s = "";
        for(int i = 0; i < int(numbers.size()); ++i)
            s += to_string(numbers[i]);
        return s;
    }
};
```

结果不对还需调整

```c++
class Solution {
    int compare(int& a, int& b) {
        string s1 = to_string(a) + to_string(b);
        string s2 = to_string(b) + to_string(a);
        return s1<s2;
    }
public:
    string PrintMinNumber(vector<int> numbers) {
        if(numbers.empty()) return "";
        int j;
        for(int i = 0; i < numbers.size(); i++) {
            for(j = i; j > 0 && compare(numbers[i], numbers[j-1]) < 0; j--)
                numbers[j] = numbers[j-1];
            numbers[j] = numbers[i];
        }
         
        string result = "";
        for(int e:numbers)
            result += to_string(e);
        return result;
    }
};
```

```cpp
class Solution {
public:
    //如果题目要求得出最大的数字，可以将比较器转换成从大到小排序即可
   //其中，数字转化为字符串的使用方法，参考别人的代码%>_<%
 static int compare(const string& st1,const string& st2)
    {
        string s1=st1+st2;
        string s2=st2+st1;
        return s1<s2;//降序排列，改为大于就是升序排列！！！
    }
    string PrintMinNumber(vector<int> numbers) {
        string result;
        if(numbers.size()<=0) return result;
        vector<string> vec;
        for(unsigned int i=0;i<numbers.size();i++)
        {
            stringstream ss;//使用输入输出流，头文件要包含#include<sstream>
            ss<<numbers[i];//读入数字给流处理
            string s = ss.str();//转换成字符串
            vec.push_back(s);//将字符串s压入vec中
        }
        //排序，传入比较器，从小到大排序
        sort(vec.begin(),vec.end(),compare);
        for(unsigned int i=0;i<vec.size();i++)
        {
            result.append(vec[i]);//拼接字符串，这就是最小的数字
        }
        return result;
    }
};
```

```cpp
链接：https://www.nowcoder.com/questionTerminal/8fecd3f8ba334add803bf2a06af1b993
来源：牛客网

//more about lambda,you can reference:http://blog.csdn.net/taoyanqi8932/article/details/52541312
class Solution {
public:
    string PrintMinNumber(vector<int> numbers) { 
        sort(numbers.begin(),numbers.end(),[](const int& a,const int& b){
        return to_string(a)+to_string(b)<to_string(b)+to_string(a);});
        string res;
        for (auto c:numbers)   
            res+=to_string(c);
        return res;
    }
};
```

```cpp
class Solution {
    string itos(int x) {
        return (x > 9 ? itos(x / 10) : "") + char(x % 10 + '0');
    }
    bool cmp(int a, int b) {
        return itos(a) + itos(b) < itos(b) + itos(a);
    }
public:
    string PrintMinNumber(vector<int> a) {
        sort(a.begin(), a.end(), cmp);
        string s = "";
        for(int i = 0; i < int(a.size()); ++i) s += itos(a[i]);
        return s;
    }
};
```
