# 二叉搜索树与双向链表

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

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
    void ConvertHelper(TreeNode* p, TreeNode*& pre) {// 中序遍历
        if(!p) return;
        ConvertHelper(p->left, pre);
        p->left = pre; // 建立pre<---p
        if(pre) pre->right = p; // 建立pre--->p
        pre = p;
        ConvertHelper(p->right, pre);
    }
public:
    TreeNode* Convert(TreeNode* pRootOfTree) {
        TreeNode* pLastInList = NULL;
        ConvertHelper(pRootOfTree, pLastInList);
        while(pRootOfTree->left)
            pRootOfTree = pRootOfTree->left;
        return pRootOfTree;
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
    TreeNode* pre = NULL;
    TreeNode* pResult = NULL;
public:
    TreeNode* Convert(TreeNode* p) {
        if(!p) return NULL;
        Convert(p->left);
        p->left = pre;// 建立 pre <=== p
        if(pre) pre->right = p;// 建立 pre ===> p
        pre = p;
        pResult = !pResult ? p : pResult;//第一次保留结果后，不再更新
        Convert(p->right);
        return pResult;
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
    TreeNode* pre = NULL;
    TreeNode* pResult = NULL;
public:
    TreeNode* Convert(TreeNode* p) {
        TreeNode *head = NULL, *pre = NULL;//head 输出，pre记录上一次出栈值
        stack<TreeNode*> s;
        while(p || s.size()) {
            while(p) {
                s.push(p);
                p = p->left;
            }
            if(s.size()) {
                p = s.top();
                s.pop();
                if(pre) {
                    pre->right = p; // 建立 pre ===> p
                    p->left = pre;  // 建立 pre <=== p
                }
                head = !head ? p : head; // 第一次保留结果后，不再更新
                pre = p;
                p = p->right;
            }
        }
        return head;
    }
};
```

## Morris遍历

- Morris遍历:Morris Traversal，将二叉树重构为所有结点只有右子树的一条链  
- 非递归、O(1)空间复杂度  

```cpp
//链接：https://www.nowcoder.com/questionTerminal/947f6eb80d944a84850b0538bf0ec3a5
//来源：牛客网

public class Solution {
    public TreeNode Convert(TreeNode pRootOfTree) {
        TreeNode p = pRootOfTree, pre = null, res = null;
        while (p != null) {
            while (p.left != null) {
                TreeNode q = p.left;
                while (q.right != null) {
                    q = q.right;
                }
                q.right = p;
                TreeNode tmp = p.left;
                p.left = null;
                p = tmp;
            }
            p.left = pre;
            if (pre == null) {
                res = p;
            } else {
                pre.right = p;
            }
            pre = p;
            p = p.right;
        }
        return res;
    }
}
```
