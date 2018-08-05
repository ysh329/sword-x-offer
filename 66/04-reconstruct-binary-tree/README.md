# 重建二叉树  

输入某二叉树的前序遍历和中序遍历的结果，请重建出该二叉树。假设输入的前序遍历和中序遍历的结果中都不含重复的数字。例如输入前序遍历序列{1,2,4,7,3,5,6,8}和中序遍历序列{4,7,2,1,5,3,8,6}，则重建二叉树并返回。

## 递归

```cpp
/**
 * Definition for binary tree
 * struct TreeNode {
 *     int val;
 *     TreeNode *left;
 *     TreeNode *right;
 *     TreeNode(int x) : val(x), left(NULL), right(NULL) {}
 * };
 */
class Solution {
public:
    TreeNode* reConstructBinaryTree(vector<int> pre,vector<int> vin) {
        if(pre.empty() || vin.empty() || pre.size()!=vin.size())
            return NULL;
        TreeNode* root = new TreeNode(pre[0]);
        helper(root, pre, vin);
        return root;
    }
    void helper(TreeNode *root, vector<int> pre, vector<int> vin)
    {
        if(pre.empty() && vin.empty())
            return;
        vector<int> pre_left, pre_right;
        vector<int> vin_left, vin_right;
        bool root_found = false;
        for(int eidx=0; eidx<vin.size(); eidx++)
        {
            if(root->val==vin[eidx])
                root_found = true;
            else if(!root_found)
                vin_left.push_back(vin[eidx]);
            else
                vin_right.push_back(vin[eidx]);
        }
        for(int eidx=1; eidx<pre.size(); eidx++)
        {
            int counter = count(vin_left.begin(), vin_left.end(), pre[eidx]);
            if(counter)
                pre_left.push_back(pre[eidx]);
            else
                pre_right.push_back(pre[eidx]);
        }
        if(pre_left.size()>=1)
        {
            root->left = new TreeNode(pre_left[0]);
            helper(root->left, pre_left, vin_left);
        }
        if(pre_right.size()>=1)
        {
            root->right = new TreeNode(pre_right[0]);
            helper(root->right, pre_right, vin_right);
        }
        return;
    }
};
```

## 非递归  

