# 翻转单词顺序列

牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

## 双索引

```cpp
class Solution {
public:
    string ReverseSentence(string str) {
        if(str.empty()) return str;
        reverse(str.begin(), str.end());//也可放到单词翻转后
        int s = 0, e = 0;//确定单词起始
        while(s<str.size()) {
            while(s<str.size() && str[s]==' ') s++;
            e = s;
            while(e<str.size() && str[e]!=' ') e++;
            reverse(str.begin()+s, str.begin()+e);
            s = e;
        }
        return str;
    }
};
```

```cpp
class Solution {
public:
    string ReverseSentence(string str) {
        reverse(str.begin(), str.end());
        string::size_type s = 0, e;//string::npos类型为string::size_type，由于后面有和该值的比较，因而类型均为string::size_type
        while((e=str.find(' ', s)) != string::npos) {//find第二个参数表示从下标s开始查找
            reverse(str.begin()+s, str.begin()+e);
            s = e + 1;
        }
        reverse(str.begin()+s, str.end());//对str中最后一个词（或无空格时）做了反转
        return str;
    }
};
```

## 栈

```cpp
class Solution {
public:
    string ReverseSentence(string str) {
        stack<string> Words;
        int currBlankPos = 0;
        int perBlankPos = 0;
        string subString;
        while(currBlankPos>=0) {
            currBlankPos = str.find_first_of(' ', perBlankPos); // 找到空格分隔字符串（找到word压如栈里头）
            subString = str.substr(perBlankPos, currBlankPos < 0 ? (str.length() - perBlankPos) : currBlankPos - perBlankPos); // 按长度截取单词
            perBlankPos = currBlankPos + 1;
            Words.push(subString); //把单词压如栈
        }
        subString.clear();
        while (!Words.empty()) {
            subString += Words.top(); Words.pop();
            if(!Words.empty()) subString += " "; // 需不需要加空格
        }
        return subString;
    }
};
```

## 逐个处理

```cpp
class Solution {
public:
    string ReverseSentence(string str) {
        string res = "", tmp = "";
        for(unsigned int i = 0; i < str.size(); ++i){
            if(str[i] == ' ') res = " " + tmp + res, tmp = "";
            else tmp += str[i];
        }
        if(tmp.size()) res = tmp + res;
        return res;
    }
}; 
```
