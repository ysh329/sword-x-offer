# 树的子结构

输入两棵二叉树A，B，判断B是不是A的子结构。（ps：我们约定空树不是任意一个树的子结构）

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
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        bool result = false;
        if(pRoot1!=NULL && pRoot2!=NULL)
        {
            if(pRoot1->val == pRoot2->val)
                result = DoesTree1HasTree2(pRoot1, pRoot2);
            if(!result)
                result = HasSubtree(pRoot1->left, pRoot2);
            if(!result)
                result = HasSubtree(pRoot1->right, pRoot2);
        }
        return result;
    }
    
    bool DoesTree1HasTree2(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        if(pRoot2==NULL) // 子树到叶子
            return true;
        if(pRoot1==NULL) // 树1到叶子，子树没到叶子
            return false;
        if(pRoot1->val != pRoot2->val)
            return false;
        return DoesTree1HasTree2(pRoot1->left,  pRoot2->left) &&
               DoesTree1HasTree2(pRoot1->right, pRoot2->right);
    }
};
```

## 非递归

## 转为字符串查找问题

正确率11.11%，没通过：

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
    bool HasSubtree(TreeNode* pRoot1, TreeNode* pRoot2)
    {
        if(pRoot1==NULL || pRoot2==NULL) return false;
        vector<int> v1, v2;
        PreOrderTravel(pRoot1, v1);
        PreOrderTravel(pRoot2, v2);
        string s1, s2;
        for(int eidx=0; eidx<v1.size(); eidx++)
            s1 += to_string(v1[eidx]);
        for(int eidx=0; eidx<v2.size(); eidx++)
            s2 += to_string(v2[eidx]);
        //printf("s1:%s\ts2:%s\n", s1.c_str(), s2.c_str());
        return s1.find(s2)>=0 ? true: false;
    }
    void PreOrderTravel(TreeNode* pRoot, vector<int> &v)
    {
        if(pRoot==NULL) return;
        v.push_back(pRoot->val);
        PreOrderTravel(pRoot->left, v);
        PreOrderTravel(pRoot->right, v);
        return;
    }
};
```

