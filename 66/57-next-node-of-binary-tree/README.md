# 二叉树的下一个结点

给定一个二叉树和其中的一个结点，请找出中序遍历顺序的下一个结点并且返回。注意，树中的结点不仅包含左右子结点，同时包含指向父结点的指针。


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
        // 否则继续向上,遍历其父节点的父节点，重复之前的判断，返回结果
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
