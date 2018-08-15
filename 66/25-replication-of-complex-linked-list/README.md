# 复杂链表的复制

输入一个复杂链表（每个节点中有节点值，以及两个指针，一个指向下一个节点，另一个特殊指针指向任意一个节点），返回结果为复制后复杂链表的head。（注意，输出结果中请不要返回参数中的节点引用，否则判题程序会直接返回空）

## 原始解法

```cpp
/*
struct RandomListNode {
    int label;
    struct RandomListNodenext, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
    void CopyListNode(RandomListNode* pHeadNew, RandomListNode* pHead) {
        RandomListNode* p = pHeadNew;
        pHead = pHead->next;
        while(pHead) {
            RandomListNode* pNode = new RandomListNode(pHead->label);
            p->next = pNode; p = p->next;
            pHead = pHead->next;
        } return;
    }
    
    void CopyRandNode(RandomListNode* pHead, RandomListNode* pHeadNew) {
        RandomListNode* p = pHead;
        RandomListNode* pNew = pHeadNew;
        while(p) {
            if(p->random) {
                RandomListNode* pRand = p->random;
                int step = 0;
                RandomListNode* pCur = pHead;
                while(pCur!=pRand) {
                    pCur = pCur->next;
                    step++;
                }
                
                RandomListNode* pCurNew = pHeadNew;
                while(step) {
                    pCurNew = pCurNew->next;
                    step--;
                }
                pNew->random = pCurNew;
            }
            p = p->next;
            pNew = pNew->next;
        } return;
    }
    
public:
    RandomListNode* Clone(RandomListNode* pHead) {
        if(!pHead) return NULL;
        RandomListNode* pHeadNew = new RandomListNode(pHead->label);
        if(!pHead->next) return pHeadNew;
        
        CopyListNode(pHeadNew, pHead);
        CopyRandNode(pHead, pHeadNew);
        return pHeadNew;
    }
};
```
