# 反转链表

输入一个链表，反转链表后，输出新链表的表头。

## 循环头插法

```cpp
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
        if(pHead==NULL) return NULL;
        ListNode *pre = NULL;
        ListNode *next = NULL;
        while(pHead!=NULL)
        {
            next = pHead->next;
            pHead->next = pre;
            pre = pHead;
            pHead = next;
        }
        return pre;
    }
};
```

```cpp
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* ReverseList(ListNode* pHead) {
        ListNode *pre = NULL;
        ListNode *next = NULL;
        for(ListNode *p = pHead; p; )
        {
            next = p->next;
            p->next = pre;
            pre = p;
            p = next;
        }
        return pre;
    }
};
```
