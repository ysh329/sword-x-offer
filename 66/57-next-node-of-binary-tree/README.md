# 二叉树的下一个结点

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。

## 根据中序遍历特点

```
1. 以给定的节点`pNode`开始找，中序遍历顺序为：左根右；  
2. 则下一节点应从给定节点`pNode`的右子节点开始找；  
  2.1 若存在右子节点：  
    2.1.1 右子节点没左子节点，则返回右子节点，为下一个节点；  
    2.1.2 右子节点有左子节点，则一直找左子节点的左子节点，直到为`nullptr`，返回最后一个非`nullptr`的左子节点，为下一个节点；  
  2.2 若不存在右子节点，找该节点是否有父节点：  
    2.2.1 不存在父节点，即自己就是父节点，返回`nullptr`，遍历结束；  
    2.2.2 存在父节点：  
      2.2.2.1 若该节点为其父节点为左孩子，则返回父节点；  
      2.2.2.2 若该节点为其父节点的右边孩子，则重复2.2，继续找父节点。
```

```cpp
/*
struct TreeLinkNode {
    int val;
    struct TreeLinkNode *left;
    struct TreeLinkNode *right;
    struct TreeLinkNode *next;
    TreeLinkNode(int x) :val(x), left(NULL), right(NULL), next(NULL) {
        
    }
};
*/
class Solution {
public:
    TreeLinkNode* GetNext(TreeLinkNode* pNode) {
        // 1.二叉树为空
        if(!pNode) return nullptr;
        
        // 2.右孩子存在，则设置一个指针从该节点的右孩子出发，
        // 一直沿着指向左子结点的指针,
        // 找到的叶子节点即为下一个节点；
        if(pNode->right) {
            pNode = pNode->right;
            while(pNode->left)
                pNode = pNode->left;
            return pNode;
        }
        
        // 3.节点不是根节点。
        // 如果该节点是其父节点的左孩子，则返回父节点；
        // 否则（该节点为父节点的右孩子）继续向上,遍历其父节点的父节点，重复之前的判断，返回结果
        while(pNode->next) {
            TreeLinkNode *parent = pNode->next;
            if(parent->left==pNode) //注意：是parent->left，不是parent
                return parent;      // 返回父节点，即下一个
            pNode = pNode->next;
        }
        return nullptr;
    }
};
```

## 先找到根节点得到中序遍历

 1. 找到根节点；  
 2. 以中序遍历方式得到中序遍历结果(中序遍历顺序：左根右)；  
 3. 找到要查的节点对应在中序遍历结果中的下一个节点。
 
```cpp
/*
struct TreeLinkNode {
    int val;
    struct TreeLinkNode *left;
    struct TreeLinkNode *right;
    struct TreeLinkNode *next;
    TreeLinkNode(int x) :val(x), left(NULL), right(NULL), next(NULL) {
        
    }
};
*/
class Solution {
    vector<TreeLinkNode*> midTravelRes;
    void midTravel(TreeLinkNode *pRoot) {
        if(!pRoot) return;
        midTravel(pRoot->left);
        midTravelRes.emplace_back(pRoot);
        midTravel(pRoot->right);
    }
public:
    TreeLinkNode* GetNext(TreeLinkNode* pNode) {
        if(!pNode) return nullptr
        TreeLinkNode *pRoot = pNode;
        while(pRoot->next) pRoot = pRoot->next;
        midTravel(pRoot);
        vector<TreeLinkNode*>::iterator resIter = find(midTravelRes.begin(), midTravelRes.end(), pNode);
        return (resIter==midTravelRes.end()) ? nullptr : *(resIter+1);
    }
};
```

补充：查找`iterator`类型的元素`resIter`所在`vector`的位置  
1. `int resIterIdx = resIter - midTravelRes.begin()`  
2. `int resIterIdx = distance(midTravelRes.begin(), resIter);`
3. 若要取得`resIter`的下一个元素，则可直接操作`*(resIter+1)`来得到  
4. 注意上下界的检查  
 
 
