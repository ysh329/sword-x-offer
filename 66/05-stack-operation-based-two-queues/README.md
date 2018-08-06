# 用两个栈实现队列  

用两个栈来实现一个队列，完成队列的Push和Pop操作。 队列中的元素为int类型。

## 常规解法

```cpp
class Solution
{
public:
    void push(int node) {
        stack1.push(node);
    }

    int pop() {
        if(stack1.empty() && stack2.empty())
        {
            printf("ERROR: empty\n");
            return -1;
        }
        if(!stack2.empty())
        {
            int node = stack2.top();
            stack2.pop();
            return node;
        }
        while(!stack1.empty())
        {
            stack2.push(stack1.pop());
            stack1.pop();
        }
        return pop();
    }

private:
    stack<int> stack1;
    stack<int> stack2;
};
```
