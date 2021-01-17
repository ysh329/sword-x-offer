# 从尾到头打印链表 

输入一个链表，按链表值从尾到头的顺序返回一个ArrayList。

## 栈1

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
        if(!head) return result;
        while(head) {
            result.push_back(head->val);
            head = head->next;
        }
        reverse(result.begin(), result.end());
        return result;
    }
};
```

## 栈2

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
        vector<int> res;
        if (!head) return res;
        stack<int> s;
        while(head) {
            s.push(head->val);
            head = head->next;
        }
        while(s.size()) {
            res.push_back(s.top());
            s.pop();
        }
        return res;
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
        if(!head) return vector<int>();
        vector<int> result;
        helper(head, result);
        reverse(result.begin(), result.end());
        return result;
    }
    void helper(ListNode* head, vector<int> &result) {
        if(!head) return;
        result.push_back(head->val);
        helper(head->next, result);
        return;
    }
};
```
