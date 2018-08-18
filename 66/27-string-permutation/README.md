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
public:
    vector<string> Permutation(string str) {
        vector<string> result;
        if(str.empty()) return result;
        Permutation(str, result, 0);
        sort(result.begin(), result.end());
        return result;
    }
    void Permutation(string s, vector<string>& result, int fixed_idx) {
        if(fixed_idx==s.length()-1 &&
           !count(result.begin(), result.end(), s))
                result.push_back(s);
        else {
            for(int i = fixed_idx; i < s.length(); i++) {
                swap(s[i], s[fixed_idx]); // swap
                Permutation(s, result, fixed_idx+1);
                swap(s[i], s[fixed_idx]); // swap back
            }
        }
    }
};
```
