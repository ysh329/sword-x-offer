# 删除链表中重复的结点

在一个排序的链表中，存在重复的结点，请删除该链表中重复的结点，重复的结点不保留，返回链表头指针。 例如，链表1->2->3->3->4->4->5 处理后为 1->2->5


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
        if(pHead==NULL || pHead->next==NULL) // 0或1个节点，直接返回
            return pHead;
        if(pHead->val==pHead->next->val) // 第一个节点与第二个重复
        {
            ListNode *pNode = pHead->next;
            while(pNode!=NULL && pNode->val==pHead->val)
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

下面代码不正确：
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
        if(!pHead) return pHead;
        // 计算头结点
        while(pHead) {
            if(pHead->next && pHead->val==pHead->next->val)
                pHead = pHead->next->next;
        }
        //计算后续结点的去重
        ListNode *p = pHead;
        while(p) {
            if(p->next->next &&
               p->next->val == p->next->next->val) {
                p->next = p->next->next->next;
                p = p->next;
            }
        }
        return pHead;
    }
};
```
