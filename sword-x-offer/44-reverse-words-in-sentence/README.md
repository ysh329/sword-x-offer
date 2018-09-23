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
        if(str.empty()) return str;
        reverse(str.begin(), str.end());
        string::size_type s = 0, e = 0; //后面有和string::npose比较，因而类型均为string::size_type
        while((e=str.find(' ', s))!=string::npos) { //find第二个参数表示从下标s开始查找
            reverse(str.begin()+s, str.begin()+e);
            s = e + 1;
        }
        reverse(str.begin()+s, str.end()); // 最后一个单词因找不到空格需要从s翻转
        return str;
    }
};
```

## 栈

```cpp
class Solution {
public:
    string ReverseSentence(string str) {
        stack<string> words;
        int spaceIdx = 0, next = 0;
        while(spaceIdx>=0) {
            spaceIdx = str.find_first_of(' ', next); // 找到空格分隔字符串
            string w = str.substr(next, 
                                  spaceIdx==string::npos ?
                                  str.length()-next : spaceIdx-next); // 按长度截取单词
            next = spaceIdx + 1;
            words.push(w);
        }
        string res;
        while(!words.empty()) {
            res += words.top(); words.pop();
            res += words.empty() ? "" : " ";
        }
        return res;
    }
};
```

## 逐个处理

```cpp
class Solution {
public:
    string ReverseSentence(string str) {
        string res = "", nonSpace = "";
        for(unsigned int i = 0; i < str.size(); i++) {
            if(str[i]==' ') res = " " + nonSpace + res, nonSpace = "";
            else nonSpace += str[i];
        }
        if(nonSpace.size()) res = nonSpace + res;
        return res;
    }
};
```
