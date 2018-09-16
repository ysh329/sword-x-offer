# 删除链表中重复的结点

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5

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
        if(!pHead || !pHead->next) // 0或1个节点，直接返回
            return pHead;
        if(pHead->val == pHead->next->val) // 第一个节点与第二个重复
        {
            ListNode *pNode = pHead->next;
            while(pNode && pNode->val==pHead->val)
                pNode = pNode->next;
            return deleteDuplication(pNode);
        }
        else //从不重复的节点开始链接，最后返回
        {
            pHead->next = deleteDuplication(pHead->next);
            return pHead;
        }

    }
};
```

## 非递归

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
