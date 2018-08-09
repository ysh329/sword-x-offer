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

## 递归

代码不正确，还需要想想如何实现~！

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
        unsigned int tail_flag = 0;
        unsigned int count = 0;
        kNode = helper(pListHead, &count, k, &tail_flag);
        return kNode;
    }
    
    ListNode* helper(ListNode* pListHead, unsigned int *count, unsigned int k, unsigned int *tail_flag)
    {
        if(*tail_flag==0)
        {
            if(pListHead->next==NULL)
            {
                *tail_flag = 1;
                *count += 1;
                return pListHead;
            }
            else
            {
                return helper(pListHead->next, count, k, tail_flag);
            }
        }
        if(*tail_flag==1)
        {
            if(*count==k)
                return pListHead;
            *count += 1;
            return helper(pListHead->next, count, k, tail_flag);
        }
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
