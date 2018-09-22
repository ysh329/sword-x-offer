# 序列化二叉树

请实现两个函数，分别用来序列化和反序列化二叉树

## 前序遍历

### 前序遍历1

- 对于序列化：使用前序遍历，递归的将二叉树的值转化为字符，并且在每次二叉树的结点不为空时，在转化val所得的字符之后添加一个','作为分割。对于空节点则以 '#' 代替；
- 对于反序列化：按照前序顺序，递归的使用字符串中的字符创建一个二叉树(特别注意：在递归时，递归函数的参数一定要是`char **`，这样才能保证每次递归后指向字符串的指针会随着递归的进行而移动)；
- `*str`是指向字符串中str这个字符的指针，因为指针要往后移，要指向下一个字符，也就是说过程中我们要改变指针，所以应该传这个指针的地址，所以二级指针。

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

### 前序遍历2

- 根据前序遍历规则完成序列化与反序列化；
- 序列化指的是遍历二叉树为字符串；
- 反序列化指的是依据字符串重新构造成二叉树；
- 依据前序遍历序列来序列化二叉树，因为前序遍历序列是从根结点开始的。当在遍历二叉树时碰到Null指针时，这些Null指针被序列化为一个特殊的字符“#”；
- 结点之间的数值用逗号隔开。
    
下面是Java代码：

```java
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

根据Java改写的cpp代码，本地测试通过，OJ未通过：
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
    stringstream ss;
    int index = -1;
public:
    // 序列化
    void tree2str(TreeNode *p) {
        if(!p) {
            ss << "#,";
            return;
        }
        ss << p->val;
        Serialize(p->left);
        Serialize(p->right);
        return;
    }
    char* Serialize(TreeNode *root) {    
        ss.str("");
        tree2str(root);
        string res = ss.str();
        return const_cast<char*>(res.c_str());
    }
    // 反序列化
    vector<string> split(const string& s, const string& delim) {
        vector<string> elems;
        size_t pos = 0;
        size_t len = s.length();
        size_t delim_len = delim.length();
        if(delim_len == 0) return elems;
        while(pos < len) {
            int find_pos = s.find(delim, pos);
            if(find_pos < 0) {
                elems.push_back(s.substr(pos, len - pos));
                break;
            }
            elems.push_back(s.substr(pos, find_pos - pos));
            pos = find_pos + delim_len;
        }
        return elems;
    }
    TreeNode* str2tree(vector<string>& str_vec) {
        index++;
        TreeNode* p = nullptr;
        if(index<int(str_vec.size()) && str_vec[index]!="#") {
            int val = stoi(str_vec[index]);
            p = new TreeNode(val);
            p->left = str2tree(str_vec);
            p->right = str2tree(str_vec);
        }
        return p;
    }
    TreeNode* Deserialize(char *str) {
        string s = str;
        vector<string> str_vec = split(s, ",");
        TreeNode *root = str2tree(str_vec);
        return root;
    }
};
```

### 前序遍历3

- 时间复杂度：O(N)  
- 缺点：与大小端有关

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
    vector<int> buf;
public:
    void dfs(TreeNode *p) {
        if(!p) buf.emplace_back(0x233);
        else {
            buf.emplace_back(p->val);
            dfs(p->left);
            dfs(p->right);
        }
    }
    char* Serialize(TreeNode *p) {
        buf.clear();
        dfs(p);
        int *res = new int[buf.size()];
        for(unsigned int i=0; i<buf.size(); i++) res[i] = buf[i];
        return (char *)res;
    }
    TreeNode* dfs2(int*& p) {//注这里是指针的引用，以代替二级指针
        if(*p==0x233) {
            ++p;
            return nullptr;
        }
        TreeNode *res = new TreeNode(*p);
        ++p;
        res->left = dfs2(p);
        res->right = dfs2(p);
        return res;
    }
    TreeNode* Deserialize(char *str) {
        int *p = (int*)str;
        return dfs2(p);
    }
};
```
