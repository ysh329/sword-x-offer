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
};*/
class Solution {
public:
    vector<int> PrintFromTopToBottom(TreeNode* root) {
        vector<int> result;
        deque<TreeNode*> current;
        current.push_back(root);
        helper(current, result);
        return result;
    }
    void helper(deque<TreeNode*> &current, vector<int> &result)
    {
        if(current.empty())
            return;
        deque<TreeNode*> next;
        while(current.size())
        {
            TreeNode *node = current.front();
            if(node!=NULL) result.push_back(node->val);
            if(node->left!=NULL) next.push_back(node->left);
            if(node->right!=NULL) next.push_back(node->right);
            current.pop_front();
        }
        helper(next, result);
    }
};
```
