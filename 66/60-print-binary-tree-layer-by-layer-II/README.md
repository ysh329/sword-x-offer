# 把二叉树打印成多行

从上到下按层打印二叉树，同一层结点从左至右输出。每一层输出一行。

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
        vector<vector<int> > res;
        if(!pRoot) return res;
        queue<TreeNode*> q1, q2;
        q1.push(pRoot);
        while(q1.size() || q2.size()){
            vector<int> v1, v2;
            while(q1.size()){
                TreeNode *p = q1.front(); q1.pop();
                v1.push_back(p->val);
                if(p->left)  q2.push(p->left);
                if(p->right) q2.push(p->right);
            }
            if(v1.size())
                res.push_back(v1);
            while(q2.size()){
                TreeNode *p = q2.front(); q2.pop();
                v2.push_back(p->val);
                if(p->left)   q1.push(p->left);
                if(p->right)  q1.push(p->right);
            }
            if(v2.size())
                res.push_back(v2);
        }
        return res;
    }
};
```

## 递归

### 递归1

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

### 递归2

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
    vector<vector<int> > res;
    void dfs(TreeNode *pRoot, unsigned int depth=0){
        if(!pRoot) return;
        if(depth+1 > res.size())
            res.resize(depth+1);
        res[depth].emplace_back(pRoot->val);
        dfs(pRoot->left,  depth+1);
        dfs(pRoot->right, depth+1);
    }
    vector<vector<int> > Print(TreeNode* pRoot) {
        res.clear();
        dfs(pRoot);
        return res;
    }
};
```
