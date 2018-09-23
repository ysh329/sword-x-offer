# 二叉树的深度

输入一棵二叉树，求该树的深度。从根结点到叶结点依次经过的结点（含根、叶结点）形成树的一条路径，最长路径的长度为树的深度。

## 层序遍历

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
    int TreeDepth(TreeNode* pRoot) {
        int depth = 0;
        if(!pRoot) return depth;
        queue<TreeNode*> que; que.push(pRoot);
        while(que.size()) {
            depth++;
            int current_layer_node_size = que.size();
            for(int nidx=0; nidx<current_layer_node_size; nidx++) {
                TreeNode* node = que.front();
                que.pop();
                if(node->left) que.push(node->left);
                if(node->right) que.push(node->right);
            }
        }
        return depth;
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
    int TreeDepth(TreeNode* pRoot) {
        if(!pRoot) return 0;
        int left = TreeDepth(pRoot->left);
        int right = TreeDepth(pRoot->right);
        return (left>right?left:right) + 1;
    }
};
```

```cpp
class Solution {
public:
    int TreeDepth(TreeNode* pRoot){
        if(!pRoot) return 0;
        return max(TreeDepth(pRoot->left), TreeDepth(pRoot->right)) + 1;
    }
};
```
