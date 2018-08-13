# 从上往下打印二叉树

从上往下打印出二叉树的每个节点，同层节点从左至右打印。

## 队列

```cpp
/*
struct TreeNode {
	int val;
	struct TreeNode *left;
	struct TreeNode *right;
	TreeNode(int x) :
			val(x), left(NULL), right(NULL) {
	}
};*/
class Solution {
public:
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        vector<int> result;
        if(root==NULL) return result;
        queue<TreeNode*> que;
        que.push(root);
        while(!que.empty())
        {
            TreeNode *node = que.front(); que.pop();
            result.push_back(node->val);
            if(node->left) que.push(node->left);
            if(node->right) que.push(node->right);
        }
        return result;
    }
};
```
