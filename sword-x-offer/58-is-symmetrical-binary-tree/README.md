# 对称的二叉树

请实现一个函数，用来判断一颗二叉树是不是对称的。注意，如果一个二叉树同此二叉树的镜像是同样的，定义其为对称的。


## 递归

### 递归1

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
    bool cmp(TreeNode *l, TreeNode *r)
    {
        if(!l) return l==r;
        if(!r) return false;
        return l->val==r->val &&
               cmp(l->left, r->right) &&
               cmp(l->right, r->left);
    }
    bool isSymmetrical(TreeNode* pRoot)
    {
        if(!pRoot) return true;
        return cmp(pRoot->left, pRoot->right);
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
    bool isSymmetrical(TreeNode* pRoot) {
        return same(pRoot, pRoot);//注：这里是pRoot，pRoot，不是pRoot->left, pRoot->right
    }
    bool same(TreeNode* l, TreeNode* r) {
        if(l && r && l->val==r->val)
            return same(l->left, r->right) && same(l->right, r->left);
        return !l && !r;
    }
};
```

## BFS

用栈或者队列

### BFS1：队列

用队列来保存，注意栈的先入先出的特点，注意取与存时候的顺序

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
    bool isSymmetrical(TreeNode* pRoot) {
        if(!pRoot) return true;
        queue<TreeNode*> q;
        q.push(pRoot->left);
        q.push(pRoot->right);
        while(q.size()) {
            TreeNode *l = q.front(); q.pop();
            TreeNode *r = q.front(); q.pop();
            if(!l && !r) continue;
            if(!l || !r || l->val!=r->val) return false;
            q.push(l->left);  q.push(r->right);
            q.push(l->right); q.push(r->left);
        }
        return true;
    }
};
```

### BFS2：栈

用栈来保存，注意栈的先入后出的特点，注意取与存时候的顺序

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
    bool isSymmetrical(TreeNode* pRoot) {
        if(!pRoot) return true;
        stack<TreeNode*> s;
        s.push(pRoot->left);
        s.push(pRoot->right);
        while(s.size()) {
            TreeNode *r = s.top(); s.pop();
            TreeNode *l = s.top(); s.pop();
            if(!r && !l) continue;
            else if(!r || !l || r->val!=l->val) return false;
            s.push(l->left);  s.push(r->right);
            s.push(l->right); s.push(r->left);
        }
        return true;
    }
};
```
