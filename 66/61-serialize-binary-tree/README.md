# 序列化二叉树

请实现两个函数，分别用来序列化和反序列化二叉树


```cpp

链接：https://www.nowcoder.com/questionTerminal/cf7e25aa97c04cc1a68c8f040e71fb84
来源：牛客网

/*
 1. 对于序列化：使用前序遍历，递归的将二叉树的值转化为字符，并且在每次二叉树的结点
不为空时，在转化val所得的字符之后添加一个' ， '作为分割。对于空节点则以 '#' 代替。
 2. 对于反序列化：按照前序顺序，递归的使用字符串中的字符创建一个二叉树(特别注意：
在递归时，递归函数的参数一定要是char ** ，这样才能保证每次递归后指向字符串的指针会
随着递归的进行而移动！！！)
3. *str是指向字符串中 str这个字符 的指针，因为指针要往后移，要指向下一个字符嘛，也就是说过程中我们要改变指针，所以应该传这个指针的地址，所以二级指针
*/

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

Leetcode做法
```cpp
链接：https://www.nowcoder.com/questionTerminal/cf7e25aa97c04cc1a68c8f040e71fb84
来源：牛客网

/*
public class TreeNode {
    int val = 0;
    TreeNode left = null;
    TreeNode right = null;
 
    public TreeNode(int val) {
        this.val = val;
 
    }
 
}
*/
/*
    算法思想：根据前序遍历规则完成序列化与反序列化。所谓序列化指的是遍历二叉树为字符串；所谓反序列化指的是依据字符串重新构造成二叉树。
    依据前序遍历序列来序列化二叉树，因为前序遍历序列是从根结点开始的。当在遍历二叉树时碰到Null指针时，这些Null指针被序列化为一个特殊的字符“#”。
    另外，结点之间的数值用逗号隔开。
*/
public class Solution {
    int index = -1;   //计数变量
  
    String Serialize(TreeNode root) {
        StringBuilder sb = new StringBuilder();
        if(root == null){
            sb.append("#,");
            return sb.toString();
        }
        sb.append(root.val + ",");
        sb.append(Serialize(root.left));
        sb.append(Serialize(root.right));
        return sb.toString();
  }
    TreeNode Deserialize(String str) {
        index++;
        //int len = str.length();
        //if(index >= len){
        //    return null;
       // }
        String[] strr = str.split(",");
        TreeNode node = null;
        if(!strr[index].equals("#")){
            node = new TreeNode(Integer.valueOf(strr[index]));
            node.left = Deserialize(str);
            node.right = Deserialize(str);
        }
        return node;
  }
}
```

O(N)

```cpp
链接：https://www.nowcoder.com/questionTerminal/cf7e25aa97c04cc1a68c8f040e71fb84
来源：牛客网

typedef TreeNode node;
typedef TreeNode* pnode;
typedef int* pint;
class Solution {
    vector<int> buf;
    void dfs(pnode p){
        if(!p) buf.push_back(0x23333);
        else{
            buf.push_back(p -> val);
            dfs(p -> left);
            dfs(p -> right);
        }
    }
    pnode dfs2(pint& p){
        if(*p == 0x23333){
            ++p;
            return NULL;
        }
        pnode res = new node(*p);
        ++p;
        res -> left = dfs2(p);
        res -> right = dfs2(p);
        return res;
    }
public:
    char* Serialize(TreeNode *p) {
        buf.clear();
        dfs(p);
        int *res = new int[buf.size()];
        for(unsigned int i = 0; i < buf.size(); ++i) res[i] = buf[i];
        return (char*)res;
    }
    TreeNode* Deserialize(char *str) {
        int *p = (int*)str;
        return dfs2(p);
    }
};
```
