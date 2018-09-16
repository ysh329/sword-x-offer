# 按之字形顺序打印二叉树

请实现一个函数按照之字形打印二叉树，即第一行按照从左到右的顺序打印，第二层按照从右至左的顺序打印，第三行按照从左到右的顺序打印，其他行以此类推。


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
        unsigned int layer_idx = 1;
        queue<TreeNode*> q;
        q.push(pRoot);
        while(q.size()) {
            vector<int> layer_vals;
            const unsigned int node_size = q.size();
            for(unsigned int nidx = 0; nidx < node_size; nidx++) {
                TreeNode *p = q.front(); q.pop();
                layer_vals.emplace_back(p->val);
                if(p->left) q.push(p->left);
                if(p->right) q.push(p->right);
            }
            if(layer_idx%2==0)
                reverse(layer_vals.begin(), layer_vals.end());
            res.emplace_back(layer_vals);
            layer_idx++;
        }
        return res;
    }
};
```

### 非递归2：两个栈

1. 创建奇数栈s1与偶数栈s2，分别用来保存奇数层节点，偶数层节点。层数从第一层算起；
2. 奇数栈的存储顺序从左往右，偶数栈的存储顺序从右往左。这样确保了top拿到结果的顺序为之字形；
3. 当弹出奇数栈节点时，以从左到右的方式，保存节点的左右孩子到偶数栈，因栈先进后出，保证偶数栈出栈的顺序为先右后左；
4. 当弹出偶数栈节点时，以从右到左的方式，保存节点的右左孩子到奇数栈，因栈先进后出，保证奇数栈出栈的顺序为先左后右。

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
        stack<TreeNode*> s1, s2; // s1: odd index stack, s2: even index stack
        s1.push(pRoot);          // stack index starts from 1
        while(s1.size() || s2.size()) {
            vector<int> layer_vals;
            if(s1.size()) {
                while(s1.size()) {
                    TreeNode *p = s1.top(); s1.pop();
                    layer_vals.emplace_back(p->val);
                    if(p->left)  s2.push(p->left);
                    if(p->right) s2.push(p->right);
                }
            }
            else if(s2.size()) {
                while(s2.size()) {
                    TreeNode *p = s2.top(); s2.pop();
                    layer_vals.emplace_back(p->val);
                    if(p->right) s1.push(p->right);
                    if(p->left)  s1.push(p->left);
                }
            }
            res.emplace_back(layer_vals);
        }
        return res;
    }
};
```

## 递归

- 递归每层节点保存为结果  
- 按层数为列表的索引值存为子列表    
- 将奇数层反转

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
    bool Traversal(TreeNode* pRoot, unsigned int depth) {
        if(!pRoot) return true;
        if(res.size() == depth)
            res.emplace_back(vector<int>{pRoot->val});
        else
            res[depth].emplace_back(pRoot->val);
        return Traversal(pRoot->left, depth+1) &&
               Traversal(pRoot->right, depth+1);
    }
public:
    vector<vector<int> > res;
    vector<vector<int> > Print(TreeNode* pRoot) {
        Traversal(pRoot, 0);
        for(int layer_idx = 0; layer_idx<res.size(); layer_idx++) {
            vector<int>& layer_vals = res[layer_idx];
            if(layer_idx&1 == 1) //奇数层反转
                reverse(layer_vals.begin(), layer_vals.end());
        }
        return res;
    }
};
```
