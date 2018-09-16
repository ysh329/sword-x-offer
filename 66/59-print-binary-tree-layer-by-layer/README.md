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
        stack<TreeNode*> s1, s2; //s1: odd index stack, s2: even index stack
        s1.push(pRoot);
        while(s1.size() || s2.size()) {
            vector<int> layer_vals;
            if(s1.size()) {
                while(s1.size()) {
                    TreeNode *node = s1.top(); s1.pop();
                    layer_vals.emplace_back(node->val);
                    if(node->left)  s2.push(node->left);
                    if(node->right) s2.push(node->right);
                }
                res.emplace_back(layer_vals);
            }
            else if(s2.size()) {
                while(s2.size()) {
                    TreeNode *node = s2.top(); s2.pop();
                    layer_vals.emplace_back(node->val);
                    if(node->right) s1.push(node->right);
                    if(node->left)  s1.push(node->left);
                }
                res.emplace_back(layer_vals);
            }
        }
        return res;
    }
};
```

## 递归

```python
链接：https://www.nowcoder.com/questionTerminal/91b69814117f4e8097390d107d2efbe0
来源：牛客网

# Python 解法，用递归遍历每一层节点，并按层数为列表的索引值存为子列表
# 然后把奇数索引的子列表倒序即可
 
# -*- coding:utf-8 -*-
# class TreeNode:
#     def __init__(self, x):
#         self.val = x
#         self.left = None
#         self.right = None
class Solution:
    def Print(self, pRoot):
        # write code here
        res = list()
        def Traversal(node, depth=0):
            if not node:
                return True
            if len(res) == depth:
                res.append([node.val])
            else:
                res[depth].append(node.val)
            return Traversal(node.left, depth+1) and Traversal(node.right, depth+1)
        Traversal(pRoot)
        for i, v in enumerate(res):
            if i % 2 == 1:
                v.reverse()
        return res
```
