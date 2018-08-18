# 字符串的排列

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。  

- 输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

## 分析

![img](./permutationOfString.png)

## 常规解法:递归

### 递归1

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

### 递归2

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

### 递归3

```cpp
class Solution {
    void dfs(string s, int fixed_idx) {
        if(fixed_idx==s.size()) res.push_back(s);
        else {
            for(int i=fixed_idx; i<s.size(); i++) {
                if(i!=fixed_idx && s[i]==s[fixed_idx]) continue;//字符一样，但索引不同，算一种情况，不做交换
                swap(s[i], s[fixed_idx]);
                dfs(s, fixed_idx+1);
            }
        } return;
    }
public:
    vector<string> res;
    vector<string> Permutation(string str) {
        if(str.empty()) return res;
        dfs(str, 0);
        return res;
    }
};
```

## STL

```cpp
class Solution {
public:
    vector<string> Permutation(string str) {
        vector<string> res;
        if(str.empty()) return res;
        do
            res.push_back(str);
        while(next_permutation(str.begin(), str.end()));
        return res;
    }
};
```

## 非递归

- 迭代：字典生成算法

```
链接：https://www.nowcoder.com/questionTerminal/fe6b651b66ae47d7acce78ffdd9a96c7
来源：牛客网

public ArrayList<String> Permutation(String str) {
       ArrayList<String> res = new ArrayList<>();
 
        if (str != null && str.length() > 0) {
            char[] seq = str.toCharArray();
            Arrays.sort(seq); //排列
            res.add(String.valueOf(seq)); //先输出一个解
 
            int len = seq.length;
            while (true) {
                int p = len - 1, q;
                //从后向前找一个seq[p - 1] < seq[p]
                while (p >= 1 && seq[p - 1] >= seq[p]) --p;
                if (p == 0) break; //已经是“最小”的排列，退出
                //从p向后找最后一个比seq[p]大的数
                q = p; --p;
                while (q < len && seq[q] > seq[p]) q++;
                --q;
                //交换这两个位置上的值
                swap(seq, q, p);
                //将p之后的序列倒序排列
                reverse(seq, p + 1);
                res.add(String.valueOf(seq));
            }
        }
 
        return res;
    }
     
    public static void reverse(char[] seq, int start) {
        int len;
        if(seq == null || (len = seq.length) <= start)
            return;
        for (int i = 0; i < ((len - start) >> 1); i++) {
            int p = start + i, q = len - 1 - i;
            if (p != q)
                swap(seq, p, q);
        }
    }
     
    public static void swap(char[] cs, int i, int j) {
        char temp = cs[i];
        cs[i] = cs[j];
        cs[j] = temp;
    }
```
