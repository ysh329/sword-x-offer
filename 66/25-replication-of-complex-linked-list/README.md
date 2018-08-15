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

输出结果为空，不正确

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
    void CopyNode(RandomListNode* pHeadNew, RandomListNode* pHead) {
        RandomListNode* p = pHead;
        RandomListNode* pNew = pHeadNew;
        while(p) {
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
    void Reconnect(RandomListNode* pHeadNew) {
        RandomListNode* pNew = pHeadNew;
        while(pNew) {
            pNew->next = (pNew->next && pNew->next->next) ? pNew->next->next->next:NULL;
            pNew = pNew->next;
        } return;
    }
public:
    RandomListNode* Clone(RandomListNode* pHead) {
        if(!pHead) return NULL;
        RandomListNode* pHeadNew = new RandomListNode(pHead->label);
        if(!pHead->next) return pHeadNew;
        
        // copy node
        CopyNode(pHeadNew, pHead);
        // connect rand
        ConnectRand(pHead);
        // reconnect
        Reconnect(pHeadNew);
        return pHeadNew;
    }
};
```

## 递归

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
 
/*递归思想：把大问题转化若干子问题
  此题转化为一个头结点和除去头结点剩余部分，剩余部分操作和原问题一致
*/
class Solution {
public:
    RandomListNode* Clone(RandomListNode* pHead)
    {
        if(pHead==NULL)
            return NULL;
         
        //开辟一个新节点
        RandomListNode* pClonedHead=new RandomListNode(pHead->label);
        pClonedHead->next = pHead->next;
        pClonedHead->random = pHead->random;
         
        //递归其他节点
        pClonedHead->next=Clone(pHead->next);
         
        return pClonedHead;
    }
};
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
