# 链表中环的入口结点

给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

## 分析

1. 设定两个指针p1与p2，p1初始化为pHead，p2初始化为pHead->next；  
2. p1一次走一步，p2一次走两步。当两个指针相等且非空时（相等为空则无环），设定计数器loop_length=0；  
3. p1与p2继续走，p1每走一次loop_length++，再次相遇时，loop_length为链表中环的长度；
4. 重新设定指针p1与p2为pHead，均每次走一步，让p1先走loop_length，再让p2开始走，直到相遇则为环的入口。

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
    ListNode* EntryNodeOfLoop(ListNode* pHead) {
        if(!pHead || !pHead->next) return nullptr;
        ListNode *pSlow = pHead;
        ListNode *pFast = pHead->next;
        while(pSlow!=pFast) {
            if(!pFast) return nullptr;
            pSlow = pSlow->next;
            pFast = pFast->next->next;
        }
        int loop_length = 1;
        pSlow = pSlow->next;
        pFast = pFast->next->next;
        while(pSlow!=pFast) {
            if(!pFast) return nullptr;
            loop_length++;
            pSlow = pSlow->next;
            pFast = pFast->next->next;
        }
        pSlow = pFast = pHead;
        while(loop_length--) pFast = pFast->next;
        while(pFast==pSlow) {
            pFast = pFast->next;
            pSlow = pSlow->next;
        }
        return pSlow;
    }
};
```
