# 平衡二叉树

输入一棵二叉树，判断该二叉树是否是平衡二叉树。

```cpp
class Solution {
    int TreeDepth(TreeNode* pRoot) {
        if (!pRoot) return 0;
        int left = TreeDepth(pRoot->left);
        int right = TreeDepth(pRoot->right);
        return 1+(left>right?left:right);
    }
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
        if (!pRoot) return true;
        int left = TreeDepth(pRoot->left);
        int right = TreeDepth(pRoot->right);
        if (left-right>1 || left-right<-1) return false;
        return (IsBalanced_Solution(pRoot->left) && IsBalanced_Solution(pRoot->right));
    }
};
```


```cpp
class Solution {
    bool IsBalanced(TreeNode* pRoot, int *depth)
    {
        if(!pRoot) {
            *depth = 0;
            return true;
        }
        int left = 0;
        int right = 0;
        if(IsBalanced(pRoot->left, &left) && IsBalanced(pRoot->right, &right)) {
            int diff = left - right;
            if (diff<=1 && diff>=-1) {
                *depth = 1 + (left>right?left:right);
                return true;
            }
        }
        return false;
    }
};
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
        int depth = 0;
        return IsBalanced(pRoot, &depth);
    }
```
