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

//打印第level行的节点，有打印输出则返回true，否则返回false 
bool printLevel(Tree tree, int level) {
	if(!tree || level < 0) {
		return false;
	}
	if(level == 0) {
		cout << tree->val << " ";
		return true;
	}
	return printLevel(tree->left, level - 1) + printLevel(tree->right, level - 1);
}
 
//层序遍历，当某一行没有打印时break掉循环 
void levelOrder(Tree tree) {
	for(int i = 0; ; i++) {
		if(!printLevel(tree, i)) {
			break;
		}
	}
}

```
