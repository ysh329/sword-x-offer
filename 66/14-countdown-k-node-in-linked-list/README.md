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

- 链接：https://www.nowcoder.com/questionTerminal/529d3ae5a407492994ad2a246518148a  
- 来源：牛客网  

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
    unsigned int cnt = 0;
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k) {
        if(pListHead==NULL)
            return NULL;
        ListNode* node=FindKthToTail(pListHead->next, k);
        if(node!=NULL)
            return node;
        cnt++;
        if(cnt==k)
            return pListHead;
        else
            return NULL;
    }
};
```

- 下面的递归代码不正确，还需要想想如何不需要全局变量情况下实现~！

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

- 下面代码基于第一个递归代码改的，也不正确  

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
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k)
    {
        if(pListHead==NULL || k<=0) return NULL;
        unsigned int cnt = 0;
        ListNode *kNode = FindKthToTail(pListHead, k, &cnt);
        return kNode;
    }
    
    ListNode* FindKthToTail(ListNode* pListHead, unsigned int k, unsigned int *cnt) {
        if(pListHead==NULL)
            return NULL;
        FindKthToTail(pListHead->next, k, cnt);
        *cnt++;
        if(*cnt==k)
            return pListHead;
        return NULL;
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
