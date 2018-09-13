# 链表中环的入口结点

给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

## 分析

1. 设定两个指针pFast=pHead->next->next(注：pHead->next!=nullptr), pSlow=pHead；  
2. p1一次走一步，p2一次走两步。当两个指针相等且非空时（相等为空则无环），设定计数器loop_length=0；  
3. p1与p2继续走，p1每走一次loop_length++，再次相遇时，loop_length为链表中环的长度；  
4. 重新设定指针p1与p2为pHead，均每次走一步，让p1先走loop_length，再让p2开始走，直到相遇则为环的入口。

该过程可进一步化简：

- 当第一次相遇时，假设pFast,pSlow都走了x步，pFast走了2x，pSlow走了x，相遇必然是在环内，pFast比pSlow多走了一圈，那么一圈长度相当于2x-x=x，即pSlow走过的路程竟然也是x，即一圈的长度。  
- 那么，就可以省略上面的第二步中的loop_length变量，以及第4步中让p1从pHead处走loop_length步，因为此时，pSlow与pFast就位于距离pHead的loop_length的位置，现在设定pFast为pHead，与pSlow均每次走一步，直到二者一样，即相遇，则为环的入口节点。

简化后的完整流程如下：

1. pFast = pHead->next->next, pSlow = pHead;  
2. pFast一次2步，pSlow一次1步，直到相遇且非nullptr；  
3. 设pFast = pHead，pFast改为一次1步，与pSlow同时走，直到相遇，相遇点为环入口。

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
        ListNode *pFast = pHead;
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
        ListNode *pFast = pHead;
        ListNode *pSlow = pHead;
        while(pFast->next && pSlow) {
            pFast = pFast->next->next;
            pSlow = pSlow->next;
            if(pFast == pSlow) {
                pFast = pHead;
                while(pFast != pSlow) {
                    pFast = pFast->next;
                    pSlow = pSlow->next;
                }
                return pFast;
            }
        }
        return nullptr;
    }
};
```
