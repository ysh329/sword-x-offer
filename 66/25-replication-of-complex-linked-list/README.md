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
链接：https://www.nowcoder.com/questionTerminal/f836b2c43afc4b35ad6adc41ec941dba
来源：牛客网

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
public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        if(pHead==NULL)
            return NULL;
         
        //定义一个哈希表
        unordered_multimap<RandomListNode*,RandomListNode*> table;
         
        // 开辟一个头结点
        RandomListNode* pClonedHead=new RandomListNode(pHead->label);
        pClonedHead->next=NULL;
        pClonedHead->random=NULL;
         
        // 将头结点放入map中
        table.insert(make_pair(pHead,pClonedHead));
         
        //设置操作指针
        RandomListNode* pNode=pHead->next;
        RandomListNode* pClonedNode=pClonedHead;
         
        // 第一遍先将简单链表复制一下
        while(pNode!=NULL)
        {
            // 不断开辟pNode的拷贝结点
            RandomListNode* pClonedTail=new RandomListNode(pNode->label);
            pClonedTail->next=NULL;
            pClonedTail->random=NULL;
             
            //连接新节点，更新当前节点
            pClonedNode->next=pClonedTail;
            pClonedNode=pClonedTail;
             
            //将对应关系  插入到哈希表中
            table.insert(make_pair(pNode,pClonedTail));
             
            //向后移动操作节点
            pNode=pNode->next;
        }
         
        //需从头开始设置random节点，设置操作指针
        pNode=pHead;
        pClonedNode=pClonedHead;
         
        // 根据map中保存的数据，找到对应的节点
        while(pNode!=NULL)
        {
             
            if(pNode->random!=NULL)
            {
                //找到对应节点，更新复制链表
                pClonedNode->random=table.find(pNode->random)->second;
            }
             
            //向后移动操作节点
            pNode=pNode->next;
            pClonedNode=pClonedNode->next;
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
链接：https://www.nowcoder.com/questionTerminal/f836b2c43afc4b35ad6adc41ec941dba
来源：牛客网

class Solution {
    map<RandomListNode*, RandomListNode*> mp;
    set<RandomListNode*> vis;
    void dfs1(RandomListNode* u){
        if(u && mp.find(u) == mp.end()) {
            mp[u] = new RandomListNode(u -> label);
            dfs1(u -> next);
            dfs1(u -> random);
        }
    }
    void dfs2(RandomListNode* u){
        if(u && vis.find(u) == vis.end()){
            if(u -> next) mp[u] -> next = mp[u -> next];
            if(u -> random) mp[u] -> random = mp[u -> random];
            vis.insert(u);
            dfs2(u -> next);
            dfs2(u -> random);
        }
    }
public:
    RandomListNode* Clone(RandomListNode* pHead){
        if(!pHead) return NULL;
        mp.clear();
        vis.clear();
        dfs1(pHead);
        dfs2(pHead);
        return mp[pHead];
    }
};
```
