# 二叉搜索树与双向链表

输入一棵二叉搜索树，将该二叉搜索树转换成一个排序的双向链表。要求不能创建任何新的结点，只能调整树中结点指针的指向。

## Morris遍历

- Morris遍历:Morris Traversal，将二叉树重构为所有结点只有右子树的一条链  
- 非递归、O(1)空间复杂度  

```cpp
链接：https://www.nowcoder.com/questionTerminal/947f6eb80d944a84850b0538bf0ec3a5
来源：牛客网

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
