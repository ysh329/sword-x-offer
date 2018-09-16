# 把二叉树打印成多行

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

# 按之字形顺序打印二叉树

请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。

做法与上一题类似，只是不需要反转层中结果。

## 非递归

### 非递归1：队列

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
public:
    vector<vector<int> > Print(TreeNode* pRoot) {
        if(!pRoot) return vector<vector<int> >();
        vector<vector<int> > res;
        queue<TreeNode*> q;
        q.push(pRoot);
        while(q.size()) {
            vector<int> layer_vals;
            unsigned int node_size = q.size();
            while(node_size--){
                TreeNode *p = q.front(); q.pop();
                layer_vals.emplace_back(p->val);
                if(p->left) q.push(p->left);
                if(p->right) q.push(p->right);
            }
            res.emplace_back(layer_vals);
        }
        return res;
    }
};
```

### 非递归2：两个队列

```cpp

```

## 递归

- 递归每层节点保存为结果  
- 按层数为列表的索引值存为子列表  

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
    void Traversal(TreeNode* pRoot, unsigned int depth) {
        if(!pRoot) return;
        if(res.size() == depth)
            res.emplace_back(vector<int>{pRoot->val});
        else
            res[depth].emplace_back(pRoot->val);
        Traversal(pRoot->left,  depth+1);
        Traversal(pRoot->right, depth+1);
    }
public:
    vector<vector<int> > res;
    vector<vector<int> > Print(TreeNode* pRoot) {
        Traversal(pRoot, 0);
        return res;
    }
};
```
