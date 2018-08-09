# 链表中倒数第k个结点

输入一个链表，输出该链表中倒数第k个结点。

## 栈

- stack  

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
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        ListNode *kNode = NULL;
        if(pListHead==NULL || k<=0) return kNode;
        
        stack<ListNode *> s;
        unsigned int count = 0;
        
        while(pListHead!=NULL)
        {
            ++count;
            s.push(pListHead);
            pListHead = pListHead->next;
        }
        if(k>count) return kNode;
        
        k -= 1;
        while(k--) s.pop();
        kNode = s.top();
        return kNode;
    }
};
```

- vector

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
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        ListNode *kNode = NULL;
        if(pListHead==NULL || k<=0) return kNode;
        
        vector<ListNode *> vec;
        unsigned int count = 0;
        
        while(pListHead!=NULL)
        {
            ++count;
            vec.push_back(pListHead);
            pListHead = pListHead->next;
        }
        if(k>count) return kNode;
        kNode = vec[count-k];
        return kNode;
    }
};
```

## 双指针

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
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        if(pListHead==NULL || k<=0) return NULL;
        int count = 0;
        ListNode *kNode = pListHead;
        while(pListHead)
        {
            count++;
            if(count>k)
                kNode = kNode->next;
            pListHead = pListHead->next;
        }
        return count>=k ? kNode : NULL;
    }
};
```
