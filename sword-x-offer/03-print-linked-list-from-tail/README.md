# 从尾到头打印链表 

输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。

## 栈

```cpp
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        vector<int> result;
        if(head==NULL)
            return result;
        while(head)
        {
            result.push_back(head->val);
            head = head->next;
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

## 递归

```cpp
/**
*  struct ListNode {
*        int val;
*        struct ListNode *next;
*        ListNode(int x) :
*              val(x), next(NULL) {
*        }
*  };
*/
class Solution {
public:
    vector<int> printListFromTailToHead(ListNode* head) {
        if(!head)
            return vector<int>();
        vector<int> result;
        helper(head, result);
        reverse(result.begin(), result.end());
        return result;
    }
    void helper(ListNode* head, vector<int> &result)
    {
        if(head==NULL)
            return;
        result.push_back(head->val);
        helper(head->next, result);
        return;
    }
};
```
