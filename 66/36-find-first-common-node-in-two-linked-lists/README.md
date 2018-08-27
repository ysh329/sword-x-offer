# 两个链表的第一个公共结点  

输入两个单向链表，找出它们的第一个公共结点。

## 暴力破解

```cpp
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        for(ListNode* p1=pHead1; p1; p1=p1->next)
            for(ListNode* p2=pHead2; p2; p2=p2->next)
                if(p1==p2) return p1;
        return NULL;
    }
};
```

## 栈

```cpp
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
    stack<ListNode*> getListNodeStack(ListNode* p) {
        stack<ListNode*> s;
        while(p) {
            s.push(p);
            p = p->next;
        }
        return s;
    }
public:
    ListNode* FindFirstCommonNode( ListNode* pHead1, ListNode* pHead2) {
        stack<ListNode*> s1 = getListNodeStack(pHead1);
        stack<ListNode*> s2 = getListNodeStack(pHead2);
        ListNode* pCommon = NULL;
        while(s1.size() && s2.size() && s1.top()==s2.top()) {
            pCommon = s1.top();
            s1.pop(); s2.pop();
        }
        return pCommon;
    }
};
```

## 链表特点

1. 先分别计算两个单项链表长度len1, len2   
2. 让长的如pHead1先走abs(len1-len2)个结点  
3. 验证pHead1与pHead2是否相同
4. 与另一个链表一起走到下一步next 
重复3,4，若相同则返回其中任何以一个，若其中一个链表为NULL，则结果为NULL（没找到公共结点）  

```cpp
/*
struct ListNode {
	int val;
	struct ListNode *next;
	ListNode(int x) :
			val(x), next(NULL) {
	}
};*/
class Solution {
    int GetLinkListLength(ListNode* p) {
        int len = 0;
        while(p) {len++; p=p->next;}
        return len;
    }
    ListNode* WalkStep(ListNode* p, int step) {
        while(step--) p=p->next;
        return p;
    }
public:
    ListNode* FindFirstCommonNode(ListNode* pHead1, ListNode* pHead2) {
        if(!pHead1 || !pHead2) return NULL;
        int len1 = GetLinkListLength(pHead1);
        int len2 = GetLinkListLength(pHead2);
        if(len1>len2) pHead1 = WalkStep(pHead1, len1-len2);
        else pHead2 = WalkStep(pHead2, len2-len1);
        while(pHead1 && pHead2) {
            if(pHead1==pHead2) return pHead1;
            pHead1 = pHead1->next;
            pHead2 = pHead2->next;
        }
        return NULL;
    }
};
```

## set

```cpp
链接：https://www.nowcoder.com/questionTerminal/6ab1d9a29e88450685099d45c9e31e46
来源：牛客网

/*
struct ListNode {
    int val;
    struct ListNode *next;
    ListNode(int x) :
            val(x), next(NULL) {
    }
};*/
typedef ListNode node;
typedef node* pnode;
class Solution {
public:
    ListNode* FindFirstCommonNode(pnode p, pnode q) {
        set<pnode> s;
        while(p || q){
            if(p){
                if(s.find(p) == s.end()) s.insert(p), p = p -> next;
                else return p;
            }
            if(q){
                if(s.find(q) == s.end()) s.insert(q), q = q -> next;
                else return q;
            }
        }
        return NULL;
    }
};
```


## 其他

```cpp
链接：https://www.nowcoder.com/questionTerminal/6ab1d9a29e88450685099d45c9e31e46
来源：牛客网

public class Solution {
    /**
     * 思路：如果有公共节点，1）若两个链表长度相等，那么遍历一遍后，在某个时刻，p1 == p2
     *                   2)若两个链表长度不相等，那么短的那个链表的指针pn（也就是p1或p2）
     *                     必先为null，那么这时再另pn = 链表头节点。经过一段时间后，
     *                     则一定会出现p1 == p2。
     *      如果没有公共节点：这种情况可以看成是公共节点为null，顾不用再考虑。
     */
    public ListNode FindFirstCommonNode(ListNode pHead1, ListNode pHead2) {
        ListNode p1 = pHead1;
        ListNode p2 = pHead2;
        while(p1 != p2) {
            if(p1 != null) p1 = p1.next;    //防止空指针异常
            if(p2 != null) p2 = p2.next;
            if(p1 != p2) {                  //当两个链表长度不想等
                if(p1 == null) p1 = pHead1;
                if(p2 == null) p2 = pHead2;
            }
        }
        return p1;
    }
}
```

```cpp
链接：https://www.nowcoder.com/questionTerminal/6ab1d9a29e88450685099d45c9e31e46
来源：牛客网

//用map做的，时间复杂度O(nlog(n))
classSolution {
public:
    ListNode* FindFirstCommonNode( ListNode *pHead1, ListNode *pHead2) {
        map<ListNode*, int> m;
        ListNode *p = pHead1;
        while(p != NULL) {
            m[p] = 1;
            p = p->next;
        }
        p = pHead2;
        while(p != NULL) {
            if(m[p]) {
                return p;
            }
            p = p->next;
        }
        return NULL;
    }
};
```

```cpp
链接：https://www.nowcoder.com/questionTerminal/6ab1d9a29e88450685099d45c9e31e46
来源：牛客网

//原理其他方法一样，写法比较简单，同时代码有解释。
class Solution {
public:
   ListNode* FindFirstCommonNode(ListNode* pHead1, ListNode* pHead2) {
       /*
        假定 List1长度: a+n  List2 长度:b+n, 且 a<b
        那么 p1 会先到链表尾部, 这时p2 走到 a+n位置,将p1换成List2头部
        接着p2 再走b+n-(n+a) =b-a 步到链表尾部,这时p1也走到List2的b-a位置，还差a步就到可能的第一个公共节点。
        将p2 换成 List1头部，p2走a步也到可能的第一个公共节点。如果恰好p1==p2,那么p1就是第一个公共节点。  或者p1和p2一起走n步到达列表尾部，二者没有公共节点，退出循环。 同理a>=b.
        时间复杂度O(n+a+b)
         
       */
        ListNode* p1 = pHead1;
        ListNode* p2 = pHead2;
        while(p1 != p2) {
            if(p1 != NULL) p1 = p1->next;   
            if(p2 != NULL) p2 = p2->next;
            if(p1 != p2) {                  
                if(p1 == NULL) p1 = pHead2;
                if(p2 == NULL) p2 = pHead1;
            }
        }
        return p1;
     
}
        
};
```

```cpp
class Solution {
public:
    ListNode* FindFirstCommonNode( ListNode *pHead1, ListNode *pHead2) {
        ListNode *p1 = pHead1;
        ListNode *p2 = pHead2;
        while(p1!=p2){
            p1 = (p1==NULL ? pHead2 : p1->next);
            p2 = (p2==NULL ? pHead1 : p2->next);
        }
        return p1;
    }
};
```
