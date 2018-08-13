# 栈的压入、弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

## 常规解法

```cpp
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        bool result = false;
        if(pushV.empty() || popV.empty() ||
           pushV.size()!=popV.size())
            return result;
        vector<int> s;
        while(true)
        {
            // push
            if(!pushV.empty())
            {
                s.emplace_back(pushV[0]);
                pushV.erase(pushV.begin());
            }
            else
            {
                reverse(s.begin(), s.end());
                result = popV==s ? true : false;
                break;
            }
            // pop
            if(s.back()==popV[0]) // 用s.back获取vector最后一个元素
            {
                s.pop_back();// 不能用erase删除最后一个元素
                popV.erase(popV.begin());
            }
        }
        return result;
    }
};
```

```cpp
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        if(pushV.empty() || popV.empty() || pushV.size()!=popV.size())
            return false;
        vector<int> stack;
        for(int push_idx=0, pop_idx=0; push_idx<pushV.size(); )
        {
            stack.push_back(pushV[push_idx++]);
            while(pop_idx<popV.size() && stack.back()==popV[pop_idx])
            {
                stack.pop_back();
                pop_idx++;
            }
        }
        return stack.empty();
    }
};
```

## 递归  

结果不正确

```cpp
class Solution {
public:
    bool IsPopOrder(vector<int> pushV,vector<int> popV) {
        if(popV.empty()) return false;
        if(popV.size()==1) return popV==pushV;
        int eidx = 0;
        for(eidx=0; eidx<popV.size(); eidx++)
            if(popV[eidx]==pushV[0])
                break;
        vector<int> pushV_left(pushV.begin()+1, pushV.begin()+eidx);
        vector<int> popV_left(popV.begin(), popV.begin()+eidx-1);
        vector<int> pushV_right(pushV.begin()+eidx, pushV.end());
        vector<int> popV_right(popV.begin()+eidx, popV.end());
        //Arrays.copyOfRange(pushA, 1, eidx+1),Arrays.copyOfRange(popA, 0, eidx)) &&
        //Arrays.copyOfRange(pushA, eidx+1, length),Arrays.copyOfRange(popA, eidx+1, length));
        return IsPopOrder(pushV_left, popV_left) &&
               IsPopOrder(pushV_right, popV_right);
    }
};
```
