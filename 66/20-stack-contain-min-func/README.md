# 包含min函数的栈  

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

## 常规解法

```cpp
class Solution {
public:
    stack<int> s1;
    stack<int> s2; // min stack
    void push(int value) {
        s1.push(value);
        if(s2.empty() || value<s2.top())
            s2.push(value);
        else
            s2.push(s2.top());
    }
    void pop() {
        s1.pop();
        s2.pop();
    }
    int top() {
        return s1.top();
    }
    int min() {
        if(s1.empty() || s2.empty())
        {
            printf("error: empty\n");
            exit(0);
        }
        return s2.top();
    }
};
```
