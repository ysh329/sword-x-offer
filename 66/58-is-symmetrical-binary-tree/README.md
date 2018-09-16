# 对称的二叉树

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。


## 递归

```cpp
/*
struct TreeNode {
    int val;
    struct TreeNode *left;
    struct TreeNode *right;
    TreeNode(int x) :
            val(x), left(NULL), right(NULL) {
    }
};
*/
class Solution {
    bool cmpRoot(TreeNode *pLeft, TreeNode *pRight) {
        if(!pLeft) return pLeft==pRight;
        if(!pRight) return false;
        if(pLeft->val != pRight->val) return false;
        return cmpRoot(pLeft->left, pRight->right) &&
            cmpRoot(pLeft->right, pRight->left);
    }
public:
    bool isSymmetrical(TreeNode* pRoot) {
        if(!pRoot) return true;
        return cmpRoot(pRoot->left, pRoot->right);
    }
};
```
