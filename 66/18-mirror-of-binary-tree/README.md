# 二叉树的镜像

操作给定的二叉树，将其变换为源二叉树的镜像。

```
二叉树的镜像定义：源二叉树 
    	    8
    	   /  \
    	  6   10
    	 / \  / \
    	5  7  9 11
    	镜像二叉树
    	    8
    	   /  \
    	  10   6
    	 / \  / \
    	11  9 7  5
```

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
    void Mirror(TreeNode *pRoot) {
        if(pRoot==NULL) return;
        TreeNode *tmp;
        tmp = pRoot->left;
        pRoot->left = pRoot->right;
        pRoot->right = tmp;
        Mirror(pRoot->left);
        Mirror(pRoot->right);
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
    void Mirror(TreeNode *pRoot) {
        if(pRoot) {
            swap(pRoot->left, pRoot->right);
            Mirror(pRoot->left);
            Mirror(pRoot->right);
        }
    }
};
```

## 非递归

- 栈  

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
    void Mirror(TreeNode *pRoot) {
        if (pRoot == NULL)return;
        stack<TreeNode*> st;
        TreeNode* p = NULL;
        st.push(pRoot);
        while (st.size())
        {
            p = st.top();
            st.pop();
            swap(p->left, p->right);
            if (p->left) st.push(p->left);
            if (p->right) st.push(p->right);
        }
    }
};
```

- 队列  

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
    void Mirror(TreeNode *pRoot) {
        if (pRoot == NULL)return;
        queue<TreeNode*> qu;
        TreeNode* p = NULL;
        qu.push(pRoot);
        while (qu.size())
        {
            p = qu.front();
            qu.pop();
            swap(p->left, p->right);
            if (p->left)qu.push(p->left);
            if (p->right)qu.push(p->right);
        }
    }
};
```

- 层遍历  

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
    void Mirror(TreeNode *pRoot) {
        if(!pRoot) return;
        vector<TreeNode*> vv;
        vv.push_back(pRoot);
        while(!vv.empty()) {
            vector<TreeNode*> t;
            for (auto &i : vv) {
                swap(i->left, i->right);
                if (i->left) t.push_back(i->left);
                if (i->right) t.push_back(i->right);
            }
            vv.swap(t);
        }
    }
};
```
