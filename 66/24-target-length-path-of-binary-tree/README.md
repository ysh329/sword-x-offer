# 二叉树中和为某一值的路径

输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

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
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        vector<vector<int>> paths;
        if(root) {
            vector<int> path;
            int currentSum = 0;
            FindPath1d(root, expectNumber, paths, path, currentSum);
        }
        return paths;
    }
    void FindPath1d(TreeNode* root,
                    int expectSum,
                    vector<vector<int>>& paths,
                    vector<int>& path,
                    int& currentSum)
    {
        currentSum += root->val;
        path.push_back(root->val);
        bool isRootLeaf = (root->left==NULL && root->right==NULL);
        if (expectSum==currentSum && isRootLeaf)
        {
            paths.push_back(path);
            path.empty();
        }
        if (root->left) FindPath1d(root->left, expectSum, paths, path, currentSum);
        if (root->right) FindPath1d(root->right, expectSum, paths, path, currentSum);
        currentSum -= root->val;
        path.pop_back();
    }
};
```

## 非递归

```cpp
```
