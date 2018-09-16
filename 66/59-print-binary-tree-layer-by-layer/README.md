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
