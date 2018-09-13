# 链表中环的入口结点

给一个链表，若其中包含环，请找出该链表的环的入口结点，否则，输出null。

## 常规解法

1. 设定两个指针pFast(注：pHead->next!=nullptr), pSlow；  
2. p1一次走一步，p2一次走两步。直到两个指针相等且非空时（相等为空则无环），设定计数器loop_length=0；  
3. p1与p2继续走，p1每走一次loop_length++，再次相遇时，loop_length为链表中环的长度；  
4. 重新设定指针p1与p2为pHead，均每次走一步，让p1先走loop_length，再让p2开始走，直到相遇则为环的入口。

该过程可进一步化简：

- 当第一次相遇时，假设pFast,pSlow都走了x步，pFast走了2x，pSlow走了x，相遇必然是在环内，pFast比pSlow多走了一圈，那么一圈长度相当于2x-x=x，即pSlow走过的路程竟然也是x，即一圈的长度。  
- 那么，就可以省略上面的第二步中的loop_length变量，以及第4步中让p1从pHead处走loop_length步，因为此时，pSlow与pFast就位于距离pHead的loop_length的位置，现在设定pFast为pHead，与pSlow均每次走一步，直到二者一样，即相遇，则为环的入口节点。

简化后的完整流程如下：

1. 设定两个指针: pFast = pHead, pSlow = pHead;  
2. pFast一次2步，pSlow一次1步，直到相遇且非nullptr；  
3. 设pFast = pHead，pFast和pSlow同时每次走1步，直到相遇，相遇点为环入口。

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
        while(pSlow && pFast->next) {
            pSlow = pSlow->next;
            pFast = pFast->next->next;
            if(pSlow == pFast) {
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

## 断链法

- 设定两个指针：pSlow = pHead, pFast = pHead->next；  
- 每次先断开pSlow->next，用pFast来pSlow，pFast再向前走一步，：pSlow->next = nullptr, pSlow = pFast, pFast = pFast->next；
- 若出现环，则在先前已经被断开：pSlow->next = nullptr，此节点也是pFast->next为nullptr的节点，则更新后的pFast为nullptr，pSlow则为环入口节点。  

注：若链表无环，则返回的pSlow为倒数第一个节点。

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
        ListNode *pFast = pHead->next;
        ListNode *pSlow = pHead;
        while(pFast) {
            pSlow->next = nullptr;
            pSlow = pFast;
            pFast = pFast->next;
        }
        return pSlow;
    }
};
```

## 哈希表

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
        map<ListNode*, int> mp;
        for(ListNode *p = pHead; p; p=p->next) {
            ;
        }
    }
};
```

## set

```cpp
```
