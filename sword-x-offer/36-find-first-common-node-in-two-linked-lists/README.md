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

### 链表特点1

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

### 链表特点2 

1. p1, p2同时后向遍历  
2. 若p1, p2中某指针第一次为NULL，则其重新赋值为另一个链表头结点（如，若p1为NULL，则将pHead2赋值给它）  
3. 重复1,2，p1与p2都完成第一次遍历，此时重复1，若存在共同结点，则第二次遍历，p1与p2将会共同遇到相同的结点  

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
    ListNode* FindFirstCommonNode( ListNode *pHead1, ListNode *pHead2) {
        ListNode *p1 = pHead1;
        ListNode *p2 = pHead2;
        while(p1!=p2) {
            p1 = !p1 ? pHead2 : p1->next;
            p2 = !p2 ? pHead1 : p2->next;
        }
        return p1;
    }
};
```

## set

- O(n)

```cpp
链接：https://www.nowcoder.com/questionTerminal/6ab1d9a29e88450685099d45c9e31e46
来源：牛客网

/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
            val(x), next(NULL) {
    }
};*/
typedef ListNode node;
typedef node* pnode;
class Solution {
public:
    ListNode* FindFirstCommonNode(pnode p, pnode q) {
        set<pnode> s;
        while(p || q){
            if(p){
                if(s.find(p) == s.end()) s.insert(p), p = p -> next;
                else return p;
            }
            if(q){
                if(s.find(q) == s.end()) s.insert(q), q = q -> next;
                else return q;
            }
        }
        return NULL;
    }
};
```

## 哈希表

- O(n)

```cpp
链接：https://www.nowcoder.com/questionTerminal/6ab1d9a29e88450685099d45c9e31e46
来源：牛客网

classSolution {
public:
    ListNode* FindFirstCommonNode( ListNode *pHead1, ListNode *pHead2) {
        map<ListNode*, int> m;
        ListNode *p = pHead1;
        while(p) {
            m[p] = 1;
            p = p->next;
        }
        p = pHead2;
        while(p) {
            if(m[p]) return p;
            p = p->next;
        }
        return NULL;
    }
};
```

