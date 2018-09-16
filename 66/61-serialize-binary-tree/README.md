# 序列化二叉树

请实现两个函数，分别用来序列化和反序列化二叉树


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
    char* Serialize(TreeNode *root) {
        if(!root)
            return nullptr;
        string str;
        Serialize(root, str);
        char *result = new char[str.length()+1];
        for(int i=0; i<str.length(); i++)
            result[i] = str[i];
        result[str.length()] = '\0';
        return result;
    }

    void Serialize(TreeNode *root, string& str) {    
        if(!root) {
            str += "#";
            return;
        }
        str += to_string(root->val);
        str += ',';
        Serialize(root->left, str);
        Serialize(root->right, str);
    }

    TreeNode* Deserialize(char *str) {
        if(!str) return nullptr;
        TreeNode* root = Deserialize(&str);
        return root;
    }

    TreeNode* Deserialize(char **str) {
        if(**str=='#') {
            ++(*str);
            return nullptr;
        }
        int num = 0;
        while(**str!='\0' && **str!=',') {
            num = num * 10 + ((**str) - '0');
            ++*(str);
        }
        TreeNode* root = new TreeNode(num);
        if(**str=='\0')
            return root;
        else
            (*str)++;
        root->left = Deserialize(str);
        root->right = Deserialize(str);
        return root;
    }
};
```
