# 包含min函数的栈  

定义栈的数据结构，请在该类型中实现一个能够得到栈中所含最小元素的min函数（时间复杂度应为O（1））。

## 常规解法

- 辅助最小栈（s2）元素数目与主栈（s1）相同  

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

- 辅助最小栈（s2）元素数目小于等于主栈（s1）  

```cpp
#include <assert.h>
class Solution {
public:
    stack<int> s1;
    stack<int> s2;
    void push(int value) {
        s1.push(value);
        if(s2.empty() || value<s2.top())
            s2.push(value);
    }
    void pop() {
        assert(!s1.empty() && !s2.empty());
        if(s1.top()==s2.top())
            s2.pop();
        s1.pop();
    }
    int top() {
        assert(!s1.empty());
        return s1.top();
    }
    int min() {
        assert(!s1.empty() && !s2.empty());
        return s2.top();
    }
};
```
