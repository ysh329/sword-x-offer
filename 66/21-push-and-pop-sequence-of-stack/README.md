# 栈的压入、弹出序列

输入两个整数序列，第一个序列表示栈的压入顺序，请判断第二个序列是否可能为该栈的弹出顺序。假设压入栈的所有数字均不相等。例如序列1,2,3,4,5是某栈的压入顺序，序列4,5,3,2,1是该压栈序列对应的一个弹出序列，但4,3,5,1,2就不可能是该压栈序列的弹出序列。（注意：这两个序列的长度是相等的）

## 

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
