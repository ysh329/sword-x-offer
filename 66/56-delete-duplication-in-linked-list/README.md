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
