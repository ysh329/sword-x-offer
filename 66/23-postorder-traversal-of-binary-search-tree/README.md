# 二叉搜索树的后序遍历序列

输入一个整数数组，判断该数组是不是某二叉搜索树的后序遍历的结果。如果是则输出Yes,否则输出No。假设输入的数组的任意两个数字都互不相同。

## 递归  

- 有个坑在于，为空oj要求返回false，因而需另一个函数，对子数组做空数组为true的判断  

```cpp
class Solution {
public:
    bool VerifySquenceOfBST(vector<int> sequence) {
        if(sequence.empty()) return false;
        return helper(sequence);
    }
    bool helper(vector<int> sequence) {
        if(sequence.size()<=1) return true;
        int root = sequence.back(); sequence.pop_back();
        int eidx = 0;
        vector<int> pre, post;
        for(eidx=0; eidx<sequence.size(); eidx++)
            if(sequence[eidx]>root)
                break;
            else
                pre.push_back(sequence[eidx]);
        for(; eidx<sequence.size(); eidx++)
            if(sequence[eidx]<root)
                break;
            else
                post.push_back(sequence[eidx]);
        if(eidx!=sequence.size()) return false;
        return helper(pre) && helper(post);
    }
};
```
