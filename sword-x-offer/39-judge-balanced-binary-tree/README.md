# 平衡二叉树

输入一棵二叉树，判断该二叉树是否是平衡二叉树。

## 从上到下

```cpp
class Solution {
    int getDepth(TreeNode* pRoot) {
        if(!pRoot) return 0;
        int left = getDepth(pRoot->left);
        int right = getDepth(pRoot->right);
        return max(left, right) + 1;
    }
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
        if(!pRoot) return true;
        int left = getDepth(pRoot->left);
        int right = getDepth(pRoot->right);
        if(abs(left-right)>1) return false;
        return IsBalanced_Solution(pRoot->left) && IsBalanced_Solution(pRoot->right);
    }
};
```

## 从下到上

```cpp
class Solution {
    bool IsBalanced(TreeNode* pRoot, int* depth) {
        if(!pRoot) {
            *depth = 0;
            return true;
        }
        int left = 0, right = 0;
        if(IsBalanced(pRoot->left, &left) &&
           IsBalanced(pRoot->right, &right)) {
            if(abs(left-right)<=1) {
                *depth = 1 + max(left, right);
                return true;
            }
        }
        return false;
    }
public:
    bool IsBalanced_Solution(TreeNode* pRoot) {
        int depth = 0;
        return IsBalanced(pRoot, &depth);
    }
};
```
