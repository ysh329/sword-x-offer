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

## 哈希表

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
public:
    RandomListNode* Clone(RandomListNode* pHead) {
        if(!pHead) return NULL;
        unordered_multimap<RandomListNode*, RandomListNode*> table;
        RandomListNode* pClonedHead = new RandomListNode(pHead->label);
        table.insert(make_pair(pHead, pClonedHead));
        RandomListNode* p = pHead->next;
        RandomListNode* pCloned = pClonedHead;
        while(p) { // 复制next
            RandomListNode* pClonedNew = new RandomListNode(p->label);
            pCloned->next = pClonedNew;
            pCloned = pClonedNew;
            table.insert(make_pair(p, pClonedNew));
            p = p->next;
        }
        p = pHead; pCloned = pClonedHead;
        while(p) { // 复制random
            if(p->random)
                pCloned->random = table.find(p->random)->second;
            p = p->next;
            pCloned = pCloned->next;
        }
        return pClonedHead;
    }
};
```

## 三步法

- 在原链表每个元素后接新赋值的元素节点，之后建立随机成员，再断链  

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
public:
    void CloneNodes(RandomListNode* pHead) {
        RandomListNode* p = pHead;
        while(p) {
            RandomListNode* pCloned = new RandomListNode(p->label);
            pCloned->next = p->next;
            p->next = pCloned;
            p = pCloned->next;
        }
    }
    void ConnectRandomNodes(RandomListNode* pHead) {
        RandomListNode* p = pHead;
        while(p) {
            if(p->random)
                p->next->random = p->random->next;
            p= p->next?p->next->next:NULL;
        }
    }
    RandomListNode* ReConnectNodes(RandomListNode* pHead) {
        RandomListNode* p = pHead;
        RandomListNode* pClonedHead = NULL;
        RandomListNode* pCloned = NULL;
        if(p) {
            pClonedHead = pCloned = p->next;
            p->next = pCloned->next;
            p = p->next;
        }
        while(p) {
            pCloned->next = p->next;
            pCloned = pCloned->next;
            p->next = pCloned->next;
            p = p->next;
        } return pClonedHead;
    }
    RandomListNode* Clone(RandomListNode* pHead) {
        CloneNodes(pHead);
        ConnectRandomNodes(pHead);
        return ReConnectNodes(pHead);
    }
};
```

下面代码输出结果为空，很奇怪

```cpp
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
    void CopyNode(RandomListNode* pHead) {
        RandomListNode* p = pHead;
        while(p) {
            RandomListNode *pNew = new RandomListNode(p->label);
            pNew->next = p->next;
            p->next = pNew;
            p = pNew->next;
        } return;
    }
    void ConnectRand(RandomListNode* pHead) {
        RandomListNode* p = pHead;
        while(p) {
            if(p->random)
                p->next->random = p->random->next;
            p = p->next ? p->next->next : NULL;
        } return;
    }
    RandomListNode* Reconnect(RandomListNode* pHead) {
        RandomListNode* p = pHead;
        RandomListNode* pNew = p->next;
        while(pNew) {
            pNew->next = pNew->next ? pNew->next->next : NULL;
            pNew = pNew->next;
        } return pHead ? pHead->next : NULL;
    }
public:
    RandomListNode* Clone(RandomListNode* pHead) {
        CopyNode(pHead);// copy node
        ConnectRand(pHead);// connect rand
        return Reconnect(pHead);// reconnect
    }
};
```

## 递归

- 递归方法由于没有考虑random的复制，因而不对，但可通过OJ  

```python
# -*- coding:utf-8 -*-
# class RandomListNode:
#     def __init__(self, x):
#         self.label = x
#         self.next = None
#         self.random = None
class Solution:
    # 返回 RandomListNode
    def Clone(self, head):
        if not head: return
        newNode = RandomListNode(head.label)
        newNode.random = head.random
        newNode.next = self.Clone(head.next)
        return newNode
```

## 图

```cpp
/*
struct RandomListNode {
    int label;
    struct RandomListNode *next, *random;
    RandomListNode(int x) :
            label(x), next(NULL), random(NULL) {
    }
};
*/
class Solution {
    map<RandomListNode*, RandomListNode*> mp;
    set<RandomListNode*> vis;
    void dfs1(RandomListNode* p) {
        if(p && mp.find(p) == mp.end()) {
            mp[p] = new RandomListNode(p->label);
            dfs1(p->next);
            dfs1(p->random);
        }
    }
    void dfs2(RandomListNode* p) {
        if(p && vis.find(p) == vis.end()) {
            if(p->next) mp[p]->next = mp[p->next];
            if(p->random) mp[p]->random = mp[p->random];
            vis.insert(p);
            dfs2(p->next);
            dfs2(p->random);
        }
    }
public:
    RandomListNode* Clone(RandomListNode* pHead){
        if(!pHead) return NULL;
        mp.clear(); vis.clear();
        dfs1(pHead); dfs2(pHead);
        return mp[pHead];
    }
};
```
