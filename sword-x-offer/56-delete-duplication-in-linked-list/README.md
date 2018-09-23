# 删除链表中重复的结点

- 在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。   
- 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5。  

## 非递归

### 非递归1：先找头结点

1. 找到头节点。找到后确保链表中所有数不是一样的（用last_val保留上一节点的值，判断得到的头节点pHead是否为最后一个节点，是否与上一个节点的值相同）；  
2. 去重复。根据当前节点p，观察p->next与p->next->next的值是否相同；  
    2.1 若相同，则将p->next指向存在的p->next->next->next，且继续continue，后续去重；  
    2.2 若不同，则继续下一个节点p = p->next。  

```cpp
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead)
    {
        if(!pHead || !pHead->next) return pHead;
        int last_val = pHead->val;
        // 找到第一个头节点
        while(pHead) {
            if(pHead->next && pHead->val==pHead->next->val) {
                last_val = pHead->next->val;
                pHead = pHead->next->next ? pHead->next->next : nullptr;
            }
            else break;
        }
        // 检查头节点是否为最后一个节点，且上一节点值一样
        if(pHead && !pHead->next && pHead->val==last_val)
            pHead = pHead->next;
        // 去重
        ListNode *p = pHead;
        while(p) {
            if(p->next && p->next->next &&
               p->next->val==p->next->next->val) {
                p->next = p->next->next->next;
                continue;
            }
            p = p->next;
        }
        return pHead;
    }
};
```

### 非递归2：虚拟一个头结点

```cpp
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead)
    {
        ListNode *pStart = new ListNode(-1); //临时头结点
        pStart->next = pHead;                //起始结点(临时)
        ListNode *pLast = pStart;            //上一结点
        ListNode *p = pHead;                 //用于遍历的结点
        while(p && p->next) {
            if(p->val == p->next->val) {
                int p_val = p->val;
                while(p && p_val == p->val)
                    p = p->next;
                pLast->next = p;
            }
            else {
                pLast = p;
                p = p->next;
            }
        }
        return pStart->next;
    }
};
```

### 非递归3：虚拟一个头结点

```cpp
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead) {
        if(!pHead || !pHead->next) return pHead;
        ListNode* pStart = new ListNode(-1); //新建节点防止头结点被删除
        pStart->next = pHead;
        ListNode* pre = pStart;
        ListNode* p = pHead;
        ListNode* next = nullptr;
        while(p && p->next) {
            next=p->next;
            if(p->val==next->val) {
                while(next && p->val==next->val) //向后重复查找
                    next = next->next;
                pre->next = next; //用不重复的结点重新定义
                p = next;
            }
            else { //如果当前节点和下一个节点值不等，则向后移动一位
                pre = p;
                p = p->next;
            }
        }
        return pStart->next;
    }
};
```

## 递归

```cpp
/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
        val(x), next(NULL) {
    }
};
*/
class Solution {
public:
    ListNode* deleteDuplication(ListNode* pHead) {
        if(!pHead || !pHead->next) return pHead; //少于两个结点返回
        if(pHead->val == pHead->next->val) { //相同，继续查重
            ListNode *pNext = pHead->next;
            while(pNext && pHead->val==pNext->val)
                pNext = pNext->next;
            return deleteDuplication(pNext);//直接返回或作为pHead->next的值返回
        }
        else { //不同，继续下一结点
            pHead->next = deleteDuplication(pHead->next);
            return pHead;
        }
    }
};
```
