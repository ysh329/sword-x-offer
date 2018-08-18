# 字符串的排列

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。  

- 输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

## 分析

![img](./permutationOfString.png)

## 常规解法

- 类似DFS二叉树  
- for循环从根节点DFS到叶子，得到一种排列  
- 之后便回溯，即swap back退回一层，DFS另一种排列  
- 以此类推，直到所有情形结束

```cpp
class Solution {
    void Permutation(string s, vector<string>& res, int fixed_idx) {
        if(fixed_idx==s.length()-1 &&
           !count(res.begin(), res.end(), s))
            res.push_back(s);
        else {
            for(int i=fixed_idx; i<s.size(); i++) {
                swap(s[i], s[fixed_idx]);
                Permutation(s, res, fixed_idx+1);
                swap(s[i], s[fixed_idx]);
            }
        }
    }
public:
    vector<string> Permutation(string str) {
        vector<string> res;
        if(str.empty()) return res;
        Permutation(str, res, 0);
        sort(res.begin(), res.end());
        return res;
    }
};
```


```cpp
class Solution {
public:
    vector<string> Permutation(string str) {
        vector<string> res;
        Permutation(str, res, 0);
        return res;
    }
    void Permutation(string s, vector<string>& res, int fixed_idx) {
        if(fixed_idx==s.size()-1)
                res.push_back(s);
        else {
            sort(s.begin()+fixed_idx, s.end());//字典序
            unordered_set<char> us;//记录用过的字符
            for(int i = fixed_idx; i < s.length(); i++) {
                if(!count(us.begin(), us.end(), s[i])) {//仅和没交换的，做交换
                    us.insert(s[i]);
                    swap(s[i], s[fixed_idx]); // swap
                    Permutation(s, res, fixed_idx+1);
                    swap(s[i], s[fixed_idx]); // swap back
                }
            }
        }
    }
};
```
