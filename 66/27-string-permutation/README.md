# 字符串的排列

输入一个字符串,按字典序打印出该字符串中字符的所有排列。例如输入字符串abc,则打印出由字符a,b,c所能排列出来的所有字符串abc,acb,bac,bca,cab和cba。  

- 输入一个字符串,长度不超过9(可能有字符重复),字符只包括大小写字母。

## 分析

![img](./permutationOfString.png)

- 以下所有解法均为字典序生成算法的递归形式和非递归形式  
- [字典序全排列算法研究 - pmars - 博客园](http://www.cnblogs.com/pmars/archive/2013/12/04/3458289.html)



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

```java
链接：https://www.nowcoder.com/questionTerminal/fe6b651b66ae47d7acce78ffdd9a96c7
来源：牛客网

/**
     * 2、字典序排列算法
     *
     * 可参考解析： http://www.cnblogs.com/pmars/archive/2013/12/04/3458289.html  （感谢作者）
     *
     * 一个全排列可看做一个字符串，字符串可有前缀、后缀。
     * 生成给定全排列的下一个排列.所谓一个的下一个就是这一个与下一个之间没有其他的。
     * 这就要求这一个与下一个有尽可能长的共同前缀，也即变化限制在尽可能短的后缀上。
     *
     * [例]839647521是1--9的排列。1—9的排列最前面的是123456789，最后面的987654321，
     * 从右向左扫描若都是增的，就到了987654321，也就没有下一个了。否则找出第一次出现下降的位置。
     *
     * 【例】 如何得到346987521的下一个
     * 1，从尾部往前找第一个P(i-1) < P(i)的位置
     * 3 4 6 <- 9 <- 8 <- 7 <- 5 <- 2 <- 1
     * 最终找到6是第一个变小的数字，记录下6的位置i-1
     *
     * 2，从i位置往后找到最后一个大于6的数
     * 3 4 6 -> 9 -> 8 -> 7 5 2 1
     * 最终找到7的位置，记录位置为m
     *
     * 3，交换位置i-1和m的值
     * 3 4 7 9 8 6 5 2 1
     * 4，倒序i位置后的所有数据
     * 3 4 7 1 2 5 6 8 9
     * 则347125689为346987521的下一个排列
     *
     * @param str
     * @return
     */
 
public ArrayList<String> Permutation2(String str){
        ArrayList<String> list = new ArrayList<String>();
        if(str==null || str.length()==0){
            return list;
        }
        char[] chars = str.toCharArray();
        Arrays.sort(chars);
        list.add(String.valueOf(chars));
        int len = chars.length;
        while(true){
            int lIndex = len-1;
            int rIndex;
            while(lIndex>=1 && chars[lIndex-1]>=chars[lIndex]){
                lIndex--;
            }
            if(lIndex == 0)
                break;
            rIndex = lIndex;
            while(rIndex<len && chars[rIndex]>chars[lIndex-1]){
                rIndex++;
            }
            swap(chars,lIndex-1,rIndex-1);
            reverse(chars,lIndex);
 
            list.add(String.valueOf(chars));
        }
 
        return list;
    }
 
    private void reverse(char[] chars,int k){
        if(chars==null || chars.length<=k)
            return;
        int len = chars.length;
        for(int i=0;i<(len-k)/2;i++){
            int m = k+i;
            int n = len-1-i;
            if(m<=n){
                swap(chars,m,n);
            }
        }
 
    }
```
