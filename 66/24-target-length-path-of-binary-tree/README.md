# 二叉树中和为某一值的路径

输入一颗二叉树的跟节点和一个整数，打印出二叉树中结点值的和为输入整数的所有路径。路径定义为从树的根结点开始往下一直到叶结点所经过的结点形成一条路径。(注意: 在返回值的list中，数组长度大的数组靠前)

## DFS+递归

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
            helper(root, expectNumber, paths, path, currentSum);
        }
        return paths;
    }
    void helper(TreeNode* root,
                int expectSum,
                vector<vector<int>>& paths,
                vector<int>& path,
                int& currentSum)
    {
        currentSum += root->val;
        path.push_back(root->val);
        bool isRootLeaf = (root->left==NULL && root->right==NULL);
        if (expectSum==currentSum && isRootLeaf)
            paths.push_back(path);
        if (root->left) helper(root->left, expectSum, paths, path, currentSum);
        if (root->right) helper(root->right, expectSum, paths, path, currentSum);
        currentSum -= root->val;
        path.pop_back();
    }
};
```

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
    vector<vector<int>> paths;
    vector<int> path;
public:
    vector<vector<int> > FindPath(TreeNode* root,int expectNumber) {
        if(root) {
            path.push_back(root->val);
            if(expectNumber-root->val==0 && !root->left && !root->right)
                paths.push_back(path);
            if(root->left) FindPath(root->left, expectNumber-root->val);
            if(root->right) FindPath(root->right, expectNumber-root->val);
            path.pop_back();
        }
        return paths;
    }
};
```

## 非递归

 

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
    vector<vector<int> > FindPath(TreeNode *root, int expectNum){
        vector<vector<int>> paths;
        if(!root)
            return paths;
        vector<int> curPath;
        int sum = 0;
        stack<TreeNode *> s;
        s.push(root);
        TreeNode *cur = root; //当前节点
        TreeNode *last = NULL; //保存上一个节点
        while(s.size()) {
            if(cur) { // 先序遍历
                s.push(cur);
                sum += cur->val;
                curPath.push_back(cur->val);
                if (!cur->left && !cur->right && sum==expectNum)
                    paths.push_back(curPath);
                cur = cur->left; // 左子树
            }
            else {
                if (s.top()->right && s.top()->right!=last)
                    cur = s.top()->right; // 右子树
                else {// 回溯
                    last = s.top();
                    s.pop();
                    curPath.pop_back();
                    sum -= last->val;
                }
            }
        }
        return paths;
    }
};
```

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
    vector<vector<int> > FindPath(TreeNode* root, int expectNumber) {
        stack<TreeNode*> s;
        vector<int> curPath;
        vector<vector<int> > paths;
        while(root || s.size()){
            while(root) { //能左就左，否则向右
                s.push(root); curPath.push_back(root->val); expectNumber -= root->val;
                root = root->left ? root->left : root->right;
            }
            root = s.top();
            if (expectNumber == 0 && !root->left && !root->right)
                paths.push_back(curPath);
            s.pop(); curPath.pop_back(); expectNumber += root->val;
            //右子数没遍历就遍历，如果遍历就强迫出栈
            if (!s.empty() && s.top()->left == root)
                root = s.top()->right;
            else //强迫出栈
                root = NULL; 
        }
        return paths;
    }
};
```
