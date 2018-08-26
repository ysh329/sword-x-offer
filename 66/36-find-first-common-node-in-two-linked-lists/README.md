# 两个链表的第一个公共结点  

输入两个单向链表，找出它们的第一个公共结点。

## 暴力破解

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
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        for(ListNode* p1=pHead1; p1; p1=p1->next)
            for(ListNode* p2=pHead2; p2; p2=p2->next)
                if(p1==p2) return p1;
        return NULL;
    }
};
```

## 栈

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
    stack<ListNode*> getListNodeStack(ListNode* p) {
        stack<ListNode*> s;
        while(p) {
            s.push(p);
            p = p->next;
        }
        return s;
    }
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        stack<ListNode*> s1 = getListNodeStack(pHead1);
        stack<ListNode*> s2 = getListNodeStack(pHead2);
        ListNode* pCommon = NULL;
        while(s1.size() && s2.size() && s1.top()==s2.top()) {
            pCommon = s1.top();
            s1.pop(); s2.pop();
        }
        return pCommon;
    }
};
```

## 链表特点

1. 先分别计算两个单项链表长度len1, len2   
2. 让长的如pHead1先走abs(len1-len2)个结点  
3. 验证pHead1与pHead2是否相同
4. 与另一个链表一起走到下一步next 
重复3,4，若相同则返回其中任何以一个，若其中一个链表为NULL，则结果为NULL（没找到公共结点）  

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
    int GetLinkListLength(ListNode* p) {
        int len = 0;
        while(p) {len++; p=p->next;}
        return len;
    }
    ListNode* WalkStep(ListNode* p, int step) {
        while(step--) p=p->next;
        return p;
    }
public:
    ListNode* FindFirstCommonNode(ListNode* pHead1, ListNode* pHead2) {
        if(!pHead1 || !pHead2) return NULL;
        int len1 = GetLinkListLength(pHead1);
        int len2 = GetLinkListLength(pHead2);
        if(len1>len2) pHead1 = WalkStep(pHead1, len1-len2);
        else pHead2 = WalkStep(pHead2, len2-len1);
        while(pHead1 && pHead2) {
            if(pHead1==pHead2) return pHead1;
            pHead1 = pHead1->next;
            pHead2 = pHead2->next;
        }
        return NULL;
    }
};
```
