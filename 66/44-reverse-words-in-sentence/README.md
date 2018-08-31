# 翻转单词顺序列

牛客最近来了一个新员工Fish，每天早晨总是会拿着一本英文杂志，写些句子在本子上。同事Cat对Fish写的内容颇感兴趣，有一天他向Fish借来翻看，但却读不懂它的意思。例如，“student. a am I”。后来才意识到，这家伙原来把句子单词的顺序翻转了，正确的句子应该是“I am a student.”。Cat对一一的翻转这些单词顺序可不在行，你能帮助他么？

##

```cpp
class Solution {
public:
    string ReverseSentence(string str) {
        if(str.empty()) return str;
        //单词内翻转
        int start = 0;
        int end = 0;
        while(end<str.size()) {
            while(str[end]!=" ") {
                end++;
            }
            reverse(str.begin()+start, str.begin()+end);
            start = end; end++;
        }
        //整体翻转
        reverse(str.begin(), str.end());
        return str;
    }
};
```
